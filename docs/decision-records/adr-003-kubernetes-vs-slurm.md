# ADR-003: Kubernetes vs Slurm for AI Workloads

## Status

Draft

## Context

AI platforms often need to support online inference, batch jobs, distributed training, and HPC-style workloads.

## Decision

Use Kubernetes on Amazon EKS for cloud-native online inference and platform workloads. Use AWS ParallelCluster + Slurm, or an equivalent platform, for HPC-style workloads where scheduling semantics, user expectations, and MPI/HPC ecosystem alignment are stronger.

## Rationale

Kubernetes is strong for service-oriented workloads, cloud-native operations, and platform integration. Slurm remains strong for HPC batch scheduling, queueing, and established research/HPC workflows.

## Consequences

The AI platform must expose clear workload placement guidance rather than forcing all workloads into one scheduler.
