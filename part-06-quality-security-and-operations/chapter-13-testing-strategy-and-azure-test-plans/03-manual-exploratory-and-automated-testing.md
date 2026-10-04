# Manual, Exploratory, and Automated Testing

[← Risk-based Testing](02-risk-based-testing.md) · [Chapter 13](README.md) · [Next: Test Plans Artifacts →](04-test-plans-suites-cases-and-runs.md)

## Complementary evidence

**Scripted manual testing** follows defined steps and expected results. It helps with regulated evidence, user acceptance, visual/physical workflows, and scenarios expensive to automate.

**Exploratory testing** is simultaneous learning, design, and execution guided by a time-boxed charter. It is not random clicking.

**Automated testing** executes repeatable checks quickly and consistently. It excels at regression, data combinations, concurrency, and frequent CI feedback.

Automate stable, valuable, repeatable behavior when lifecycle savings exceed maintenance. Keep human judgment for usability, novelty, ambiguity, accessibility nuance, and investigation.

## Exploratory charter

A useful charter defines:

- Mission and risk.
- Scope and exclusions.
- Personas/data/environment.
- Heuristics or tours.
- Time box.
- Evidence to capture.
- Stop/continue criteria.
- Debrief questions.

Azure Test & Feedback tools can capture notes, screenshots, recordings, and bugs connected to context. Protect sensitive data before attaching evidence.

## Automation design

Tests should have deterministic setup/cleanup, clear assertions, isolated ownership, useful diagnostics, and stable selectors/contracts. A passing test with no meaningful assertion is automation theatre.

Do not automate a broken process blindly. Simplify the product/testability first: APIs, dependency injection, stable identifiers, controllable clocks, data builders, and observable state.

## Manual case quality

Write intent and expected observable outcome, not fragile click-by-click detail unless regulated execution requires it. Parameterize meaningful data. Maintain reusable shared steps sparingly; excessive reuse can make cases hard to understand and change.

## Human and automated traceability

An Azure Test Plans test case can be associated with an automated test for requirement-level reporting. The automated test code remains versioned with application source; the work item expresses business traceability. Avoid mapping thousands of trivial unit tests individually.

## Interview preparation

**What should stay manual?**  
Work requiring human perception/judgment, rapidly changing behavior, one-off investigation, physical constraints, or low return on automation.

**What makes exploratory testing disciplined?**  
A risk-based charter, time box, heuristics, captured evidence, and debrief that updates cases/automation/product understanding.

**When automate a regression?**  
When it is important, repeatable, stable enough, frequent, and cheaper to maintain automatically than execute manually.

## Practical exercise

Take one feature and create a manual case, exploratory charter, and automated regression. Execute all three, compare defects/evidence, then decide which artifacts should survive the release.

## Official references

- [Exploratory testing and Test & Feedback](https://learn.microsoft.com/azure/devops/test/perform-exploratory-tests)
- [Automated testing with Azure Test Plans](https://learn.microsoft.com/azure/devops/test/automated-testing-overview)

[Next: Test Plans, Suites, Cases, and Runs →](04-test-plans-suites-cases-and-runs.md)
