# Triggers and Pull Request Validation

> Chapter 5 — YAML Pipeline Fundamentals

[← Previous](01-yaml-structure-and-schema.md) · [Chapter home](README.md) · [Next →](03-stages-jobs-steps-tasks-and-scripts.md)

## Purpose

Triggers decide which event and source version create a run. A badly scoped trigger wastes capacity, misses required validation, creates loops, or validates a different revision than expected.

## Trigger categories

| Trigger | Typical purpose |
|---|---|
| Continuous integration | Run when selected branches receive pushes |
| Pull request | Validate proposed integration |
| Scheduled | Nightly, periodic, maintenance, or exhaustive validation |
| Pipeline completion/resource | Run when a producing pipeline qualifies |
| Manual | Investigation, controlled operation, or parameterized execution |

Behavior differs by repository type. For Azure Repos Git, PR validation is commonly configured as a build-validation branch policy. Do not assume a YAML pr block behaves identically across Azure Repos and GitHub.

## CI filters

Define explicit branches and paths. Include/exclude logic should be tested with representative changes.

```yaml
trigger:
  branches:
    include:
    - main
    - release/*
  paths:
    exclude:
    - docs/**
```

Path exclusions can save capacity but may miss shared configuration or documentation that affects build/release. Prefer correctness over optimization.

## Pull-request validation

A useful PR run validates the proposed merge against the protected target, not merely the isolated source tip. Configure branch policy to require the appropriate pipeline.

PR validation should be:

- Fast enough to support small changes
- Deterministic
- Isolated from privileged secrets
- Clear when it fails
- Re-run when source or target invalidates evidence
- Limited to checks needed before integration

Contributor-controlled code can execute during validation. Treat forks and untrusted branches accordingly.

## Scheduled validation

Use schedules for long-running, cross-platform, integration, security, or dependency checks that do not need to block every PR. Remember that schedules defined in the UI can take precedence over YAML schedules. Document the authoritative configuration.

## Pipeline-resource triggers

A downstream pipeline can consume a qualified upstream run through a pipeline resource. Filter branches, stages, and tags carefully. Ensure the consumed artifact and source version are explicit.

Avoid legacy completion triggers when pipeline resources provide clearer source and artifact semantics.

## Trigger loops

A pipeline that commits generated content or updates branches can retrigger itself. Prevent loops through path filters, separate repositories, explicit commit policies, or avoiding source mutation from CI.

## Diagnosis

When no run appears:

1. Confirm the YAML version and pipeline definition.
2. Check repository type and supported trigger configuration.
3. Inspect branch and path filters.
4. Check UI override settings.
5. Verify branch policy for Azure Repos PR validation.
6. Confirm permissions and service hooks/resources.
7. Check whether batching or skip directives apply.
8. Review schedule timezone and always/changed behavior.

When the wrong source builds, inspect Build.SourceBranch, Build.SourceVersion, PR variables, and the policy-provided merge context.

## Common mistakes

- Enabling implicit CI unintentionally
- Expecting Azure Repos PR validation from GitHub-style YAML
- Excluding shared paths that affect all services
- Running deployment secrets in PR validation
- Scheduled UI trigger silently overriding YAML
- Triggering downstream deployment from any upstream branch
- Self-triggering commits
- Using a large full pipeline for every documentation change

## Interview preparation

**Q: How is Azure Repos PR validation normally enforced?**  
Through a build-validation policy on the target branch, pointing to the required pipeline.

**Q: CI trigger versus pipeline-completion trigger?**  
CI responds to source changes. Pipeline completion responds to a qualifying run of another pipeline and can consume its artifacts.

**Q: Why are path filters risky?**  
A shared file can affect components outside its path. Incorrect exclusions silently skip required validation.

**Q: Why isolate PR validation from secrets?**  
The PR controls executed code; exposed secrets or trusted network access can be exfiltrated or misused.

## Further reading

- [Build triggers in Azure Pipelines](https://learn.microsoft.com/en-us/azure/devops/pipelines/build/triggers)
- [Scheduled triggers](https://learn.microsoft.com/azure/devops/pipelines/process/scheduled-triggers)
- [Pipeline completion triggers](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/pipeline-triggers)
- [Azure Repos branch policies](https://learn.microsoft.com/azure/devops/repos/git/branch-policies)
