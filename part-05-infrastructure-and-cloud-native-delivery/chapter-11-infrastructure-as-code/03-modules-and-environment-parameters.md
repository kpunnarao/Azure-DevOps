# Modules and Environment Parameters

[← Bicep, ARM, and Terraform](02-bicep-arm-and-terraform.md) · [Chapter 11](README.md) · [Next: Terraform State →](04-terraform-state-locking-and-remote-backends.md)

## Modules are infrastructure APIs

A module encapsulates a cohesive resource capability behind inputs and outputs. Its public contract includes parameter types/defaults, naming, resources created, outputs, required permissions, lifecycle/replacement behavior, supported versions, and policy assumptions.

Good boundaries follow ownership and lifecycle: network foundation, registry, database, or application hosting unit. A module that creates an entire enterprise from dozens of booleans becomes hard to test and upgrade.

## Environment variation

Use one module implementation with small, reviewed environment parameter sets:

```text
modules/
  container-registry/
environments/
  dev/parameters
  production/parameters
```

Parameters express genuine variation: region, SKU, capacity, network IDs, allowed identities, retention, and availability settings. Do not copy entire modules per environment.

Never store secrets in parameter files or Terraform `.tfvars` committed to Git. Pass secret references or retrieve values using managed identity. Terraform state can still contain sensitive values even when an input is marked sensitive.

## Stable interface design

- Use descriptive typed inputs and safe defaults.
- Validate allowed patterns/ranges.
- Avoid exposing every low-level provider property.
- Output identifiers, endpoints, and principal IDs needed by consumers.
- Do not output credentials.
- Document replacement effects.
- Keep implicit resource creation minimal.
- Include examples and negative tests.
- Version releases and publish migration notes.

Bicep modules can be shared through private registries or template specs. Terraform modules can use a registry or versioned source reference. Pin consumers; a branch reference can change without consumer review.

## Composition

The root module composes capabilities and environment policy. Avoid deeply nested modules and circular dependencies. Pass explicit outputs to inputs instead of recomputing names or reading remote state broadly.

For cross-stack values, prefer stable platform discovery mechanisms or narrowly exposed outputs. Remote-state consumption can expose more data than intended and tightly couple lifecycles.

## Versioning

A change is breaking if it renames/removes inputs or outputs, changes defaults materially, replaces resources, changes names/identity, requires broader permissions, or alters ownership. Release a new major contract and give consumers a migration path.

## Interview preparation

**Module versus copy/paste?**  
A module provides one tested, versioned contract and upgrade path; copies drift and multiply fixes.

**What belongs in an environment file?**  
Only deliberate environmental variation—not resource logic, credentials, or an entire duplicated definition.

**Why avoid too many outputs?**  
They create coupling and may expose sensitive/internal implementation details that prevent module evolution.

## Practical exercise

Create a registry module with SKU, location, retention, and network inputs plus ID/login-server outputs. Instantiate Development and Production with different parameter files. Introduce a breaking naming change and write migration guidance without destroying the registry.

## Official references

- [Bicep modules](https://learn.microsoft.com/azure/azure-resource-manager/bicep/modules)
- [Bicep best practices](https://learn.microsoft.com/azure/azure-resource-manager/bicep/best-practices)
- [Terraform module development](https://developer.hashicorp.com/terraform/language/modules/develop)

[Next: Terraform State, Locking, and Remote Backends →](04-terraform-state-locking-and-remote-backends.md)
