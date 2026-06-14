# Reference Architecture

## High-level architecture

```mermaid
flowchart LR
    Users[Users / Enterprise Apps] --> APIGW[Amazon API Gateway / Application Load Balancer]
    APIGW --> Auth[Identity / AuthZ / Policy Layer]
    Auth --> Router[AI Request Router]

    Router --> Bedrock[Amazon Bedrock / Amazon Bedrock AgentCore]
    Router --> EKS[Amazon EKS AI Workload Platform]
    Router --> SageMaker[Amazon SageMaker AI / Amazon SageMaker HyperPod]
    Router --> HPC[AWS ParallelCluster + Slurm]
    Router --> External[Neo Cloud / NVIDIA AI Factory]

    Bedrock --> KB[Knowledge Bases for Amazon Bedrock / Vector Store]
    Bedrock --> Tools[Agent Tools via Amazon Bedrock AgentCore Gateway / AWS Lambda / APIs]

    EKS --> VLLM[vLLM / Triton / NIM-compatible Serving]
    EKS --> Kueue[Kueue / Scheduler Layer]
    EKS --> GPU[GPU Node Pools / Device Plugin / DCGM]

    SageMaker --> Train[Distributed Training]
    HPC --> Batch[HPC / Batch Workloads]

    subgraph Governance
      IAM[AWS IAM / AWS IAM Identity Center]
      SCP[AWS Organizations SCPs / AWS IAM Permission Boundaries]
      Guardrails[Guardrails / Policy]
      Cost[AWS Budgets / AWS Cost and Usage Reports / Cost Allocation]
    end

    subgraph Observability
      CW[Amazon CloudWatch]
      Prom[Prometheus]
      Grafana[Grafana]
      Logs[Central Logs]
      SLO[SLO Reports]
    end

    Bedrock --> CW
    EKS --> Prom
    GPU --> Prom
    Prom --> Grafana
    CW --> SLO
    Logs --> SLO
```

## Control plane responsibilities

The AI control plane must answer:

- Who can access which model, tool, data source, and environment?
- Which workloads run on Amazon Bedrock, Amazon EKS, Amazon SageMaker AI, AWS ParallelCluster + Slurm, AWS Trainium, AWS Inferentia, or NVIDIA GPU platforms?
- How are inference SLOs measured?
- How are GPU and token costs attributed?
- How are tenant boundaries enforced?
- How are agent actions governed and audited?
- How are production incidents handled?

## Initial architecture choices

| Area | Default choice | Reason |
|---|---|---|
| Enterprise agent runtime | Amazon Bedrock AgentCore | Managed agent runtime, identity, gateway, memory, and observability |
| General enterprise RAG | Knowledge Bases for Amazon Bedrock | Managed retrieval path and lower platform burden |
| Custom model inference | Amazon EKS + vLLM/Triton | Better control over serving, scaling, and custom runtime needs |
| Distributed training | Amazon SageMaker HyperPod or Amazon EKS | Depends on team maturity and workload scale |
| HPC batch | AWS ParallelCluster + Slurm | Better cultural and scheduler fit for HPC-style workloads |
| GPU-heavy AI factory | NVIDIA stack / Neo Cloud / on-prem | Best for sustained GPU-heavy platforms and specialized networking |
