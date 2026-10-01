# Build Once, Deploy Many

[← Chapter 6](README.md) · [Next: Deterministic Builds →](02-deterministic-and-reproducible-builds.md)

## Purpose

**Build once, deploy many** means CI creates one immutable release candidate and every environment consumes that exact artifact. Development, test, staging, and production may use different configuration, but they must not receive separately compiled binaries.

## Why rebuilding is dangerous

A later rebuild can resolve a newer transitive dependency, use a changed base image, run on a different toolchain, or include an altered script. Even if the Git commit is unchanged, the output may differ. Tests then certify one binary while production receives another.

The desired chain is:

```text
commit → CI run → tests → immutable artifact
                              ↓
                       Dev → Test → Prod
```

Promotion changes the artifact's **release state**, not its contents.

## Design rules

1. Build from a known source revision.
2. Restore pinned dependencies and record tool versions.
3. Run tests before publishing the candidate.
4. Publish once with a unique identity.
5. Download the same artifact in every deployment stage or pipeline.
6. Keep environment-specific values outside the binary.
7. record the artifact name, version, hash or image digest, run ID, and commit SHA.

Configuration can come from environment variables, deployment manifests, a configuration service, or a secret store. Never place production secrets in the CI artifact.

A common Azure Pipelines pattern is:

```yaml
steps:
- script: ./build.sh
  displayName: Build and test
- task: PublishPipelineArtifact@1
  inputs:
    targetPath: '$(Build.ArtifactStagingDirectory)'
    artifact: 'application'
```

Deployment consumes the published artifact rather than invoking the compiler again.

## What “immutable” requires

A unique filename alone is insufficient. Prevent overwriting, avoid mutable container tags as the sole identifier, restrict publisher permissions, and retain released outputs. For containers, deploy the image digest when possible; a tag such as `latest` can point somewhere else later.

## Failure patterns

- Building separately inside every environment.
- Injecting configuration by modifying packaged files after approval.
- Publishing a mutable version repeatedly.
- Copying an artifact manually without its metadata.
- Testing a debug build and releasing an independently produced optimized build.

If transformation is unavoidable, treat its output as a new artifact and run the required validation again.

## Operational checklist

- Can production identify the exact CI run?
- Can that run identify the commit and dependency inputs?
- Are artifact hashes or image digests recorded?
- Can an old release be retrieved for rollback?
- Are configuration and secrets supplied at deployment time?
- Is promotion auditable and permission-controlled?

## Interview preparation

**Why not rebuild the same commit for production?**  
Source equality does not guarantee binary equality. Dependencies, tools, base images, time, and build infrastructure can change. Promotion preserves the tested artifact.

**How do you handle environment differences?**  
Keep deploy-time configuration outside the compiled artifact and bind it through approved configuration and secret mechanisms.

**Is copying a package to another feed still build once?**  
Yes, if the bytes and identity remain verifiably unchanged. Prefer promoting metadata or views when the package system supports it.

## Practical exercise

Produce an artifact, calculate its checksum, and deploy it to two local directories using different configuration files. Verify the checksum is identical in both. Then rebuild from the same commit after changing one dependency and observe why the rebuilt output cannot inherit the original approval.

## Official references

- [Publish and download pipeline artifacts](https://learn.microsoft.com/azure/devops/pipelines/artifacts/pipeline-artifacts)
- [Azure Pipelines artifacts overview](https://learn.microsoft.com/azure/devops/pipelines/artifacts/artifacts-overview)

[Next: Deterministic and Reproducible Builds →](02-deterministic-and-reproducible-builds.md)
