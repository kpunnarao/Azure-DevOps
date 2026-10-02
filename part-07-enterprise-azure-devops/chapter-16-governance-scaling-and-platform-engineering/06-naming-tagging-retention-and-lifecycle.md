# Naming, Tagging, Retention, and Lifecycle

[← Capacity](05-parallelism-capacity-and-performance.md) · [Chapter 16](README.md) · [Next: Extension Governance →](07-extension-governance.md)

## Metadata is operational design

Names help humans find resources; immutable IDs preserve identity; tags/metadata express ownership, environment, cost, classification, lifecycle, and support. Avoid encoding every attribute into a brittle name.

A naming standard should define resource type, product/service, environment, region when relevant, sequence/uniqueness, character limits, examples, collision handling, and rename/migration consequences.

## Minimum metadata

For projects, repos, pipelines, pools, feeds, service connections, environments, Azure resources, and registries capture:

- Accountable owner/team and service catalog ID.
- Purpose/workload.
- Environment and data/security classification.
- Cost center.
- Criticality/SLO/tier.
- Lifecycle state and review/expiry date.
- Support/runbook/contact.
- Source/template/module/version where applicable.

Validate metadata automatically and maintain a reconciliation inventory.

## Retention classes

Define classes for PR runs, main CI, release candidates, production releases, packages/images, test evidence, audit/security findings, telemetry, work items, and backups. Base periods on rollback, incident discovery, legal/compliance, support, cost, and privacy.

Coordinate lifecycles: retaining a deployment record without artifact, source, scan, and config is weak. Protect released content from automatic cleanup and test recovery.

## Archive and deletion

Archive inactive projects/repos/pipelines through read-only/disabled state where possible before deletion. Confirm ownership, activity over a full business cycle, dependencies/service hooks, legal hold, export/backup, package consumers, credentials, and restore test.

Deletion must use exact resolved targets, independent approval, dry-run/inventory, and post-validation. Avoid wildcard/environment-variable bulk deletion.

## Naming changes

Renames can break URLs, scripts, service hooks, policies, badges, package endpoints, dashboards, and external integrations even if internal redirects exist. Inventory consumers and provide a migration window.

## Interview preparation

**Why not put owner in name?**  
Ownership changes. Use governed metadata/service catalog; names should remain stable.

**How determine retention?**  
Classify by business/legal/privacy/rollback/incident value, coordinate related evidence, and balance cost with tested recovery.

**Archive versus delete?**  
Archive reduces active clutter/access while preserving recoverability; delete only after dependency/evidence and restoration requirements are satisfied.

## Practical exercise

Create a catalog for ten Azure DevOps resource types, validate names/tags, classify retention, and run a tabletop project decommission including export and restore evidence.

## Official references

- [Azure Pipelines retention](https://learn.microsoft.com/azure/devops/pipelines/policies/retention)
- [Delete and recover Azure DevOps projects](https://learn.microsoft.com/azure/devops/organizations/projects/delete-project)
- [Azure resource naming/tagging guidance](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming)

[Next: Extension Governance →](07-extension-governance.md)
