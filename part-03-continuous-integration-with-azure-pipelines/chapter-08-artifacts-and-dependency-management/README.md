# Chapter 8 — Artifacts and Dependency Management

[← Part III — Continuous Integration with Azure Pipelines](../README.md)

CI consumes dependencies and produces outputs. This chapter explains how to move both safely: pipeline artifacts carry run outputs, Azure Artifacts feeds distribute versioned packages, lock files make resolution repeatable, and promotion plus retention preserve a trustworthy supply chain.

## Why this chapter matters

A successful build can still be unsafe if it restored an unexpected package, overwrote a released version, exposed a feed broadly, or deleted the evidence needed for rollback. Dependency management is therefore part of CI architecture and security—not a storage afterthought.

## What you will learn

- Choose between pipeline artifacts, legacy build artifacts, and package feeds.
- Design feeds around ownership, visibility, and supported package ecosystems.
- Apply feed scope, roles, pipeline identities, and upstream sources correctly.
- Use immutable package versions and collision-free release policies.
- Pin dependency graphs and validate lock files during CI.
- Promote a proven package through maturity views without rebuilding it.
- Coordinate retention, cleanup, recovery, and supply-chain response.

## Topics

| # | Topic | Outcome |
|---:|---|---|
| 1 | [Pipeline Artifacts and Build Artifacts](01-pipeline-artifacts-and-build-artifacts.md) | Transfer CI outputs deliberately |
| 2 | [Azure Artifacts Feeds and Package Types](02-azure-artifacts-feeds-and-package-types.md) | Select the right package distribution model |
| 3 | [Feed Scope, Permissions, and Upstream Sources](03-feed-scope-permissions-and-upstream-sources.md) | Apply least privilege and controlled resolution |
| 4 | [Package Immutability and Versioning](04-package-immutability-and-versioning.md) | Prevent replacement and version collisions |
| 5 | [Dependency Pinning and Lock Files](05-dependency-pinning-and-lock-files.md) | Make dependency restoration reviewable |
| 6 | [Package Promotion and Release Maturity](06-package-promotion-and-release-maturity.md) | Advance the same package through views |
| 7 | [Retention, Cleanup, and Supply-Chain Risk](07-retention-cleanup-and-supply-chain-risk.md) | Preserve evidence while controlling cost |

## Guided chapter lab

Create a project-scoped feed and a sample library/application pair. Publish a unique prerelease library version, consume it through an authenticated restore, commit the lock file, and run a locked CI restore. Promote the tested version from `@local` to a prerelease view and then to a release view without republishing.

Also publish an application pipeline artifact and prove that another job can download it. Record which mechanism is suitable for a deployable bundle versus a reusable dependency.

Finally, review feed roles and build-service identities, configure an upstream source in a sandbox, and document retention, recycle-bin recovery, and incident response for a compromised dependency.

> Use a sandbox organization and disposable package names. Package versions remain reserved even after deletion, so do not experiment with a version you expect to reuse.

## Completion criteria

You are ready to move on when you can choose the correct artifact mechanism, explain package immutability and views, authorize the correct pipeline identity, reproduce a locked dependency graph, and trace a released package to its producer and consumers.

## Chapter navigation

[← Chapter 7](../chapter-07-reusable-pipelines-and-yaml-templates/README.md) · [Part III overview](../README.md)
