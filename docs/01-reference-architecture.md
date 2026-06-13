# Reference Architecture

## High-level architecture

```mermaid
flowchart LR
    Users[Users / Enterprise Apps] --> APIGW[API Gateway / App Load Balancer]
    APIGW --> Auth[Identity / AuthZ / Policy Layer]
    Auth --> Router[AI Request Router]

    Router --> Bedrock[Amazon Bedrock / AgentCore]
    Router --> EKS[EKS AI Workload Platform]
    Router --> SageMaker[SageMaker / HyperPod]
    Router --> HPC[AWS ParallelCluster / Slurm]
    Router --> External[Neo Cloud / NVIDIA AI Factory]

    Bedrock --> KB[Knowledge Bases / Vector Store]
    Bedrock --> Tools[Agent Tools via Gateway / Lambda / APIs]

    EKS --> VLLM[vLLM / Triton / NIM-compatible Serving]
    EKS --> Kueue[Kueue / Scheduler Layer]
    EKS --> GPU[GPU Node Pools / Device Plugin / DCGM]

    SageMaker --> Train[Distributed Training]
    HPC --> Batch[HPC / Batch Workloads]

    subgraph Governance
      IAM[IAM / IAM Identity Center]
      SCP[SCPs / Permission Boundaries]
      Guardrails[Guardrails / Policy]
      Cost[Budgets / CUR / Cost Allocation]
    end

    subgraph Observability
      CW[CloudWatch]
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
- Which workloads run on Bedrock, EKS, SageMaker, Slurm, Trainium, Inferentia, or NVIDIA GPU platforms?
- How are inference SLOs measured?
- How are GPU and token costs attributed?
- How are tenant boundaries enforced?
- How are agent actions governed and audited?
- How are production incidents handled?

## Initial architecture choices

| Area | Default choice | Reason |
|---|---|---|
| Enterprise agent runtime | Bedrock AgentCore | Managed agent runtime, identity, gateway, memory, and observability |
| General enterprise RAG | Bedrock Knowledge Bases | Managed retrieval path and lower platform burden |
| Custom model inference | EKS + vLLM/Triton | Better control over serving, scaling, and custom runtime needs |
| Distributed training | SageMaker HyperPod or EKS | Depends on team maturity and workload scale |
| HPC batch | AWS ParallelCluster + Slurm | Better cultural and scheduler fit for HPC-style workloads |
| GPU-heavy AI factory | NVIDIA stack / Neo Cloud / on-prem | Best for sustained GPU-heavy platforms and specialized networking |
