# Actionable Alerts and Incident Management

[← Deployment Markers](05-deployment-markers-and-release-annotations.md) · [Chapter 15](README.md) · [Next: Delivery Performance →](07-delivery-performance-metrics.md)

## An alert demands action

Page on urgent user-impact symptoms or rapid error-budget burn. Create tickets/messages for lower urgency. Dashboards support investigation; they are not alerts.

Every alert needs owner, severity, affected service/environment, condition/window, customer impact, current value/threshold, version/change context, dashboard/query, runbook, escalation, and resolution criteria.

## Signal design

Prefer availability, latency, correctness, queue delay, and critical business failures over raw CPU alone. Resource saturation can be useful when it is predictive/actionable. Use multiple evaluation periods, dimensions, and dynamic thresholds where appropriate, but test behavior.

Missing telemetry needs explicit treatment. An alert query returning no rows may mean healthy, broken ingestion, or down service.

Azure Monitor action groups route notifications/automation; alert processing rules can manage behavior at scale or during planned maintenance. Use secure webhooks/managed identities where supported. Monitor notification delivery and avoid a single channel.

## Incident process

1. Detect and acknowledge.
2. Assign incident commander and communications/operations roles proportionate to severity.
3. Establish impact, scope, timeline, and current version.
4. Stabilize: stop rollout, shed load, disable feature, rollback/forward.
5. Preserve evidence and record decisions.
6. Diagnose without delaying restoration unnecessarily.
7. Validate customer recovery and monitor.
8. Communicate closure.
9. Run a blameless review and track actions to completion.

A runbook is a decision aid with prerequisites, safe commands, verification, rollback, owners, and escalation—not a wall of outdated commands.

## Alert quality

Measure precision/actionability, missed incidents, acknowledgement time, pages per responder, duplicate alerts, alert-to-incident ratio, runbook success, and toil. Tune after incidents and architecture changes. Never solve noise by muting unreviewed critical alerts indefinitely.

## Interview preparation

**Symptom versus cause alert?**  
Symptom alert pages on user harm; cause telemetry assists diagnosis or warns when a known leading indicator is reliably actionable.

**What is first during incident?**  
Protect people/service: establish command, scope impact, stop further exposure, and choose safe mitigation while preserving evidence.

**What makes an alert actionable?**  
Named owner, meaningful urgency, context, reliable signal, documented action, and clear resolution.

## Practical exercise

Create a burn-rate/page alert and a lower-urgency saturation ticket alert. Test action group delivery, inject failure, run the incident roles, restore service, and eliminate one noisy duplicate.

## Official references

- [Azure Monitor alert best practices](https://learn.microsoft.com/azure/azure-monitor/alerts/best-practices-alerts)
- [Azure Monitor action groups](https://learn.microsoft.com/azure/azure-monitor/alerts/action-groups)

[Next: Delivery Performance Metrics →](07-delivery-performance-metrics.md)
