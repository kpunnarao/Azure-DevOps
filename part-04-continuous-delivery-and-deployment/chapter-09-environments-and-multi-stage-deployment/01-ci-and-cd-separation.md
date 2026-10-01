# CI and CD Separation

[← Chapter 9](README.md) · [Next: Environment Strategy →](02-environment-strategy.md)

## Purpose

Continuous integration creates and validates an immutable candidate. Continuous delivery moves that candidate through progressively stronger validation and keeps production release-ready. Continuous deployment automatically releases qualified candidates to production. Teams may practice continuous delivery without fully automatic production deployment.

## Separate responsibilities, preserve lineage

```text
CI: source → restore → build → test → scan → immutable artifact
CD: exact artifact → configure → deploy → validate → expose → observe
```

CI normally needs source and package-read access. CD needs artifact-read access plus environment deployment authority. Combining all privileges into one always-on job increases blast radius.

Separation can use stages in one YAML pipeline, separate YAML pipelines connected by a pipeline resource, or distinct systems. The design is valid when artifact identity and evidence survive the boundary.

## Why not rebuild in CD?

A rebuild can change dependencies, base images, compiler behavior, timestamps, or scripts. Production would then receive bytes that the CI tests never approved. CD should download a specific successful run's artifact, verify its identity, and apply environment-specific configuration externally.

A release record should connect:

- Source commit and CI run.
- Artifact name, version, checksum, or image digest.
- CD run and template version.
- Target environment/resource.
- Effective deployment identity.
- Checks, approvers, timestamps, and health evidence.

## Trigger strategy

Automatic CD can begin when an authoritative pipeline publishes a successful candidate. Filter the producing pipeline's branch and stages deliberately; do not consume an unspecified “latest” build for production. Manual selection may be appropriate for regulated or infrequent releases, but the selected version must remain explicit.

A production approval is not a substitute for automated validation. An approver should review summarized risk and evidence, not manually repeat the pipeline.

## Security boundaries

- PR validation must not gain production credentials.
- Only protected branch artifacts should qualify for release.
- A deployment pipeline should receive only the environment authority it needs.
- Production secrets should be retrieved as late as possible.
- Artifact and template provenance should be checked before privileged execution.
- Deployment scripts are production code and require review.

## Common mistakes

- Rebuilding separately for Test and Production.
- Letting CD silently select the latest artifact.
- Giving a single service identity owner access everywhere.
- Running production deployment from untrusted PR YAML.
- Calling a scheduled manual process “continuous delivery.”
- Losing the link between CI evidence and the deployed artifact.

## Interview preparation

**CI/CD versus continuous deployment?**  
CI integrates and validates changes. Continuous delivery keeps qualified changes releasable through automated deployment/validation. Continuous deployment automatically releases every qualified change to production.

**One pipeline or separate pipelines?**  
Either can work. Separate pipelines strengthen ownership and privilege boundaries; one multi-stage pipeline simplifies lineage. Choose based on trust, regulatory, and operational needs.

**What crosses the boundary?**  
An immutable artifact plus provenance and validation evidence—not a new source build.

## Practical exercise

Take a Part III artifact and create a deployment pipeline that accepts only a specific producing run. Compare its checksum in Development and Test. Draw the identities and permissions used by CI and CD and remove any authority not required.

## Official references

- [What is continuous delivery?](https://learn.microsoft.com/devops/deliver/what-is-continuous-delivery)
- [Pipeline resource triggers](https://learn.microsoft.com/azure/devops/pipelines/process/pipeline-triggers)

[Next: Environment Strategy →](02-environment-strategy.md)
