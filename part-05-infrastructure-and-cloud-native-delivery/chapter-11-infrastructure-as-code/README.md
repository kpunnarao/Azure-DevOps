# Chapter 11 — Infrastructure as Code

[← Part V — Infrastructure and Cloud-Native Delivery](../README.md)

Infrastructure as Code expresses cloud resources and policy as reviewable, testable definitions. This chapter develops the mental model required to use IaC safely: desired state, provider behavior, module contracts, remote state, previews, drift, lifecycle, and destructive-change controls.

## Why this chapter matters

IaC can reproduce infrastructure, but it can also reproduce mistakes at enormous scale. A green syntax check does not prove a safe plan; a clean plan does not guarantee a successful apply; and a successful apply does not prove a healthy system. Expert practice combines declarative definitions with identity, review, policy, runtime validation, and recovery.

## Topics

| # | Topic | Practical outcome |
|---:|---|---|
| 1 | [Declarative and Imperative Provisioning](01-declarative-and-imperative-provisioning.md) | Choose the correct control style |
| 2 | [Bicep, ARM, and Terraform](02-bicep-arm-and-terraform.md) | Select tooling using explicit criteria |
| 3 | [Modules and Environment Parameters](03-modules-and-environment-parameters.md) | Build reusable, versioned infrastructure contracts |
| 4 | [Terraform State, Locking, and Remote Backends](04-terraform-state-locking-and-remote-backends.md) | Protect infrastructure identity and concurrency |
| 5 | [Plan, What-if, and Approval](05-plan-what-if-and-approval.md) | Review proposed change safely |
| 6 | [Drift, Idempotency, and Lifecycle](06-drift-idempotency-and-lifecycle.md) | Reconcile change without hiding ownership |
| 7 | [Policy, Testing, and Destroy Protection](07-policy-testing-and-destroy-protection.md) | Establish layered guardrails |

## Guided chapter lab

Provision a sandbox network, registry, and identity using modules. Implement separate development and production parameter files without duplicating resource definitions.

Your pipeline must:

1. Format, lint, and validate.
2. Authenticate with workload identity federation.
3. Produce a Bicep what-if or saved Terraform plan.
4. Run policy/security/cost checks.
5. Require approval for the protected environment.
6. Apply exactly the reviewed input where the tool supports it.
7. Run post-deployment assertions.
8. Detect a controlled out-of-band change.
9. show and stop a destructive change.
10. Clean up the sandbox through a separately approved workflow.

Record tool and provider versions, module references, source commit, plan/what-if output, identity, target scope, deployment/state identity, and validation results.

## Troubleshooting exercise

Diagnose invalid credentials, insufficient Azure RBAC, provider version mismatch, stale Terraform lock, state drift, Bicep what-if noise, replacement of a stateful resource, and policy denial. For each, classify the cause as configuration, identity, state, provider/API, policy, concurrency, or actual platform drift.

## Completion criteria

You are ready for Chapter 12 when you can explain who owns current state, distinguish preview from guarantee, recover a failed/stale lock safely, design a stable module interface, and stop an unapproved destructive plan.

## Chapter navigation

[← Part IV](../../part-04-continuous-delivery-and-deployment/README.md) · [Chapter 12 →](../chapter-12-containers-acr-and-kubernetes/README.md)
