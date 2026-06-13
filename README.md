# AWS Enterprise AI Control Plane

A reference architecture for enterprise AI workloads on AWS, covering landing zones, Bedrock agents, EKS inference, GPU scheduling, observability, governance, and cost control.

## Architecture thesis

Enterprise AI will not be won by model access alone. The harder architecture problem is the control plane around identity, data, agents, inference, GPU capacity, observability, cost, security, and operational risk.

This repository captures architecture decisions, tradeoffs, operational patterns, and reference designs for building enterprise AI platforms on AWS.

## Target audiences

- AWS Solutions Architects
- AI/ML and GenAI Specialist Solutions Architects
- Cloud Infrastructure Architects
- Platform Engineering leaders
- AI Infrastructure / GPU platform teams
- Enterprise technology leaders evaluating AI platform design

## Pillars

1. AWS Landing Zone for AI
2. Bedrock / AgentCore production agent architecture
3. EKS AI workload platform
4. GPU / Slurm / SageMaker HyperPod / NVIDIA AI Factory extension

## What this repo demonstrates

- Architecture decision-making, not just deployment
- Well-Architected thinking across security, reliability, operational excellence, performance, cost, and sustainability
- Enterprise AI operating model design
- Production observability and runbook thinking
- Clear tradeoff analysis across Bedrock, EKS, SageMaker, Slurm, Trainium/Inferentia, and NVIDIA GPU platforms

## Current status

Phase 0: architecture foundation.

Start here:

- `docs/00-start-here.md`
- `docs/01-reference-architecture.md`
- `docs/02-workload-placement-matrix.md`
- `docs/03-well-architected-mapping.md`
- `docs/decision-records/`
- `docs/runbooks/`
- `lab/local-simulation/`
- `lab/aws-blueprint/`
