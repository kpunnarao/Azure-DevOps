# Package Immutability and Versioning

[← Scope and Permissions](03-feed-scope-permissions-and-upstream-sources.md) · [Chapter 8](README.md) · [Next: Lock Files →](05-dependency-pinning-and-lock-files.md)

## A published version is permanent identity

In Azure Artifacts, a particular package name and version is immutable. Once published, that version is reserved—even if the package is later deleted. This prevents consumers from receiving different bytes under the same identity and is why experimental versions must be unique.

A failed publish retry should recognize “version already exists” as an identity collision, not respond by deleting and overwriting the package.

## Version strategy

Use the ecosystem's version semantics consistently. For SemVer-style packages:

- Stable: `2.4.1`
- Prerelease: `2.5.0-beta.3`
- CI candidate: `2.5.0-ci.20261001.1842`

Include an atomic run ID or counter where concurrent builds can publish. Avoid `latest` as a version. A package-manager range such as `^2.4.0` may intentionally float at restore time, but the lock file should record the exact resolved graph for the application build.

## Publication ownership

Official versions should be published by an authoritative CI/release identity after required tests. Restrict publisher permissions, protect version-source tags or files, and ensure two pipelines cannot claim the same release.

Capture:

- Package name/version.
- Content hash where available.
- Producing run ID and commit.
- Dependency-lock and toolchain identity.
- Test/security evidence.
- Promotion view and time.

## Handling defects

Never replace a defective released version. Deprecate or unlist it where ecosystem features allow, communicate impact, block it through policy if necessary, and release a corrected version. Deletion can reduce accidental use but does not erase exposure or free the version.

For a compromised version, identify every consumer through dependency manifests, lock files, restore logs, or inventory; issue a safe version; invalidate credentials if needed; and preserve evidence.

## Common mistakes

- Using a branch name as the complete version.
- Publishing the same version from retries or parallel builds.
- Assuming deletion allows reuse.
- Modifying a package locally after CI testing.
- Promoting by rebuilding and republishing.
- Giving developers routine publisher rights to production feeds.

## Interview preparation

**Why reserve a deleted version?**  
Allowing reuse would let the same coordinate identify different content, breaking reproducibility and enabling substitution attacks.

**How do you version every main-branch build?**  
Combine the intended base version/prerelease label with an Azure Pipelines run ID or atomic counter, then retain the mapping to commit and hash.

**What if a released package is wrong?**  
Do not overwrite it. Mark it deprecated or block it, publish a corrected version, notify consumers, and investigate affected deployments.

## Practical exercise

Publish two unique prerelease versions from CI. Attempt to republish the first and observe the failure. Delete a disposable version and confirm it cannot be reused. Write a policy for version source, collision handling, and emergency deprecation.

## Official references

- [Azure Artifacts key concepts and immutability](https://learn.microsoft.com/azure/devops/artifacts/artifacts-key-concepts)
- [Package versioning guidance](https://learn.microsoft.com/azure/devops/artifacts/concepts/package-versioning)

[Next: Dependency Pinning and Lock Files →](05-dependency-pinning-and-lock-files.md)
