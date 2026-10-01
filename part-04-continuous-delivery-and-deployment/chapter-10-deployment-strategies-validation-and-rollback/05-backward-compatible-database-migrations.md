# Backward-compatible Database Migrations

[← Feature Flags](04-feature-flags.md) · [Chapter 10](README.md) · [Next: Health Checks →](06-health-checks-and-zero-downtime.md)

## State makes deployment difficult

During rolling, canary, or blue-green delivery, old and new application versions may access the same database. A schema change must support this overlap and recovery. The standard approach is **expand, migrate, contract**.

## Expand

Make additive, backward-compatible changes:

- Add nullable columns or columns with safe defaults.
- Add new tables, indexes, views, or APIs.
- Keep old columns and behavior.
- Deploy code that can tolerate both representations.

Avoid renaming/dropping columns, narrowing types, or introducing an immediately required field in the same release that first uses it.

## Migrate

Backfill existing data in bounded, observable batches. Throttle to protect production, checkpoint progress, make the operation restartable, and validate counts/invariants. For a transition period, the application may dual-read or dual-write; define conflict resolution and measure divergence.

Large index/constraint operations can lock or consume resources. Use the database platform's online/concurrent capabilities where supported and test realistic volume.

## Contract

After every supported application version uses the new representation and migration is verified:

1. Stop old reads/writes.
2. Observe for a safety window.
3. Remove compatibility code and feature flag.
4. Enforce new constraints.
5. Drop old schema in a later release.

Contract is a separate change, not automatic cleanup at the end of the first deployment.

## Migration ownership

Use a single controlled migration executor, not every application replica racing at startup. Maintain a schema-version ledger, idempotency, lock/concurrency control, timeouts, backup/recovery plan, and least-privilege database identity. Separate schema authority from ordinary runtime data access.

## Rollback implications

Code rollback is safe only while the schema remains compatible. Data transformation may be irreversible even if DDL can be reversed. Prefer roll-forward after writes begin under the new behavior, unless a tested reverse migration and data-reconciliation plan exists.

## Common mistakes

- Destructive migration before new code is stable.
- Adding a non-null column without safe handling on a large table.
- Running migrations from every pod.
- Long unbounded transaction/lock.
- Assuming backup restore meets recovery objectives.
- Removing old schema before delayed workers/consumers upgrade.

## Interview preparation

**How rename a production column?**  
Add the new column, deploy compatible dual-read/write behavior, backfill and validate, move reads, stop old writes, then drop the old column in a later release.

**Why not roll back the database automatically?**  
New writes and transformations may lose meaning or data under the old schema. Roll-forward is often safer.

**Who runs migrations?**  
A controlled, observable, single executor with narrowly scoped credentials—not every application instance.

## Practical exercise

Add a replacement column using three releases: expand, migrate/dual-write, and contract. Keep old code running during the first two. Interrupt the backfill and prove it resumes safely. Measure locks and validate invariants.

## Official references

- [Azure Well-Architected guidance for deployment and data](https://learn.microsoft.com/azure/well-architected/operational-excellence/safe-deployments)
- [Zero-downtime deployment considerations](https://learn.microsoft.com/azure/architecture/guide/multitenant/considerations/updates)

[Next: Health Checks and Zero Downtime →](06-health-checks-and-zero-downtime.md)
