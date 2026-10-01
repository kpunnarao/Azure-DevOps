# Dependency Pinning and Lock Files

[← Package Immutability](04-package-immutability-and-versioning.md) · [Chapter 8](README.md) · [Next: Package Promotion →](06-package-promotion-and-release-maturity.md)

## Control the resolved graph

A manifest expresses dependency intent; a lock file records the exact direct and transitive versions selected by the resolver, often with integrity data. Commit application lock files when the ecosystem supports them and make CI fail if restore would change the lock unexpectedly.

Examples include `package-lock.json`, `npm-shrinkwrap.json`, `packages.lock.json`, `poetry.lock`, `Pipfile.lock`, and Maven/Gradle dependency-locking mechanisms. Follow the relevant ecosystem's guidance, especially for published libraries whose consumers may need controlled version ranges.

## CI restore modes

Use the tool's clean, locked, or frozen restore mode:

- npm: `npm ci`
- .NET/NuGet lock file: `dotnet restore --locked-mode`
- Python tools: the resolver's lock-sync/frozen behavior
- Gradle: dependency locking with verification as appropriate

The exact command is less important than the invariant: CI must not silently choose a new graph without a reviewed lock-file change.

## Pin more than packages

Also control:

- Build SDK/runtime.
- Package manager.
- Container base image.
- Actions, scripts, or binary tools downloaded during CI.
- Git submodules.
- Infrastructure modules/providers.
- Central YAML-template version.

Verify checksums or signatures where the ecosystem supports them. An exact version from an untrusted source is precisely pinned malicious code, so source trust and pinning work together.

## Update workflow

Automated dependency update tools can open focused pull requests. Each update should show version and lock changes, release/security context, test evidence, and owner. Grouping many unrelated updates reduces review clarity; never auto-merge solely because a scanner says “safe.”

Emergency vulnerability updates may move faster but must still create a new reviewed graph and artifact. Do not edit a deployed package in place.

## Caches and locks

Key dependency caches from the lock file and platform. The cache may contain package content, but the resolver must validate that it satisfies the locked graph and integrity rules. Delete the cache and prove the restore remains correct. Never commit the cache directory.

## Interview preparation

**Manifest versus lock file?**  
The manifest states allowed dependencies; the lock records the exact resolved graph used for a repeatable application build.

**Should libraries commit lock files?**  
It depends on the ecosystem and whether the lock governs library development/tests or consumer resolution. Document the intent; do not force consumers to an inappropriate internal graph.

**Does pinning solve supply-chain risk?**  
It limits unexpected change and improves review/reproduction, but source trust, integrity verification, scanning, provenance, and update policy are still required.

## Practical exercise

Generate a lock file, commit it, and enable locked CI restore. Change only the manifest and observe CI failure. Update the lock intentionally, inspect transitive changes, then restore with an empty cache and compare the resolved graph.

## Official references

- [Secure access to packages](https://learn.microsoft.com/azure/devops/artifacts/secure-your-feeds)
- [Pipeline caching](https://learn.microsoft.com/azure/devops/pipelines/release/caching)

[Next: Package Promotion and Release Maturity →](06-package-promotion-and-release-maturity.md)
