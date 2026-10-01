# Expressions, Conditions, Dependencies, and Outputs

> Chapter 5 — YAML Pipeline Fundamentals

[← Previous](06-variables-parameters-and-variable-groups.md) · [Chapter home](README.md) · [Next →](08-artifacts-logging-timeouts-and-cancellation.md)

## Purpose

Most “mysterious” pipeline behavior comes from evaluation timing or a dependency that does not exist. Compile-time expressions shape the plan; runtime conditions decide whether planned nodes execute; outputs travel only through supported dependency contexts.

## Expression forms

| Syntax | Evaluated | Available data |
|---|---|---|
| ${{ expression }} | Compile time | Parameters and statically available variables |
| $[ expression ] | Runtime | Runtime variables; no parameters |
| $(variable) | Before task execution | Current macro variable value |

Use bracket form for variable names containing dots or dynamic names.

## Conditions

Default job/stage behavior requires dependencies to succeed. Replacing condition replaces the default, so include succeeded() when appropriate.

```yaml
condition: and(
  succeeded(),
  eq(variables['Build.SourceBranch'], 'refs/heads/main')
)
```

Useful status functions include succeeded, failed, succeededOrFailed, canceled, and always. Always can run after cancellation, but agent/job cancellation timeout still applies.

Nothing computed inside a job is available to decide whether that same job starts.

## Dependencies

Use dependsOn to form a directed acyclic graph.

```yaml
jobs:
- job: Build
  steps: []

- job: UnitTests
  dependsOn: Build
  steps: []

- job: Analysis
  dependsOn: Build
  steps: []
```

UnitTests and Analysis can run in parallel after Build if capacity exists.

## Output variables

A named step can set an output:

```yaml
- bash: |
    echo "##vso[task.setvariable variable=packageVersion;isOutput=true]1.2.3"
  name: versionStep
```

A directly dependent downstream job maps the output through dependencies. Stage outputs use stageDependencies or the documented context for the location. Deployment jobs and matrix jobs have additional syntax differences.

Use output variables for small metadata, not files or secrets. Publish files as artifacts.

## Compile-time insertion

Template expressions can include or generate mappings, sequences, jobs, or steps. The resulting nodes exist before runtime. A runtime condition can skip an existing node but cannot create a new job.

## Cancellation

A condition based only on branch may still evaluate true after cancellation if parent state is not considered. Combine business logic with status functions. Cleanup steps should be bounded and idempotent.

## Debugging skipped work

1. Confirm the node exists in the compiled plan.
2. Read dependency results.
3. Inspect fully expanded condition inputs.
4. Check exact branch reference.
5. Confirm the output-producing step has a name.
6. Confirm isOutput and dependency link.
7. Check quote and case behavior.
8. Distinguish Skipped, Failed, and Canceled.

## Common mistakes

- Using runtime values in compile-time expressions
- Using a variable set in a job to control the start of that job
- Omitting succeeded() from a custom condition
- Referencing an output without dependsOn
- Confusing step displayName with step name
- Passing artifacts as huge encoded variables
- Using always() for unsafe deployment
- Comparing a short branch name with refs/heads/main

## Interview preparation

**Q: Compile-time versus runtime expression?**  
Compile time shapes the plan using parameters and static values. Runtime evaluates variables and state after the run begins.

**Q: Why is an output empty downstream?**  
Check named producing step, isOutput, dependency relationship, correct dependencies/stageDependencies syntax, and whether the producing step ran.

**Q: What happens when you specify condition?**  
It replaces the default condition. Include dependency success logic when needed.

**Q: Can runtime logic add a job?**  
No. Jobs must exist in the compiled plan; runtime conditions can only decide whether planned work executes.

## Further reading

- [Expressions](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/expressions)
- [Pipeline conditions](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/conditions)
- [Define variables and outputs](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables)
