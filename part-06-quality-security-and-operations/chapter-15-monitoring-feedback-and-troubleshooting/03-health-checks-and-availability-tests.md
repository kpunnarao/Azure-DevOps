# Health Checks and Availability Tests

[← Azure Monitor](02-azure-monitor-and-application-insights.md) · [Chapter 15](README.md) · [Next: SLIs and Error Budgets →](04-slis-slos-and-error-budgets.md)

## Health from multiple viewpoints

Internal readiness/liveness probes govern orchestration. External availability tests answer whether a client can reach and use the service from selected locations. Synthetic journeys test representative functionality. Real-user monitoring reveals actual experience. Use complementary layers.

A health endpoint should be fast, deterministic, safe, and reveal minimal public detail. Separate simple public status from protected diagnostics.

## Application Insights availability

Standard availability tests can send recurring HTTP(S) requests from multiple locations, validate response/performance, and alert. They can test public endpoints without application code changes and support request configuration such as method/headers/certificate checks under current capabilities.

Current Microsoft guidance states classic URL ping tests retire on September 30, 2026. Use Standard tests for current design and verify migration/pricing.

Private/internal services need another execution path, such as controlled custom tests or agents within the network. Do not open a private endpoint merely to make a public monitor work.

## Useful synthetic design

- Use multiple independent locations.
- Validate content/transaction outcome, not only HTTP 200.
- Use dedicated least-privilege test accounts.
- Avoid mutating production or clean up idempotently.
- Measure DNS, TLS, redirect, latency, and dependency effects.
- Tag test traffic so analytics can separate it.
- Alert only after enough locations/attempts to reduce false positives.
- Protect test secrets and respect third-party rate limits.

## Health model

Define component health and customer-journey health. A database may be degraded while cached reads succeed; infrastructure may look green while checkout fails. Health is not binary across a distributed system.

Tie availability evidence to SLI calculation and release gates, but avoid using a single endpoint as universal truth.

## Interview preparation

**Probe versus availability test?**  
A probe is usually local orchestration feedback; an availability test exercises externally observable reachability/behavior from a monitoring location.

**Why multi-location?**  
It distinguishes regional/network path issues and reduces false conclusions from one observer.

**Should health call every dependency?**  
Not for every probe. External functional tests may cover critical paths; liveness should avoid dependency-triggered restart storms.

## Practical exercise

Create a Standard availability test from several locations with content validation. Break DNS/TLS/content separately, inspect results, and compare with Kubernetes readiness/liveness and actual user telemetry.

## Official references

- [Application Insights availability tests](https://learn.microsoft.com/azure/azure-monitor/app/availability)
- [Health Endpoint Monitoring pattern](https://learn.microsoft.com/azure/architecture/patterns/health-endpoint-monitoring)

[Next: SLIs, SLOs, and Error Budgets →](04-slis-slos-and-error-budgets.md)
