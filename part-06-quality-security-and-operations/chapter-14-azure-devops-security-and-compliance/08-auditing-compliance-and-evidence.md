# Auditing, Compliance, and Evidence

[← SDLC Scanning](07-security-scanning-across-the-sdlc.md) · [Chapter 14](README.md)

## Evidence, not screenshots

Compliance translates obligations into controls, operation, evidence, review, and remediation. A screenshot is easy to collect but weak: it may lack scope, time, completeness, integrity, and reproducibility.

For each control define:

- Objective and threat/failure.
- Owner and frequency.
- Preventive/detective/corrective mechanism.
- Systems and scope.
- Evidence source and query.
- Retention, access, integrity, and time synchronization.
- Exception and remediation workflow.
- Test that proves the control actually works.

## Azure DevOps audit

Azure DevOps Services auditing records organization-level state-changing activities and supports filtering/export under current product constraints. Current Microsoft documentation describes auditing availability for organizations backed by Microsoft Entra ID and notes preview status; confirm tenant/service features before using it as the only regulatory source.

Grant audit-log viewing narrowly. Export to a controlled monitoring/SIEM path if required for longer retention, correlation, alerting, or immutability. Monitor audit configuration/export failures.

## Evidence map

Useful evidence may include:

- Entra sign-in, Conditional Access, PIM, and lifecycle records.
- Azure DevOps membership, access levels, groups, ACLs, PAT lifecycle, policies, and audit events.
- Branch policies, PR reviewers, commits, and signatures.
- Pipeline definition/template versions, run logs, approvals/checks, artifact provenance.
- Service-connection authorization and Azure RBAC changes.
- Scan findings, triage, exceptions, and remediation.
- Environment deployments, health evidence, incidents, and change records.
- Key Vault/ACR/AKS/Azure activity and diagnostic logs.

No single log covers the whole control.

## Evidence protection

Minimize personal/sensitive values, redact secrets, encrypt, restrict access, record hashes/chain of custody where needed, use UTC/time sync, apply legal retention/deletion rules, and separate evidence administrator from ordinary operators for high-assurance contexts.

Test evidence retrieval before an audit. An export that silently stopped six months ago is not a control.

## Audit versus monitoring

Auditing answers who changed what and when; monitoring detects system/security conditions; traceability connects source, build, artifact, deployment, and requirement. They overlap but are not interchangeable.

## Interview preparation

**How prove least privilege?**  
Provide identity/group/access inventory, effective-permission evidence, role justification, periodic reviews, JIT records, exceptions, and removal outcomes—not only a policy document.

**What makes evidence trustworthy?**  
Known authoritative source, complete scope/time, integrity, controlled access, retention, reproducible query, and independent review.

**How handle an exception?**  
Business reason, scope, risk, compensating controls, accountable approval, expiry, monitoring, and remediation plan.

## Practical exercise

Choose five controls and build an evidence matrix. Export a sandbox audit interval, correlate a group change to approver and downstream permission, verify timestamps/integrity, redact sensitive data, and test restoration/retrieval.

## Official references

- [Azure DevOps auditing](https://learn.microsoft.com/azure/devops/organizations/audit/azure-devops-auditing)
- [Azure DevOps compliance](https://learn.microsoft.com/azure/devops/organizations/security/data-protection)
- [Microsoft Service Trust Portal](https://servicetrust.microsoft.com/)

## Chapter review

Trace one privileged production change across Entra identity, Azure DevOps effective permissions, source review, pipeline resource checks, workload identity, Azure RBAC, secret access, scan evidence, deployment, and audit export. Identify every place evidence could be bypassed or lost.

[Chapter 15 — Monitoring, Feedback, and Troubleshooting →](../chapter-15-monitoring-feedback-and-troubleshooting/README.md)
