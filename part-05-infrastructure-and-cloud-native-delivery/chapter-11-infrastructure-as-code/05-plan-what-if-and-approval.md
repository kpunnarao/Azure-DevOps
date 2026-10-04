# Plan, What-if, and Approval

[← Terraform State](04-terraform-state-locking-and-remote-backends.md) · [Chapter 11](README.md) · [Next: Drift and Lifecycle →](06-drift-idempotency-and-lifecycle.md)

## Preview proposed change

Terraform `plan` refreshes relevant state, evaluates configuration, and proposes actions. Bicep/ARM what-if predicts changes without applying them. Both improve review; neither is a transaction or guarantee. Remote state, input, policy, permissions, API behavior, or the real platform can change before apply.

Classify actions:

- Create.
- In-place update.
- Replacement (create/destroy ordering matters).
- Delete.
- Read/unknown until apply.
- No-op or output-only change.

Review identity, scope/subscription, provider/tool versions, module versions, inputs, and refresh errors—not just the resource count.

## Terraform saved plan

A strong workflow creates a binary plan, runs policy and approval against its human-readable representation, then applies that exact saved plan. Protect the plan as a sensitive artifact because it can contain configuration and cleartext sensitive values.

Before apply, confirm the saved plan belongs to the source revision, state lineage, target environment, tool/provider versions, and pipeline run. Expire it quickly. Backend locking during apply still matters.

## Bicep what-if

What-if supports deployment scopes and predicts changes through ARM, but documented limitations can produce noise or incomplete results, including some nested/template-link scenarios and provider defaults. Review false positives without blanket suppression. Use the appropriate validation level and combine preview with policy and post-deployment checks.

## Approval design

An infrastructure approval should summarize:

- Target and effective identity.
- New/changed/replaced/deleted critical resources.
- Security/network/public exposure.
- Cost and availability effect.
- Data/state implications.
- Policy exceptions.
- Recovery or restore plan.
- Diff/plan provenance and expiry.

The approver should not approve a plan that the pipeline later regenerates differently.

## Pull-request safety

PR validation may format, validate, test modules, and produce a read-only preview using a restricted identity. Do not apply untrusted PR code to shared infrastructure or expose privileged credentials/state. Forks require especially strict isolation.

## Interview preparation

**Does a clean plan guarantee apply success?**  
No. Conditions and APIs can change, unknown values resolve later, permissions may differ, and runtime validation can still fail.

**Why save Terraform plan?**  
It lets apply execute the exact reviewed proposal instead of recalculating after approval, subject to state and provider checks.

**What should block automatically?**  
Unexpected deletions/replacements, public exposure, privilege expansion, policy violations, wrong scope, missing evidence, and unreviewed exceptions.

## Practical exercise

Produce a plan/what-if for create, update, replacement, and delete. Annotate each risk. Save and apply the approved Terraform plan or bind Bicep deployment to the same source/parameters. Change remote state before apply and observe safety behavior.

## Official references

- [Terraform plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Bicep what-if](https://learn.microsoft.com/azure/azure-resource-manager/bicep/deploy-what-if)
- [Azure Pipelines approvals and checks](https://learn.microsoft.com/azure/devops/pipelines/process/approvals)

[Next: Drift, Idempotency, and Lifecycle →](06-drift-idempotency-and-lifecycle.md)
