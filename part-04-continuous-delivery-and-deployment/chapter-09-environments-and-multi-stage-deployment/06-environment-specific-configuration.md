# Environment-specific Configuration

[← Variables, Secure Files, and Key Vault](05-variable-groups-secure-files-and-key-vault.md) · [Chapter 9](README.md) · [Next: Approvals and Locks →](07-approvals-checks-and-exclusive-locks.md)

## One artifact, external configuration

The application binary or image should be identical in Development, Test, and Production. Environment behavior changes through externally supplied configuration: endpoints, feature settings, scale, connection references, and secret identifiers.

Separate:

- **Build-time values:** genuinely affect compilation and therefore artifact identity.
- **Deploy-time values:** bind the artifact to an environment.
- **Runtime values:** can change safely while the application runs.
- **Secrets:** retrieved through a protected channel, never committed.

If a “configuration transform” modifies compiled or packaged content, the environments no longer run the same artifact. Prefer platform settings, mounted configuration, deployment manifests, or configuration services.

## Hierarchy and precedence

Define and document precedence, for example:

```text
safe application defaults
  < versioned environment config
  < deployment parameters
  < protected runtime config
  < secret references
```

Avoid the same key in many layers. Validate a resolved configuration schema before deployment and log non-secret keys/source—not secret values.

## Configuration as code

Store non-secret configuration in Git when review and reproducibility matter. Use schemas, types, allowed ranges, policy tests, and environment overlays. Avoid duplicating whole files: a small override reduces drift.

Secrets must be referenced, not embedded. A versioned manifest can identify a Key Vault secret name or managed-identity resource without containing the credential.

## Safe change management

Configuration changes can be as dangerous as code. They need ownership, review, testing, audit, rollout, and recovery. A direct production portal edit should be an emergency exception followed by reconciliation back into the declared source.

Prevent environment-name string concatenation from selecting credentials or subscriptions. Protected resources should remain explicit and reviewable.

## Observability

At startup, emit a sanitized configuration fingerprint: application version, environment, region/stamp, non-secret feature set, configuration revision, and dependency endpoints. This helps explain why identical artifacts behave differently. Never include tokens, passwords, connection strings, or sensitive customer settings.

## Interview preparation

**Why avoid environment-specific builds?**  
They invalidate build-once evidence and create different binaries for each environment. External configuration preserves artifact identity.

**Should all configuration be in variable groups?**  
No. Large structured non-secret configuration is easier to review and validate in files or a configuration service. Variable groups are useful for shared values and protected secrets.

**How do you detect drift?**  
Compare declared and effective sanitized configuration, use IaC/policy checks, monitor portal changes, and reconcile emergency edits.

## Practical exercise

Deploy one checksum-identical artifact twice with two non-secret configuration sets and secret references. Add schema validation. Generate a sanitized fingerprint and prove that a wrong endpoint fails before traffic is routed.

## Official references

- [Azure App Configuration best practices](https://learn.microsoft.com/azure/azure-app-configuration/howto-best-practices)
- [Variables in Azure Pipelines](https://learn.microsoft.com/azure/devops/pipelines/process/variables)

[Next: Approvals, Checks, and Exclusive Locks →](07-approvals-checks-and-exclusive-locks.md)
