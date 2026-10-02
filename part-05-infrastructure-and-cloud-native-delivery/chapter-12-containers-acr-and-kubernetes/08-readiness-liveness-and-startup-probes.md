# Readiness, Liveness, and Startup Probes

[← Helm, Kustomize, and GitOps](07-helm-kustomize-and-gitops.md) · [Chapter 12](README.md)

## Three different decisions

- **Startup probe:** Has slow initialization completed? Until it succeeds, liveness and readiness do not run.
- **Readiness probe:** Should this Pod receive Service traffic now? Failure removes it from ready endpoints without restarting the container.
- **Liveness probe:** Is the container irrecoverably stuck and likely to recover through restart? Repeated failure restarts it according to policy.

Using one deep dependency endpoint for all three can cause cascading failure.

## Example

```yaml
startupProbe:
  httpGet:
    path: /health/startup
    port: 8080
  periodSeconds: 5
  failureThreshold: 30

readinessProbe:
  httpGet:
    path: /health/ready
    port: 8080
  periodSeconds: 5
  timeoutSeconds: 2
  failureThreshold: 3
  successThreshold: 2

livenessProbe:
  httpGet:
    path: /health/live
    port: 8080
  periodSeconds: 10
  timeoutSeconds: 2
  failureThreshold: 3
```

Tune from measured startup and response distributions. Defaults and values above are illustrative.

## Probe design

Liveness should be cheap and local: process/event-loop progress or unrecoverable deadlock. If the database goes down, restarting every application Pod usually increases load and removes capacity.

Readiness can include dependencies necessary to serve correctly, but use caution: one shared outage could mark the whole fleet unready. Consider degraded modes, circuit breakers, and retaining capacity for diagnostic/error responses.

Startup protects legitimately slow initialization from premature liveness restarts. It should still fail eventually when startup is genuinely stuck.

HTTP, TCP, exec, and gRPC probes have different semantics and overhead. Exec probes create processes and may be expensive at high density. Secure endpoints and avoid sensitive diagnostic details.

## Rollout interaction

A Deployment counts available replicas based on readiness. Combine probes with `minReadySeconds`, rollout surge/unavailable settings, graceful SIGTERM handling, `preStop` only when justified, and sufficient termination grace. Readiness does not replace external synthetic/business validation.

## Troubleshooting

Inspect Pod conditions and events, restart count/reason, prior container logs, probe URL/port/scheme, bind address, timeouts, CPU throttling, OOM, startup duration, network policy, and application logs. `Running` with `Ready=False` usually points to readiness or required conditions.

## Common mistakes

- Liveness checks every downstream dependency.
- Probe timeout shorter than normal tail latency.
- No startup probe for a slow application.
- Readiness succeeds before cache/migration/listener is ready.
- All replicas restart simultaneously.
- A probe mutates state or requires expensive authentication.
- Treating readiness as end-to-end customer validation.

## Interview preparation

**What happens when readiness fails?**  
The container continues running, but the Pod becomes not ready and is removed from matching Service endpoints.

**What happens when liveness fails repeatedly?**  
Kubelet restarts the container according to its restart policy.

**Why add startup probe?**  
It gives slow initialization its own budget and delays liveness/readiness so normal liveness can remain responsive.

## Practical exercise

Implement separate endpoints. Make startup slow, dependency unavailable, and event loop deadlocked in turn. Confirm startup prevents premature restart, readiness removes traffic without restart, and liveness restarts only the deadlocked process. Observe EndpointSlices and events.

## Official references

- [Kubernetes probes](https://kubernetes.io/docs/concepts/workloads/pods/probes/)
- [Configure liveness, readiness, and startup probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)
- [AKS application best practices](https://learn.microsoft.com/azure/aks/developer-best-practices-resource-management)

## Part V review

Trace this complete chain:

```text
reviewed IaC → safe preview/apply → governed ACR/AKS
     → minimal scanned image → immutable digest
     → rendered desired state → GitOps reconciliation
     → scheduling/pull/startup/readiness → customer health
```

Return to the [Part V overview](../README.md), complete the Part project and cleanup review, and retain the architecture decisions, runbooks, evidence, and teaching notes.
