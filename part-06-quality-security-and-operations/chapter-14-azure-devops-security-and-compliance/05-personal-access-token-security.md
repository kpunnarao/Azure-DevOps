# Personal Access Token Security

[← Workload Identity](04-managed-identities-service-principals-and-federation.md) · [Chapter 14](README.md) · [Next: Secret Lifecycle →](06-key-vault-secure-files-and-secret-rotation.md)

## PATs are bearer passwords

A Personal Access Token acts as the creating user's Azure DevOps credential within selected scopes and lifetime. Anyone holding it can act without MFA for each request. Avoid PATs when Microsoft Entra OAuth, managed identity, service principal, service connection, Git Credential Manager, or a supported credential provider works.

Use PATs only for short-lived personal scripts, one-time testing, or legacy integrations awaiting migration.

## Minimum safe configuration

- Organization-scoped rather than global.
- Minimum resource/action scopes.
- Shortest practical lifetime.
- Descriptive name without token/personal data.
- One token per purpose.
- Stored in an approved secret store/credential manager.
- Never in Git, variable files, work items, URLs, logs, command history, or images.
- Owner and backup owner, expiry alert, rotation, and migration plan.

Do not use a personal PAT for shared production automation. Offboarding or account changes can break it, and accountability remains tied to the individual.

## Organization governance

Tenant/organization policy can restrict global/full-scope PATs, maximum lifetime, and leaked-token behavior. Maintain allow lists only as reviewed, expiring exceptions. Inventory through supported administration/lifecycle APIs subject to permissions.

Audit logs record lifecycle events such as creation/revocation, but token usage is not necessarily sufficient to determine whether a PAT is active or stale. Validate dependencies before revocation, then remove aggressively.

## Leak response

1. Revoke immediately; do not wait to verify exploitation.
2. Preserve alert/audit/source evidence.
3. Remove secret from current content and history where required.
4. Rotate any downstream credential/data exposed through its access.
5. Identify scope, owner, repositories, packages, builds, releases, and time window.
6. Review audit/activity logs and anomalous changes.
7. remediate persistence/backdoors.
8. Replace with stronger authentication and close the leak path.

Deleting a committed token without revocation is not remediation.

## Rotation

Create replacement with equal-or-smaller scope, update consumer securely, validate, revoke old token, and monitor. Avoid overlapping tokens longer than necessary. Rotation is not a substitute for migration.

## Interview preparation

**When is PAT appropriate?**  
Only when a safer supported method is unavailable, with narrow organization/scope/lifetime and explicit migration.

**Why not store one in a secret variable indefinitely?**  
Storage reduces exposure but does not fix long-lived bearer risk, user coupling, scope, rotation, or weak workload identity.

**First action on leak?**  
Revoke the token immediately, then investigate and rotate affected downstream access.

## Practical exercise

Inventory sandbox PATs by owner, scope, age, expiry, and purpose. Replace one with an Entra token. Create a deliberately narrow disposable PAT, verify denied operations, rotate it, and rehearse leak response.

## Official references

- [Use personal access tokens](https://learn.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate)
- [Azure DevOps authentication guidance](https://learn.microsoft.com/azure/devops/integrate/get-started/authentication/authentication-guidance)

[Next: Key Vault, Secure Files, and Secret Rotation →](06-key-vault-secure-files-and-secret-rotation.md)
