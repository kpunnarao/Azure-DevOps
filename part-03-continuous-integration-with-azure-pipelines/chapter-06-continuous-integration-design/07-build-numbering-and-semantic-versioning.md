# Build Numbering and Semantic Versioning

[← Coverage and Analysis](06-code-coverage-and-static-analysis.md) · [Chapter 6](README.md) · [Next: Containers and Provenance →](08-container-builds-provenance-and-retention.md)

## Different identities answer different questions

A pipeline run needs an identity for operations; a package needs a version for consumers; source needs a commit identity. Do not force one value to serve every purpose.

- **`Build.BuildId`** is a unique, immutable Azure Pipelines run identifier.
- **`Build.BuildNumber`** is a customizable human-facing run name and must remain unique enough for your workflow.
- **Commit SHA** identifies source.
- **Semantic version** communicates package compatibility and precedence.
- **Artifact hash or image digest** identifies content.

Record the mapping among them.

## Run-number design

A useful run number is sortable, recognizable, and collision-resistant:

```yaml
name: '$(Date:yyyyMMdd).$(Rev:r)'

steps:
- script: |
    echo "Run ID: $(Build.BuildId)"
    echo "Run number: $(Build.BuildNumber)"
    echo "Commit: $(Build.SourceVersion)"
```

The `Rev` counter increments to make otherwise identical names unique and resets when another part of the name changes. Do not depend on a mutable display name as the only durable foreign key; retain the run ID.

## Semantic Versioning

Semantic Versioning uses `MAJOR.MINOR.PATCH`:

- Increment **MAJOR** for incompatible public API changes.
- Increment **MINOR** for backward-compatible functionality.
- Increment **PATCH** for backward-compatible fixes.
- Add a prerelease suffix such as `-beta.3` for lower-precedence candidates.
- Build metadata such as `+build.123` does not change precedence.

Not every artifact exposes a public API, but a consistent version policy still helps. Define who decides versions, whether releases come from tags or a version file, and how concurrent builds avoid collisions.

## Practical version strategy

One workable model is:

- PR builds: no official package, or `1.5.0-pr.482.3`.
- Main CI: `1.5.0-ci.20261001.27`.
- Release candidate: `1.5.0-rc.2`.
- Release: `1.5.0`.

Published package versions are normally immutable. Never make retry logic overwrite an existing version. If a run published a bad `1.5.0`, correct the problem and publish a new version according to repository and package policy.

For containers, human-readable version tags aid discovery, but deploy using or record the immutable digest. A branch tag, `latest`, or even a release tag can be moved unless registry governance prevents it.

## Common mistakes

- Generating the same package version in two parallel runs.
- Deriving release identity from a local clock alone.
- Reusing a version after deleting a package.
- Confusing SemVer build metadata with precedence.
- Letting a rerun silently replace released content.
- Losing the mapping between package, commit, and run.

## Interview preparation

**Build number versus package version?**  
The build number labels a pipeline run; the package version is part of a consumer-facing dependency contract. They may be related but are not interchangeable.

**How do you guarantee uniqueness?**  
Include an atomic pipeline counter or immutable run ID and enforce package-feed immutability. Coordinate official release versions through tags or a controlled release process.

**Why record an image digest if it has a version tag?**  
The digest is content-addressed. Tags are convenient references that may be mutable.

## Practical exercise

Define numbering for PR, main, release-candidate, and release runs. Launch two builds close together and verify no collision. Publish two prerelease versions and compare precedence. Create a manifest mapping version, run ID, run number, commit, and checksum.

## Official references

- [Configure run and build numbers](https://learn.microsoft.com/azure/devops/pipelines/process/run-number)
- [Semantic Versioning 2.0.0](https://semver.org/)
- [Package immutability in Azure Artifacts](https://learn.microsoft.com/azure/devops/artifacts/artifacts-key-concepts)

[Next: Container Builds, Provenance, and Retention →](08-container-builds-provenance-and-retention.md)
