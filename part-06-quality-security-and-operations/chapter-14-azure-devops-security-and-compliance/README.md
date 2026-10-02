# Chapter 14 — Azure DevOps Security and Compliance

[← Part VI — Quality, Security, and Operations](../README.md)

Security in Azure DevOps spans human identity, application identity, licensing/access levels, permissions, protected resources, tokens, secrets, source and pipeline scanning, audit, and evidence retention. This chapter provides one model for reasoning across those layers.

## Topics

| # | Topic | Practical outcome |
|---:|---|---|
| 1 | [Authentication, Authorization, and Microsoft Entra ID](01-authentication-authorization-and-entra-id.md) | Separate identity proof from effective access |
| 2 | [Access Levels, Groups, and Permissions](02-access-levels-groups-and-permissions.md) | Diagnose and administer Azure DevOps access |
| 3 | [Least Privilege and Separation of Duties](03-least-privilege-and-separation-of-duties.md) | Reduce unilateral high-impact actions |
| 4 | [Managed Identities, Service Principals, and Federation](04-managed-identities-service-principals-and-federation.md) | Use short-lived workload identity |
| 5 | [Personal Access Token Security](05-personal-access-token-security.md) | Minimize and govern bearer tokens |
| 6 | [Key Vault, Secure Files, and Secret Rotation](06-key-vault-secure-files-and-secret-rotation.md) | Control secret lifecycle end to end |
| 7 | [Security Scanning Across the SDLC](07-security-scanning-across-the-sdlc.md) | Combine preventive and detective analysis |
| 8 | [Auditing, Compliance, and Evidence](08-auditing-compliance-and-evidence.md) | Produce reliable, minimized evidence |

## Guided chapter lab

Create an identity and access inventory for a sandbox organization:

1. Connect/verify Microsoft Entra tenant governance.
2. Map users, groups, application identities, access levels, and effective permissions.
3. Identify direct grants, explicit denies, inactive users, administrators, PATs, service connections, and open protected resources.
4. Replace one PAT/client secret with a managed or federated identity.
5. Apply selected-pipeline authorization to a secret variable group/service connection.
6. Enable representative secret, dependency, code, IaC, and container scanning.
7. Create an exception with owner and expiry.
8. Generate an audit evidence package with integrity and redaction controls.
9. Simulate offboarding and credential leakage.
10. Verify revocation at Microsoft Entra, Azure DevOps, pipeline-resource, Azure RBAC, and secret-store layers.

## Threat-model questions

- Can a contributor modify YAML and obtain a production secret?
- Can one identity change code, policy, approval, and target?
- Can a self-hosted agent retain credentials between jobs?
- Can a compromised workload publish artifacts or modify source?
- Can an administrator bypass a control without detection?
- Do audit logs contain enough context and remain protected?
- Can a scanner outage or skipped task look green?
- Can a former employee's PAT or SSH key still work?

## Completion criteria

You are ready for Chapter 15 when you can compute effective access, trace a workload identity across systems, respond to a leaked PAT/secret, defend scan gates and exceptions, and map a compliance control to trustworthy technical evidence.

## Chapter navigation

[← Chapter 13](../chapter-13-testing-strategy-and-azure-test-plans/README.md) · [Chapter 15 →](../chapter-15-monitoring-feedback-and-troubleshooting/README.md)
