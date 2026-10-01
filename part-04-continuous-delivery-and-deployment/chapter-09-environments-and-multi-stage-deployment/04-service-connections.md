# Service Connections

[← Deployment Jobs](03-deployment-jobs-and-deployment-history.md) · [Chapter 9](README.md) · [Next: Variables, Secure Files, and Key Vault →](05-variable-groups-secure-files-and-key-vault.md)

## Controlled external identity

A service connection stores or brokers the information Azure Pipelines needs to authenticate to Azure, a container registry, Kubernetes, GitHub, or another external service. It is a protected resource with user permissions, pipeline permissions, and checks.

For Azure Resource Manager, prefer workload identity federation when supported. It exchanges a short-lived token rather than storing a client secret or certificate. Azure permissions still come from the associated Microsoft Entra identity and Azure RBAC.

> Identity platforms evolve. Review current Microsoft guidance when creating or converting federated service connections; do not copy issuer or credential details from an old tutorial.

## Least-privilege design

Separate connections by trust boundary and purpose:

- Nonproduction versus production.
- Application deployment versus infrastructure administration.
- Read-only validation versus change authority.
- Subscription/resource-group/resource scope.
- Team/application ownership.

Grant the underlying identity the narrowest Azure role at the narrowest scope. A narrowly scoped service connection mapped to Owner at subscription scope is not least privilege.

Authorize selected pipelines explicitly. Avoid “Grant access permission to all pipelines” for privileged connections. Add branch control, required-template, or approval checks where risk requires them.

## Lifecycle and ownership

Record owner, purpose, tenant/subscription, underlying principal, RBAC assignments, authorized pipelines, checks, creation method, review date, and rotation/migration plan. Monitor use and stale connections.

For stored-secret connections that cannot yet use federation:

- Store the credential only in the managed connection.
- Set expiry alerts and rotate before expiration.
- Never copy it into YAML or a variable group.
- Restrict who can administer/use the connection.
- Migrate when the integration supports short-lived credentials.

## Common failures

| Symptom | Likely layer |
|---|---|
| Pipeline resource authorization error | Azure DevOps pipeline permission |
| Connection succeeds but deployment denied | Azure RBAC or target permission |
| Token/audience/issuer error | Federation configuration |
| Target unavailable | Network/DNS/firewall/private endpoint |
| Connection name from variable fails | Service connection selection/check limitation |

Approvals and checks documentation notes that service connections cannot be dynamically specified by variable. Keep protected resource references statically reviewable.

## Interview preparation

**Service connection versus variable group?**  
A service connection represents authenticated access to an external system; a variable group stores values. Putting a cloud credential in variables loses connection-specific permissions, checks, and lifecycle controls.

**Does WIF remove permission risk?**  
It removes a stored long-lived secret, but an overprivileged federated identity remains dangerous. Scope, pipeline authorization, conditions, and monitoring still matter.

**Azure DevOps permission versus Azure RBAC?**  
Azure DevOps decides whether the pipeline may use the connection; Azure RBAC decides what its identity may do in Azure. Both must allow the operation.

## Practical exercise

Create a sandbox federated connection scoped to one resource group. Authorize one pipeline, attempt use from another, and inspect the effective Azure role. Remove one permission and diagnose the resulting failure without granting Owner.

## Official references

- [Azure Resource Manager service connections](https://learn.microsoft.com/azure/devops/pipelines/library/connect-to-azure)
- [Service connections](https://learn.microsoft.com/azure/devops/pipelines/library/service-endpoints)
- [Pipeline resource security](https://learn.microsoft.com/azure/devops/pipelines/security/resources)

[Next: Variable Groups, Secure Files, and Key Vault →](05-variable-groups-secure-files-and-key-vault.md)
