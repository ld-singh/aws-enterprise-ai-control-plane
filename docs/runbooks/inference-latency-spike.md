# Runbook: Inference Latency Spike

## Symptoms

- TTFT or p95 latency exceeds SLO
- Increased queue wait time
- Higher token generation duration
- Model endpoint saturation
- User-facing timeout increase

## First checks

1. Is the problem isolated to one model, tenant, region, or endpoint?
2. Did request volume increase?
3. Did average prompt size or output token count increase?
4. Are GPU utilization, memory, or CPU saturation abnormal?
5. Are downstream tools, vector stores, or data APIs slow?

## Immediate mitigations

- Route low-priority traffic to fallback model
- Temporarily reduce max output tokens
- Scale serving replicas if capacity exists
- Shed non-critical batch workloads
- Raise queue priority for production inference

## Post-incident actions

- Update capacity model
- Adjust autoscaling thresholds
- Review routing policy
- Add alert on leading indicators, not only final latency
