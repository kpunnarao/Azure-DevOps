# Canary Deployment

[← Blue-green Deployment](02-blue-green-deployment.md) · [Chapter 10](README.md) · [Next: Feature Flags →](04-feature-flags.md)

## Progressive exposure

A canary deploys the new version to a small, representative cohort, observes it, and increases exposure only when defined signals remain healthy. The objective is controlled blast radius plus real-production evidence.

Cohorts can be a percentage of traffic, selected tenants/users, regions, deployment stamps, internal employees, or dedicated instances. Prefer stable cohort assignment for stateful user journeys.

## Azure Pipelines strategy

Deployment jobs support a canary strategy with lifecycle hooks and increments for applicable targets/tasks. Platform-native traffic mechanisms may still be required. Do not assume that declaring `canary` automatically creates weighted routing.

A conceptual structure is:

```yaml
strategy:
  canary:
    increments: [5, 25, 50]
    deploy:
      steps:
      - script: ./deploy-canary.sh
    routeTraffic:
      steps:
      - script: ./set-traffic-weight.sh
    postRouteTraffic:
      steps:
      - script: ./evaluate-health.sh
```

Verify task support and actual traffic behavior for your target platform.

## Designing increments

Choose steps based on:

- Severity of a potential failure.
- Traffic required for statistical confidence.
- Time needed for delayed defects to appear.
- Ability to isolate cohort metrics.
- Remaining healthy capacity.
- Recovery time.
- Peak/seasonal behavior.

A 1% cohort may still include thousands of important customers, or may provide almost no signal. Percentage alone is not risk.

## Promotion and abort policy

Define before rollout:

- Success: error, latency, saturation, and business metrics within threshold.
- Guardrail: zero tolerance for security, data-integrity, or critical-flow failure.
- Observation window: long enough for workload cycles.
- Abort: stop new exposure and isolate or reverse the canary.
- Authority: who may override automation and how it is recorded.

Compare canary against a simultaneous control/baseline to distinguish deployment impact from a general incident.

## Common mistakes

- Selecting only friendly low-risk users and missing representative behavior.
- Monitoring fleet averages that hide canary failure.
- Advancing on absence of alerts instead of explicit success.
- Making intervals too short for delayed effects.
- Using one shared database with an incompatible schema.
- Lacking automatic stop or rehearsed traffic reversal.

## Interview preparation

**Canary versus blue-green?**  
Blue-green focuses on parallel environments and a switch; canary focuses on gradual exposure and measurement. They can be combined.

**What metrics gate a canary?**  
Customer outcomes plus golden signals—latency, traffic, errors, saturation—and workload-specific integrity/security measures, compared to a valid baseline.

**What if signal volume is low?**  
Use a larger or longer cohort, synthetic transactions, risk-based manual review, or a different strategy. Do not claim confidence from insufficient data.

## Practical exercise

Route 5%, 25%, then 100% to a candidate. Tag telemetry by version/cohort. Inject increased error rate only in the canary, ensure fleet averages do not hide it, and automatically halt before the next increment.

## Official references

- [Deployment-job canary strategy](https://learn.microsoft.com/azure/devops/pipelines/process/deployment-jobs)
- [Continuous delivery and deployment rings](https://learn.microsoft.com/devops/deliver/what-is-continuous-delivery)

[Next: Feature Flags →](04-feature-flags.md)
