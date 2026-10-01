# Chapter 5 — YAML Pipeline Fundamentals

[← Part III — Continuous Integration with Azure Pipelines](../README.md)

## Chapter purpose

This chapter establishes the Azure Pipelines execution model. YAML is compiled into a run plan before agents execute work. Stages organize lifecycle boundaries, jobs are schedulable units, and steps run sequentially within a job. Data availability depends on evaluation time, scope, dependencies, and execution location.

Understanding these rules is more important than memorizing task names.

## Learning objectives

- Read and validate Azure Pipelines YAML structure
- Configure CI, PR, schedule, and pipeline-resource triggers
- Choose stages, jobs, steps, tasks, and scripts appropriately
- Compare hosted, self-hosted, scale-set, and managed agent options
- Use agent pools, capabilities, demands, and parallel capacity
- Distinguish variables, parameters, variable groups, and secrets
- Apply expression syntax, conditions, dependencies, and outputs
- Publish artifacts and diagnostic evidence
- Design timeout, cancellation, retry, and cleanup behavior

## Execution hierarchy

```text
Pipeline
└── Stage
    └── Job
        └── Step
            ├── Task
            └── Script
```

Stages run sequentially by default. Jobs without dependencies in the same stage can run in parallel when capacity exists. Steps in one job run sequentially on the same assigned agent.

## Topics

1. [YAML Structure and Schema](01-yaml-structure-and-schema.md)
2. [Triggers and Pull Request Validation](02-triggers-and-pull-request-validation.md)
3. [Stages, Jobs, Steps, Tasks, and Scripts](03-stages-jobs-steps-tasks-and-scripts.md)
4. [Hosted and Self-Hosted Agents](04-hosted-and-self-hosted-agents.md)
5. [Agent Pools, Capabilities, and Demands](05-agent-pools-capabilities-and-demands.md)
6. [Variables, Parameters, and Variable Groups](06-variables-parameters-and-variable-groups.md)
7. [Expressions, Conditions, Dependencies, and Outputs](07-expressions-conditions-dependencies-and-outputs.md)
8. [Artifacts, Logging, Timeouts, and Cancellation](08-artifacts-logging-timeouts-and-cancellation.md)

## Chapter lab

Build a pipeline with:

- Branch-filtered CI
- Pull-request build validation
- Parameters for supported build modes
- Separate build and test jobs
- Parallel platform tests
- Published test results
- One output variable consumed downstream
- One pipeline artifact downloaded by a dependent job
- Cleanup that runs on failure but respects cancellation
- Explicit job timeouts
- Diagnostic logging without secrets

Break each major feature intentionally and record the evidence used to diagnose it.

## Common misconceptions

| Misconception | Correct model |
|---|---|
| YAML runs top to bottom exactly as written | YAML first compiles into a plan, then dependencies schedule work |
| A variable and a parameter are interchangeable | Parameters are typed compile-time inputs; variables are strings with runtime scopes |
| Consecutive jobs share a machine | Jobs may run on different agents |
| An always condition guarantees unlimited cleanup | Cancellation and job timeout still constrain execution |
| A secret variable is safe to echo | Masking is defensive, not authorization to print secrets |
| More jobs always make a pipeline faster | Parallelism needs capacity and can add artifact/setup overhead |

## Completion

- [ ] Draw compile and execution phases
- [ ] Explain every trigger in the lab
- [ ] Demonstrate agent isolation between jobs
- [ ] Diagnose a capability mismatch
- [ ] Use all three variable expression forms correctly
- [ ] Pass data across a dependency
- [ ] Publish and download an artifact
- [ ] Demonstrate failure and cancellation cleanup
