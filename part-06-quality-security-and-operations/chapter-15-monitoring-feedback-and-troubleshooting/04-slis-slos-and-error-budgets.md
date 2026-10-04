# SLIs, SLOs, and Error Budgets

[← Health and Availability](03-health-checks-and-availability-tests.md) · [Chapter 15](README.md) · [Next: Deployment Markers →](05-deployment-markers-and-release-annotations.md)

## Reliability language

- **SLI:** measured indicator of service behavior, such as good requests / valid requests.
- **SLO:** target for an SLI over a window, such as 99.9% successful eligible requests over 28 days.
- **SLA:** external/business agreement and consequences; not interchangeable with SLO.
- **Error budget:** allowed unreliability, `1 - SLO`, over the window.

For one million eligible requests, 99.9% permits 1,000 bad requests under that definition. The definition of eligible/good matters more than decimal places.

## Design customer-centered SLIs

Examples:

- Availability: successful valid requests / total valid requests.
- Latency: valid requests below threshold / total valid requests.
- Correctness: completed accurate transactions / attempted transactions.
- Freshness: data produced within required age.
- Durability: retained records / committed records.

Exclude only explicitly justified events. Maintenance exclusion can hide customer impact. Define missing telemetry, client cancellations, synthetic traffic, retries, and dependency failures.

## Windows and burn rate

Rolling windows reflect current experience; calendar windows align with reporting. Burn rate compares budget consumption speed to the sustainable rate. Multi-window alerts can detect rapid catastrophic burn and slower sustained degradation without paging on every tiny fluctuation.

An error budget policy states actions when consumption is high: pause risky releases, prioritize reliability, require progressive exposure, or escalate review. It is a decision tool, not permission to intentionally cause errors.

## Ownership

Product and engineering jointly choose reliability aligned to user need and cost. 100% targets are usually unrealistic and may discourage change or produce dishonest measurement. Different critical journeys can have different SLOs.

Validate the SLI query as code: denominator, filters, time zones, sampling, ingestion delay, and version changes. A broken query can create fictitious reliability.

## Interview preparation

**SLO versus SLA?**  
SLO is an internal reliability target; SLA is a customer/business agreement that may include remedies.

**Why error budget?**  
It balances delivery and reliability using actual user-impact allowance rather than subjective arguments.

**What is burn rate?**  
How quickly error budget is being consumed relative to a sustainable rate over the SLO window.

## Practical exercise

Define availability and latency SLIs with precise good/valid events. Calculate budget for 99.9%, simulate an outage, create fast/slow burn alerts, and write a release policy for remaining budget.

## Official references

- [Azure Well-Architected reliability metrics](https://learn.microsoft.com/azure/well-architected/reliability/metrics)
- [Google SRE workbook: SLOs](https://sre.google/workbook/implementing-slos/)

[Next: Deployment Markers and Release Annotations →](05-deployment-markers-and-release-annotations.md)
