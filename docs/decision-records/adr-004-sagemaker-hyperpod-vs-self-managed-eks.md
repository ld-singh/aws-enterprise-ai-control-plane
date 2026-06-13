# ADR-004: SageMaker HyperPod vs Self-Managed EKS

## Status

Draft

## Context

Large-scale training workloads require distributed orchestration, failure recovery, cluster management, and efficient accelerator usage.

## Decision

Use SageMaker HyperPod for large-scale managed training environments where reduced operational burden and training-specific capabilities matter. Use self-managed EKS when platform teams need deeper control or when workloads are closer to inference/platform services.

## Consequences

Architecture documentation must clarify operational ownership, cost model, monitoring approach, and team skill requirements for each path.
