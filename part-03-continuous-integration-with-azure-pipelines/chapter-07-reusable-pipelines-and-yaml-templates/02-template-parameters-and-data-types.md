# Template Parameters and Data Types

[← Template Types](01-step-job-stage-and-variable-templates.md) · [Chapter 7](README.md) · [Next: Conditional Insertion →](03-conditional-insertion-and-each-loops.md)

## Parameters form an API

A reusable template is an internal API. Parameter names, types, defaults, allowed values, meaning, and compatibility expectations are its contract. Parameters are resolved during template parsing, before jobs execute; variables can change later and are better for runtime state.

## Available design tools

Common parameter types include `string`, `number`, `boolean`, `object`, `step`, `stepList`, `job`, `jobList`, `deployment`, `deploymentList`, `stage`, and `stageList`. Not every type is valid in every context, so verify the schema.

```yaml
parameters:
- name: vmImage
  type: string
  default: ubuntu-latest
  values:
  - ubuntu-latest
  - windows-latest

- name: runIntegration
  type: boolean
  default: false

- name: projects
  type: object
  default: []

- name: additionalSteps
  type: stepList
  default: []
```

Typed booleans avoid the classic mistake where a non-empty string such as `"false"` behaves truthily. Allowed `values` prevent unsupported choices and improve discoverability.

## Data, not code

A string parameter inserted into a script can become command injection:

```yaml
# Unsafe if commandArgs is not fully trusted
- script: ./build.sh ${{ parameters.commandArgs }}
```

Prefer constrained choices that the template maps to fixed commands. If customization genuinely requires steps, use a `stepList`, decide where it runs, and understand that the consumer is supplying executable pipeline structure.

Parameters are not secret storage. Compile-time values may appear in expanded YAML, logs, or metadata. Pass secrets through protected runtime mechanisms and avoid echoing them.

## Contract design

- Use safe, unsurprising defaults.
- Require values when no safe default exists.
- Name by intent: `publishPackages`, not `flag2`.
- Prefer a small object with a documented schema for related settings.
- Reject invalid combinations early.
- Avoid exposing implementation details likely to change.
- Document whether an empty list means “none” or “use defaults.”
- Add compile tests for every supported variant.

An `object` is flexible but weakly constrained. Validate expected keys with compile-time logic where possible and keep the object shallow. Excessive options usually signal that the template owns too many responsibilities.

## Parameters versus variables

Use a parameter to choose the pipeline's structure, such as whether a stage exists or which jobs are generated. Use a variable for a value needed during execution, such as a task output, agent-provided path, or value from a protected variable group. Runtime variables cannot retroactively change a compile-time plan.

## Interview preparation

**Why prefer parameters over variables for template inputs?**  
Parameters are typed and resolved while the plan is created, supporting structural insertion and early validation. Variables are strings and runtime-oriented.

**When is `stepList` risky?**  
When consumers can insert arbitrary executable steps into a privileged job. Limit its placement and protect the surrounding resources.

**Should a template accept a service connection name?**  
It can, but the connection must still be explicitly authorized and protected. Avoid constructing resource identities from uncontrolled text.

## Practical exercise

Create a job template with a string constrained to two VM images, a boolean test switch, an object list of projects, and a `stepList`. Test valid and invalid inputs. Replace one raw command string with a constrained choice.

## Official references

- [Template parameters](https://learn.microsoft.com/azure/devops/pipelines/process/template-parameters)
- [Template parameter data types](https://learn.microsoft.com/azure/devops/pipelines/yaml-schema/parameters-parameter)

[Next: Conditional Insertion and Each Loops →](03-conditional-insertion-and-each-loops.md)
