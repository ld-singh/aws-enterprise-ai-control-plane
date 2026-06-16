# Start Here

## Project thesis

Enterprise AI architecture is primarily a control-plane problem: organizations must consistently govern who can use models, tools, data, and accelerator capacity; decide where each workload belongs; and measure reliability, security, and cost across multiple execution platforms.

This reference architecture organizes those concerns around shared enterprise controls and a portfolio of AWS workload platforms. It favors managed services where they meet the requirement and introduces customer-managed infrastructure only when runtime control, workload semantics, portability, or measured economics justify the additional operational ownership.

> **Design boundary:** This material describes target-state patterns and tradeoffs. It is not a turnkey deployment or an AWS-endorsed architecture. Service availability, quotas, security controls, performance, and fitness must be validated for each workload and organization.

## Target architecture

The target scope is an enterprise AI control plane spanning:

- shared identity, authorization, network, data, audit, policy, and cost controls;
- managed foundation-model and agent workloads on Amazon Bedrock and Amazon Bedrock AgentCore;
- custom model serving and GPU scheduling on Amazon EKS;
- managed ML lifecycle and distributed training on Amazon SageMaker AI and Amazon SageMaker HyperPod;
- HPC and queued batch workloads on AWS ParallelCluster + Slurm;
- accelerator selection across AWS Trainium, AWS Inferentia, and NVIDIA GPUs; and
- observability, SLOs, capacity management, incident response, and workload-level unit economics.

It does not provide a turnkey deployment, prescribe one platform for every workload, or replace workload-specific security review, benchmarking, quota validation, and cost analysis.

## Reading guide

| Document | Question it answers |
|---|---|
| [README](../README.md) | What is this repository, and what is in scope? |
| [Reference architecture](01-reference-architecture.md) | What components and control-plane responsibilities make up the target design? |
| [Workload placement matrix](02-workload-placement-matrix.md) | Which platform and accelerator fit a given workload and operating model? |
| [Well-Architected mapping](03-well-architected-mapping.md) | How does the design address operational excellence, security, reliability, performance, cost, and sustainability? |
| [Security model](04-security-model.md) | Which shared and platform-specific controls apply to each workload placement path? |
| [Decision records](decision-records/) | Why were the major platform boundaries and defaults selected? |
| [Runbooks](runbooks/) | What would example operational response procedures look like? |
| [Diagrams](diagrams/) | Where are the visual architecture materials described? |

## Suggested paths

| If you have... | Read... |
|---|---|
| A quick orientation | The README's **Architecture thesis** and **Architecture scope** sections |
| 5 minutes | The [reference architecture](01-reference-architecture.md), then the [placement matrix](02-workload-placement-matrix.md) |
| A platform decision to make | The [placement matrix](02-workload-placement-matrix.md) and relevant [decision record](decision-records/) |
| A design review to prepare | The [Well-Architected mapping](03-well-architected-mapping.md), [security model](04-security-model.md), then the [runbooks](runbooks/) |

## How to use this material

Treat the defaults as hypotheses. For each workload, validate data boundaries, regional service support, model and framework compatibility, quotas, failure modes, operational ownership, performance, and total cost before selecting a platform.
