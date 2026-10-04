# Least Privilege and Separation of Duties

[← Access Levels and Permissions](02-access-levels-groups-and-permissions.md) · [Chapter 14](README.md) · [Next: Workload Identity →](04-managed-identities-service-principals-and-federation.md)

## Limit capability and concentration

Least privilege limits identity, action, resource, environment, time, network, and conditions to what is necessary. Separation of duties prevents one ordinary identity from proposing, approving, executing, and concealing a high-impact change.

Apply both to humans, pipelines, agents, service connections, build-service tokens, extensions, webhooks, and external integrations.

## High-risk combinations

- Modify protected source + bypass review.
- Modify pipeline/template + use production connection.
- Administer service connection + approve own deployment.
- Write package/image + deploy to production.
- Administer audit settings + erase/export evidence.
- Manage group membership + grant self collection administration.
- Control self-hosted agent + access production secret.
- Create PAT + use unrestricted organization scope.

Not every small team can staff distinct people. Use protected branches, independent resource ownership, automated checks, just-in-time privilege, strong audit, and required peer approval to compensate.

## Time and context

Use Microsoft Entra PIM/JIT groups where appropriate for administrative roles. Make elevation time-limited, justified, approved, notified, and audited. Maintain monitored break-glass access for identity/control-plane outages; test it and rotate credentials.

Service identities should have one purpose and environment. A production deployment identity should not write source or publish CI artifacts; a CI identity should not administer production.

## Pipeline-specific controls

- Limit job authorization scope and repository access.
- Authorize protected resources to selected pipelines.
- Use required templates/checks outside YAML.
- Treat fork/PR code as untrusted.
- Use disposable isolated agents for secret-bearing jobs.
- Separate build and deployment identities.
- Pin external templates/tasks.
- Restrict who can edit/queue privileged pipelines.

## Interview preparation

**Does DevOps mean developers receive production admin?**  
No. DevOps integrates ownership and feedback; it does not eliminate risk-appropriate authorization or independent control.

**How implement separation with a small team?**  
Peer review, resource-owned checks, automated evidence, JIT privilege, distinct service identities, alerting, and audited break-glass.

**What is privilege creep?**  
Access accumulates through role changes, temporary exceptions, nested groups, and abandoned automation. Scheduled review/removal controls it.

## Practical exercise

Create an authority matrix for source, templates, pipelines, agents, artifacts, production connections, approvals, audit, and break-glass. Identify any unilateral production path and reduce it without adding a manual low-value handoff.

## Official references

- [Make Azure DevOps secure](https://learn.microsoft.com/azure/devops/organizations/security/security-overview)
- [Secure Azure Pipelines](https://learn.microsoft.com/azure/devops/pipelines/security/overview)
- [Pipeline resource security](https://learn.microsoft.com/azure/devops/pipelines/security/resources)

[Next: Managed Identities, Service Principals, and Federation →](04-managed-identities-service-principals-and-federation.md)
