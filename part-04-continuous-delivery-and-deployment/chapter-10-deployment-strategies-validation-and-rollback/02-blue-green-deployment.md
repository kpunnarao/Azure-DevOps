# Blue-green Deployment

[← Run-once and Rolling](01-run-once-and-rolling-deployments.md) · [Chapter 10](README.md) · [Next: Canary Deployment →](03-canary-deployment.md)

## Parallel production environments

Blue-green deployment maintains two equivalent production-capable environments:

- **Blue:** currently serving traffic.
- **Green:** receives and validates the new release.

After green passes validation, a router, load balancer, DNS layer, gateway, or platform slot sends traffic to green. Blue remains available for rapid traffic reversal until the observation window closes.

Colors are roles, not permanent environment names. After a successful switch, green is live and blue becomes the next candidate.

## Workflow

1. Confirm blue health and current artifact identity.
2. Prepare green infrastructure and deploy the exact candidate.
3. Apply compatible configuration and migrations.
4. Run internal smoke, security, and data-path validation.
5. Warm caches and capacity safely.
6. Shift test traffic, then production traffic.
7. Monitor technical and business signals.
8. Keep blue intact for the defined recovery window.
9. Retire or recycle blue only after confidence and retention criteria.

## Advantages and costs

Blue-green provides clear isolation, production-like validation, and fast traffic rollback. It requires duplicate capacity, reliable routing, synchronized configuration, and careful handling of shared state.

Traffic reversal is not complete rollback if green wrote irreversible data, emitted messages, changed external systems, or applied destructive schema migration. State compatibility determines reversibility.

## Routing concerns

DNS changes can be slow because of caching. Load balancer or slot swaps are usually more predictable, but long-lived connections, WebSockets, sessions, background workers, and queued consumers still need drain/transition logic.

Prevent both colors from performing singleton jobs simultaneously. Elect one scheduler or make background processing idempotent.

## Validation

Validate from outside the routing boundary as well as inside green. Check readiness, dependencies, representative transactions, telemetry ingestion, certificates, authorization, and customer-visible behavior. Compare blue and green metrics using the same traffic context where feasible.

## Common mistakes

- Calling a replacement-in-place deployment blue-green.
- Rebuilding for green.
- Treating a traffic switch as proof that state can roll back.
- Forgetting workers and scheduled jobs.
- Destroying blue immediately.
- Allowing configuration drift between colors.
- Using DNS without accounting for client caching.

## Interview preparation

**Blue-green versus rolling?**  
Blue-green switches between parallel environments; rolling replaces subsets in place. Blue-green offers a clearer traffic reversal at higher capacity cost.

**What makes rollback unsafe?**  
Non-backward-compatible database changes, irreversible side effects, messages consumed by old code, or external operations performed after cutover.

**How long retain blue?**  
Long enough to observe the risks that matter, but not so long that state/config drift makes reversal unsafe. Define this using workload behavior and cost.

## Practical exercise

Deploy two slots or namespaces, route traffic to blue, validate green, then switch. Generate long-lived traffic and a background job. Reverse traffic and document which application/data effects did not reverse automatically.

## Official references

- [Continuous-delivery progressive exposure](https://learn.microsoft.com/devops/deliver/what-is-continuous-delivery)
- [Deployment considerations for multitenant solutions](https://learn.microsoft.com/azure/architecture/guide/multitenant/considerations/updates)

[Next: Canary Deployment →](03-canary-deployment.md)
