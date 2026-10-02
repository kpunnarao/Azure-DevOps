# Logs, Metrics, Traces, and Observability

[← Chapter 15](README.md) · [Next: Azure Monitor and Application Insights →](02-azure-monitor-and-application-insights.md)

## Signals

- **Logs:** timestamped structured events with context.
- **Metrics:** numeric time series efficient for trends and alerting.
- **Distributed traces:** request path represented by trace and spans across services.
- **Profiles:** where CPU/memory/time is spent.
- **Events/changes:** deployments, configuration, flags, scaling, incidents.
- **Business signals:** whether users accomplish intended outcomes.

Observability is the ability to infer internal state from emitted evidence; it is not synonymous with collecting every log.

## Correlation

Propagate trace/context IDs through HTTP, queues, background work, and dependencies. Include service, version, environment, region, instance, operation, and outcome consistently. OpenTelemetry provides vendor-neutral APIs/semantic conventions for logs, metrics, and traces.

Do not put secrets, tokens, raw personal data, payment details, or unrestricted request bodies into telemetry. Classification, redaction, access, retention, residency, and deletion apply to observability data.

## Design from questions

Start with questions:

- Which version is failing?
- Which tenants/cohorts/regions are affected?
- Is failure in our service or dependency?
- When did latency begin and what changed?
- Are retries amplifying load?
- Did the customer transaction complete correctly?

Then define signal, attributes, cardinality, sampling, retention, and owner. High-cardinality fields such as user ID in metric dimensions can explode cost; keep them in appropriately protected logs/traces if needed.

## Sampling and cost

Trace sampling controls volume, but preserve errors and high-value paths where possible. Understand head/tail or rate-based decisions and weighting before calculating rates. Logs need levels and structured schemas; debug logging should be time-bound.

Telemetry pipelines themselves need monitoring for dropped data, ingestion delay, quota, exporter failure, query latency, and cost.

## Interview preparation

**Metrics versus logs?**  
Metrics efficiently show aggregate behavior and alert; logs provide discrete context. Traces connect distributed request work. Use them together.

**What is cardinality?**  
Number of unique dimension combinations. Unbounded IDs in metrics increase cost and reduce usability.

**Is no alert evidence of health?**  
No. Telemetry may be missing, sampling wrong, thresholds poor, or user traffic absent. Monitor observability health and explicit success signals.

## Practical exercise

Instrument one request across two services and a queue. Correlate spans, structured logs, and metrics. Add an accidental high-cardinality field and secret-like field, then redesign/redact and calculate cost impact.

## Official references

- [Azure Monitor overview](https://learn.microsoft.com/azure/azure-monitor/fundamentals/overview)
- [Azure Monitor OpenTelemetry](https://learn.microsoft.com/azure/azure-monitor/app/opentelemetry-overview)

[Next: Azure Monitor and Application Insights →](02-azure-monitor-and-application-insights.md)
