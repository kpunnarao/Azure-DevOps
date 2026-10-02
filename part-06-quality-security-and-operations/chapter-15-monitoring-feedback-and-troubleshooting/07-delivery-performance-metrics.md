# Delivery Performance Metrics

[← Alerts and Incidents](06-actionable-alerts-and-incident-management.md) · [Chapter 15](README.md) · [Next: Pipeline Analytics →](08-pipeline-analytics-and-troubleshooting.md)

## Measure the system

Common DORA delivery measures include:

- Deployment frequency.
- Change lead time.
- Change failure rate.
- Failed deployment recovery time.
- Recent DORA research also discusses rework rate in its software-delivery performance model.

Use current definitions from the research source and define locally which deployments, timestamps, failures, and recovery events qualify.

These measures balance throughput and stability. They are not individual productivity scores and should not become targets that encourage tiny meaningless deployments, hidden failures, or premature incident closure.

## Supporting measures

- PR review and queue time.
- CI feedback and time to first actionable failure.
- Pipeline queue/duration/retry/infrastructure failure.
- Batch size and work-in-progress age.
- Deployment duration and progressive-exposure time.
- Flake rate and escaped defects.
- Error-budget consumption.
- Manual handoff/wait time.
- Reliability/security debt age.

Use flow analysis to find constraints. Faster coding does not help if approval or environment queues dominate lead time.

## Data model

Connect work/commit → PR → CI artifact → deployment → incident/recovery. Preserve immutable IDs and timestamps. Distinguish business lead time from code-commit lead time. Account for rollbacks, feature flags, configuration-only releases, canceled runs, and multi-service deployments.

Report distributions and percentiles, not only averages. Segment by service and deployment type; aggregate comparisons can mislead.

## Responsible use

Metrics should help a stable team improve its own system over time. Avoid ranking teams with different architectures, compliance burdens, incident classifications, or data quality. Pair quantitative trend with qualitative review.

When a metric improves suddenly, verify the process did not redefine or stop recording unfavorable events.

## Interview preparation

**Why pair speed and stability?**  
Optimizing only speed can increase failures; optimizing only avoidance can create large risky batches. High performance seeks frequent small change with reliable recovery.

**What is lead time?**  
Define it explicitly. DORA change lead time commonly follows code committed to successfully running in production; broader idea-to-value time is useful but different.

**How avoid gaming?**  
Use balanced measures, transparent definitions, automated lineage, distributions, qualitative review, and no individual ranking.

## Practical exercise

Calculate metrics from ten sample deployments and two incidents. Change definitions and observe results, validate outliers, find the largest waiting state, and propose one experiment with a guardrail metric.

## Official references

- [DORA research](https://dora.dev/research/)
- [DORA guides](https://dora.dev/guides/)

[Next: Pipeline Analytics and Troubleshooting →](08-pipeline-analytics-and-troubleshooting.md)
