# Azure Artifacts Feeds and Package Types

[← Pipeline Artifacts](01-pipeline-artifacts-and-build-artifacts.md) · [Chapter 8](README.md) · [Next: Scope and Permissions →](03-feed-scope-permissions-and-upstream-sources.md)

## What a feed provides

An Azure Artifacts feed is an organizational package repository. It stores immutable package versions, controls producer and consumer access, exposes package-manager endpoints, supports upstream sources, and provides views for release maturity.

Azure Artifacts supports common ecosystems including NuGet, npm, Maven, Python, and Universal Packages. Verify current service support and client requirements for your organization.

## Choose a package type

Use the ecosystem-native type when the content is a dependency for that ecosystem. Native metadata and client behavior provide dependency resolution, integrity, and standard developer experience.

Use Universal Packages for collections of files that do not naturally belong to another supported ecosystem—for example, a versioned model bundle, tool distribution, or compiled assets. They are not a substitute for container registries or pipeline artifacts.

## Feed design

Create feeds around a clear ownership and trust boundary, not automatically one per repository or one global feed for everything. Ask:

- Who publishes?
- Who consumes?
- Does the package cross project boundaries?
- Are versions internal or public-facing?
- Which upstream sources are approved?
- Who promotes versions?
- What retention and recovery are required?
- Could a package name collide with a public package?

A smaller number of well-owned feeds can simplify discovery and policy. Overly broad feeds increase permission and dependency-confusion risk; too many feeds create authentication and operational sprawl.

## Naming and metadata

Package names and versions become contracts. Use consistent, ecosystem-appropriate names and meaningful descriptions. Include repository, license, ownership, and source information where supported. Make the CI producer able to map a package version back to a run and commit.

Publishing should occur from an authoritative protected branch or release workflow, not an arbitrary developer workstation for production packages.

## Authentication

Developers and pipelines authenticate differently. Pipelines normally use their build-service identity and feed permissions; local clients use supported credential providers or authenticated configuration. Never commit personal access tokens into package-manager configuration. Put feed endpoints in configuration, and acquire credentials through the supported runtime mechanism.

## Common mistakes

- Storing a deployable container as a Universal Package.
- Publishing official packages from PR validation.
- Creating a global feed with every build service as owner.
- Reusing names that can resolve unexpectedly from public registries.
- Treating a package feed as backup storage.
- Publishing without repository and run traceability.

## Interview preparation

**When would you use Universal Packages?**  
For versioned file collections that do not fit a supported language package ecosystem and are not better represented as a container or transient run artifact.

**One feed per project?**  
Not automatically. Scope feeds by ownership, sharing, trust, permission, retention, and naming requirements.

**Who should publish?**  
A controlled CI identity on an authoritative workflow with the minimum publisher role; humans should not be the ordinary production publisher.

## Practical exercise

Classify five outputs—a NuGet library, npm UI component, Docker image, deployment ZIP, and large model files—by storage mechanism. Create a sandbox feed, publish one prerelease package from CI, and confirm a read-only consumer can restore but cannot publish.

## Official references

- [Azure Artifacts key concepts](https://learn.microsoft.com/azure/devops/artifacts/artifacts-key-concepts)
- [Azure Artifacts feeds](https://learn.microsoft.com/azure/devops/artifacts/concepts/feeds)

[Next: Feed Scope, Permissions, and Upstream Sources →](03-feed-scope-permissions-and-upstream-sources.md)
