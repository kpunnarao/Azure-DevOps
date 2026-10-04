# Bicep, ARM, and Terraform

[← Declarative Provisioning](01-declarative-and-imperative-provisioning.md) · [Chapter 11](README.md) · [Next: Modules and Parameters →](03-modules-and-environment-parameters.md)

## Tool models

| Tool | Strength | State model |
|---|---|---|
| ARM JSON | Native Azure deployment format and broad API coverage | Azure deployment/control-plane state |
| Bicep | Concise Azure-native language compiled to ARM JSON | Azure Resource Manager |
| Terraform | Multi-provider graph, ecosystem, reusable modules | Explicit Terraform state plus provider refresh |

Bicep is not an execution service; it transpiles to ARM JSON and Azure Resource Manager deploys it. Terraform providers translate its resource graph into API operations and maintain state that maps configuration addresses to remote objects.

## Choose by operating context

Favor Bicep when the scope is primarily Azure, immediate Azure API support and native deployment semantics matter, and the organization prefers no separate Terraform state.

Favor Terraform when one workflow coordinates multiple providers, the team already operates Terraform state/module governance, or ecosystem modules/providers deliver material value.

Use ARM JSON directly when required by an integration or when diagnosing/generated output, but prefer Bicep for hand-authored Azure templates.

The wrong answer is selecting solely by syntax. Evaluate skills, support model, API lag, testing, state operations, module registry, policy, licensing, identity, recovery, and existing estate.

## Do not dual-own

Never have Bicep and Terraform manage the same resource/property without an explicit ownership design. Each reconciler may undo the other. Migration requires inventory, freeze, import/adoption, equivalence validation, and removal of the old owner—not simply deploying both.

## Version control

Pin Terraform CLI and provider constraints; commit the dependency lock file. Version Bicep CLI/tooling in CI. Pin reusable modules to immutable releases or digests where supported. Review upgrades separately because provider or API behavior can change even when your configuration does not.

## Secrets and outputs

Mark sensitive Terraform outputs, but remember the values can still exist in state. Bicep secure parameters protect deployment input handling but do not make it acceptable to output secrets. Prefer resource identities and secret references over passing credential values through IaC.

## Interview preparation

**Bicep versus Terraform?**  
Bicep is Azure-native and uses ARM deployment semantics without separate state; Terraform is provider-based, multi-platform, and relies on protected state. Choose from scope and operating capability.

**Why keep ARM knowledge when using Bicep?**  
Bicep compiles to ARM, uses ARM scopes/resource providers/deployments, and errors often expose ARM concepts.

**Can a team use both?**  
Yes, across clearly separated ownership boundaries. Do not let them reconcile the same properties.

## Practical exercise

Describe the same small Azure resource in Bicep and Terraform. Compare preview, apply, state/deployment records, import/existing-resource behavior, and cleanup. Write an architecture decision record choosing one for a realistic team.

## Official references

- [Bicep overview](https://learn.microsoft.com/azure/azure-resource-manager/bicep/overview)
- [ARM templates overview](https://learn.microsoft.com/azure/azure-resource-manager/templates/overview)
- [Terraform on Azure overview](https://learn.microsoft.com/azure/developer/terraform/overview)

[Next: Modules and Environment Parameters →](03-modules-and-environment-parameters.md)
