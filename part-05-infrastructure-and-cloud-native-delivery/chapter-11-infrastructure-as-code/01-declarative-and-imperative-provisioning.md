# Declarative and Imperative Provisioning

[← Chapter 11](README.md) · [Next: Bicep, ARM, and Terraform →](02-bicep-arm-and-terraform.md)

## Two control styles

**Declarative** configuration describes the desired end state. The engine compares desired and observed state and decides which operations reconcile them. Bicep, ARM templates, Terraform configuration, and Kubernetes manifests are declarative.

**Imperative** automation describes commands in order: create a group, add a network, configure a rule. Azure CLI and PowerShell scripts are commonly imperative, though they can query and conditionally converge.

Declarative does not mean “no ordering” or “automatically safe.” Dependencies, lifecycle rules, provider/API behavior, and replacement semantics still matter.

## Selection guide

Use declarative IaC for long-lived managed resources where repeatability, review, drift detection, and lifecycle ownership matter. Use imperative commands for investigation, orchestration between systems, one-time data movement, or operations poorly represented by the declarative provider.

A strong workflow often combines them:

```text
imperative pipeline orchestration
   → declarative validate/preview/apply
   → imperative functional verification
```

Keep the boundary explicit. A script that creates resources outside the declared model may become hidden state.

## Idempotency

An idempotent operation can be repeated without unintended additional change once the desired state is reached. Declarative tools aim for convergence, but user scripts, nondeterministic values, unstable provider fields, and external actors can break it.

Examples:

- Good: deterministic name derived from stable scope.
- Risky: current timestamp embedded in a resource name.
- Good: update a firewall rule to an exact set.
- Risky: append the same rule on every run.

## Desired, recorded, and actual state

Terraform compares configuration, state, and provider-read remote objects. Azure Resource Manager evaluates submitted deployment definitions against Azure. Neither model eliminates platform defaults or out-of-band changes. Understand what the tool records, refreshes, ignores, or deletes.

## Common mistakes

- Replacing a declarative resource with CLI creation because apply failed.
- Assuming repeated script success means idempotency.
- Generating random names on every plan.
- Letting two tools own the same property.
- Treating deployment success as application health.
- Importing existing resources without documenting ownership.

## Interview preparation

**Is Terraform imperative because it creates an ordered plan?**  
No. Configuration declares desired relationships; Terraform creates an operational graph to reconcile them.

**When is imperative automation appropriate?**  
For orchestration, diagnostics, one-time operations, or unsupported actions—provided it is observable, retry-safe, and does not create unmanaged infrastructure.

**What is convergence?**  
Repeated reconciliation moves actual state toward declared state until no meaningful change remains.

## Practical exercise

Create one resource with an ad hoc CLI command and another declaratively. Run each twice, change a property outside the tool, and compare visibility and correction. Convert the script-created resource into declared ownership using an import/adoption process.

## Official references

- [Azure IaC overview](https://learn.microsoft.com/devops/deliver/what-is-infrastructure-as-code)
- [Bicep overview](https://learn.microsoft.com/azure/azure-resource-manager/bicep/overview)
- [Terraform configuration language](https://developer.hashicorp.com/terraform/language)

[Next: Bicep, ARM, and Terraform →](02-bicep-arm-and-terraform.md)
