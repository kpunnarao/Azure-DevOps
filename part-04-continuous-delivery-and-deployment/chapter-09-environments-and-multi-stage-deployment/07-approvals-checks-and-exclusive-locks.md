# Approvals, Checks, and Exclusive Locks

[← Environment Configuration](06-environment-specific-configuration.md) · [Chapter 9](README.md) · [Next: Separation of Duties →](08-separation-of-duties.md)

## Controls owned by the resource

Stage conditions are written in YAML and controlled by pipeline authors. Approvals and checks are configured on protected resources by resource owners, so a YAML change cannot simply remove them. Supported resources include environments, service connections, repositories, variable groups, secure files, and agent pools.

Before a stage begins, all checks on every resource it consumes must succeed.

## Check order

Azure Pipelines evaluates categories in this order:

1. Static checks: branch control, required template, evaluate artifact.
2. Pre-check approvals.
3. Dynamic checks: approval, Azure Function, REST API, business hours, Azure Monitor alerts.
4. Post-check approvals.
5. Exclusive lock.

Checks within a category run in creation order and can be reevaluated on a configured interval. A rejection terminally fails the gate; a timeout prevents the stage and can later support retry according to platform behavior.

## Designing approvals

An approval should present artifact identity, source/change summary, automated evidence, risk, deployment plan, and recovery plan. Use groups with maintained membership, decide whether self-approval is permitted, set a meaningful timeout, and require comments for exceptions.

Deferred approvals can separate the human decision from the time it becomes effective. Business-hours checks can enforce a window without requiring an approver to wait online.

## Branch and dynamic checks

Branch control should use fully qualified names such as `refs/heads/main` and can require branch protection. Dynamic checks should be idempotent, authenticated, observable, and fast enough for their retry/timeout policy. Decide whether an unavailable external check fails closed.

Evaluate artifact currently has specific artifact support constraints; verify current documentation before relying on it as a universal policy engine.

## Exclusive locks

A lock prevents conflicting runs from using a protected resource simultaneously.

- `runLatest`: the latest run proceeds; older waiting runs are superseded. This is the default lock behavior.
- `sequential`: every run acquires the lock in order.

Use `runLatest` when only newest desired state matters. Use `sequential` when every change must deploy, such as ordered database or tenant operations. A lock prevents concurrency; it does not make a non-idempotent deployment safe.

## Interview preparation

**Why not put manual approval in YAML?**  
A repository author could modify or remove it. Resource-owned checks create an independent control plane.

**What if a stage uses three protected resources?**  
All applicable checks across all three must complete before the stage starts.

**When choose sequential lock?**  
When skipping an intermediate deployment would violate ordering or state-transition requirements. Otherwise, deploying only the newest desired state may be safer and faster.

## Practical exercise

Add branch control, pre-approval, business hours, and an exclusive lock to a sandbox environment. Queue two runs and test `runLatest`, then `sequential`. Document check ordering, timeout, bypass authority, and audit evidence.

## Official references

- [Define approvals and checks](https://learn.microsoft.com/azure/devops/pipelines/process/approvals)
- [Pipeline resource security](https://learn.microsoft.com/azure/devops/pipelines/security/resources)

[Next: Separation of Duties →](08-separation-of-duties.md)
