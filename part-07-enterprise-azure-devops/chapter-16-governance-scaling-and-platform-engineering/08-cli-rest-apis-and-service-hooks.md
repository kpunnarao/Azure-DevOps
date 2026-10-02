# CLI, REST APIs, and Service Hooks

[← Extension Governance](07-extension-governance.md) · [Chapter 16](README.md) · [Next: Enterprise Tradeoffs →](09-cost-continuity-migration-and-developer-experience.md)

## Automation principles

Use CLI for operator workflows, REST/SDK for repeatable integrations and inventory, and service hooks/webhooks for event-driven notification. Automation needs application identity, least privilege, idempotency, pagination, retries, rate-limit handling, schema/version control, audit, and reconciliation.

Prefer Microsoft Entra application identity over PAT for Azure DevOps Services automation. Never put credentials in command arguments, source, or webhook URLs.

## Safe API client

- Pin/document API version.
- Set bounded timeouts.
- Retry only transient/idempotent operations with exponential backoff and jitter.
- Honor throttling headers.
- Handle pagination/continuation tokens.
- Use request/correlation and idempotency keys where supported.
- Validate response and desired postcondition.
- Redact logs.
- Protect destructive operations with exact target, preview, approval, and concurrency control.
- Record caller, intent, before/after, and result.

A successful HTTP status may represent queued asynchronous work; poll the correct operation state.

## Reconciliation over scripts

For governance, periodically inventory actual state and compare to declared policy: projects, repos, branch policies, administrators, pools, pipelines, service connections, variable groups, extensions, hooks, PAT policy, and retention. Generate a proposed change report before enforcement.

Avoid automation that resets team-specific settings it does not own. Declare ownership and exceptions.

## Service hooks

Design consumers for duplicate, delayed, out-of-order, and missing delivery. Authenticate/sign/validate events when supported, restrict endpoints, queue quickly, process asynchronously, deduplicate by event ID, store checkpoint, retry safely, dead-letter failures, monitor subscription/consumer health, and provide replay/reconciliation.

Never rely solely on a webhook for compliance evidence; periodic reconciliation detects missed events.

## CLI cautions

Set organization/project context explicitly in automation, avoid interactive defaults, quote values safely, use JSON output for parsing, check exit/status, and verify target tenant/org before writes. Human-readable output is not a stable API.

## Interview preparation

**Webhook versus polling?**  
Webhook reduces latency/load; polling/reconciliation detects missing events and current truth. Critical integrations often use both.

**How handle 429/5xx?**  
Respect retry guidance, bounded exponential backoff/jitter, idempotency, circuit breaking, and alert after the budget is exhausted.

**Why not PAT?**  
It is user-bound long-lived bearer material. Application identities provide clearer lifecycle and short-lived tokens where supported.

## Practical exercise

Build a read-only inventory client with pagination and Entra identity. Add dry-run governance diff. Create a sandbox service hook consumer that deduplicates and dead-letters, then simulate duplicates/out-of-order/unavailability.

## Official references

- [Azure DevOps REST API reference](https://learn.microsoft.com/rest/api/azure/devops/)
- [Azure DevOps CLI](https://learn.microsoft.com/azure/devops/cli/)
- [Service hooks](https://learn.microsoft.com/azure/devops/service-hooks/overview)

[Next: Cost, Continuity, Migration, and Developer Experience →](09-cost-continuity-migration-and-developer-experience.md)
