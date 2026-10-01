# Variables, Parameters, and Variable Groups

> Chapter 5 — YAML Pipeline Fundamentals

[← Previous](05-agent-pools-capabilities-and-demands.md) · [Chapter home](README.md) · [Next →](07-expressions-conditions-dependencies-and-outputs.md)

## Purpose

Azure Pipelines has multiple data mechanisms. Their evaluation time, type, scope, mutability, and security differ.

## Comparison

| Mechanism | Time | Type | Mutability | Best use |
|---|---|---|---|---|
| Runtime parameter | Template parsing | Typed | Fixed for run | Select structure or approved options |
| YAML/UI variable | Compile or runtime syntax | String | Can vary by scope/run | Configuration and conditions |
| Output variable | Runtime after producing step/job | String | Produced once per context | Pass small values downstream |
| Variable group | Runtime/library resource | String/secret | Centrally managed | Shared non-secret and secret values |
| Secure file/secret store | Runtime protected resource | File/value | Managed externally | Sensitive material |

## Parameters

Parameters are typed and available during template parsing. They can determine which jobs, stages, or steps exist.

```yaml
parameters:
- name: runExtendedTests
  type: boolean
  default: false

steps:
- ${{ if eq(parameters.runExtendedTests, true) }}:
  - script: ./run-extended-tests.sh
```

Do not use parameters for secrets. Parameter values can be visible during plan construction and UI selection.

## Variables

Variables are strings and can exist at pipeline, stage, job, UI, group, or runtime scopes. More local YAML scope normally takes precedence over broader scope.

Syntax matters:

- Template: ${{ variables.name }} — compile time
- Macro: $(name) — runtime before a task
- Runtime: $[ variables.name ] — runtime expression, typically full value

A variable set during one step can be available to later steps in the same job, but not retroactively to the current step's already-expanded macro input.

## Variable groups

Variable groups share values across pipelines and are protected resources in relevant scenarios. Authorize only required pipelines. Apply approvals/checks where the platform supports and risk requires them.

Linking an Azure Key Vault-backed group does not remove the need to control pipeline and agent access. Any job receiving a secret can potentially expose it.

## Secret handling

- Mark secret variables appropriately
- Do not echo them
- Do not put secrets in parameters, source YAML, output variables, artifacts, or cache keys
- Map secrets explicitly to environment variables
- Avoid command-line arguments
- Restrict pipeline and group authorization
- Rotate after suspected exposure
- Prefer workload identity over stored credentials

Secret masking is not a data-loss-prevention system and may not mask substrings or transformed values.

## Naming and precedence

Avoid system-reserved prefixes and ambiguous names. Use uppercase environment variables in scripts consistently and document mapping. Do not reuse one name at many scopes unless override behavior is intentional.

## Common mistakes

- Selecting pipeline structure with runtime variables
- Using untyped string variables where a boolean parameter is needed
- Storing secrets in a variable template
- Authorizing a variable group to all pipelines
- Printing environment diagnostics containing secrets
- Assuming a macro changes earlier in the same task
- Shadowing a root variable at job scope unintentionally
- Passing large structured data through variables

## Interview preparation

**Q: Parameter versus variable?**  
A parameter is typed and resolved while compiling the pipeline plan. A variable is a string evaluated through compile-time or runtime syntax and can have runtime scope.

**Q: Can parameters contain secrets?**  
They should not. Use protected secret mechanisms and tightly controlled runtime access.

**Q: Why does a variable show an old value with template syntax?**  
Template expressions are resolved before runtime changes occur.

**Q: How do you share secrets across pipelines?**  
Use a protected variable group or secret store integration, authorize only necessary pipelines, and prefer eliminating credentials through workload identity.

## Practical exercise

Create a typed parameter that includes an optional test job, a root variable overridden at job scope, a runtime-set variable consumed by a later step, and a protected variable group. Predict every value before running.

## Further reading

- [Define variables](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables)
- [Runtime parameters](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/runtime-parameters)
- [Variable groups](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/variable-groups)
