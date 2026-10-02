# Infrastructure and Environment Provisioning

[← CI and Artifacts](03-ci-quality-and-artifact-design.md) · [Chapter 17](README.md) · [Next: Deployment and Approvals →](05-multi-stage-deployment-and-approvals.md)

## Required infrastructure

Provision in a sandbox:

- Resource groups and metadata.
- Network/subnets/private DNS/egress as justified.
- ACR.
- AKS or alternative runtime defined by ADR.
- Key Vault and managed/workload identities.
- Database/storage.
- Log Analytics/Application Insights/Azure Monitor alerts.
- Optional App Configuration.
- Azure DevOps environments/service connections through approved bootstrap.

Use Bicep or Terraform modules with versioned interfaces and separate environment parameters. No secrets in parameters/state/output.

## Pipeline

Validate format/schema, module tests, security/policy/cost, then create what-if/saved plan. Use federated identity and environment-specific least privilege. Require approval for protected apply. Run functional post-deployment assertions.

For Terraform, remote state uses encryption, Entra authorization, locking, version/recovery, and lifecycle separation. For Bicep, retain deployment/what-if identity and understand deletion semantics.

## Environments

Create Development, Test, and Production Azure DevOps environments explicitly. Authorize selected pipelines. Model purpose, data, network, identity, config, checks, monitoring, reset, cost, and lifecycle. Production-like Test must represent topology/security behavior without copying sensitive data.

## Destructive safety

Protect registry/database/state/Key Vault and other critical resources using plan policy, lifecycle/locks where appropriate, narrow delete permissions, backup, and separate decommission workflow. Demonstrate an attempted destructive change being blocked.

## Drift

Make one safe out-of-band sandbox change. Detect through scheduled/read-only preview, classify ownership, reconcile through code, and preserve audit. Do not auto-apply drift correction with privileged identity.

## Failure demonstrations

- Wrong subscription/target scope check.
- Insufficient RBAC.
- Concurrent state lock.
- Policy rejection of public exposure.
- Replacement of stateful resource.
- Private-network DNS/reachability issue.
- Restore of a disposable state/data backup.

## Acceptance evidence

Module docs/tests, parameter matrix, preview/plan and approval, state/deployment records, identity/RBAC diagram, policy results, deployment outputs, post-validation, drift report, restore evidence, and cost inventory.

## Expert review questions

What is authoritative state? Can plan contain secrets? What can the IaC identity delete? How does Test differ from Production? How is backend/bootstrap recovered? Which resources cannot be recreated from code alone?

[Next: Multi-stage Deployment and Approvals →](05-multi-stage-deployment-and-approvals.md)
