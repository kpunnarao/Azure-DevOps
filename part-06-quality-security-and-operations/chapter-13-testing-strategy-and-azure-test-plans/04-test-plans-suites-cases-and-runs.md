# Test Plans, Suites, Cases, and Runs

[← Manual and Exploratory Testing](03-manual-exploratory-and-automated-testing.md) · [Chapter 13](README.md) · [Next: Traceability →](05-requirements-to-test-traceability.md)

## Azure Test Plans model

- **Test plan:** testing scope for a sprint, milestone, or release.
- **Test suite:** grouping/query of test cases within a plan.
- **Test case:** reusable work item containing steps, expected results, parameters, links, and automation association.
- **Test point:** test case × configuration × tester assignment in a suite.
- **Test run/result:** an execution and its outcome/evidence.
- **Configuration:** test matrix dimension such as browser, OS, device, or edition.

A plan references cases. Updating a shared case affects every plan/suite referencing it. Copy/clone only when an independent baseline is genuinely required.

## Suite types

- **Static suite:** curated group for regression, exploratory follow-up, or release scope.
- **Requirement-based suite:** links cases to a requirement work item and supports traceability.
- **Query-based suite:** dynamically includes cases matching a work-item query.

Choose by intent. A static suite gives explicit curation; a query suite reduces manual membership but depends on metadata quality.

## Case design

Give cases business-readable titles, clear preconditions, concise actions, observable expected outcomes, suitable priority/risk/area, parameters, configuration, and links. Separate independent scenarios so one failure is diagnostic. Avoid steps dependent on undocumented prior cases.

Use shared steps only for genuinely stable, common sequences; changing shared steps can affect many cases.

## Runs and evidence

Record actual outcome, failure step, environment/configuration, build/version, attachments, bug link, and comments. A blocked test is not passed; state why it could not execute and who owns resolution.

Test Plan licensing/access matters: Stakeholder users cannot access Test Plans; full management features require Basic + Test Plans or eligible Visual Studio subscriptions, while capability details vary. Verify current entitlement before designing process around casual participants.

## Maintenance

Review duplicate/obsolete cases, stale requirements, unused configurations, ownership, automation associations, and historic evidence retention each release. Do not delete evidence required for audit without policy.

## Interview preparation

**Case versus test point?**  
A case defines the scenario; a test point is that case under a specific configuration/tester in a suite.

**Why does changing a case affect several plans?**  
Plans/suites reference the shared test-case work item rather than copying it.

**Static versus requirement suite?**  
Static is manually curated; requirement-based groups cases under a requirement and strengthens traceability.

## Practical exercise

Create one plan with static, requirement-based, and query suites. Add parameterized cases and two configurations, assign test points, execute a run, file a bug with evidence, then update a shared case and inspect impact.

## Official references

- [Create and manage test plans](https://learn.microsoft.com/azure/devops/test/create-a-test-plan)
- [Create and manage test suites](https://learn.microsoft.com/azure/devops/test/create-a-test-suite)
- [Create manual test cases](https://learn.microsoft.com/azure/devops/test/create-test-cases)

[Next: Requirements-to-test Traceability →](05-requirements-to-test-traceability.md)
