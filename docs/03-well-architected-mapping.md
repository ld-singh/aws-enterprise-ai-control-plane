# Well-Architected Mapping

## Operational Excellence

See the [observability model](05-observability-model.md) for the telemetry architecture, core SLIs, dashboard model, tenant showback, and incident-response signals.

- Runbooks for inference latency, agent tool failure, GPU saturation, data ingestion failure, and tenant cost spikes
- Change management via ADRs
- SLOs for TTFT, latency, error rate, queue wait time, and cost per request
- Deployment pipeline with rollback strategy

## Security

See the [security model](04-security-model.md) for the detailed control architecture, ownership boundaries, tradeoffs, and validation checklist.

- Multi-account isolation
- AWS IAM Identity Center
- AWS Organizations service control policies (SCPs) and AWS IAM permission boundaries
- Amazon VPC endpoints for private service access
- Tenant isolation patterns: silo, pool, bridge
- Agent tool access policies
- Secrets management
- Image scanning and admission controls

## Reliability

- Multi-AZ service design
- Graceful degradation from agentic flow to retrieval-only flow
- Retry and circuit breaker patterns
- Queue-based workload admission
- Model endpoint health checks
- Checkpointing for training workloads

## Performance Efficiency

See the [observability model](05-observability-model.md) for TTFT, latency, tokens/sec, queue wait time, and accelerator-utilization signals.

- Model placement strategy
- Batch vs online workload separation
- GPU utilization tracking
- Autoscaling strategy
- Token streaming and TTFT measurement
- Workload routing between Amazon Bedrock, Amazon EKS, Amazon SageMaker AI, and HPC platforms

## Cost Optimization

See the [observability model](05-observability-model.md) for cost-per-request measurement and tenant-level showback.

- Cost allocation tags
- Tenant-level chargeback/showback
- GPU utilization dashboards
- Amazon Bedrock token cost monitoring
- Spot/on-demand decision matrix
- AWS Trainium/AWS Inferentia vs GPU evaluation

## Sustainability

- Right-sizing
- Autoscaling down idle capacity
- Batch scheduling during lower demand windows
- Avoiding permanently idle GPU clusters
