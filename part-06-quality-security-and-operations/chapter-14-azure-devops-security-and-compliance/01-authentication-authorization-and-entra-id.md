# Authentication, Authorization, and Microsoft Entra ID

[← Chapter 14](README.md) · [Next: Access Levels and Permissions →](02-access-levels-groups-and-permissions.md)

## Separate the layers

**Authentication** proves an identity. **Authorization** determines permitted actions. **Access level** licenses/enables product features. A successfully authenticated user may still lack a feature entitlement or resource permission.

Connecting Azure DevOps Services to Microsoft Entra ID enables centralized lifecycle, single sign-on, MFA, Conditional Access, group governance, and application identities. On-premises Azure DevOps Server has different identity/authentication capabilities; do not assume Services guidance applies unchanged.

## Identity types

- Human users through Microsoft Entra ID.
- Microsoft Entra service principals/applications.
- Managed identities for Azure-hosted automation.
- Azure DevOps build-service identities and job access tokens.
- External/guest users under tenant policies.
- Legacy PAT/SSH/basic-like credentials where still supported.

Use a nonhuman identity for unattended automation. Do not run shared production integration under an employee account.

## Authorization path

For a request, ask:

1. Which identity/token is presented?
2. Which tenant and Azure DevOps organization recognize it?
3. Is the identity an organization member?
4. What access level is assigned?
5. Which direct/indirect groups and roles apply?
6. What object/resource permissions and explicit denies apply?
7. Are pipeline authorization and checks satisfied?
8. For Azure operations, what Azure RBAC/data-plane permissions apply?

Microsoft Entra application API permissions do not automatically grant Azure DevOps resource permissions. Application identities must be added and authorized inside Azure DevOps.

## Authentication choices

For Azure DevOps Services, prefer Microsoft Entra delegated OAuth for interactive applications and managed identity/service principal for unattended automation. Tokens are short-lived and can benefit from enterprise controls. Azure DevOps OAuth is deprecated for new registrations; follow current migration guidance.

Conditional Access affects sign-in/token issuance, but workload and emergency scenarios require testing. Never weaken tenant-wide controls to solve one poorly designed integration.

## Offboarding

Disable/revoke the account/session in Entra, remove Azure DevOps organization/group access, revoke PATs/SSH keys and app consents, transfer ownership, rotate shared exposure, and review audit activity. Group-based access simplifies this but verify propagation and nested membership.

## Interview preparation

**Authentication versus access level versus permission?**  
Authentication proves identity; access level enables product features; permissions/roles authorize actions on scopes and objects.

**Why connect Azure DevOps to Entra?**  
Central identity lifecycle, MFA/Conditional Access, group management, application identities, and enterprise audit/governance.

**Can Entra app permission alone access a repository?**  
No. The application identity must be added to Azure DevOps and receive the required access level/groups/resource permission.

## Practical exercise

Trace one human and one application identity from Entra through Azure DevOps access level, groups, repository, pipeline resource, and Azure RBAC. Remove access at each layer and record the different failure.

## Official references

- [Authenticate to Azure DevOps with Microsoft Entra ID](https://learn.microsoft.com/azure/devops/integrate/get-started/authentication/entra)
- [Authentication guidance](https://learn.microsoft.com/azure/devops/integrate/get-started/authentication/authentication-guidance)
- [Azure DevOps security overview](https://learn.microsoft.com/azure/devops/organizations/security/security-overview)

[Next: Access Levels, Groups, and Permissions →](02-access-levels-groups-and-permissions.md)
