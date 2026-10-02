# Policy, Testing, and Destroy Protection

[← Drift and Lifecycle](06-drift-idempotency-and-lifecycle.md) · [Chapter 11](README.md)

## Layered verification

IaC needs several test layers:

1. Formatting and syntax.
2. Static linting and provider/schema validation.
3. Module unit/contract or snapshot tests.
4. Security, compliance, and cost policy.
5. Plan/what-if assertions.
6. Disposable-environment integration tests.
7. Post-deployment functional and resilience checks.
8. Scheduled drift and policy-compliance checks.

A passing syntax check says nothing about public exposure or recoverability. A successful apply says nothing about workload health.

## Policy as code

Use Azure Policy or an approved plan-scanning framework to deny or audit prohibited regions/SKUs, public endpoints, missing encryption/logging/tags, excessive privilege, and noncompliant Kubernetes/registry settings. Keep policy versioned and tested with permitted and rejected examples.

A Terraform `check` block is useful for ongoing assertions, but current Terraform behavior reports failed checks as warnings rather than blocking the operation. Use variable validation, preconditions/postconditions, tests, and external policy gates where failure must block.

## Destructive-change protection

Use multiple layers:

- `prevent_destroy` for selected Terraform resources.
- Azure resource locks for critical shared/stateful resources.
- Protected environments and independent approval.
- Plan parsing/policy that blocks deletes/replacements.
- Deployment-stack deny settings where appropriate.
- Backup/restore and point-in-time recovery.
- Narrow identities: ordinary deployment may not need delete permission.
- A dedicated, audited decommission workflow.

`prevent_destroy` is not absolute: removing a resource from configuration can bypass the lifecycle rule's presence, and state operations can alter ownership. Azure locks can also block legitimate recovery/updates. Understand and test every guardrail.

## Safe decommissioning

Inventory dependencies and activity, notify owners, disable or detach before delete where possible, preserve state/data, observe a complete business cycle, remove consumers, obtain explicit approval, delete with a precise target, validate side effects, and retain evidence. Never run broad destroy from an unresolved environment variable or wildcard.

## Tests and credentials

PR tests should use no credentials or a restricted read-only sandbox identity. Integration applies belong in isolated disposable scopes with quotas and automatic cleanup. Ensure cleanup cannot target production by verifying subscription, tenant, resource group, tags, and pipeline environment.

## Interview preparation

**Why multiple layers of destroy protection?**  
Each can be bypassed or has gaps. Code lifecycle, platform lock, policy, identity, approval, backup, and procedure address different failure modes.

**Terraform check versus precondition?**  
A failed `check` is nonblocking warning behavior; preconditions/postconditions participate in resource validation and can block. Choose by enforcement need.

**How test IaC?**  
From cheap static tests through plan/policy and disposable apply, then post-deployment behavior and continuous drift checks.

## Practical exercise

Add a policy rejecting public storage, a module test, `prevent_destroy`, an Azure lock, and a protected delete workflow. Prove an unsafe plan fails. In a disposable scope, follow the approved decommission procedure and restore from backup.

## Official references

- [Terraform tests](https://developer.hashicorp.com/terraform/language/tests)
- [Terraform check blocks](https://developer.hashicorp.com/terraform/language/block/check)
- [Terraform lifecycle](https://developer.hashicorp.com/terraform/language/meta-arguments/lifecycle)
- [Lock Azure resources](https://learn.microsoft.com/azure/azure-resource-manager/management/lock-resources)

## Chapter review

Explain how source, module version, credentials, plan, state/deployment record, policy, approval, actual resources, and recovery evidence form one infrastructure trust chain. Then continue to packaging workloads as secure, immutable containers.

[Chapter 12 — Containers, ACR, and Kubernetes →](../chapter-12-containers-acr-and-kubernetes/README.md)
