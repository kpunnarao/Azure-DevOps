# Chapter 15 — Monitoring, Feedback, and Troubleshooting

[← Part VI — Quality, Security, and Operations](../README.md)

This chapter connects telemetry to decisions. It explains how to instrument a system, define reliability objectives, correlate releases with behavior, create actionable alerts, respond to incidents, measure delivery, and troubleshoot Azure Pipelines systematically.

## Topics

| # | Topic | Practical outcome |
|---:|---|---|
| 1 | [Logs, Metrics, Traces, and Observability](01-logs-metrics-traces-and-observability.md) | Design correlated diagnostic evidence |
| 2 | [Azure Monitor and Application Insights](02-azure-monitor-and-application-insights.md) | Build an Azure-native telemetry path |
| 3 | [Health Checks and Availability Tests](03-health-checks-and-availability-tests.md) | Observe customer-accessible behavior |
| 4 | [SLIs, SLOs, and Error Budgets](04-slis-slos-and-error-budgets.md) | Turn reliability into release policy |
| 5 | [Deployment Markers and Release Annotations](05-deployment-markers-and-release-annotations.md) | Correlate change with behavior |
| 6 | [Actionable Alerts and Incident Management](06-actionable-alerts-and-incident-management.md) | Notify owners with useful context |
| 7 | [Delivery Performance Metrics](07-delivery-performance-metrics.md) | Improve throughput and stability responsibly |
| 8 | [Pipeline Analytics and Troubleshooting](08-pipeline-analytics-and-troubleshooting.md) | Diagnose delivery failures by phase |

## Guided chapter lab

Instrument the sample service with OpenTelemetry/Application Insights. Create a workbook/dashboard showing traffic, latency, errors, saturation, dependency behavior, build version, deployment cohort, and one business outcome.

Then:

1. Define availability and latency SLIs/SLOs.
2. Calculate a rolling error budget and burn-rate alerts.
3. Add an external availability test.
4. Write a deployment marker containing artifact digest, commit, run, environment, and feature state.
5. Route an alert through an action group to a sandbox incident workflow.
6. Inject a release regression and dependency failure.
7. Correlate traces/logs/metrics and identify the changed version.
8. Stop or reverse the rollout and verify restoration.
9. Analyze pipeline pass rate, duration, queue time, and failure ownership.
10. Run a blameless review and update one test, alert, runbook, and pipeline control.

## Completion criteria

You are ready to complete Part VI when you can start from a customer symptom, follow correlated telemetry to a version/change, apply an SLO-based decision, coordinate recovery, and turn the incident into measurable engineering improvement.

## Chapter navigation

[← Chapter 14](../chapter-14-azure-devops-security-and-compliance/README.md) · [Part VI overview](../README.md)
