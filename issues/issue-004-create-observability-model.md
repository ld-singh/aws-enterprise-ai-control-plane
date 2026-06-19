# Issue 004: Create observability model

Create `docs/05-observability-model.md` covering Amazon CloudWatch, OpenTelemetry, Prometheus, Grafana, logs, traces, agent observability, GPU metrics, and SLOs.

Acceptance criteria:

- Covers Amazon CloudWatch metrics, logs, traces, and alarms
- Covers OpenTelemetry instrumentation and required trace attributes
- Covers Amazon Bedrock and Amazon Bedrock AgentCore observability
- Covers Amazon EKS metrics, Prometheus, Grafana, and DCGM/GPU metrics as an extension path
- Defines TTFT, latency, tokens/sec, queue wait time, error rate, cost per request, and tenant-level showback
- Includes incident-response signals, assumptions, decisions, tradeoffs, operational considerations, and official references
- Is linked from the README, start-here guide, reference architecture, and Well-Architected mapping
