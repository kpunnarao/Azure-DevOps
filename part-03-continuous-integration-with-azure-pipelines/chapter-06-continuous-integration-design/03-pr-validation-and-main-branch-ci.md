# PR Validation and Main-Branch CI

[← Deterministic Builds](02-deterministic-and-reproducible-builds.md) · [Chapter 6](README.md) · [Next: Test Pyramids →](04-test-pyramids-and-quality-gates.md)

## Two feedback loops

Pull-request validation and main-branch CI serve related but different purposes.

| Workflow | Primary question | Desired behavior |
|---|---|---|
| PR validation | Is this proposed change safe to merge? | Fast, isolated, no privileged side effects |
| Main-branch CI | Is the integrated branch releasable? | Authoritative build, full evidence, immutable output |

A PR run may execute compilation, linting, unit tests, secret scanning, and selected integration tests. The main-branch run normally repeats essential tests in the merged context, performs broader checks, assigns the release identity, and publishes the official artifact.

## Why the merged context matters

Two branches can pass independently yet conflict after merging. Branch policies should require a current validation result, but the post-merge build remains the source of truth for release candidates. Do not promote an artifact built from an obsolete PR head unless your merge strategy guarantees that it is exactly the committed result and your controls prove that relationship.

For Azure Repos Git, configure build validation through branch policies. YAML `pr:` trigger behavior differs by repository provider, so understand whether validation is configured in Azure Repos policies, GitHub checks, or YAML.

## Security boundary

PR code is untrusted until accepted. A contributor can modify scripts and pipeline YAML. Therefore:

- Do not expose production secrets to ordinary PR validation.
- Do not publish official packages from untrusted forks.
- Grant the job token only the repository and resources it needs.
- Avoid running PR-controlled code on a sensitive persistent agent.
- Review changes to pipeline and template files carefully.
- Separate privileged post-merge activity from validation.

## Example trigger intent

```yaml
trigger:
  branches:
    include:
    - main

pr:
  branches:
    include:
    - main
  paths:
    exclude:
    - docs/**
```

This communicates intent, but repository-specific trigger behavior and branch policies must be verified. Path exclusions can improve speed, yet a documentation-only assumption is unsafe if documentation generates code or packaging metadata.

## Designing useful PR feedback

Order steps so inexpensive, high-signal failures happen first: formatting, manifest validation, compile, unit tests, then heavier integration tests. Publish test results even on failure so developers can diagnose the run. Cancel superseded PR runs when supported, but do not let cancellation hide a required gate.

A status check should be:

- Required for protected branches.
- Bound to the correct pipeline and branch.
- Resistant to stale success.
- Clear about failure ownership.
- Fast enough that developers do not bypass it.

## Common mistakes

- Running deployment or publishing from every PR.
- Giving forked code access to secrets.
- Validating only the feature branch, not the proposed merge.
- Skipping main-branch CI because the PR was green.
- Using broad path exclusions that miss shared build files.
- Creating so many mandatory checks that feedback becomes unusable.

## Interview preparation

**Why repeat tests after merge?**  
The merged commit is a new integration state. Main-branch CI verifies the exact source used to create the release candidate.

**How do you make PR validation both fast and safe?**  
Front-load cheap tests, cache safely, parallelize independent work, select tests using proven dependency information, cancel superseded runs, and keep privileged operations out.

**Where should Azure Repos PR validation be configured?**  
As a build-validation branch policy. YAML trigger syntax alone should not be assumed to enforce the protected-branch policy.

## Practical exercise

Design a PR and main-branch workflow for one service. Classify every step as both required/optional and trusted/untrusted. Measure time-to-first-failure. Then write a policy for forked PRs, secrets, package publishing, and agent selection.

## Official references

- [Configure repository triggers](https://learn.microsoft.com/azure/devops/pipelines/repos/azure-repos-git)
- [Branch policies and build validation](https://learn.microsoft.com/azure/devops/repos/git/branch-policies)
- [Secure access to Azure Repos](https://learn.microsoft.com/azure/devops/pipelines/security/secure-access-to-repos)

[Next: Test Pyramids and Quality Gates →](04-test-pyramids-and-quality-gates.md)
