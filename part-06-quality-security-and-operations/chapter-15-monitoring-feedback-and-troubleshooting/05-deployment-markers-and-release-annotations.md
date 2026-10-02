# Deployment Markers and Release Annotations

[← SLIs and Error Budgets](04-slis-slos-and-error-budgets.md) · [Chapter 15](README.md) · [Next: Alerts and Incidents →](06-actionable-alerts-and-incident-management.md)

## Make change visible

When health changes, responders first ask “what changed?” A deployment marker is a timestamped event correlated with telemetry. It should identify application/service, environment, region/ring, immutable artifact version/digest, commit, build/release run, deployment strategy/increment, configuration/flag revision, and outcome.

Do not include secrets or sensitive change content.

## Emit lifecycle events

Record:

- Deployment started.
- Artifact placed.
- Traffic increment changed.
- Feature flag/configuration changed.
- Schema migration phase.
- Deployment completed/failed/rolled back.
- Incident mitigation.

A single completion marker cannot explain degradation that began during a canary increment.

## Application Insights/Azure Monitor

Application Insights release annotations and custom events can overlay changes on performance data. Azure Pipelines/Azure services integration details evolve, so verify current method; a robust fallback is a structured custom event or API call from the deployment pipeline using a protected identity.

Use UTC timestamps and a stable schema. Make emission idempotent or include a unique deployment event ID to avoid duplicate ambiguity after retries.

## Correlation

Stamp runtime telemetry with application version/digest and cohort. A marker shows the change time; per-request version dimensions prove which version served the failed request. Preserve mapping from runtime version to source and release evidence.

Dashboard release comparison should include baseline, candidate, traffic volume, latency/errors, dependencies, business outcome, and observation window.

## Common mistakes

- Marker says “deployment” but not version/environment.
- Annotation uses mutable build name only.
- No marker for flag/config/schema change.
- Pipeline fails after deployment but marker says success.
- Sampling/filtering removes release events.
- Clock/time-zone mismatch.
- Deployment marker treated as causal proof rather than correlation clue.

## Interview preparation

**Why markers if Git history exists?**  
Git shows source history, not the exact time/content/environment/cohort of production change.

**Marker versus version tag in telemetry?**  
Marker shows event timing and metadata; version attribute identifies individual telemetry. Use both.

**Does correlation prove causation?**  
No. It prioritizes investigation; compare cohorts/baselines and inspect traces/dependencies.

## Practical exercise

Emit structured events for deployment start, 10% canary, 100%, and rollback. Overlay them on error/latency charts, query affected version, and link back to pipeline run and digest.

## Official references

- [Application Insights release annotations](https://learn.microsoft.com/azure/azure-monitor/app/failures-performance-transactions#release-annotations)
- [Track custom events](https://learn.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics)

[Next: Actionable Alerts and Incident Management →](06-actionable-alerts-and-incident-management.md)
