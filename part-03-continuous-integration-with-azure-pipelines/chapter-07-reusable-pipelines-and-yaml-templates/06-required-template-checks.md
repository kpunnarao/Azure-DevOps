# Required Template Checks

[← Template Versioning](05-template-versioning-and-compatibility.md) · [Chapter 7](README.md) · [Next: Governance and Flexibility →](07-governance-flexibility-and-abstraction.md)

## Policy outside editable YAML

If a repository author can remove the security step in the same pull request as the code change, that step is convention, not a hard control. Azure Pipelines approvals and checks are configured on protected resources outside the YAML file. A **Required template** check can require any pipeline consuming that resource to extend a specified template.

This is useful for service connections, environments, agent pools, variable groups, secure files, and other protected resources where supported.

## Conceptual pattern

A platform team creates a governed `extends` template:

```yaml
# approved-start.yml
parameters:
- name: buildSteps
  type: stepList
  default: []

stages:
- stage: GovernedCI
  jobs:
  - job: Build
    steps:
    - script: ./required-security-scan.sh
    - ${{ each step in parameters.buildSteps }}:
      - ${{ step }}
```

A consumer extends it:

```yaml
extends:
  template: approved-start.yml@securityTemplates
  parameters:
    buildSteps:
    - script: ./build.sh
```

The protected resource's check names the required template. When the pipeline attempts to use that resource, Azure Pipelines evaluates the check.

## What this does—and does not—guarantee

It can require a structural relationship with an approved template. The template can constrain accepted tasks or properties and can reject unexpected YAML during parsing. It does not automatically make every script safe, prove code quality, or prevent the pipeline from using some different unprotected resource. Resource permissions, branch protection, template-repository protection, secret controls, agent isolation, and review still matter.

Checks are managed by resource owners, not pipeline authors. This separation is the reason they are stronger than an ordinary step.

## Design checklist

- Put the template in a tightly protected repository.
- Pin the required template according to organizational version policy.
- Keep mandatory controls impossible to bypass through a flexible hook.
- Restrict consumer `stepList` placement.
- Reject disallowed task types or inputs with a clear compile error.
- Test permitted, rejected, and bypass-attempt examples.
- Protect the resource and template repository administration path.
- Create an audited, time-limited emergency exception process.

## Common mistakes

- Calling a template “required” without configuring a resource check.
- Allowing arbitrary consumer steps before the mandatory scan.
- Protecting one service connection while an equivalent unprotected one exists.
- Enforcing an unversioned template reference.
- Forgetting that administrators and resource owners form part of the trust model.
- Producing rejection messages too cryptic for teams to remediate.

## Interview preparation

**Why is a required-template check stronger than documentation?**  
It is evaluated by a protected resource outside the consumer's editable pipeline and can block use when the approved structure is absent.

**Can the template reject a task?**  
An `extends` template can inspect supplied structure during expansion and deliberately cause a YAML error for disallowed keys or task types, within expression capabilities.

**What else must be protected?**  
The central repository, resource ownership, service connections, secrets, agent pools, branches, and the alternate paths that could bypass the guarded resource.

## Practical exercise

Create an `extends` template accepting a `stepList`. Permit scripts but reject one selected task type. Attach it as a required-template check to a sandbox resource, verify a compliant pipeline, and capture the error from a noncompliant pipeline.

## Official references

- [Approvals and checks: required template](https://learn.microsoft.com/azure/devops/pipelines/process/approvals)
- [Security through templates](https://learn.microsoft.com/azure/devops/pipelines/security/templates)

[Next: Governance, Flexibility, and Abstraction →](07-governance-flexibility-and-abstraction.md)
