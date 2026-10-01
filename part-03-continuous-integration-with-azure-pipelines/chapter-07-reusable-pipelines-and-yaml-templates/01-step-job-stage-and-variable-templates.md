# Step, Job, Stage, and Variable Templates

[← Chapter 7](README.md) · [Next: Template Parameters →](02-template-parameters-and-data-types.md)

## Choose the smallest meaningful boundary

Azure Pipelines templates can insert reusable variables, steps, jobs, or stages. The right choice expresses ownership and lifecycle—not merely the amount of YAML removed.

| Template | Best fit | Typical example |
|---|---|---|
| Variable | Shared non-secret values | Tool version or naming convention |
| Step | Reusable operation inside a job | Restore, scan, or publish results |
| Job | Isolated unit with pool and outputs | Build/test on one platform |
| Stage | Lifecycle boundary and dependencies | CI, compliance, or deployment stage |
| `extends` | Organization-approved outer structure | Governed pipeline skeleton |

A step template inherits its containing job's agent and workspace. A job template can choose its own pool, strategy, services, variables, and outputs. A stage template can model dependencies and larger lifecycle boundaries. Use `extends` when consumers should supply controlled inputs to a standard pipeline structure.

## Example: step template

```yaml
# templates/dotnet-test.yml
parameters:
- name: project
  type: string

steps:
- script: dotnet test ${{ parameters.project }} --configuration Release
  displayName: Test ${{ parameters.project }}
```

Consumer:

```yaml
steps:
- template: templates/dotnet-test.yml
  parameters:
    project: src/Orders.Tests/Orders.Tests.csproj
```

Do not pass an arbitrary shell fragment as a string parameter. Prefer data such as a project path or enum-like choice, then keep executable structure inside the trusted template.

## Composition principles

- Each template should have one coherent responsibility.
- Make inputs and outputs explicit.
- Keep secrets in protected variable groups or service connections, not template files.
- Avoid deep nesting that makes expansion impossible to understand.
- Give jobs and steps stable, meaningful names when downstream references depend on them.
- Publish a minimal example beside every public template.
- Prefer composition of a few clear templates over one parameter-heavy mega-template.

Templates are expanded before runtime. The resulting pipeline must remain within Azure Pipelines limits on files, nesting, and parse resources; excessive abstraction can fail compilation as well as harm maintainability.

## Common mistakes

- Using a stage template for a three-line command.
- Placing repository-specific assumptions in a central step.
- Duplicating a job template when a matrix would express the variation.
- Treating variable templates as a secret store.
- Hiding every command behind multiple template layers.
- Changing a template job name and breaking output references.

## Interview preparation

**When would you use a job rather than a step template?**  
When the reusable unit needs its own agent, workspace isolation, services, matrix strategy, timeout, or outputs.

**What does `extends` add?**  
It lets a pipeline inherit an outer governed structure and can reject disallowed content during template expansion.

**Can templates reduce security?**  
Yes. A template that accepts executable strings, imports an untrusted version, or silently grants broad resources can centralize a vulnerability.

## Practical exercise

Extract duplicated restore/test commands into a step template, then promote it to a job template with an explicit pool and output. Compare the expanded plans and document which abstraction communicates the design more clearly.

## Official references

- [YAML templates](https://learn.microsoft.com/azure/devops/pipelines/process/templates)
- [Template schema](https://learn.microsoft.com/azure/devops/pipelines/yaml-schema/template)

[Next: Template Parameters and Data Types →](02-template-parameters-and-data-types.md)
