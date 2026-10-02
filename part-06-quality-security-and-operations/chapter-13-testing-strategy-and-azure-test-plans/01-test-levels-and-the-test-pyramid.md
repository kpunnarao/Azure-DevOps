# Test Levels and the Test Pyramid

[← Chapter 13](README.md) · [Next: Risk-based Testing →](02-risk-based-testing.md)

## Place feedback deliberately

A test portfolio usually needs many fast isolated tests, fewer boundary tests, and a small number of expensive end-to-end journeys:

```text
                End-to-end
        Integration / contract
          Component / service
                 Unit
```

The pyramid is an economic model, not a quota. The cheapest level that can reliably expose the risk should own it.

## Levels

- **Unit:** one function/class/module with controlled collaborators; fast and diagnostic.
- **Component/service:** deployable unit through public behavior with dependencies controlled.
- **Contract:** producer/consumer compatibility without full end-to-end setup.
- **Integration:** real interaction with database, queue, identity, network, or external API.
- **End-to-end:** complete critical journey across deployed system.
- **Nonfunctional:** performance, resilience, security, accessibility, usability, recovery, and compliance.

Static type/lint/security analysis is valuable but not an execution-test level.

## Why top-heavy suites fail

End-to-end tests are broad but slow, costly, environmentally sensitive, and hard to diagnose. They should prove a few critical journeys and integration assumptions, not every validation rule. Push business edge cases downward; retain higher tests for wiring and behavior impossible to prove in isolation.

A microservice portfolio may need strong contract tests; an analytics system may emphasize data-quality/integration tests; safety-critical logic may need unusually exhaustive unit and formal analysis. Architecture changes the shape.

## Quality attributes

Functional correctness alone is incomplete. For each workload identify response time, throughput, availability, recoverability, security, accessibility, compatibility, and data integrity risks. Test them at suitable stages and environments.

## Common mistakes

- Counting tests rather than measuring risk covered.
- Mocking so heavily that the test proves the mock.
- Duplicating identical assertions at every level.
- Running all expensive tests on every edit.
- Replacing integration testing with unit coverage.
- Treating production monitoring as a substitute for pre-release tests.
- Making the pyramid an enforced percentage.

## Interview preparation

**Why not automate everything end to end?**  
Cost, speed, flakiness, diagnosability, and limited scenario coverage. Push precise behavior lower and keep representative journeys at the top.

**Unit versus integration boundary?**  
A unit test controls external collaborators; an integration test validates a real boundary such as database, filesystem, network, or service.

**Where test resilience?**  
At component/integration and production-like system levels with controlled fault injection and clear safety boundaries.

## Practical exercise

List ten failure modes for one feature. Assign each to the cheapest reliable level, justify any duplication, estimate execution time and diagnostic value, then implement one test at four levels and compare feedback.

## Official references

- [Azure Test Plans overview](https://learn.microsoft.com/azure/devops/test/overview)
- [Review continuous test results](https://learn.microsoft.com/azure/devops/pipelines/test/review-continuous-test-results-after-build)

[Next: Risk-based Testing →](02-risk-based-testing.md)
