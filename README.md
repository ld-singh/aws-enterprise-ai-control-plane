# AWS Enterprise AI Control Plane

## Architecture thesis

This repository is a reference architecture for governing and placing enterprise AI workloads on AWS. Its thesis is simple: model access is only one part of an AI platform; the durable architecture is the control plane around identity, data access, workload placement, agent tools, inference, accelerator capacity, observability, security, reliability, and cost.

The design uses a portfolio approach rather than forcing every workload onto one platform. Amazon Bedrock and AgentCore cover managed model and agent capabilities, Amazon EKS covers custom inference, SageMaker and HyperPod cover managed ML and distributed training, and AWS ParallelCluster with Slurm covers HPC-oriented workloads. Trainium, Inferentia, NVIDIA GPUs, and external capacity are evaluated as accelerator or sourcing choices within that model.

> **Design boundary:** This repository provides reference patterns and decision guidance, not a turnkey deployment or an AWS-endorsed architecture. Validate service capabilities, security controls, quotas, regional availability, performance, and cost against workload-specific requirements before implementation.

## Architecture scope

| In scope | Out of scope |
|---|---|
| Enterprise identity, authorization, network, data, audit, and cost controls | A turnkey deployable platform |
| Placement across Bedrock, AgentCore, EKS, SageMaker, HyperPod, and ParallelCluster/Slurm | A universal recommendation for every AI workload |
| Custom inference, managed agents, RAG, training, batch, HPC, and accelerator choices | Model development tutorials or application-specific prompt design |
| Operational concerns such as SLOs, observability, incident response, capacity, and unit economics | Workload-specific benchmarks, validated service limits, or service guarantees |
| Architecture decisions, tradeoffs, diagrams, and example runbooks | Replacement for AWS documentation, security review, or workload testing |

## Navigate the repository

| Read this | Use it for |
|---|---|
| [Start here](docs/00-start-here.md) | Choose a reading path and understand the document set |
| [Reference architecture](docs/01-reference-architecture.md) | See the target control plane, workload platforms, and shared governance layers |
| [Workload placement matrix](docs/02-workload-placement-matrix.md) | Compare managed services, Kubernetes, ML platforms, HPC, and accelerators |
| [Well-Architected mapping](docs/03-well-architected-mapping.md) | Review the design against AWS Well-Architected concerns |
| [Architecture decision records](docs/decision-records/) | Inspect the major platform and accelerator tradeoffs |
| [Runbooks](docs/runbooks/) | See example operational response procedures |
| [Diagrams](docs/diagrams/) | Find architecture diagram notes and assets |
| [Local simulation](lab/local-simulation/) | Explore the local lab area |
| [AWS blueprint](lab/aws-blueprint/) | Explore the AWS implementation blueprint area |

## Repository status

The repository is an evolving architecture foundation. Some sections are design guidance or placeholders rather than complete implementations; each decision should be tested before adoption.
