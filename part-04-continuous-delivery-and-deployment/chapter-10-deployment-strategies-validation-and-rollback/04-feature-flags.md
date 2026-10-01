# Feature Flags

[← Canary Deployment](03-canary-deployment.md) · [Chapter 10](README.md) · [Next: Database Migrations →](05-backward-compatible-database-migrations.md)

## Separate deployment from exposure

A feature flag lets deployed code choose behavior at runtime. Teams can deploy dark, expose to internal users, opt-in customers, or a percentage cohort, measure the result, and disable the behavior without redeploying.

Flags complement deployment strategies:

- Deployment ring controls which machines receive code.
- Canary traffic controls which requests reach a version.
- Feature flag controls which users or requests execute behavior.

## Flag types

- **Release flag:** temporary control for incomplete/new functionality.
- **Experiment flag:** assigns variants and measures outcomes.
- **Operational flag:** disables expensive or risky behavior.
- **Permission/entitlement:** durable access control; should not be managed like a temporary release flag.

Every temporary flag needs owner, creation date, intended cohorts, default behavior, removal condition, and expiry. Stale flags multiply code paths and testing burden.

## Safe implementation

Evaluate flags through a resilient service or local cache. Choose a fail-safe default. Do not make application availability depend entirely on a remote flag lookup. Protect administrative access, audit changes, and treat high-impact flips like production changes.

Telemetry must include the evaluated variant without exposing personal data. Compare technical health and intended business outcome. Test both enabled and disabled branches, including after the flag configuration service is unavailable.

A kill switch is useful only if the flag check itself works during the incident and disabling the feature does not leave inconsistent data.

## Feature flags are not authorization

A UI-hidden feature may still be callable. Enforce security authorization independently. Likewise, never place secrets in flag values.

## Rollout sequence

1. Deploy compatible dormant code with default off.
2. Enable for the team/test accounts.
3. Validate telemetry and support behavior.
4. Expand to a representative opt-in/cohort.
5. Increase exposure with health gates.
6. Set the final default.
7. Remove old code and flag after the stability window.

If both implementations write data differently, design compatibility and cleanup before exposure.

## Interview preparation

**Flag versus branch?**  
A branch isolates source before integration; a flag lets integrated/deployed code control runtime exposure. Long-lived branches accumulate merge risk.

**Flag versus rollback?**  
A flag can disable one behavior quickly, but it cannot fix infrastructure, schema, memory, startup, or pervasive defects. Maintain deployment recovery.

**How prevent flag debt?**  
Ownership, expiry, inventory, usage telemetry, automated stale-flag checks, and removal in the definition of done.

## Practical exercise

Add a disabled release flag to a small feature. Expose it to an internal cohort, record variant telemetry, simulate flag-service failure, use the kill switch, then remove the old branch and flag through a cleanup change.

## Official references

- [Progressive experimentation with feature flags](https://learn.microsoft.com/devops/operate/progressive-experimentation-feature-flags)
- [Azure App Configuration feature management](https://learn.microsoft.com/azure/azure-app-configuration/feature-management-overview)

[Next: Backward-compatible Database Migrations →](05-backward-compatible-database-migrations.md)
