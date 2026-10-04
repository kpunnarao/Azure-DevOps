# Monitoring, Rollback, and Operations

[← Security and Compliance](06-security-testing-and-compliance.md) · [Chapter 17](README.md) · [Next: Documentation and Assessment →](08-documentation-demo-and-assessment.md)

## Telemetry specification

Instrument requests, dependencies, exceptions, traces, structured logs, platform/container metrics, queue/database behavior, and a business completion metric. Include environment, region, service, immutable version/digest, cohort, operation, and correlation ID; exclude secrets/personal data.

Monitor telemetry ingestion health, sampling, retention, access, and cost.

## Reliability

Define precise availability/correctness and latency SLIs, 28/30-day SLOs, error budget, fast/slow burn alerts, and release policy. Create external Standard availability test(s), internal probes, and customer-path synthetic transaction.

Build dashboard/workbook:

- Traffic/error/latency/saturation.
- Version and cohort comparison.
- Dependencies and exceptions.
- Kubernetes/AKS and Azure resource health.
- Business transaction.
- SLO/error-budget burn.
- Deployment/flag/config markers.
- Alert and incident link.

## Incident exercise

Inject a candidate-only latency/error regression. Alert must route through an action group with context/runbook. Assign commander/operations/communications, stop rollout, scope affected cohort, preserve evidence, decide flag/traffic rollback/roll-forward, restore, verify customer journey/SLO, and communicate.

Also simulate missing telemetry. It must pause high-risk progression rather than count as success.

## Recovery

Retain previous image digest/config, compatible schema, and tested traffic reversal. Define database/data reconciliation and queues/external side effects. Practice backup restore against RTO/RPO separately from deployment rollback.

## Post-incident learning

Create timeline, impact, detection, contributing conditions, mitigation, recovery time, what worked/failed, and assigned actions. Avoid single-person blame. Update at least one test, alert, pipeline control, runbook, architecture decision, or paved road; verify closure.

## Operational readiness

Service catalog, owner/on-call, dependencies, dashboards/alerts, SLO, runbooks, capacity, certificates/secrets, backup/restore, maintenance, cost, vulnerability/upgrade, and decommission paths are documented and tested.

## Acceptance evidence

Queries/dashboards, SLI definitions, alert rules/action tests, markers, incident timeline, exact recovery digest, recovery validation, RTO/RPO exercise, review/actions, and updated control.

## Expert review questions

Why these SLOs? Could sampling hide failure? What if dependency and deployment fail together? When is roll-forward safer? How prove recovery rather than pipeline completion? How is alert delivery itself monitored?

[Next: Documentation, Demo, and Assessment →](08-documentation-demo-and-assessment.md)
