# ADR-005: Trainium/Inferentia vs NVIDIA GPU

## Status

Draft

## Context

AWS AI infrastructure can use AWS-designed accelerators or NVIDIA GPU-based platforms.

## Decision

Evaluate Trainium/Inferentia for AWS-native cost/performance optimization where framework and model compatibility are acceptable. Use NVIDIA GPU platforms where ecosystem compatibility, CUDA/NCCL maturity, specific model support, or hybrid/on-prem portability are stronger requirements.

## Consequences

The architecture must include a hardware placement decision matrix rather than assuming one accelerator family for all workloads.
