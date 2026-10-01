# Container Builds, Provenance, and Retention

[← Build Numbering](07-build-numbering-and-semantic-versioning.md) · [Chapter 6](README.md)

## A container is a release artifact

A container image should be built once, tested, scanned, identified by digest, and promoted. Rebuilding the Dockerfile for each environment repeats the same provenance error as recompiling a binary.

## Secure build design

Use multi-stage builds to keep compilers and temporary files out of the runtime image. Choose a minimal trusted base image, pin it according to your update policy, run as a non-root user where possible, use `.dockerignore`, and never copy credentials into a layer. Build secrets require a mechanism that does not preserve them in history or cache.

```yaml
steps:
- task: Docker@2
  inputs:
    command: buildAndPush
    repository: 'contoso/orders'
    dockerfile: 'src/orders/Dockerfile'
    containerRegistry: 'approved-registry'
    tags: |
      $(Build.BuildId)
      $(Build.SourceVersion)
```

Tagging with run and commit aids traceability, but capture the registry-reported digest after push. Prefer deploying the digest or an immutable, governed reference.

## Provenance record

For every releasable image, retain:

- Source repository and commit SHA.
- Pipeline definition and template revision.
- Run ID and run number.
- Builder/agent image and key tool versions.
- Dependency lock files and base-image digest.
- Image digest and registry/repository.
- Test, scan, and policy results.
- Software bill of materials when required.
- Approval and environment-deployment history.

An SBOM inventories components; it does not by itself prove the build was trustworthy. Signing and attestations can strengthen verification, but only when identities, keys, policies, and verification at deployment are managed correctly.

## Base-image policy

Pinning a digest makes a build stable but also freezes vulnerabilities. Establish an automated process to detect a newer approved base, rebuild from source, rerun tests and scans, and release a new image. Reproducibility and patching are complementary: one explains what was built; the other deliberately creates and validates a newer artifact.

## Retention

Retention is part of recoverability and audit design. Keep enough data to reconstruct what was released:

- Release artifact or registry image.
- Source and pipeline/template revision.
- Test and security evidence.
- Deployment and approval record.
- Checksums, digests, and version mapping.

Ordinary PR artifacts may expire quickly; production releases may require months or years based on rollback, legal, or compliance needs. Align pipeline-run, artifact-feed, and container-registry policies—retaining a run is unhelpful if the referenced image has already been deleted.

## Incident questions

If a vulnerability is discovered, can you answer:

1. Which released images contain the component?
2. Which source commits and dependency graphs produced them?
3. Which environments run each digest?
4. Can the team rebuild with a patched base and repeat validation?
5. Can policy prevent redeployment of the vulnerable digest?

If not, provenance is incomplete.

## Interview preparation

**Why pin a base-image digest if security updates are important?**  
Pinning prevents an unreviewed input change. A separate update process deliberately selects a patched digest and creates a newly tested release candidate.

**Is an SBOM the same as provenance?**  
No. An SBOM lists components. Provenance describes how, where, and from which inputs an artifact was produced.

**What should retention protect?**  
The deployable artifact plus enough source, pipeline, dependency, test, security, and deployment evidence to support rollback and investigation.

## Practical exercise

Build a multi-stage image, push it with run and commit tags, retrieve its digest, and write a provenance manifest. Scan it, generate or inspect an SBOM, and trace one component back to its dependency declaration. Draft retention periods for PR, main, release-candidate, and production images.

## Official references

- [Build and push container images in Azure Pipelines](https://learn.microsoft.com/azure/devops/pipelines/ecosystems/containers/push-image)
- [Docker@2 task reference](https://learn.microsoft.com/azure/devops/pipelines/tasks/reference/docker-v2)
- [Azure Pipelines retention](https://learn.microsoft.com/azure/devops/pipelines/policies/retention)

## Chapter review

Explain how build-once/deploy-many, reproducibility, layered testing, version identity, provenance, and retention form one trust chain. Then continue to reusable pipelines, where these rules become organization-wide standards.

[Chapter 7 — Reusable Pipelines and YAML Templates →](../chapter-07-reusable-pipelines-and-yaml-templates/README.md)
