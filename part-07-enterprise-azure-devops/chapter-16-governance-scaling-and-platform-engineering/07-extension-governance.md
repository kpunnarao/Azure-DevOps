# Extension Governance

[← Naming and Lifecycle](06-naming-tagging-retention-and-lifecycle.md) · [Chapter 16](README.md) · [Next: APIs and Service Hooks →](08-cli-rest-apis-and-service-hooks.md)

## Extensions are privileged software

Marketplace extensions and pipeline tasks may read/write source, work items, build data, tokens, service connections, or external systems. Updates can change code after initial approval. Treat installation as software supply-chain onboarding.

## Review checklist

- Business need and native alternative.
- Publisher identity, reputation, support, disclosure process.
- Source availability, build/provenance, licensing.
- Requested scopes/permissions and data flows.
- External endpoints, data residency, subprocesses.
- Secrets/token handling.
- Update channel and breaking-change history.
- Dependency/vulnerability posture.
- Availability, SLA, continuity/export.
- Cost and licensing.
- Uninstall/replacement plan.
- Sandbox test and owner.

Approve the minimum scope and user/project availability. Do not install organization-wide for one team's experiment when a smaller boundary exists.

## Pipeline tasks

Pin an approved major/task version, inventory every task and extension, and test updates in a canary pipeline. A major-version pin may still receive patch/minor implementation updates depending on marketplace/task distribution; know the actual supply model.

Prefer reviewed scripts/internal tasks for sensitive operations only when your team can secure and maintain them. Internal does not automatically mean safer.

## Lifecycle

Maintain an extension catalog with owner, purpose, scopes, publisher, version/update policy, projects, data classification, cost, review date, incidents, and alternatives. Monitor new permissions, dormant use, vulnerability/advisory, publisher change, and failed updates.

Removal requires export/migration, pipeline/dashboard/work-item dependency search, communication, rollback, uninstall, token revocation, and verification of external retained data.

## Incident response

Disable/restrict extension or affected pipelines, revoke tokens/credentials, preserve audit/logs, identify accessed data/resources, notify owners/vendor/security, remove persistence, restore trusted configuration, and review other extensions from the publisher/dependency.

## Interview preparation

**Why can an extension be riskier than a library?**  
It may execute with broad organization/product permissions and update centrally, affecting many teams and sensitive data.

**Auto-update or pin?**  
Balance security fixes with change control. Use vendor/version monitoring, sandbox/canary validation, and emergency process; understand what is actually pin-able.

**What must uninstall include?**  
Dependency migration, data export/deletion, token revocation, external integration cleanup, and evidence.

## Practical exercise

Threat-model one real Marketplace extension from permissions through data egress. Produce approve/reject/conditions decision, canary test, owner, review date, and exit plan.

## Official references

- [Install and manage extensions](https://learn.microsoft.com/azure/devops/marketplace/install-extension)
- [Develop Azure DevOps extensions](https://learn.microsoft.com/azure/devops/extend/overview)
- [Extension permissions/scopes](https://learn.microsoft.com/azure/devops/integrate/get-started/authentication/oauth)

[Next: CLI, REST APIs, and Service Hooks →](08-cli-rest-apis-and-service-hooks.md)
