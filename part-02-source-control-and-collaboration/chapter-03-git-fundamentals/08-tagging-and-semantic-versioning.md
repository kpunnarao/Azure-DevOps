# Tagging and Semantic Versioning

> Chapter 3 — Git Fundamentals

[← Previous](07-atomic-commits-and-ignore-rules.md) · [Chapter home](README.md)

## Purpose

Tags give durable names to important points in history. Version numbers communicate release identity and, when a project follows Semantic Versioning, compatibility intent. They should connect source, build, artifact, and deployment evidence.

## Tags

Create an annotated release tag:

```bash
git tag -a v1.4.0 -m "Release 1.4.0"
git show v1.4.0
git push origin v1.4.0
```

Pushing a branch does not necessarily push tags. Define an explicit release process.

Treat a published release tag as immutable. If a release is bad, publish a corrected version or record that the version was withdrawn. Moving v1.4.0 to different source makes builds and incident evidence ambiguous.

## Semantic Versioning

A semantic version has MAJOR.MINOR.PATCH:

- MAJOR: incompatible public API change
- MINOR: backward-compatible functionality
- PATCH: backward-compatible bug fix

Pre-release identifiers and build metadata can extend the form, such as 2.0.0-rc.1.

Semantic Versioning is meaningful only when the public compatibility contract is defined and the project follows it. It is not automatically suitable for every internal application, data pipeline, or continuously deployed service.

## Source version versus artifact version

A robust release records:

- Source commit
- Release tag, if used
- Pipeline run
- Dependency lock state
- Artifact or image digest
- Package/application version
- Configuration version
- Deployment environment and time

A tag identifies source history. It does not by itself prove which binary was deployed.

## Version generation

Common approaches:

- Manually approved version committed or tagged
- Pipeline-generated version from tag
- Version derived from a base version plus build metadata
- Date or monotonically increasing internal release number
- Git-describe-style development identifiers

Requirements:

- Unique
- Reproducible or traceable
- Valid for the target package ecosystem
- Not overwritten after publication
- Consistent across artifact metadata and deployment telemetry

## Release branches and tags

A release branch represents a maintained line of development; a tag identifies a specific point. Do not use tags when ongoing fixes must be committed to that reference. Do not create permanent release branches merely to record a release point.

For a supported release:

1. Apply the fix to the canonical development line where possible.
2. Port it to the supported release branch according to policy.
3. Validate and publish a new patch version.
4. Tag the exact released commit.
5. Record artifact and deployment evidence.

## Signed tags and commits

Cryptographic signing can strengthen provenance by allowing verification of signer-controlled keys. It does not prove that code is safe or reviewed. Establish key lifecycle, identity binding, verification policy, and incident response before relying on signatures.

## Common mistakes

- Moving a published tag
- Tagging a local commit but forgetting to push the tag
- Producing different binaries for the same version
- Using a source tag as the only deployment evidence
- Incrementing versions inconsistently across branches
- Applying SemVer without defining the public API
- Encoding environment names into package versions unnecessarily
- Allowing release tags from untrusted or unvalidated source

## Interview preparation

**Q: Branch versus tag?**  
A branch is expected to move as commits are added. A tag is normally a stable name for a specific object, commonly a release commit.

**Q: What does 3.2.1 communicate under SemVer?**  
Major version 3, backward-compatible feature level 2, and backward-compatible patch level 1—assuming the project correctly follows Semantic Versioning.

**Q: Is a Git tag sufficient release traceability?**  
No. Also retain build, artifact, dependency, configuration, deployment, and validation evidence.

**Q: Should a bad release tag be moved?**  
Normally no. Preserve history and publish a corrected new version or mark the prior release as withdrawn.

## Practical exercise

Create an annotated v1.0.0 tag, build a uniquely versioned artifact, record its checksum, simulate a patch fix, create v1.0.1, and build a table connecting tag, commit, build, artifact, and deployment.

## Further reading

- [git-tag](https://git-scm.com/docs/git-tag)
- [Semantic Versioning](https://semver.org/)
- [Azure Repos branching guidance](https://learn.microsoft.com/en-us/azure/devops/repos/git/git-branching-guidance)
