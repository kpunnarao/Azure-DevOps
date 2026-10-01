# Health Checks and Zero Downtime

[← Database Migrations](05-backward-compatible-database-migrations.md) · [Chapter 10](README.md) · [Next: Recovery →](07-rollback-roll-forward-and-recovery.md)

## Different probes answer different questions

- **Startup:** Has initialization finished within the expected period?
- **Liveness:** Is the process irrecoverably stuck and safe to restart?
- **Readiness:** Can this instance serve traffic now?
- **Functional/synthetic:** Can an external client complete a representative journey?
- **Business health:** Are customers succeeding at expected rates?

Do not make liveness depend on every downstream service; a shared dependency outage could restart the entire fleet and make recovery worse. Readiness may consider critical dependencies, but prevent cascading removal of all capacity.

## Deployment validation

A zero-downtime sequence typically:

1. Preserve enough healthy capacity.
2. Deploy the new instance/version.
3. Wait for startup completion.
4. Pass readiness before adding traffic.
5. Drain old instances gracefully.
6. Route a limited cohort.
7. Run external smoke/synthetic checks.
8. Observe golden and business signals.
9. Continue, pause, or abort.

Health must be evaluated over time, not from one successful HTTP 200.

## Signals and thresholds

Track latency distribution, traffic, errors, saturation, dependency health, resource exhaustion, queue lag, data-integrity indicators, and customer outcomes. Tag telemetry with version, region, environment, and cohort so a small failing rollout does not disappear in fleet averages.

Define thresholds, comparison baseline, observation window, missing-data behavior, and decision authority before deployment. Azure Pipelines can query Azure Monitor alerts as a resource check; the monitoring system and alert must already be trustworthy.

## Health endpoint security

Expose minimal information publicly. Detailed dependency status can aid attackers. Authenticate diagnostic endpoints where appropriate, rate-limit them, avoid expensive checks, and separate public “healthy/not healthy” from internal diagnostics.

Synthetic tests must use isolated test accounts/data and clean up after themselves.

## What zero downtime does not mean

It does not promise zero errors under every failure. It means planned deployment preserves agreed availability and correctness. Schema locks, DNS caching, session loss, cache cold start, connection draining, certificate changes, and external rate limits can still violate that goal.

## Interview preparation

**Liveness versus readiness?**  
Liveness decides whether to restart a process; readiness decides whether to route traffic to it. Conflating them can create restart storms.

**Why use business metrics?**  
Infrastructure can appear healthy while checkout, login, message processing, or another customer outcome fails.

**What should happen when telemetry is missing?**  
Define it explicitly. For high-risk production rollout, inability to observe should usually pause or fail closed rather than count as healthy.

## Practical exercise

Implement startup, liveness, readiness, and one functional synthetic check. Inject a downstream outage and ensure it does not restart every instance. Tag telemetry by version and make a rollout halt on a canary-only error increase.

## Official references

- [Health Endpoint Monitoring pattern](https://learn.microsoft.com/azure/architecture/patterns/health-endpoint-monitoring)
- [Monitoring and diagnostics best practices](https://learn.microsoft.com/azure/architecture/best-practices/monitoring)
- [Azure Monitor checks in pipelines](https://learn.microsoft.com/azure/devops/pipelines/process/approvals)

[Next: Rollback, Roll-forward, and Recovery →](07-rollback-roll-forward-and-recovery.md)
