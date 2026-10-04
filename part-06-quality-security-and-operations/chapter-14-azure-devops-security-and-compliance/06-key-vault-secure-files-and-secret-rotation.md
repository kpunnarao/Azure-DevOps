# Key Vault, Secure Files, and Secret Rotation

[← PAT Security](05-personal-access-token-security.md) · [Chapter 14](README.md) · [Next: SDLC Scanning →](07-security-scanning-across-the-sdlc.md)

## Secret lifecycle

A secret has issuer/source, owner, authorized consumers, distribution path, runtime use, rotation, revocation, audit, and recovery. “Stored in Key Vault” addresses only part of that lifecycle.

Prefer eliminating secrets through managed/federated identity. When secrets remain, retrieve them late, expose them briefly, and never persist them into artifacts, caches, container layers, state, logs, telemetry, or test evidence.

## Mechanism selection

- **Key Vault:** centralized secret/version lifecycle, access policy/RBAC, audit, network controls.
- **Key Vault-linked variable group:** maps selected secret names and fetches current values at run time; added/deleted names require mapping update; integration supports secrets, not keys/certificates.
- **AzureKeyVault pipeline task/direct SDK:** explicit retrieval, potentially better control for private network/identity scenarios.
- **Secure files:** sensitive file-shaped material such as certificates or provisioning/keystore files, with protected-resource permissions/checks.
- **Secret variable:** small scalar where external retrieval is not practical.

Key Vault keys/certificates should use purpose-built cryptographic/certificate integration rather than exporting private material when possible.

## Agent handling

Secret-bearing jobs should run on trusted isolated agents. Limit environment scope, avoid command-line exposure, disable verbose shell tracing, scrub workspaces, secure process/crash dumps, and prevent untrusted code from running before or after secret use.

Masking is best-effort and may not hide substrings/transformed values. Never test masking by printing a real secret.

## Rotation design

1. Issue new version.
2. Allow old/new overlap if protocol supports it.
3. Update/restart consumers safely.
4. Verify new credential through telemetry.
5. Revoke old version.
6. Monitor failures and record completion.

For single-active credentials, use coordinated maintenance or dual endpoints. Define emergency rotation with owner and expected recovery time.

## Interview preparation

**Key Vault group refresh behavior?**  
Existing mapped secret values update at run time; new/deleted secret names require changing the variable-group mapping.

**Secure file versus certificate in Key Vault?**  
Prefer nonexportable Key Vault-backed cryptographic use when supported; secure files suit tools that truly require file material.

**Why isolated agents?**  
Secrets can leak through workspace residue, processes, caches, logs, or later jobs on persistent workers.

## Practical exercise

Rotate a sandbox secret with overlap, deploy, verify, revoke old, and observe audit. Use a disposable secure file with selected-pipeline authorization and prove unauthorized access fails. Inspect cleanup without printing contents.

## Official references

- [Link variable groups to Key Vault](https://learn.microsoft.com/azure/devops/pipelines/library/link-variable-groups-to-key-vaults)
- [Secure files](https://learn.microsoft.com/azure/devops/pipelines/library/secure-files)
- [Azure Key Vault best practices](https://learn.microsoft.com/azure/key-vault/general/best-practices)

[Next: Security Scanning Across the SDLC →](07-security-scanning-across-the-sdlc.md)
