# Drift, Idempotency, and Lifecycle

[← Plan and Approval](05-plan-what-if-and-approval.md) · [Chapter 11](README.md) · [Next: Policy and Protection →](07-policy-testing-and-destroy-protection.md)

## Drift

Drift is a meaningful difference between declared, recorded, and actual configuration. It can come from emergency portal changes, automatic platform behavior, another controller, provider normalization, policy remediation, or unmanaged resources.

Not every difference should be “fixed” automatically. First identify ownership and impact. An automatic reconciliation of an incident responder's temporary firewall rule could cause an outage; leaving it indefinitely creates security debt.

## Drift workflow

1. Detect through scheduled read-only plan/what-if, policy, or inventory.
2. Classify expected platform behavior, authorized emergency change, defect, or unauthorized change.
3. Assign owner and severity.
4. Reconcile by updating code, reverting the remote change, importing/adopting, or defining an explicit shared-ownership exception.
5. Re-preview and approve.
6. Preserve audit evidence and prevent recurrence.

Never give a drift job write authority merely because it detects differences.

## Idempotency tests

Apply to a disposable environment, validate behavior, then plan again. The second plan should show no unexplained change. Perpetual diffs often indicate unstable ordering, provider defaults, computed fields, nondeterministic names/timestamps, or two owners.

Use `ignore_changes` only for a deliberately externally managed attribute. It hides changes from Terraform action; it does not make drift safe. Document external owner and monitoring.

## Lifecycle decisions

Terraform lifecycle settings include `create_before_destroy`, `prevent_destroy`, `ignore_changes`, and replacement triggers. They change orchestration and can propagate through dependencies. They are guardrails, not recovery.

Bicep incremental deployments do not delete every resource omitted from a template. Complete-mode behavior has caveats and deployment stacks provide explicit managed-resource lifecycle choices such as detach/delete when resources become unmanaged. Evaluate current platform semantics before using removal as cleanup.

## Resource ownership

Every property should have one authoritative writer. If autoscaling controls replica count, IaC should intentionally delegate it. If Policy remediates a setting, encode compatible desired state. If an operator changes a setting during an incident, reconcile it afterward.

## Interview preparation

**What causes perpetual diff?**  
Provider/API normalization, unordered data, computed defaults, nondeterminism, externally managed properties, or conflicting reconcilers.

**When use ignore_changes?**  
Sparingly, when another named system legitimately owns an attribute and monitoring covers it—not to silence unexplained drift.

**Does idempotent mean safe?**  
No. A consistently destructive configuration may be idempotent after it deletes the resource. Safety also requires policy, review, validation, and recovery.

## Practical exercise

Modify a tag remotely, change an autoscaled field, and create one unmanaged resource. Run drift detection, classify each, and choose reconciliation. Add an unstable timestamp to configuration, observe perpetual change, then remove it.

## Official references

- [Terraform lifecycle](https://developer.hashicorp.com/terraform/language/meta-arguments/lifecycle)
- [Azure deployment modes](https://learn.microsoft.com/azure/azure-resource-manager/templates/deployment-modes)
- [Azure deployment stacks](https://learn.microsoft.com/azure/azure-resource-manager/bicep/deployment-stacks)

[Next: Policy, Testing, and Destroy Protection →](07-policy-testing-and-destroy-protection.md)
