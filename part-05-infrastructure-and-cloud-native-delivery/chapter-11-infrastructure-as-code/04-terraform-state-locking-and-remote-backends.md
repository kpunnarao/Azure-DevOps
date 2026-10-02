# Terraform State, Locking, and Remote Backends

[← Modules and Parameters](03-modules-and-environment-parameters.md) · [Chapter 11](README.md) · [Next: Plan and Approval →](05-plan-what-if-and-approval.md)

## Why state exists

Terraform state maps configuration addresses to remote object identities and stores attributes used for planning. It is operationally critical and may contain secrets or sensitive infrastructure data. Losing, corrupting, or exposing it can prevent safe change or disclose credentials.

Local state is unsuitable for shared production automation. Use an approved remote backend with encryption, access control, audit logs, versioning/recovery, and locking support.

## State isolation

Separate state by lifecycle and blast radius—not merely by convenience. A network foundation, shared cluster, and application resources often deserve different states, identities, approvers, and deployment frequency. One enormous state makes every plan slower and grants excessive authority; one state per resource creates dependency sprawl.

Do not use CLI workspaces as the only production isolation when stronger separation of backend, identity, and permissions is required.

## Locking

Terraform automatically locks state for operations that can write when the backend supports locking. A failed lock should stop the operation.

Never use `-lock=false` to bypass ordinary contention. `force-unlock` is for a verified stale lock from your own failed operation. Confirm no apply is running, identify the owner/run and lock ID, preserve evidence, then unlock under an approved procedure.

The AzureRM backend can store state in Azure Blob Storage and supports locking/consistency mechanisms. Prefer Microsoft Entra authentication/workload identity over storage keys where supported, and limit data-plane permissions to the required state container/blob scope.

## Backend bootstrap

The backend itself must exist before Terraform can use it. Provision it through a separately controlled bootstrap process. Protect storage from accidental deletion, enable appropriate versioning/soft-delete/recovery features, restrict network access, and test restoration.

Backend configuration must not embed credentials. Initialize CI noninteractively and pin Terraform/provider versions.

## State operations

Import, move, remove, replace, and state repair are high-risk operations. Back up state, stop concurrent pipelines, use reviewed commands/configuration (such as `moved` blocks where appropriate), inspect the plan afterward, and document ownership changes. Never hand-edit JSON state casually.

## Interview preparation

**Why is state sensitive?**  
It contains resource identities and attributes and can include plaintext secret values even when CLI output marks them sensitive.

**Why does locking matter?**  
Two writers could plan from the same old snapshot and overwrite each other's mappings or changes.

**When force-unlock?**  
Only after proving the lock is stale and no writer is active, using the exact lock ID under an audited recovery process.

## Practical exercise

Configure an isolated remote Azure backend. Start two concurrent plans/applies to observe locking. Simulate an interrupted run, inspect lock ownership, and practice the documented stale-lock decision without bypassing a live writer. Restore a prior state version in a disposable environment.

## Official references

- [Terraform state](https://developer.hashicorp.com/terraform/language/state)
- [Terraform state locking](https://developer.hashicorp.com/terraform/language/state/locking)
- [AzureRM backend](https://developer.hashicorp.com/terraform/language/backend/azurerm)

[Next: Plan, What-if, and Approval →](05-plan-what-if-and-approval.md)
