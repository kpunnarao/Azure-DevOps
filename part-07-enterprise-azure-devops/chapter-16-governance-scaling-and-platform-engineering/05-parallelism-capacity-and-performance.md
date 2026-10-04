# Parallelism, Capacity, and Performance

[← Agent Pools](04-agent-pool-architecture-and-security.md) · [Chapter 16](README.md) · [Next: Naming and Lifecycle →](06-naming-tagging-retention-and-lifecycle.md)

## Three constraints

- **Parallel jobs:** organization/licensing concurrency entitlement.
- **Agents:** available compute workers.
- **Pipeline graph:** jobs that are ready and independent.

Ten idle agents do not help if the organization has one parallel job. Ten parallel jobs do not help a serial pipeline. Autoscaling does not eliminate cold-start delay.

## Measure demand

Collect arrivals by hour, queue duration percentiles, job duration/distribution, pool/agent utilization, ready-but-queued jobs, cold start, failure/retry, workload/trust class, and future growth.

A simple concurrency approximation is arrival rate × average job duration, then adjust for burstiness, tail duration, maintenance, failure, and desired queue SLO. Model pools separately because one blocked trust zone cannot borrow another safely.

## Optimize in order

1. Remove unnecessary triggers/work.
2. Shorten critical path via caching/build graph.
3. Parallelize independent valuable jobs.
4. Balance test shards by duration.
5. Reduce agent initialization/image pull/tool install.
6. Schedule nonurgent work off peak.
7. Right-size agents.
8. Add parallel entitlement and capacity.

Parallel work consumes more total compute and can overload package feeds, test environments, databases, or rate-limited APIs. Set downstream concurrency controls.

## Capacity policy

Define queue SLO by pipeline class, minimum warm capacity, maximum scale, burst behavior, tenant/team quotas, priority, cancellation of superseded PRs, cost budget, and degraded-mode plan. Production hotfixes may need reserved capacity.

Do not let one monorepo fan-out starve every product. Fairness may require separate pools or governance.

## Performance evidence

Report queue time separately from execution, time to first actionable failure, end-to-end feedback, tail percentiles, and cost per successful run. Average duration hides severe tails.

## Interview preparation

**Why are jobs queued with idle agents?**  
Parallel-job entitlement, demands mismatch, pool authorization, agent offline/disabled, or a different pool/trust constraint.

**How size a pool?**  
From arrival/duration distributions, burst/tail, cold starts, queue SLO, maintenance, failure headroom, trust zones, and budget—then test and adjust.

**Is more parallelism always faster?**  
No. Overhead, imbalance, resource contention, quotas, and serial critical paths limit benefit.

## Practical exercise

Use a week of synthetic job data to forecast capacity. Compare zero/one/five warm agents, model licensing, create a queue SLO, then optimize critical path before purchasing capacity.

## Official references

- [Parallel jobs](https://learn.microsoft.com/azure/devops/pipelines/licensing/concurrent-jobs)
- [Pipeline run sequence and agent allocation](https://learn.microsoft.com/azure/devops/pipelines/process/runs)
- [Pipeline reports](https://learn.microsoft.com/azure/devops/pipelines/reports/pipelinereport)

[Next: Naming, Tagging, Retention, and Lifecycle →](06-naming-tagging-retention-and-lifecycle.md)
