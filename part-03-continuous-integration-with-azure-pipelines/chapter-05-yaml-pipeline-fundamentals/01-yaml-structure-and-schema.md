# YAML Structure and Schema

> Chapter 5 — YAML Pipeline Fundamentals

[Chapter home](README.md) · [Next →](02-triggers-and-pull-request-validation.md)

## Purpose

Azure Pipelines YAML is a declarative definition that Azure DevOps parses, expands, validates, and converts into an execution plan. YAML syntax can be valid while the Azure Pipelines schema is invalid, so both layers matter.

## Minimal pipeline

```yaml
trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- script: echo "Validate the application"
  displayName: Validate
```

A pipeline can define steps directly for a single implicit job, jobs directly for a single implicit stage, or explicit stages. Do not mix incompatible root structures.

## YAML fundamentals

- Indentation defines nesting; use spaces consistently
- Sequences begin with a dash
- Mappings use key and value pairs
- Quote values when YAML might interpret them unexpectedly
- Literal block style preserves multiline script structure
- Comments begin with a hash outside quoted content
- Duplicate keys can create ambiguous or invalid behavior

Use an editor with YAML and Azure Pipelines schema support, but validate through Azure DevOps because product schema and template context determine the final result.

## Pipeline processing

A simplified model:

1. Load the root YAML from the selected source version.
2. Resolve repository resources needed for templates.
3. Expand templates and compile-time expressions.
4. Validate the resulting plan and resource permissions.
5. Evaluate stage dependencies and conditions.
6. Allocate agents for ready jobs.
7. Run steps in each job.
8. Publish logs, results, variables, and artifacts.

Runtime-generated files cannot become templates for the same run because templates must exist when the plan is compiled.

## Top-level concerns

Common root elements include:

- name
- trigger and pr where supported
- schedules
- parameters
- variables
- resources
- pool
- stages, jobs, or steps
- extends

The exact schema depends on Azure DevOps Services or Server version. Use the documentation version selector.

## Naming

Stage and job identifiers are referenced by dependencies and outputs. Keep identifiers stable, concise, and free from display-only wording. Use displayName for human-readable labels.

Example:

```yaml
stages:
- stage: Build
  displayName: Build and validate
  jobs:
  - job: Linux
    displayName: Linux build
```

## Validation routine

When parsing fails:

1. Check indentation and sequence/mapping placement.
2. Identify whether the error is YAML syntax or pipeline schema.
3. Expand or simplify templates.
4. Confirm the element is allowed at that scope.
5. Check product/version documentation.
6. Remove sections until a minimal plan validates.
7. Reintroduce one element at a time.

## Common mistakes

- Tabs or inconsistent indentation
- Putting steps beside jobs at the same scope
- Treating displayName as an identifier
- Using a task input at the step level
- Assuming GitHub Actions syntax works in Azure Pipelines
- Copying Azure DevOps Services syntax into an older Server version
- Generating YAML during a job and expecting it to alter the current plan
- Keeping one enormous pipeline file with no clear stages

## Interview preparation

**Q: Is Azure Pipelines YAML executed directly?**  
No. Azure DevOps parses and expands it into a run plan before scheduling jobs and executing steps.

**Q: YAML validation versus pipeline schema validation?**  
YAML validation checks document syntax. Pipeline schema validation checks whether Azure Pipelines elements and properties are valid in their scopes.

**Q: Why can a generated template not be used later in the same run?**  
Template expansion happens before jobs execute, so the generated file does not exist during compilation.

**Q: Stage name versus displayName?**  
The stage identifier is used in dependencies and expressions; displayName is a user-facing label.

## Practical exercise

Start with one implicit job, refactor to explicit jobs, then to stages. Introduce one indentation failure and one valid-YAML/invalid-schema failure. Compare error messages and document diagnosis.

## Further reading

- [Azure Pipelines YAML schema](https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/)
- [Steps schema](https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/steps)
- [Pipeline runs](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/runs)
