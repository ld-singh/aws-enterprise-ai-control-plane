# Workload Placement Matrix

## Purpose

This document makes workload placement decisions explicit, reviewable, and tied to business and operating constraints. It is a decision framework rather than a substitute for workload-specific validation.

The options below are not all at the same architectural layer:

- Bedrock, Bedrock AgentCore, EKS, SageMaker, and AWS ParallelCluster are workload platforms or managed service boundaries.
- Trainium, Inferentia, and NVIDIA GPUs are accelerator choices used through one or more workload platforms.
- An NVIDIA AI Factory or Neo Cloud is a broader infrastructure and sourcing model that may sit outside, alongside, or integrate with AWS.

Use the matrix in two stages: select the operating model first, then select a compatible accelerator and capacity source.

## Assumptions

- Workloads are subject to enterprise identity, network, data classification, audit, and cost-allocation controls.
- Model quality, latency, throughput, regional availability, quotas, and commercial terms are validated with workload-specific tests before commitment.
- Managed services are preferred where they meet requirements; additional control must justify additional operational ownership.
- Sensitive workloads require explicit review of data flows, service endpoints, encryption boundaries, retention, tenancy, and operator access.
- Cost comparisons use measured end-to-end unit economics, not instance price or token price in isolation.

## Decision

Adopt a portfolio placement model rather than forcing all AI workloads onto one platform. Bedrock is the default for managed foundation model consumption, AgentCore is the reference path for managed agent runtime capabilities, EKS supports custom serving and platform control, SageMaker and HyperPod support managed ML lifecycle and large-scale training needs, and ParallelCluster supports Slurm-aligned HPC workloads. Accelerator selection is a separate decision based on compatibility, performance, availability, portability, and total cost.

## Placement Matrix

