# Azure Monitor and Application Insights

[← Observability Signals](01-logs-metrics-traces-and-observability.md) · [Chapter 15](README.md) · [Next: Health and Availability →](03-health-checks-and-availability-tests.md)

## Azure-native telemetry platform

Azure Monitor brings together platform metrics, Logs/Log Analytics, Application Insights, alerts, workbooks, dashboards, managed Prometheus, diagnostic settings, and related experiences. Application Insights monitors application requests, dependencies, exceptions, traces, custom events/metrics, and distributed transactions; data is stored through a Log Analytics workspace connection.

Use the current Azure Monitor OpenTelemetry distributions for supported application stacks unless a specific constraint requires another path.

## Architecture

Define:

- Application Insights resource/workspace topology by ownership, access, residency, retention, and query needs.
- Diagnostic settings for Azure resources.
- Data collection rules/transforms where appropriate.
- Private ingestion/query requirements.
- Managed identities and RBAC for queries/alerts/export.
- Sampling and daily caps with explicit data-loss behavior.
- Archive/export/SIEM path for security/compliance.
- Cost budgets and usage monitoring.

Shared workspaces simplify cross-service analysis but expand access and transformation impact. Separate when data sensitivity, residency, ownership, or blast radius requires it.

## KQL and investigation

Kusto Query Language turns telemetry into evidence. Start with time range and affected service/version, then requests/errors, dependencies, exceptions, traces, and resource health. Join using operation/trace identifiers and deployment annotations.

Preserve reusable queries as version-controlled artifacts where possible. A dashboard without definitions/owner can drift into decorative monitoring.

## Data quality

Validate SDK initialization, service/resource names, environment/version tags, clock accuracy, sampling, exception capture, dependency instrumentation, ingestion latency, and schema changes. Emit a controlled synthetic signal and verify end-to-end arrival.

Ingestion transformations can filter or modify data but may affect every application sharing a table/workspace. Test scope carefully and never rely on destructive filtering without governance.

## Interview preparation

**Azure Monitor versus Application Insights?**  
Azure Monitor is the overall monitoring platform; Application Insights is its application-performance/observability capability.

**Why workspace design matters?**  
It controls access, retention, cost, residency, transformations, cross-service querying, and blast radius.

**What is first when telemetry disappears?**  
Verify application emission/export, credentials/network, sampling/configuration, ingestion health/quota, resource/workspace mapping, and time range before concluding the app is healthy.

## Practical exercise

Send requests, dependencies, exceptions, logs, and a custom business event. Query them by operation and version, build a workbook, restrict one reader, test retention/sampling, and create a telemetry-pipeline health alert.

## Official references

- [Application Insights overview](https://learn.microsoft.com/azure/azure-monitor/app/app-insights-overview)
- [Application Insights telemetry model](https://learn.microsoft.com/azure/azure-monitor/app/data-model-complete)
- [Azure Monitor Logs and KQL](https://learn.microsoft.com/azure/azure-monitor/logs/log-query-overview)

[Next: Health Checks and Availability Tests →](03-health-checks-and-availability-tests.md)
