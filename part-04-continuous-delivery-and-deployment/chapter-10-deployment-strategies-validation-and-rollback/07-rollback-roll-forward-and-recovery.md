# Rollback, Roll-forward, and Recovery

[← Health Checks](06-health-checks-and-zero-downtime.md) · [Chapter 10](README.md)

## Recovery is designed before release

A rollback redeploys or reroutes to a previously trusted version. A roll-forward deploys a corrective new version. A feature-flag kill switch disables behavior without changing the deployment. Choose using impact, time, compatibility, data changes, evidence, and confidence—not habit.

## Decision guide

Prefer rapid traffic reversal or rollback when:

- The previous version remains compatible with current schema/data.
- The defect is clearly isolated to the new version.
- No irreversible external side effect blocks reversal.
- The old artifact and configuration are available.
- Recovery is faster than creating and validating a fix.

Prefer roll-forward when:

- Data/schema has moved beyond backward compatibility.
- The previous version has a security or correctness defect.
- A small, well-understood fix is faster/safer.
- External side effects must be reconciled.
- Rollback itself has not been tested.

Stop exposure first when possible. Preserving a small blast radius buys investigation time.

## A recovery runbook

Include:

1. Detection thresholds and incident declaration.
2. Authority to stop, reverse, or bypass.
3. Exact previous and current artifact identities.
4. Traffic and feature-flag actions.
5. Database compatibility and data reconciliation.
6. Queue/event handling and idempotency.
7. Verification from customer-facing paths.
8. Communications and status ownership.
9. Evidence preservation.
10. Criteria to resume and retrospective actions.

Automate repeatable actions but keep them observable and interruptible. A rollback pipeline must use trusted retained artifacts; never rebuild the “old” commit.

## Recovery objectives

- **MTTD:** time to detect.
- **MTTR:** time to restore/resolve.
- **RTO:** maximum acceptable outage/recovery time.
- **RPO:** maximum acceptable data loss measured in time.

A backup is useful only if restore time and recovered data meet RTO/RPO. Deployment rollback and disaster recovery overlap but are not the same: rollback handles a bad change; disaster recovery handles loss/unavailability of the operating environment or data.

## Practice and learning

Run game days. Simulate health regression, bad configuration, expired credential, partial rollout, incompatible database change, region loss, and missing telemetry. Measure decision time, execution time, customer impact, and manual steps. Update pipelines and runbooks after every exercise or incident.

## Common mistakes

- Saying “we can redeploy the old version” without testing data compatibility.
- Rebuilding an old source revision.
- Automatically rolling back after destructive migration.
- Deleting the prior slot/image too early.
- Fixing production manually without reconciling declared state.
- Measuring deployment success but not service restoration.

## Interview preparation

**Rollback or roll-forward?**  
Evaluate which restores service fastest with least additional risk, considering schema/data compatibility, security, side effects, artifact availability, and tested procedures.

**How do you validate recovery?**  
Use external customer-path tests, health and business telemetry, data invariants, queue state, and observation over a defined window.

**Deployment recovery versus disaster recovery?**  
Deployment recovery reverses/corrects a bad change; disaster recovery restores service after major infrastructure/data loss under RTO/RPO objectives.

## Practical exercise

Run three scenarios: application error with compatible data, bad schema migration, and leaked credential. Choose recovery per scenario, execute it, and record MTTD/MTTR. Verify the old artifact checksum and reconcile any state modified before recovery.

## Official references

- [Safe deployment practices](https://learn.microsoft.com/azure/well-architected/operational-excellence/safe-deployments)
- [Azure reliability guidance](https://learn.microsoft.com/azure/well-architected/reliability/)
- [Continuous delivery](https://learn.microsoft.com/devops/deliver/what-is-continuous-delivery)

## Part IV review

You should now be able to trace and defend this operating loop:

```text
immutable candidate → least-privilege deployment → limited exposure
        → explicit health decision → expand or stop
        → rollback / roll forward → verify recovery → learn
```

Return to the [Part IV overview](../README.md), complete the project and checklist, and preserve the resulting deployment architecture, evidence policy, and recovery runbook for future teaching and project use.
