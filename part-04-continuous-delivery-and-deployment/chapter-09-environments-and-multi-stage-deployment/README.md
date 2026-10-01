# Chapter 9 — Environments and Multi-stage Deployment Pipelines

[← Part IV — Continuous Delivery and Deployment](../README.md)

This chapter builds the controlled path between a trusted CI artifact and its deployment targets. It covers the Azure Pipelines resources, identities, configuration mechanisms, approval controls, and audit history required for secure multi-stage delivery.

## Why this chapter matters

A deployment stage often has access to production credentials and destructive operations. Keeping its YAML readable is not enough. Resource owners must control which pipelines can use an environment, service connection, variable group, secure file, or agent pool, and which checks must pass before use.

Azure Pipelines **environments** represent logical deployment targets and record history, commits, and work items. **Deployment jobs** connect execution to those environments. **Protected resources** add pipeline permissions, user permissions, approvals, and checks outside repository-controlled YAML.

## Topics

| # | Topic | Practical outcome |
|---:|---|---|
| 1 | [CI and CD Separation](01-ci-and-cd-separation.md) | Promote an immutable artifact across trust boundaries |
| 2 | [Environment Strategy](02-environment-strategy.md) | Model environments around real risk and ownership |
| 3 | [Deployment Jobs and Deployment History](03-deployment-jobs-and-deployment-history.md) | Produce auditable deployment records |
| 4 | [Service Connections](04-service-connections.md) | Authenticate without overprivileged stored credentials |
| 5 | [Variable Groups, Secure Files, and Key Vault](05-variable-groups-secure-files-and-key-vault.md) | Select appropriate configuration and secret storage |
| 6 | [Environment-specific Configuration](06-environment-specific-configuration.md) | Change behavior without rebuilding the artifact |
| 7 | [Approvals, Checks, and Exclusive Locks](07-approvals-checks-and-exclusive-locks.md) | Enforce protected-resource policy and concurrency |
| 8 | [Separation of Duties](08-separation-of-duties.md) | Divide authority according to risk |

## Guided chapter lab

Create Development, Test, and Production environments. Build in CI once and deploy the published artifact with deployment jobs. Configure:

- Pipeline permissions on every environment.
- A workload-identity-federated Azure service connection with narrow Azure RBAC.
- A non-secret configuration variable group.
- A secret retrieved from an approved secret store.
- Production branch control, approval, and exclusive lock checks.
- A smoke test whose result is preserved with deployment history.

Run two deployments close together and observe the lock behavior. Attempt deployment from an unauthorized pipeline and branch. Record the effective identity and every authorization decision.

## Troubleshooting exercise

Diagnose these failures without broadening access:

1. Environment authorization error.
2. Service connection is visible but unauthorized.
3. Secret variable group access fails.
4. Key Vault secret name was added but is not mapped.
5. Two runs wait on the same production environment.
6. Deployment succeeds but does not appear in environment history.

For each, identify whether the failure belongs to YAML, pipeline permission, user permission, resource check, Azure RBAC, network reachability, or deployment-job targeting.

## Completion criteria

You are ready for Chapter 10 when you can trace the effective deployment identity, explain why checks live outside YAML, deploy the same artifact with different external configuration, and reconstruct who deployed which version to which environment.

## Chapter navigation

[← Part III](../../part-03-continuous-integration-with-azure-pipelines/README.md) · [Chapter 10 →](../chapter-10-deployment-strategies-validation-and-rollback/README.md)
