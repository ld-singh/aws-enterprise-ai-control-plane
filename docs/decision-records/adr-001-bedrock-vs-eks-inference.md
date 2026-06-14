# ADR-001: Amazon Bedrock vs Amazon EKS for Inference Workloads

## Status

Draft

## Context

Enterprise AI platforms often need both managed foundation model access and custom model hosting. A single platform choice is rarely optimal for all workloads.

## Decision

Use Amazon Bedrock as the default for managed foundation model workloads and Amazon EKS-based serving for custom model inference, open-source models, specialized serving stacks, or workloads requiring deep runtime control.

## Rationale

Amazon Bedrock reduces operational burden for managed foundation model use cases. Amazon EKS offers deeper control over serving runtime, custom containers, networking, sidecars, model routing, scaling, and GPU scheduling.

## Consequences

The platform must support two execution paths and provide common observability, security, cost attribution, and access governance across both.
