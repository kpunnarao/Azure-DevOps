# Artifacts, Logging, Timeouts, and Cancellation

> Chapter 5 — YAML Pipeline Fundamentals

[← Previous](07-expressions-conditions-dependencies-and-outputs.md) · [Chapter home](README.md)

## Purpose

Jobs are isolated and temporary. Artifacts move durable files; logs and published results provide evidence; timeouts and cancellation keep failure bounded.

## Pipeline artifacts

Publish files needed by later jobs or users:

```yaml
steps:
- publish: $(Build.ArtifactStagingDirectory)
  artifact: application
```

A dependent job can download the current run's artifact:

```yaml
steps:
- download: current
  artifact: application
```

Artifacts become available to following dependent jobs. Use explicit names and paths. Do not include source secrets, credentials, unneeded caches, or sensitive diagnostics.

Pipeline Artifacts are supported for Azure DevOps Services; Azure DevOps Server scenarios may require Build Artifacts tasks. Verify platform version.

## Logs and results

Logs should answer:

- What source and configuration ran?
- Which tool versions were used?
- What failed first?
- Which artifact was produced?
- Where are test and analysis results?

Publish structured test and coverage results rather than relying only on console text.

Use logging commands carefully. The agent interprets specially formatted stdout to set variables, upload data, create log issues, or change task outcome. Never compose logging commands from untrusted data without validation.

## Diagnostics

Enable system diagnostics only for troubleshooting and treat resulting logs as sensitive. Avoid shell tracing around secret commands. Record environment and tool versions without dumping all environment variables.

## Timeouts

Define bounded job timeouts for expected workloads. A timeout should be long enough for legitimate variation but short enough to release capacity and signal a hang.

Also consider:

- Task-specific timeout/retry behavior
- Hosted-agent maximums
- Cancellation timeout for cleanup
- External operation timeout
- Approval/check timeout in later deployment stages

A retry can help transient infrastructure operations but can hide deterministic test failures. Retry only operations that are safe and idempotent.

## Cancellation

Cancellation propagates through the dependency graph. A running process must handle termination and release resources. Cleanup should:

- Be idempotent
- Have minimal credentials
- Run only when necessary
- Respect a bounded cancellation window
- Avoid creating new deployments after cancellation
- Preserve useful diagnostics

An always condition does not make a step immortal.

## Retention

Artifact and log retention affects diagnosis, audit, and cost. Pin or retain important release runs according to policy. Do not rely on a default retention period without checking production evidence requirements.

## Common mistakes

- Sharing files through assumed agent persistence
- Publishing the entire workspace
- Logging every environment variable
- Encoding secrets in artifact names or metadata
- Infinite or excessive timeouts
- Retrying non-idempotent deployment operations
- Using always() to run dangerous actions after cancellation
- Deleting run history needed to investigate production
- Treating logs as harmless public data

## Interview preparation

**Q: How do jobs share files?**  
Publish a pipeline/build artifact in the producer and download it in a dependent consumer; do not assume the same agent workspace.

**Q: Logging command risk?**  
The agent interprets specially formatted output as control instructions. Protect secrets and do not allow untrusted content to forge commands.

**Q: Why define timeouts?**  
To bound hung work, protect capacity, and produce a clear failure. Pair with safe cancellation and diagnostics.

**Q: Can always() run after cancellation?**  
It can cause work to run under canceled status, but job cancellation timeout and termination still constrain it. Conditions must also avoid unsafe actions.

## Practical exercise

Publish a file in one job, download it in another, publish structured test results, set a warning via a logging command, force a timeout in a disposable job, and observe cleanup during cancellation.

## Further reading

- [Publish and download pipeline artifacts](https://learn.microsoft.com/azure/devops/pipelines/artifacts/pipeline-artifacts)
- [Logging commands](https://learn.microsoft.com/en-us/azure/devops/pipelines/scripts/logging-commands)
- [Jobs and timeouts](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/phases)
