# Retention, Cleanup, and Supply-Chain Risk

[← Package Promotion](06-package-promotion-and-release-maturity.md) · [Chapter 8](README.md)

## Retention is a control

Unlimited storage is expensive; aggressive deletion destroys rollback and incident evidence. Classify content by purpose and maturity.

| Content | Typical policy question |
|---|---|
| PR artifact/package | How long is review and diagnosis useful? |
| Main CI candidate | How long may it be promoted or investigated? |
| Released package | What rollback, audit, and support window applies? |
| Cached dependency | Can it be safely recreated and revalidated? |
| Compromised package | What evidence must be preserved despite blocking use? |

Azure Artifacts retention can clean older unpromoted versions according to configured rules. Promoted packages are treated differently by retention features. Deleted packages enter a recycle bin for a limited period—Microsoft documentation describes 30 days—after which deletion is permanent. Validate current behavior before relying on recovery.

Package versions remain reserved after deletion.

## Coordinate lifecycles

A release record can reference a pipeline run, package, container image, test attachment, and source commit. If any one disappears prematurely, the chain weakens. Align:

- Pipeline run/artifact retention.
- Feed retention.
- Container-registry retention.
- Source and template/tag protection.
- Security scan and SBOM retention.
- Deployment and approval history.
- Backup/export requirements where applicable.

Legal and regulatory obligations may override ordinary engineering defaults.

## Supply-chain threat model

Consider compromised publishers, leaked tokens, malicious or abandoned upstream packages, dependency confusion, tampered build agents, mutable external downloads, vulnerable transitive dependencies, and overprivileged service identities.

Controls include least-privilege roles, protected CI publishing, approved upstreams, internal namespaces, lock and integrity verification, short-lived credentials, agent isolation, scanning, provenance, immutable versions, and monitored promotion.

## Incident response

When a package is suspected:

1. Preserve logs, package metadata, hashes, producer identity, and run evidence.
2. Stop new promotion and consumption using the safest available control.
3. Identify versions and affected consumers.
4. Investigate publisher credentials, upstream origin, and build path.
5. Publish a corrected new version; never replace the old bytes.
6. Update and redeploy consumers.
7. Rotate exposed credentials and close the original control gap.
8. Document timeline and lessons.

Deletion alone is insufficient: consumers may have local caches, vendored copies, or deployed binaries.

## Cleanup safety

Test retention in a sandbox, exempt required release evidence, review dry-run/inventory data when available, and assign an owner. Monitor storage trends and failed cleanup. Never run broad deletion using an unresolved feed, project, or package variable.

## Interview preparation

**Why retain a known-bad package?**  
You may block it from consumption while preserving restricted evidence for impact analysis and audit. Policy decides whether bytes, metadata, or both are retained.

**What happens after package deletion?**  
It is recoverable from the recycle bin only for the documented period, then permanently removed; its version remains unavailable for reuse.

**How do you find affected consumers?**  
Search manifests and lock files, restore/download logs, software inventories/SBOMs, build provenance, and deployed artifact records.

## Practical exercise

Inventory a sandbox feed by package maturity and propose retention classes. Delete and restore a disposable package from the recycle bin. Write a tabletop incident for a malicious upstream version, including detection, containment, affected-consumer discovery, remediation, and evidence retention.

## Official references

- [Delete and recover packages](https://learn.microsoft.com/azure/devops/artifacts/how-to/delete-and-recover-packages)
- [Feed settings and retention](https://learn.microsoft.com/azure/devops/artifacts/feeds/feed-settings)
- [Upstream-source concepts](https://learn.microsoft.com/azure/devops/artifacts/concepts/upstream-sources)

## Part III review

You should now be able to trace this complete chain:

```text
reviewed source + locked dependencies + controlled templates
                         ↓
                 reproducible CI run
                         ↓
       tests + analysis + immutable version
                         ↓
       pipeline artifact / package / image digest
                         ↓
          promotion + retention + provenance
```

Return to the [Part III overview](../README.md), complete the capstone and self-assessment, then continue to the next part of the learning journey.
