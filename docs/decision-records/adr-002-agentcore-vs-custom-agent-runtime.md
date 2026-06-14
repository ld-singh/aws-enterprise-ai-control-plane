# ADR-002: Amazon Bedrock AgentCore vs Custom Agent Runtime

## Status

Draft

## Context

Production agents need runtime isolation, tool access control, memory, identity, tracing, and operational monitoring.

## Decision

Use Amazon Bedrock AgentCore as the primary reference path for enterprise AWS agent workloads. Allow custom agent runtimes only where framework constraints, portability, or specialized control requirements justify the extra platform burden.

## Rationale

Managed identity, gateway, memory, runtime, and observability reduce undifferentiated platform work.

## Consequences

The architecture must still define strong AWS IAM boundaries, tool access controls, Amazon VPC connectivity patterns, and agent evaluation requirements.
