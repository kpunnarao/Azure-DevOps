# Package Promotion and Release Maturity

[← Lock Files](05-dependency-pinning-and-lock-files.md) · [Chapter 8](README.md) · [Next: Retention and Supply Chain →](07-retention-cleanup-and-supply-chain-risk.md)

## Promote metadata, not bytes

Every package published directly or saved from an upstream appears in the `@local` view. Azure Artifacts also provides `@prerelease` and `@release` views by default. A package can be promoted to a view as it gains confidence.

Promotion does not rebuild, copy, or alter the package. The same immutable name/version becomes visible through a maturity view.

```text
publish/save → @local → @prerelease → @release
                  same package name, version, and content
```

This is build-once/deploy-many applied to dependencies.

## Define maturity policy

Views do not create quality by themselves. Define evidence for each transition.

Example:

- **@local:** freshly published, basic CI passed.
- **@prerelease:** integration/contract tests passed; candidate available to early consumers.
- **@release:** approved for production consumption; security, compatibility, and release evidence complete.

Assign who may promote and preserve the audit record. Do not make every publisher a feed owner.

## Consumption strategy

Developers and pipelines can connect to a particular view endpoint so normal consumers see only the appropriate maturity. Release pipelines should avoid consuming experimental `@local` packages unless explicitly testing them. Configure view permissions deliberately; view visibility and feed roles together determine access.

A package can remain in multiple views. Promotion is normally forward-moving; if a package is later found unsafe, remove/deprecate its visibility according to service capabilities and policy, notify consumers, and publish a corrected version. Never assume removing a view erases packages already restored.

## Common anti-patterns

- Rebuilding the library when moving to release.
- Copying to separate feeds and losing provenance without a clear need.
- Treating `@release` as an automated vulnerability guarantee.
- Letting any contributor promote.
- Resolving production dependencies from `@local`.
- Using views as a substitute for versioning.

Separate feeds can still be justified for hard isolation, residency, ownership, or external sharing. If copying is required, verify content hashes and preserve lineage.

## Interview preparation

**What changes during promotion?**  
View membership and maturity metadata; the immutable package bytes and version do not change.

**Why use views instead of republishing?**  
Consumers receive exactly the tested package, provenance remains intact, and maturity can be expressed without a new coordinate.

**Who should promote?**  
A narrowly authorized release process or accountable owner after defined evidence, separate from ordinary package consumers.

## Practical exercise

Publish a disposable prerelease package to `@local`, configure a test consumer, promote to `@prerelease`, run integration checks, then promote the same version to `@release`. Confirm its content hash never changes and document each approval.

## Official references

- [Package views](https://learn.microsoft.com/azure/devops/artifacts/feeds/views)
- [Promote a package](https://learn.microsoft.com/azure/devops/artifacts/feeds/views#promote-a-package)

[Next: Retention, Cleanup, and Supply-Chain Risk →](07-retention-cleanup-and-supply-chain-risk.md)
