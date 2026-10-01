# Caching, Parallelism, and Performance

[← Test Pyramids](04-test-pyramids-and-quality-gates.md) · [Chapter 6](README.md) · [Next: Coverage and Analysis →](06-code-coverage-and-static-analysis.md)

## Optimize the feedback loop

CI speed influences developer behavior, but optimization must preserve correctness. Start with measurement: queue time, agent initialization, dependency restore, compilation, tests, packaging, publishing, and the critical path across jobs. Optimize the largest repeatable bottleneck.

## Caching

Azure Pipelines `Cache@2` restores reusable files using a key and saves a cache after a successful job when required. A robust key reflects every input that makes cached data compatible.

```yaml
variables:
  npmCache: '$(Pipeline.Workspace)/.npm'

steps:
- task: Cache@2
  inputs:
    key: '"npm" | "$(Agent.OS)" | package-lock.json'
    restoreKeys: |
      "npm" | "$(Agent.OS)"
      "npm"
    path: '$(npmCache)'
- script: npm ci --cache "$(npmCache)"
```

Treat the example as a pattern: follow the package manager's supported cache directory and ensure restore still validates the lock file.

A cache is not an artifact. Caches accelerate work and may be replaced or missed; artifacts are outputs intentionally passed between jobs or retained. Never make correctness depend on a cache hit.

## Safe key design

Include the operating system, architecture when relevant, tool or format version, and dependency lock file. Use restore prefixes only when the package manager validates compatibility. Avoid caching secrets, credentials, signed outputs, or untrusted executable state that later privileged jobs will consume.

Cache poisoning is a security concern when less-trusted code can populate a key later restored by a privileged branch. Separate key namespaces and permissions across trust boundaries.

## Parallelism

Parallelize independent jobs such as test shards, platform builds, or static analysis. The achievable speedup is limited by the longest remaining dependency chain and available parallel-job capacity. More jobs can increase queueing, setup time, service throttling, and cost.

Use a matrix for a genuine support matrix—not to duplicate identical work. Shard tests using measured duration rather than test count. Publish separate results and merge reporting carefully so one shard cannot disappear without failing the gate.

## Additional techniques

- Front-load fast, high-signal checks.
- Avoid repeated repository checkouts and dependency restores.
- Share compiled outputs through artifacts where trust allows.
- Use incremental or affected-project builds only with a proven dependency graph.
- Cancel superseded PR validation.
- Keep agent images warm only when the operational cost is justified.
- Set timeouts so hung processes release capacity.

## Performance scorecard

Track median and tail duration, queue time, cache hit rate, time to first actionable failure, retry rate, flaky-test rate, and infrastructure failures. A lower average that creates worse tail latency or more false failures is not an improvement.

## Interview preparation

**What belongs in a cache key?**  
All compatibility-defining inputs: platform, architecture or tool version when relevant, and a content-derived dependency manifest or lock file.

**Why can parallelization make a pipeline slower?**  
Each job adds scheduling and initialization overhead, may contend for limited agents or external services, and can lengthen the critical path if work is imbalanced.

**Cache versus artifact?**  
A cache is an optional performance optimization; an artifact is a named output and part of the delivery data flow.

## Practical exercise

Measure a pipeline three times, add dependency caching, then measure three more times and record hit/miss behavior. Split tests into two duration-balanced shards. Compare total compute consumption, wall-clock time, and time to first failure. Disable the cache and prove correctness remains unchanged.

## Official references

- [Pipeline caching](https://learn.microsoft.com/azure/devops/pipelines/release/caching)
- [Configure parallel jobs](https://learn.microsoft.com/azure/devops/pipelines/licensing/concurrent-jobs)
- [Jobs and matrix strategies](https://learn.microsoft.com/azure/devops/pipelines/process/phases)

[Next: Code Coverage and Static Analysis →](06-code-coverage-and-static-analysis.md)
