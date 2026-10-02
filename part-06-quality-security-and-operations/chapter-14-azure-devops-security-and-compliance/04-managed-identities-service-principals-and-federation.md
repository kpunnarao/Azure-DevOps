# Managed Identities, Service Principals, and Federation

[← Least Privilege](03-least-privilege-and-separation-of-duties.md) · [Chapter 14](README.md) · [Next: PAT Security →](05-personal-access-token-security.md)

## Application identities

A service principal is an application's representation in a Microsoft Entra tenant. A managed identity is a special service principal whose credentials Azure manages.

- **System-assigned managed identity:** lifecycle tied to one Azure resource.
- **User-assigned managed identity:** independent resource reusable by selected workloads.
- **Service principal:** portable application identity using federation, certificate, or client secret.

Prefer managed identity for Azure-hosted automation. For external workloads, prefer workload identity federation or certificate over a client secret where supported.

## Federation

Workload identity federation exchanges a trusted workload assertion for a short-lived Microsoft Entra token. No reusable client secret is stored. Trust is constrained by issuer, subject, audience, tenant, and configuration.

For Azure Pipelines service connections, use the currently supported workload identity federation model and follow migration guidance. Validate issuer/subject/audience precisely; an overly broad trust can let unintended pipelines impersonate the identity.

## Accessing Azure DevOps

A managed identity/service principal used against Azure DevOps Services must be explicitly added to the organization, assigned a functional access level, groups, and resource permissions. Microsoft Entra API permissions do not replace Azure DevOps authorization. Application identities use short-lived tokens but still require lifecycle review.

## Accessing Azure

An Azure service connection represents the pipeline's path to a Microsoft Entra identity. Azure DevOps decides whether the pipeline may use the connection; Azure RBAC/data-plane permissions decide what the identity can do. Scope both.

## Lifecycle inventory

Record owner, hosting workload, tenant, client/principal IDs, federation/certificate/secret method, trust conditions, Azure DevOps membership/access/groups, Azure RBAC, service connections, repositories/pipelines, expiration/review, and incident procedure.

Disable unused identities, but investigate dependencies first. User-assigned identities outlive attached resources and need explicit cleanup.

## Interview preparation

**Managed identity versus service principal?**  
A managed identity is an Azure-managed service principal with managed credentials; a general service principal may use federation, certificate, or secret and can run outside Azure.

**What risk remains after federation?**  
Overprivileged identity, broad subject trust, compromised pipeline/repository/agent, token misuse during its life, and weak resource authorization.

**Why add an app identity to Azure DevOps?**  
Entra proves it; Azure DevOps membership/access level/permissions authorize product actions.

## Practical exercise

Use a managed identity from an Azure-hosted sandbox or a federated service principal to call one Azure DevOps read API and one narrowly scoped Azure resource. Deny writes, inspect token lifetime, and document both authorization layers.

## Official references

- [Service principals and managed identities in Azure DevOps](https://learn.microsoft.com/azure/devops/integrate/get-started/authentication/service-principal-managed-identity)
- [Azure Resource Manager workload identity connections](https://learn.microsoft.com/azure/devops/pipelines/library/connect-to-azure)
- [Workload identity federation](https://learn.microsoft.com/entra/workload-id/workload-identity-federation)

[Next: Personal Access Token Security →](05-personal-access-token-security.md)
