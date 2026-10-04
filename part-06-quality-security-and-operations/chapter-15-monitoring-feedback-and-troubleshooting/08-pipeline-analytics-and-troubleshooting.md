# Pipeline Analytics and Troubleshooting

[← Delivery Performance](07-delivery-performance-metrics.md) · [Chapter 15](README.md)

## Pipeline health

Azure Pipelines analytics can show pass-rate and duration trends, stage failures, top failing tasks, and test failures depending on service/version and data. Combine built-in views with queue time, cancellation, retries, agent utilization, cache hit rate, flaky tests, infrastructure failures, and time to first useful feedback.

Separate product/test failure from pipeline/platform failure. A high pass rate caused by skipped tests is not health.

## Phase-based diagnosis

1. **Trigger:** branch/path/PR/pipeline-resource/schedule filters and branch version.
2. **Compile:** schema, template reference, parameters, expression expansion.
3. **Authorization/checks:** repository, pool, service connection, environment, variable group, approval.
4. **Queue/scheduling:** parallel jobs, pool availability, demands/capabilities.
5. **Checkout/download:** token scope, network, LFS/submodules, artifact identity.
6. **Execution:** tool version, environment, exit code, timeout, disk/memory, script.
7. **Data flow:** variable syntax, outputs, dependencies, conditions, secret scope.
8. **Publish:** test/result/artifact/feed permissions and paths.
9. **Deployment:** target identity, checks, configuration, health, locks.
10. **Post-run:** retention, analytics ingestion, cleanup, notification.

Ask: did the node not exist, exist but skip, wait, fail, cancel, or succeed with wrong output?

## Evidence discipline

Capture run ID, source commit/branch, YAML/template revisions, parameters, resource versions, agent/pool/image, timeline, exact failing task/exit code, system diagnostics where safe, and linked service-health events. Redact credentials before sharing logs.

Enable verbose/system diagnostics only for a bounded reproduction; extra logs can reveal environment details and increase noise.

## Failure patterns

- Queue delay: parallelism/pool capacity/demand mismatch.
- Sudden hosted-agent failure: image/tool update or external outage.
- Stale cache: incompatible key or correctness relying on cache.
- Empty output: wrong name/dependency or compile/runtime syntax.
- Skipped stage: default success dependency/condition.
- 401/403: wrong effective identity or resource authorization.
- Intermittent network: retry only idempotent operation with bounded backoff.
- “Green” but no artifact: publish path empty or condition skipped.

## Improvement loop

Rank failures by lost developer time: frequency × affected people × duration. Fix top systemic causes, add early validation and clearer diagnostics, then measure recurrence. Do not hide failure with indiscriminate retries.

## Interview preparation

**First step on failed pipeline?**  
Identify the phase and first causal error using exact run/source/template/agent evidence, not the final cascade message.

**Why did a stage skip?**  
Inspect compiled plan, dependencies, default `succeeded()` behavior, condition evaluation, canceled/failed predecessors, and output reference phase.

**How distinguish platform from code?**  
Compare unchanged commits across agents/runs, service health, image/tool versions, neighboring pipelines, network/resource symptoms, and isolated reproduction.

## Practical exercise

Create failures in trigger, template compile, authorization, agent demand, condition, output variable, cache, test publishing, artifact path, and deployment health. Build a decision tree and reduce mean diagnosis time on a second run.

## Official references

- [Azure Pipeline reports](https://learn.microsoft.com/azure/devops/pipelines/reports/pipelinereport)
- [Troubleshoot Azure Pipelines](https://learn.microsoft.com/azure/devops/pipelines/troubleshooting/troubleshooting)
- [Agent diagnostics](https://learn.microsoft.com/azure/devops/pipelines/agents/agent)

## Part VI review

You should now be able to operate this assurance loop:

```text
risk → tests and controls → secure release → production signals
 → SLO decision → alert/incident → recovery → measured learning
 → improved requirement, test, pipeline, policy, or architecture
```

Return to the [Part VI overview](../README.md), complete the project and checklists, and preserve the test strategy, access review, evidence map, observability specification, SLO policy, and incident review as reusable teaching material.
