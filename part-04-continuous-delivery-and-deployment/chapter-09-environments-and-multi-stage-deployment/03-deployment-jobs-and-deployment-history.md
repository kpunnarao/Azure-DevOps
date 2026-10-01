# Deployment Jobs and Deployment History

[← Environment Strategy](02-environment-strategy.md) · [Chapter 9](README.md) · [Next: Service Connections →](04-service-connections.md)

## Deployment-aware execution

A deployment job is a specialized job that targets an environment and records deployment history. It supports deployment strategies and lifecycle hooks. Unlike an ordinary agent job, it is explicitly associated with the destination.

```yaml
jobs:
- deployment: DeployOrders
  displayName: Deploy Orders API
  environment: development
  pool:
    vmImage: ubuntu-latest
  strategy:
    runOnce:
      deploy:
        steps:
        - download: current
          artifact: application
        - script: ./deploy.sh
```

Deployment jobs do not automatically check out the source repository. This is desirable when deployment should use the downloaded immutable artifact. Add `checkout: self` only if the deployment genuinely needs reviewed repository files; consider packaging deployment logic or using a pinned template instead.

## Lifecycle hooks

Strategies can expose hooks such as `preDeploy`, `deploy`, `routeTraffic`, `postRouteTraffic`, and `on: success/failure`. Use them by purpose:

- Pre-deploy: validate prerequisites and take a recovery checkpoint.
- Deploy: apply the candidate.
- Route traffic: change exposure.
- Post-route: run health and smoke validation.
- Success/failure: finalize evidence or initiate controlled recovery.

A hook can run in an agent job or, where supported, a server job. Ensure cleanup happens under appropriate conditions and cannot hide the original failure.

## History and traceability

Environment history can show pipeline/run, status, commits, and work items associated with deployment. Resource-level history helps answer which version reached a specific target. This evidence depends on actually targeting the environment with a deployment job; merely naming a stage “Production” is not equivalent.

Add operational annotations such as artifact version, digest, deployment mode, feature-flag state, and change ticket through supported logs or external release records.

## Output variables

Deployment jobs can set output variables, but reference syntax varies by strategy and lifecycle hook. Keep cross-stage outputs small and non-secret; use artifacts or external state for larger durable data. Never make a rollback depend on an ephemeral output that is not retained.

## Troubleshooting

If history is missing, confirm the job type is `deployment`, the `environment` property resolves correctly, the pipeline is authorized, and the resource target matches. If the source directory is empty, remember there is no automatic checkout. If a hook runs on the wrong machine, inspect its pool/resource behavior.

## Interview preparation

**Why use a deployment job rather than a normal job?**  
It binds execution to an environment, records history, supports deployment strategies and lifecycle hooks, and enables resource checks.

**Does it clone the repository?**  
No, not automatically. Deployment should normally consume a published artifact.

**What should deployment history prove?**  
Which immutable version, produced by which run/commit, was deployed by which pipeline and identity to which target, when, with what result and controls.

## Practical exercise

Deploy one artifact using a normal job and then a deployment job. Compare environment history. Add pre-deploy validation, post-route smoke test, and failure cleanup. Reconstruct the deployment without relying on console memory.

## Official references

- [Deployment jobs](https://learn.microsoft.com/azure/devops/pipelines/process/deployment-jobs)
- [Azure Pipelines environments](https://learn.microsoft.com/azure/devops/pipelines/process/environments)

[Next: Service Connections →](04-service-connections.md)
