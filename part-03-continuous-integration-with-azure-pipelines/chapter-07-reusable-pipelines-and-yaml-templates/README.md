# Chapter 7 — Reusable Pipelines and YAML Templates

[← Part III — Continuous Integration with Azure Pipelines](../README.md)

Templates turn proven pipeline practices into reusable building blocks. This chapter goes beyond removing duplicate YAML: it shows how to design a stable internal product that gives teams a paved road while security and platform owners retain enforceable controls.

## Why this chapter matters

Copy-and-paste pipelines drift. Fixes reach some repositories but not others; required scans disappear; and upgrades become campaigns. A well-designed template library centralizes safe defaults, makes policy visible, and still gives application teams ownership of application-specific behavior. A badly designed library becomes a hidden framework that nobody can debug.

## What you will learn

- Select step, job, stage, and variable templates at the correct boundary.
- Design typed, validated parameters instead of loosely controlled variables.
- Shape a pipeline at compile time with conditional insertion and iteration.
- Consume centrally owned templates with pinned references.
- Evolve templates using explicit compatibility and deprecation policies.
- Enforce required templates through protected-resource checks.
- Balance organizational governance with team flexibility and clear escape paths.

## Topics

| # | Topic | Outcome |
|---:|---|---|
| 1 | [Step, Job, Stage, and Variable Templates](01-step-job-stage-and-variable-templates.md) | Choose a reusable unit deliberately |
| 2 | [Template Parameters and Data Types](02-template-parameters-and-data-types.md) | Create a safe consumer contract |
| 3 | [Conditional Insertion and Each Loops](03-conditional-insertion-and-each-loops.md) | Generate readable pipeline plans |
| 4 | [Central Template Repositories](04-central-template-repositories.md) | Share templates across projects |
| 5 | [Template Versioning and Compatibility](05-template-versioning-and-compatibility.md) | Upgrade without surprise breakage |
| 6 | [Required Template Checks](06-required-template-checks.md) | Make protected-resource policy enforceable |
| 7 | [Governance, Flexibility, and Abstraction](07-governance-flexibility-and-abstraction.md) | Operate templates as an internal product |

## Guided chapter lab

Create a central template library containing:

1. A step template for restore/build/test.
2. A job template accepting an object and a `stepList`.
3. A stage template for CI evidence.
4. A consumer pipeline pinned to a versioned template reference.
5. A deliberately rejected task to demonstrate template policy.
6. Documentation, compatibility tests, changelog, and deprecation notice.

Use Azure Pipelines' expanded plan or validation facilities to inspect compile-time output. Test both an expected-success consumer and expected-failure consumer.

## Design review questions

- Can a consumer understand the generated job without opening five files?
- Are parameter names, types, defaults, and constraints documented?
- Can an untrusted value become executable YAML?
- Is the template version pinned and recoverable?
- Who owns a breaking change, migration, and emergency rollback?
- Which controls are enforced outside editable YAML?
- Is there a documented exception process?

## Completion criteria

You are ready to continue when you can identify the correct template type, explain compile-time expansion, publish a versioned central library, reject unsafe customization, and describe where template governance ends and protected-resource governance begins.

## Chapter navigation

[← Chapter 6](../chapter-06-continuous-integration-design/README.md) · [Chapter 8 →](../chapter-08-artifacts-and-dependency-management/README.md)
