# Cost, Continuity, Migration, and Developer Experience

[← APIs and Service Hooks](08-cli-rest-apis-and-service-hooks.md) · [Chapter 16](README.md)

## Optimize the whole system

Enterprise platform decisions trade cost, resilience, speed, security, and developer experience. Cutting warm agents may increase queue time and labor cost. Retaining everything increases storage/privacy risk. Adding approvals can reduce risk or create bypasses. Make tradeoffs visible.

## Cost model

Track licenses/access levels, parallel jobs, hosted minutes, custom compute/storage/network, artifact/feed/registry storage, security tools/extensions, test environments, telemetry ingestion/retention/query, and platform labor/support toil.

Allocate by team/product where useful, but preserve shared capacity and avoid incentives that hide testing/telemetry. Optimize cost per successful safe deployment, not infrastructure bill alone.

## Continuity

Define critical Azure DevOps capabilities and dependencies: Entra, source, work items, pipelines, agents, artifacts/packages, service connections/secrets, IaC state, templates, audit, GitOps sources, monitoring, and external extensions.

For each set RTO/RPO, authoritative source, export/backup or rebuild path, owners, communication, degraded manual process, and test frequency. Microsoft service resilience does not automatically restore customer-deleted individual assets; understand product recovery boundaries.

Keep external copies only under security, privacy, and licensing rules. Test restore, not just export.

## Migration

Inventory and classify before moving. Pilot a representative low-risk product. Map identities, permissions, links/IDs, process fields/states, Git history/LFS, pipelines/classic behavior, variables/secrets, service connections, Test Plans, artifacts/feeds, agents, hooks/extensions, dashboards/analytics, retention, and audit.

Use freeze/delta/cutover/validation/rollback plans. Some objects cannot move with full history or IDs; document accepted loss and preserve read-only legacy access where required.

## Developer experience

Measure onboarding time, time to first green build/deployment, feedback latency, failure diagnosability, documentation success, self-service completion, platform reliability, cognitive load, exceptions, and support toil. Pair telemetry with interviews and journey observation.

Good DX is not removal of every control; it makes safe intent easy, errors understandable, and exceptional risk explicit.

## Interview preparation

**How justify platform cost?**  
Connect total cost to lead time, reliability, incident/security risk, labor/toil, audit effort, and product delivery—not compute alone.

**What must a continuity plan include?**  
Dependencies, ownership, RTO/RPO, source/backup/rebuild, credentials, communication, degraded operation, restore validation, and exercises.

**How measure DX?**  
Task outcomes and time, reliability, support/toil, cognitive load, adoption/exception behavior, and qualitative feedback.

## Practical exercise

Build a monthly cost model and show two optimizations with latency/reliability guardrails. Run a tabletop Azure DevOps outage and a pilot migration. Measure a developer onboarding journey before/after one paved-road improvement.

## Official references

- [Azure DevOps pricing](https://azure.microsoft.com/pricing/details/devops/azure-devops-services/)
- [Azure DevOps data protection](https://learn.microsoft.com/azure/devops/organizations/security/data-protection)
- [Migrate Azure DevOps Server to Services](https://learn.microsoft.com/azure/devops/migrate/migration-overview)

## Chapter review

Present an enterprise platform strategy that links topology, ownership, paved roads, execution trust zones, capacity, lifecycle, extension/API governance, cost, continuity, migration, and DX. Defend both the standard path and the exception process.

[Chapter 17 — End-to-End Capstone Project →](../chapter-17-end-to-end-capstone-project/README.md)
