# Conditional Insertion and Each Loops

[← Template Parameters](02-template-parameters-and-data-types.md) · [Chapter 7](README.md) · [Next: Central Repositories →](04-central-template-repositories.md)

## Shape the plan at compile time

Template expressions use `${{ }}` while Azure Pipelines compiles the execution plan. They can insert mappings or sequences, choose branches, and iterate over objects. Runtime conditions decide whether already-created stages, jobs, or steps execute.

```yaml
parameters:
- name: projects
  type: object
  default: []
- name: publish
  type: boolean
  default: false

steps:
- ${{ each project in parameters.projects }}:
  - script: dotnet test ${{ project.path }}
    displayName: Test ${{ project.name }}

- ${{ if eq(parameters.publish, true) }}:
  - script: dotnet pack --configuration Release
    displayName: Package
```

After expansion, the agent receives concrete steps. A runtime output does not exist during expansion, so it cannot be used to generate a compile-time loop.

## Conditional insertion patterns

Use `${{ if }}` to include a step, job, stage, or mapping property. Use `${{ insert }}` to merge additional properties into a mapping. Use `${{ each }}` for a known compile-time collection.

A wrapper job template can iterate through consumer jobs and add mandatory pre/post steps. This is powerful for telemetry or policy, but preserve dependencies, properties, and failure conditions carefully. An incorrectly wrapped job can change semantics.

## Compile-time versus runtime

| Need | Mechanism |
|---|---|
| Generate one job per declared service | `${{ each }}` |
| Include a scan based on a boolean parameter | `${{ if }}` |
| Run cleanup even after a failed build | Runtime `condition:` |
| Use a previous job's output | Runtime dependency expression |
| Merge caller-supplied variables | `${{ insert }}` |

Inspect the expanded plan when behavior is surprising. Ask first: “Did this node fail to exist, or did it exist and get skipped?” That distinction usually reveals compile-time versus runtime confusion.

## Maintainability rules

- Keep conditions short; move complicated policy into named parameters or smaller templates.
- Use objects with consistent keys rather than parallel arrays.
- Give generated jobs stable names.
- Document empty-collection behavior.
- Test zero, one, and many items.
- Test every conditional branch, including invalid input.
- Avoid dynamically generating a pipeline so abstract that reviewers cannot recognize the execution plan.

## Interview preparation

**Why can’t a task output control an `each` loop?**  
The loop expands before tasks run; the output does not yet exist.

**Conditional insertion versus `condition`?**  
Insertion decides whether YAML becomes part of the plan. A runtime condition evaluates whether an existing node runs.

**How do you debug generated YAML?**  
Validate the pipeline, inspect the expanded/compiled plan and parameters, simplify the expression, and verify which phase owns every value.

## Practical exercise

Build a template that iterates over two services, creates a test step for each, and conditionally adds packaging. Test with no services. Then try to use a task output in the loop, document why it fails, and replace it with a runtime condition.

## Official references

- [Template expressions](https://learn.microsoft.com/azure/devops/pipelines/process/template-expressions)
- [Expressions](https://learn.microsoft.com/azure/devops/pipelines/process/expressions)
- [Conditions](https://learn.microsoft.com/azure/devops/pipelines/process/conditions)

[Next: Central Template Repositories →](04-central-template-repositories.md)
