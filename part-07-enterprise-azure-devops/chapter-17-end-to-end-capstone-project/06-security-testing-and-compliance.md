# Security, Testing, and Compliance

[← Deployment and Approvals](05-multi-stage-deployment-and-approvals.md) · [Chapter 17](README.md) · [Next: Monitoring and Operations →](07-monitoring-rollback-and-operations.md)

## Assurance matrix

Create a table linking requirement/risk to prevention, test/scan, production detection, recovery, evidence, owner, and residual risk. Cover authentication/authorization, data protection, dependency/supply chain, agent/pipeline, infrastructure, container/Kubernetes, deployment, observability, and recovery.

## Identity review

Inventory humans, Entra groups, build-service identities, service principals/managed identities, service connections, AKS/workload identities, ACR roles, Key Vault access, database identity, PATs/SSH keys, agents, and break-glass.

Prove least privilege with allowed and denied operations. Eliminate PAT/client secret where federation works. No single ordinary identity can change code, pipeline controls, approval, and production target without independent evidence.

## Security testing

Implement secret protection, SAST, SCA, IaC scanning, container scanning, SBOM/provenance, API/DAST, authorization negative tests, Kubernetes policy, and runtime/cloud posture. Track findings by source and deployed digest. Scanner unavailable is unknown/fail according to policy.

Triage one finding and create one expiring exception with compensating control. Revoke a disposable leaked credential immediately.

## Test Plans

Create release plan with requirement-based and risk/regression suites, configurations, manual case, exploratory charter, automated association, run/results, bug linkage, and requirement quality. Use synthetic isolated data and retention/redaction.

## Compliance evidence

Map at least five controls: change approval, least privilege, secret management, vulnerability management, and deployment traceability. For each define owner, frequency, source/query, retention, integrity, access, exception, and validation.

Export relevant Azure DevOps/Entra/Azure audit evidence without secrets. Screenshots supplement but do not replace authoritative data.

## Failure demonstrations

- Ordinary PR cannot read production secret.
- Workload cannot write ACR/source.
- Vulnerability blocks promotion.
- Secret leak is revoked and investigated.
- Explicit deny/effective permission is explained.
- Audit export correlates privileged change to identity.
- Expired exception triggers action.

## Acceptance evidence

Threat model, access/effective-permission report, identity lifecycle, scan results/triage, test plan/runs, secret rotation, policy/admission result, audit/evidence map, exception register, and incident record.

## Expert review questions

Which control is preventive versus detective? What happens if scan/audit export is unavailable? Can an administrator bypass silently? Is test evidence from the exact release? How is sensitive evidence protected?

[Next: Monitoring, Rollback, and Operations →](07-monitoring-rollback-and-operations.md)