| Option | Best fit | Poor fit | Operational burden | Cost considerations | Security considerations | Architecture rationale | Example workload |
|---|---|---|---|---|---|---|---|
| **Amazon Bedrock** | Managed foundation model access, enterprise RAG, summarization, classification, and variable-demand inference where infrastructure control is not the differentiator. | Custom model binaries, unsupported architectures, low-level serving optimization, or workloads requiring direct control of accelerator scheduling and runtime internals. | **Low to medium.** AWS operates model infrastructure; the platform team still owns prompts, evaluations, quotas, resilience, data integration, observability, and governance. | Consumption pricing avoids idle accelerator fleets but can become material at sustained high volume. Compare model, token, caching, provisioned-capacity, and data-retrieval costs using cost per successful business transaction. | Control model access, IAM permissions, private connectivity where required, encryption, logging, guardrails, prompt-injection exposure, and data classification. Confirm model-provider and regional data-handling constraints. | Prefer managed model access when infrastructure control does not create workload value, while retaining governance, evaluation, resilience, and cost controls. | Internal policy assistant using approved models and governed enterprise retrieval. |
| **Bedrock AgentCore** | Agents needing a managed runtime and supporting capabilities for identity, tool access, memory, gateway integration, and observability without building the entire agent platform. | Simple single-call inference, deterministic workflows better served by conventional services, or agents requiring unsupported runtime behavior and deep infrastructure control. | **Medium.** Managed components reduce runtime work, but teams still own agent design, tool contracts, authorization, evaluation, failure handling, audit, and lifecycle controls. | Include runtime usage, model calls, tool calls, memory, observability, downstream APIs, and retry amplification. Agent loops can make per-request cost unpredictable without budgets and termination controls. | Treat tools as privileged APIs. Apply least privilege, user-context propagation, scoped credentials, approval gates for consequential actions, input/output controls, trace protection, and auditable tool invocation. | Use a managed agent runtime when its identity, tool, memory, gateway, and observability capabilities remove material platform work without weakening control boundaries. | Service-desk agent that reads approved knowledge, opens a ticket through a governed tool, and requires approval before changing an account. |
| **EKS + vLLM/Triton** | Custom or open-weight model serving, specialized containers, multi-model routing, GPU scheduling, custom autoscaling, and teams with a mature Kubernetes platform capability. | Small teams seeking the fastest managed path, highly intermittent demand that leaves GPUs idle, or workloads with no justified need for runtime and scheduler control. | **High.** Own cluster lifecycle, node images, drivers, device plugins, serving runtime, model distribution, scaling, upgrades, capacity, SLOs, security patching, and incident response. | Can improve unit economics at sustained utilization, but idle GPUs, overprovisioning, engineering effort, model loading, storage, data transfer, and capacity reservations can erase savings. Measure utilization and cost per successful inference. | Enforce workload identity, tenant isolation, network policy, admission control, image provenance, secrets handling, encrypted model storage, node hardening, API audit, and restricted privileged access. | Choose EKS when runtime, scheduler, portability, or model-serving control justifies ownership of the full platform lifecycle. | Latency-sensitive serving of an approved open-weight model with continuous batching and custom request routing. |
| **SageMaker / SageMaker HyperPod** | Managed ML development and deployment, repeatable training pipelines, distributed training, and durable accelerator clusters where training-specific orchestration and resilience justify the service boundary. | General application hosting, lightweight API workloads, or organizations unwilling to adopt SageMaker operating conventions and associated service integrations. | **Medium for SageMaker; medium to high for HyperPod.** AWS manages more infrastructure, while teams retain responsibility for environments, images, data pipelines, distributed training, capacity planning, checkpoints, monitoring, and governance. | Account for notebooks, processing, training, endpoints, persistent cluster capacity, storage, checkpoints, data transfer, and idle time. HyperPod is justified by sustained workloads and reduced disruption cost. | Separate data science and production roles, control image and package supply chains, use private connectivity where required, encrypt data and artifacts, restrict notebook egress, and audit model promotion. | Select this boundary when managed ML lifecycle and training resilience outweigh the constraints of SageMaker operating conventions. | Multi-node fine-tuning with checkpoint recovery, controlled datasets, experiment tracking, and governed model promotion. |
| **AWS ParallelCluster + Slurm** | HPC-style batch, MPI or tightly coupled jobs, research teams with Slurm workflows, queued accelerator use, and environments where scheduler semantics matter more than service-oriented orchestration. | Always-on online inference, cloud-native microservices, or teams without Slurm and HPC operational capability. | **High.** Own cluster configuration, Slurm policy, images, queues, shared storage, identity integration, scaling behavior, job accounting, upgrades, and scheduler troubleshooting. | Queue-based sharing and elastic nodes can improve utilization. Shared filesystems, head nodes, idle capacity, data staging, interconnect choices, and failed long-running jobs remain major cost drivers. | Map enterprise identity to cluster users, isolate queues and data, constrain login and compute nodes, protect shared storage, control software modules and images, audit jobs, and limit administrative access. | Select Slurm when HPC job semantics, queueing, and established user workflows matter more than service-oriented orchestration. | Genomics or simulation batch pipeline with Slurm queues, shared datasets, and multi-node compute jobs. |
| **AWS Trainium / Inferentia** | AWS-native training or inference where supported models and frameworks can be validated on the Neuron software stack and measured price/performance is favorable. | Workloads dependent on unsupported CUDA libraries, custom kernels without an acceptable porting path, or strong cross-cloud and on-premises binary portability requirements. | **Medium to high.** Infrastructure operations depend on the hosting platform; additional work includes Neuron compatibility, compilation, profiling, model changes, release testing, and specialist debugging. | Evaluate delivered throughput, latency, engineering conversion cost, capacity availability, and utilization. Lower accelerator pricing is not a saving if migration effort or model constraints dominate. | Apply the controls of the hosting platform. Also govern compiler artifacts, model provenance, dependency updates, and access to accelerator capacity and training data. | Adopt only when compatibility testing and measured economics outweigh software-porting effort and ecosystem constraints. | High-volume inference for a validated transformer model compiled and performance-tested on Inferentia. |
| **NVIDIA GPU / AI Factory / Neo Cloud** | CUDA-dependent workloads, broad framework compatibility, specialized NVIDIA software, large GPU estates, hybrid requirements, or capacity sourced from a specialist provider under a clear operating and commercial model. | Modest or bursty workloads that fit managed APIs, teams unable to operate or govern the additional platform, or external capacity that cannot satisfy enterprise data and control requirements. | **High to very high.** Burden varies by managed offering, but may include fleet lifecycle, drivers, firmware, fabric, scheduling, storage, observability, capacity management, provider integration, and hardware-level troubleshooting. | Strong ecosystem compatibility can reduce porting cost, while scarce capacity, minimum commitments, idle clusters, interconnect, data movement, support, and egress can dominate total cost. Compare owned, reserved, and externally sourced capacity using realistic utilization. | Define the trust boundary for the operator and provider, data residency, connectivity, key ownership, tenant isolation, privileged access, supply chain, logging, incident response, deletion, and exit requirements. External capacity requires explicit third-party risk review. | Use NVIDIA or specialist capacity when CUDA compatibility, software dependencies, hybrid placement, or supply requirements justify the operational and commercial complexity. | Sustained distributed model training using CUDA/NCCL and high-bandwidth GPU networking, with overflow capacity from a contracted specialist provider. |

