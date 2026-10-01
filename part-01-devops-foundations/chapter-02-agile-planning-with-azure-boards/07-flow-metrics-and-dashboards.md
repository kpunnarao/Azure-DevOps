# Flow Metrics and Dashboards

> Chapter 2 — Agile Planning with Azure Boards

[← Previous](06-capacity-estimation-velocity-and-forecasting.md) · [Chapter home](README.md) · [Next →](08-work-item-links-and-traceability.md)

## Purpose

Metrics should reveal how work behaves and support a decision. They should not become a ranking system for individual engineers.

## Core flow metrics

### Lead time

Elapsed time from work-item creation until completion. Lead time includes waiting before active work and reflects the customer's experience of the delivery system.

### Cycle time

Elapsed time from first entering an In Progress category until completion. It focuses on active-system flow, though waiting within active states is included.

### Work in progress

The number of items started but not completed. High WIP often increases queues, context switching, lead time, and uncertainty.

### Throughput

The number of work items completed in a period. Throughput is most useful when item size distribution and workflow are reasonably stable.

### Work-item age

How long an unfinished item has been in progress. Aging work is an early warning; cycle time is known only after completion.

### Cumulative flow

A cumulative flow diagram shows item counts across workflow states over time. Band width represents how much work occupies a state. A widening band often indicates a bottleneck or queue.

## Supporting metrics

- Burndown: remaining work over a fixed period
- Burnup: completed work against total scope
- Velocity: completed estimated work by sprint
- Defect arrival and escape
- Reopen rate
- Blocked time
- Deployment and pipeline health
- Outcome or adoption measures

No single chart explains the system. Combine delivery, quality, reliability, and outcome evidence.

## Reading a cumulative flow diagram

Look for:

- **Parallel bands:** stable flow
- **Widening middle band:** work accumulating in a state
- **Growing total height during sprint:** scope added
- **Flat completion line:** no throughput
- **Sudden drops:** bulk closure, reclassification, or data-quality problem
- **High WIP:** too much started relative to finishing capacity

Investigate with the team before assigning cause.

## Dashboard design

Start with audience and decisions.

### Team flow dashboard

- Sprint or service goal
- Blocked and aging work
- Cumulative flow
- Cycle-time trend
- Build and deployment health
- Escaped defects
- Current incidents

### Product dashboard

- Outcome indicator
- Feature progress
- Lead-time trend
- Scope change
- Quality and release confidence

### Leadership dashboard

- Outcome progress across products
- Major delivery risks
- Dependency health
- Trend ranges rather than isolated snapshots
- Reliability and recovery signals

## Data quality

Metrics depend on state hygiene. If teams update work items days later, lead and cycle time become misleading. Define state meanings, automate transitions only when accurate, and audit stale or inconsistent items.

Changing workflow states can affect trends. Record reporting changes and avoid silently comparing incompatible periods.

## Metrics and behavior

A metric used as a target can distort behavior:

- Velocity target → point inflation
- Ticket closure target → smaller administrative tickets
- Utilization target → excess WIP
- Zero defects target → defects hidden or reclassified
- Deployment-frequency target → meaningless releases

Use balanced signals and qualitative review. Metrics are conversation starters, not verdicts.

## Common mistakes

- Measuring individuals through team flow data
- Reporting averages without distribution or outliers
- Ignoring reopened work
- Treating lower lead time as success if quality declines
- Building dashboards with no owner or audience
- Comparing unlike teams and workflows
- Keeping obsolete widgets because they are available
- Optimizing utilization rather than flow

## Interview preparation

**Q: Lead time versus cycle time?**  
Lead time starts when the request is created; cycle time starts when active work begins. Lead time includes the pre-start queue.

**Q: What does a widening band in cumulative flow mean?**  
Work is accumulating in that workflow state. It may indicate a bottleneck, blocked work, unbalanced capacity, or a policy problem and requires investigation.

**Q: Which metrics would you put on a DevOps dashboard?**  
Choose by audience. A balanced team dashboard might show outcome progress, lead or cycle time, WIP and aging work, quality, pipeline/deployment health, and incidents.

**Q: Why is 100 percent utilization harmful?**  
A fully utilized system has no capacity to absorb variation, reviews, incidents, or urgent work. Queues grow and flow slows.

## Practical exercise

Create ten sample items with different state histories. Build lead-time, cycle-time, and cumulative-flow widgets. Identify one aging item and one bottleneck. Propose a WIP or workflow change and define how you will evaluate it.

## Further reading

- [Configure lead-time and cycle-time widgets](https://learn.microsoft.com/en-us/azure/devops/report/dashboards/cycle-time-and-lead-time)
- [Cumulative flow guidance](https://learn.microsoft.com/en-us/azure/devops/report/dashboards/cumulative-flow-cycle-lead-time-guidance)
- [Create actionable dashboards](https://learn.microsoft.com/en-us/azure/devops/report/dashboards/dashboard-focus)
