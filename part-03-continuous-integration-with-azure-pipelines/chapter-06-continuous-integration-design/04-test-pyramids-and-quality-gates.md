# Test Pyramids and Quality Gates

[← PR Validation](03-pr-validation-and-main-branch-ci.md) · [Chapter 6](README.md) · [Next: CI Performance →](05-caching-parallelism-and-performance.md)

## Test strategy is risk strategy

CI testing should provide confidence quickly, not maximize test count. A useful portfolio usually contains many fast, isolated tests and fewer expensive, broad tests:

```text
             End-to-end
          Integration/contract
       Component/service tests
             Unit tests
```

The shape is a guide, not a law. A data pipeline, embedded product, or infrastructure repository may need a different mix. Choose layers based on failure cost, system boundaries, and feedback time.

## What each layer proves

- **Unit tests:** local behavior and edge cases with minimal external dependencies.
- **Component tests:** a deployable component with controlled collaborators.
- **Contract tests:** compatibility between producers and consumers.
- **Integration tests:** real interaction with databases, queues, APIs, or infrastructure.
- **End-to-end tests:** a small number of critical user journeys across the system.
- **Nonfunctional tests:** security, accessibility, performance, resilience, and compliance where risk requires them.

Publish machine-readable test results so Azure Pipelines can show failures, duration, history, and attachments. Use `condition: succeededOrFailed()` on result-publishing steps when appropriate; otherwise the most useful diagnostics may disappear after a failed command.

## Quality gates

A gate turns evidence into a decision. Good gates are objective, owned, and hard to game. Examples include:

- Zero failed required tests.
- No new critical static-analysis or dependency vulnerabilities.
- Successful contract compatibility checks.
- Coverage of changed critical code above an agreed threshold.
- Performance regression within a defined budget.

A gate should state scope, threshold, exception process, and owner. “Coverage must be 80%” without explaining why can incentivize low-value assertions. Gate on material risk and trends, not a vanity number.

## Flaky tests

A flaky required test makes the entire control unreliable. Track flake rate, quarantine only with an owner and expiry, collect diagnostics, and fix root causes such as shared state, timing assumptions, order dependence, external instability, or insufficient isolation. Blind retries can turn real intermittent defects into false success. If retry is used, report the initial failure and count it as reliability debt.

## Stage the feedback

A practical sequence is:

1. Static validation, linting, and unit tests.
2. Build/package verification.
3. Parallel component and integration suites.
4. Targeted security and policy checks.
5. Small end-to-end smoke suite.
6. Scheduled exhaustive, performance, and resilience suites if too slow for every commit.

A nightly run does not replace PR protection for risks that must never enter the main branch.

## Interview preparation

**What is the difference between a test and a gate?**  
A test produces evidence; a gate applies a policy to that evidence and decides whether work can proceed.

**Would you fail a build when coverage decreases?**  
It depends. I would prefer changed-code coverage and critical-path expectations, use a baseline for legacy code, and prevent meaningful regression without encouraging artificial tests.

**How do you manage flaky tests?**  
Make flakiness visible, assign ownership, gather evidence, fix isolation/timing issues, and time-limit any quarantine. Retries are diagnostic mitigation, not the solution.

## Practical exercise

Map five real failure scenarios to the cheapest test layer that can detect each. Implement unit and integration suites, publish both result files, intentionally fail one test, and confirm the summary remains available. Define three gates and document their exception process.

## Official references

- [Publish Test Results task](https://learn.microsoft.com/azure/devops/pipelines/tasks/reference/publish-test-results-v2)
- [Review test results](https://learn.microsoft.com/azure/devops/pipelines/test/review-continuous-test-results-after-build)
- [Configure pipeline conditions](https://learn.microsoft.com/azure/devops/pipelines/process/conditions)

[Next: Caching, Parallelism, and Performance →](05-caching-parallelism-and-performance.md)