## Placement Rules

1. Start with Bedrock when an approved managed model meets functional, security, regional, and economic requirements.
2. Use AgentCore when agent runtime capabilities materially reduce platform work; do not use an agent where a deterministic workflow is safer and easier to operate.
3. Move to EKS-based serving only when model choice, runtime control, portability, or measured economics justify owning the platform.
4. Use SageMaker for managed ML lifecycle requirements and evaluate HyperPod for sustained, large-scale training where training resilience and cluster continuity matter.
5. Use ParallelCluster and Slurm when the workload and users are HPC-oriented. Scheduler familiarity and job semantics are valid architecture inputs.
6. Select Trainium or Inferentia only after compatibility and performance validation on the target model. Select NVIDIA where ecosystem requirements or portability justify it.
7. Treat Neo Cloud or AI Factory capacity as a sourcing and operating-model decision, not as an automatic workload-placement answer.

## Tradeoffs

| Decision pressure | Favors managed services | Favors customer-managed platforms |
|---|---|---|
| Time to first controlled workload | Bedrock, AgentCore, SageMaker | EKS or Slurm only when a platform already exists |
| Need for custom runtime behavior | Limited | EKS + vLLM/Triton |
| HPC scheduling and research workflow alignment | Limited | ParallelCluster + Slurm |
| Large-scale managed training environment | SageMaker HyperPod | EKS or Slurm when control and existing skills justify ownership |
| Lowest idle-capacity exposure | Consumption-based managed services | Elastic batch clusters with disciplined scale-down |
| Maximum accelerator and software control | Limited | EKS, Slurm, or dedicated NVIDIA platform |
| Portability across infrastructure providers | Model and API dependent | Open runtimes and NVIDIA ecosystem, with added operations |

No option removes operational responsibility. Managed services move the responsibility boundary; they do not remove the need for evaluation, security review, cost governance, observability, and incident ownership.

## Operational Considerations

- Define SLOs by workload type: time to first token, end-to-end latency, throughput, queue wait, job completion, recovery point, and recovery time.
- Require workload-level cost allocation using tokens, requests, accelerator time, job time, storage, and data transfer as appropriate.
- Validate quotas and capacity before selecting a region or committing to an accelerator family.
- Maintain an approved model and runtime catalog with owners, intended use, data classification, evaluation status, and retirement criteria.
- Separate online inference, agent execution, batch inference, experimentation, fine-tuning, and large-scale training into explicit placement classes.
- Test failure modes including provider throttling, unavailable capacity, tool failure, checkpoint recovery, node loss, model-loading failure, and unsafe agent action.
- Revisit placement when demand shape, model architecture, service capability, regulatory requirements, or unit economics materially change.

## Related Decisions

- [ADR-001: Bedrock vs EKS for Inference Workloads](decision-records/adr-001-bedrock-vs-eks-inference.md)
- [ADR-002: AgentCore vs Custom Agent Runtime](decision-records/adr-002-agentcore-vs-custom-agent-runtime.md)
- [ADR-003: Kubernetes vs Slurm for AI Workloads](decision-records/adr-003-kubernetes-vs-slurm.md)
- [ADR-004: SageMaker HyperPod vs Self-Managed EKS](decision-records/adr-004-sagemaker-hyperpod-vs-self-managed-eks.md)
- [ADR-005: Trainium/Inferentia vs NVIDIA GPU](decision-records/adr-005-trainium-inferentia-vs-nvidia-gpu.md)
