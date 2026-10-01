# Variable Groups, Secure Files, and Key Vault

[← Service Connections](04-service-connections.md) · [Chapter 9](README.md) · [Next: Environment Configuration →](06-environment-specific-configuration.md)

## Choose storage by data type

| Need | Appropriate mechanism |
|---|---|
| Shared non-secret values | Variable group or versioned config |
| Secret scalar values | Secret variables or external secret store |
| Azure Key Vault secrets at runtime | Key Vault-linked variable group or task |
| Certificate, keystore, provisioning profile | Secure file |
| Cloud authentication | Service connection/workload identity |
| Large structured non-secret config | Configuration file/service, not many variables |

Do not use one mechanism for convenience when its security and lifecycle do not match the data.

## Variable groups

Variable groups share values across pipelines. Secret variables in a group make it a protected resource. Authorize specific YAML pipelines; merely naming a group in YAML must not grant access, because a contributor could add a step that exfiltrates secrets.

```yaml
variables:
- group: orders-production
```

Secret masking is not a complete data-loss-prevention system. Avoid printing secrets, substrings may not be masked, and command-line arguments can appear in process or diagnostic output.

## Secure files

Secure files store sensitive file material such as certificates, SSH keys, keystores, and provisioning profiles. They are encrypted at rest and support pipeline permissions and checks. Download them through the supported task, minimize their lifetime on the agent, restrict file permissions, and ensure cleanup. Uploaded contents cannot simply be edited in place; manage replacement and references deliberately.

## Key Vault integration

A Key Vault-linked variable group maps selected **secret names**, and values are fetched when the pipeline runs. A changed value becomes available automatically, but newly added or deleted secret names do not automatically change the mapping. Azure Pipelines variable-group integration covers secrets, not Key Vault keys or certificates.

Network design matters. Private endpoints, firewalls, hosted-agent egress, the vault permission model, and service-connection identity all affect retrieval. For private networking, validate the current supported approach and consider a suitably networked self-hosted/managed agent with a direct Key Vault task.

## Secret rotation

Applications should tolerate overlapping old/new credentials where possible. Rotate the source, verify deployment, revoke the old value, and record completion. A secret fetched dynamically can change without a Git diff, so preserve version/audit metadata without exposing the value.

## Interview preparation

**Why authorize a variable group to selected pipelines?**  
Repository contributors can change YAML. Without pipeline authorization, they could add a step to consume or exfiltrate group secrets.

**Key Vault-linked group limitation?**  
It maps secrets only; keys/certificates are not supported through that integration, and new secret names require mapping updates.

**Secure file versus secret variable?**  
Use a secure file when a tool requires file-shaped sensitive material; use a secret variable for a scalar. Both require protected-resource permissions and careful agent handling.

## Practical exercise

Create a non-secret group, a protected secret group, and a disposable secure file. Authorize only one pipeline. Rotate a Key Vault secret value, add a new name, and observe the difference between value refresh and mapping refresh.

## Official references

- [Manage variable groups](https://learn.microsoft.com/azure/devops/pipelines/library/variable-groups)
- [Link a variable group to Azure Key Vault](https://learn.microsoft.com/azure/devops/pipelines/library/link-variable-groups-to-key-vaults)
- [Use secure files](https://learn.microsoft.com/azure/devops/pipelines/library/secure-files)

[Next: Environment-specific Configuration →](06-environment-specific-configuration.md)
