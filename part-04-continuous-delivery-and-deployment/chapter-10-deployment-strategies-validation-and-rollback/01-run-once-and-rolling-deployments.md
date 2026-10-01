# Run-once and Rolling Deployments

[← Chapter 10](README.md) · [Next: Blue-green Deployment →](02-blue-green-deployment.md)

## Run-once

Azure Pipelines `runOnce` executes deployment lifecycle hooks once for the target. It is suitable when the platform handles replacement internally, the application is a single logical target, a maintenance window is acceptable, or another mechanism such as an App Service slot controls traffic.

```yaml
strategy:
  runOnce:
    preDeploy:
      steps:
      - script: ./validate-prerequisites.sh
    deploy:
      steps:
      - script: ./deploy.sh
    postRouteTraffic:
      steps:
      - script: ./smoke-test.sh
    on:
      failure:
        steps:
        - script: ./collect-diagnostics.sh
```

Run-once describes pipeline orchestration, not availability. Whether downtime occurs depends on the target platform and deployment command.

## Rolling deployment

A rolling rollout updates a subset of instances at a time, validates them, then proceeds. It reduces simultaneous exposure and can preserve capacity, but old and new versions coexist. APIs, messages, sessions, and database schemas must therefore remain compatible.

Azure Pipelines deployment-job rolling strategy targets supported VM environment resources. Verify current resource support before designing around it.

```yaml
strategy:
  rolling:
    maxParallel: 25%
    deploy:
      steps:
      - script: ./deploy-instance.sh
    postRouteTraffic:
      steps:
      - script: ./validate-batch.sh
```

`maxParallel` controls batch size as a number or percentage. A small batch reduces blast radius but lengthens deployment and may not provide enough traffic for a meaningful signal.

## Capacity and sequencing

Account for load-balancer draining, readiness, connection/session lifetime, replica quorum, autoscaling, and failure-domain distribution. Do not take an entire availability zone or quorum set out at once.

Each batch needs:

1. Remove/drain target capacity.
2. Deploy exact artifact.
3. Start and pass readiness.
4. Return traffic gradually.
5. Observe technical and business health.
6. Continue or stop.

## Failure behavior

Decide whether a failed batch stops, restores that batch, rolls back all updated instances, or triggers roll-forward. Azure Pipelines orchestration is not a substitute for platform-aware recovery. Design idempotent steps because retries may revisit partial state.

## Interview preparation

**When use run-once?**  
For a single logical target or when the platform itself provides safe replacement/slot semantics.

**Main rolling risk?**  
Version coexistence. Contracts, database changes, caches, and messages must support both versions while the rollout runs.

**How select batch size?**  
Balance blast radius, remaining capacity, signal volume, rollout duration, failure-domain topology, and recovery speed.

## Practical exercise

Deploy three disposable instances with batches of one. Add drain, readiness, and batch validation. Break the second instance and observe stop/recovery behavior. Repeat with an unsafe contract change and document why orchestration cannot fix incompatibility.

## Official references

- [Deployment jobs and strategies](https://learn.microsoft.com/azure/devops/pipelines/process/deployment-jobs)
- [jobs.deployment.strategy schema](https://learn.microsoft.com/azure/devops/pipelines/yaml-schema/jobs-deployment-strategy)

[Next: Blue-green Deployment →](02-blue-green-deployment.md)
