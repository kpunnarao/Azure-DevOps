# Pipeline Artifacts and Build Artifacts

[← Chapter 8](README.md) · [Next: Azure Artifacts Feeds →](02-azure-artifacts-feeds-and-package-types.md)

## Three different needs

Do not use “artifact” as if it meant one storage system.

- **Pipeline artifacts** move files produced by an Azure Pipelines run between jobs, stages, or later pipelines.
- **Build artifacts** are the older pipeline file-publication mechanism and remain relevant for some Azure DevOps Server or legacy scenarios.
- **Package feeds** distribute versioned dependencies such as NuGet, npm, Maven, Python, or Universal Packages to multiple consumers.

Pipeline artifacts are generally the preferred Azure DevOps Services mechanism for pipeline outputs. They are not supported in the same way on Azure DevOps Server; check the task documentation for your platform.

## Publish and consume

```yaml
steps:
- task: PublishPipelineArtifact@1
  inputs:
    targetPath: '$(Build.ArtifactStagingDirectory)'
    artifact: application
```

A downstream job can use `DownloadPipelineArtifact@2` or the `download` shortcut. Explicitly name artifacts and download paths. In multi-stage pipelines, understand which artifacts download automatically and avoid assuming the working directory contains a prior job's files—jobs may run on different agents.

Use a `.artifactignore` file to exclude unnecessary content. Never publish secret files, credential caches, local configuration, or signing material.

## Selection guide

| Requirement | Prefer |
|---|---|
| Pass compiled app from build to deploy | Pipeline artifact |
| Store test diagnostics for a run | Pipeline artifact or dedicated test publication |
| Share reusable library with version resolution | Azure Artifacts feed |
| Support a legacy Server pipeline | Build artifact where required |
| Optimize dependency restore | Pipeline cache, not artifact |
| Deploy a container | Container registry and immutable digest |

Artifact content is immutable for practical release flow only when naming, permissions, and consumption prevent replacement or ambiguity. Record checksums and the producing run.

## Cross-pipeline use

A pipeline can consume an artifact from another run. Select the source pipeline, branch, version, and triggering behavior deliberately. “Latest” is convenient but can race with unrelated runs. A release should resolve a specific run or pipeline resource version and record it.

Artifact availability is tied to run and retention policies. If deleting a run deletes its artifacts, your rollback plan must retain the appropriate run or copy the release output to an approved long-lived repository without changing it.

## Common mistakes

- Using a feed package as a temporary job handoff.
- Using a run artifact as a shared library dependency.
- Rebuilding because a downstream agent lacks the previous workspace.
- Downloading an unspecified latest run for production.
- Publishing the entire working directory, including secrets and caches.
- Assuming pipeline-artifact tasks work identically on Azure DevOps Server.

## Interview preparation

**Pipeline artifact versus package?**  
A pipeline artifact is a run-scoped file output used in delivery flow; a package is a versioned dependency distributed through a feed with package-client semantics.

**Pipeline artifact versus cache?**  
Artifacts are intentional outputs. Caches are optional accelerators and correctness must not require a hit.

**How do you make a cross-pipeline download auditable?**  
Resolve a specific producing run, record its commit and artifact hash, authorize the consumer, and retain the source run.

## Practical exercise

Publish one directory as a pipeline artifact, download it in a job on another agent, and verify a checksum. Exclude a secret-like test file with `.artifactignore`. Then explain why the same files would or would not belong in a feed.

## Official references

- [Publish and download pipeline artifacts](https://learn.microsoft.com/azure/devops/pipelines/artifacts/pipeline-artifacts)
- [Artifacts in Azure Pipelines](https://learn.microsoft.com/azure/devops/pipelines/artifacts/artifacts-overview)

[Next: Azure Artifacts Feeds and Package Types →](02-azure-artifacts-feeds-and-package-types.md)
