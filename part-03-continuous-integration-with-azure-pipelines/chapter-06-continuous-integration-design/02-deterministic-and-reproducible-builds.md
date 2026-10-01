# Deterministic and Reproducible Builds

[← Build Once, Deploy Many](01-build-once-deploy-many.md) · [Chapter 6](README.md) · [Next: PR Validation →](03-pr-validation-and-main-branch-ci.md)

## Core idea

A repeatable build behaves predictably when given the same declared inputs. A **deterministic** build produces byte-for-byte identical output from identical inputs. A **reproducible** build can be recreated independently with the same result or with an explainable, verifiable equivalence. The terms are sometimes used loosely, but the design objective is clear: hidden inputs must not decide what ships.

## Sources of nondeterminism

| Source | Example | Control |
|---|---|---|
| Dependencies | Floating version resolves differently | Commit lock files and use locked restore |
| Toolchain | Agent image gains a new SDK | Pin SDK/compiler/container image |
| Source | Submodule or generated input moves | Pin revisions and checksum downloads |
| Environment | Locale, timezone, filesystem order | Set explicitly and sort inputs |
| Time/randomness | Timestamp embedded in archive | Normalize timestamps; seed randomness |
| Network | Remote script changes | Vendor, version, and verify external inputs |
| Concurrency | Race in generation step | Remove shared mutable state |

Microsoft-hosted agents provide clean isolation, but their images are maintained over time. If exact toolchain control matters, install an explicit version, use a pinned build container, or manage a self-hosted image deliberately.

## Declare the build contract

Treat these as inputs:

- Source commit and submodule revisions.
- Dependency manifests and lock files.
- Compiler, runtime, package manager, and build-image versions.
- Build scripts and template versions.
- Feature flags that change compilation.
- External assets and their checksums.

Do not treat an agent's previously restored folder as authoritative. A cache is an optimization; a clean restore must still produce the correct graph.

Example for a .NET repository:

```yaml
steps:
- task: UseDotNet@2
  inputs:
    packageType: sdk
    version: '8.0.404'
- script: dotnet restore --locked-mode
- script: dotnet build --no-restore --configuration Release
- script: dotnet test --no-build --configuration Release
```

The exact version is illustrative—choose and maintain a supported version for your application.

## Verification strategy

Run periodic clean-room rebuilds with caches disabled. Compare package checksums, container digests, or a normalized manifest of files. When byte equality is impossible because signing or packaging adds controlled metadata, document that variance and compare the underlying payload.

Capture a build manifest containing the run ID, commit, repository URL, template revision, dependency-lock hash, tool versions, and artifact hash. This makes incident investigation much faster.

## Common mistakes

- Pinning direct dependencies but ignoring transitive dependencies.
- Using `latest` for build images or tools.
- Downloading and executing an unverified script during CI.
- Allowing generated files to depend on local time or traversal order.
- Believing a self-hosted agent is reproducible merely because it is stable.
- Masking nondeterminism by retrying the build until it passes.

## Interview preparation

**Are clean agents sufficient for reproducibility?**  
No. They reduce residue but do not pin dependencies, toolchains, network inputs, locale, or timestamps.

**Should dependencies always be vendored?**  
Not necessarily. A locked dependency graph, trusted source, integrity verification, and retained packages can be sufficient. The risk determines the control.

**How would you diagnose two different outputs from one commit?**  
Compare build manifests, lock hashes, tool versions, environment variables, external downloads, timestamps, and generated-file ordering; then reproduce with caches disabled.

## Practical exercise

Build the same commit twice on clean workers and compare hashes. Introduce a floating dependency, observe the potential variation, then add a lock file and locked restore. Produce a small manifest beside the artifact.

## Official references

- [Microsoft-hosted and self-hosted agents](https://learn.microsoft.com/azure/devops/pipelines/agents/agents)
- [Cache dependencies with Cache@2](https://learn.microsoft.com/azure/devops/pipelines/release/caching)

[Next: PR Validation and Main-Branch CI →](03-pr-validation-and-main-branch-ci.md)
