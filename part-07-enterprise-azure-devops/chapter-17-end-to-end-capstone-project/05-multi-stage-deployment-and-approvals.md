# Multi-stage Deployment and Approvals

[← Infrastructure Provisioning](04-infrastructure-and-environment-provisioning.md) · [Chapter 17](README.md) · [Next: Security and Compliance →](06-security-testing-and-compliance.md)

## Promotion flow

Deploy the exact CI digest through Development → Test → Production. Each stage uses a deployment job targeting an Azure DevOps environment and records version, configuration revision, identity, tests, and health.

Development deploys automatically after authoritative CI. Test runs integration/contract/performance/security checks. Production begins only after resource-owned branch control, required template/policy, automated health evidence, and accountable approval.

## Identity and configuration

Use separate nonproduction/production service connections and target identities. Production has narrow scope, selected-pipeline authorization, and no PR access. Externalize configuration and retrieve secrets at runtime. Produce a sanitized configuration fingerprint.

## Deployment strategy

Implement canary, blue-green, or rolling based on the ADR. For canary, define cohorts/increments, minimum traffic, observation windows, baseline, success/abort thresholds, and override authority. For blue-green, validate green, switch traffic, retain blue for a bounded window, and account for background workers/state.

Use an exclusive lock: `runLatest` if only newest desired state matters, `sequential` if every ordered change must run.

## Database and flags

Execute expand–migrate–contract across releases. Prove old/new compatibility and resumable backfill. Use a feature flag to decouple exposure, with owner/expiry/telemetry and tested failure behavior. Flags are not authorization.

## Checks and evidence

Approver sees artifact digest, commit/work items, test/security results, change/risk, plan, health baseline, recovery, and SLO/error-budget status. Approval is not manual testing. Bypass requires restricted authority, reason, alert, and review.

## Failure demonstrations

- Unauthorized pipeline/branch cannot use environment.
- Two deployments exercise lock behavior.
- Canary health stops progression.
- Secret retrieval fails safely.
- Feature disabled without redeploy.
- Old application works with expanded schema.
- Traffic reversal uses same previous digest.

## Acceptance evidence

Environment permissions/checks, deployment history, service-connection/RBAC, approval record, configuration fingerprint, traffic/cohort telemetry, migration ledger, feature lifecycle, exact digests, and recovery timing.

## Expert review questions

Can YAML remove the approval? Does the approver know what changed? What state prevents rollback? Are missing metrics healthy? How is an interrupted migration resumed? Who can bypass and how is it detected?

[Next: Security, Testing, and Compliance →](06-security-testing-and-compliance.md)
