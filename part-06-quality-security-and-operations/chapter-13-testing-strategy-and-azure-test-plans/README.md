# Chapter 13 — Testing Strategy and Azure Test Plans

[← Part VI — Quality, Security, and Operations](../README.md)

This chapter treats testing as evidence about product risk. It explains which test level should detect each failure, where human exploration adds value, how Azure Test Plans manages reusable test assets, and how data, environments, flakiness, and coverage affect trust.

## Topics

| # | Topic | Practical outcome |
|---:|---|---|
| 1 | [Test Levels and the Test Pyramid](01-test-levels-and-the-test-pyramid.md) | Place feedback at the cheapest reliable level |
| 2 | [Risk-based Testing](02-risk-based-testing.md) | Allocate effort by likelihood and impact |
| 3 | [Manual, Exploratory, and Automated Testing](03-manual-exploratory-and-automated-testing.md) | Combine complementary evidence |
| 4 | [Test Plans, Suites, Cases, and Runs](04-test-plans-suites-cases-and-runs.md) | Operate Azure Test Plans correctly |
| 5 | [Requirements-to-test Traceability](05-requirements-to-test-traceability.md) | Show coverage and results without false confidence |
| 6 | [Test Data and Environment Management](06-test-data-and-environment-management.md) | Make tests repeatable, safe, and representative |
| 7 | [Flaky Tests, Code Coverage, and Effectiveness](07-flaky-tests-code-coverage-and-effectiveness.md) | Measure signal quality and improve the portfolio |

## Guided chapter lab

For one user journey, create:

1. A product-risk assessment.
2. Unit, component/contract, integration, and end-to-end coverage.
3. An Azure Test Plan for the release.
4. A requirement-based suite plus one static regression suite.
5. Parameterized manual test cases with expected outcomes.
6. A configuration matrix limited by risk.
7. An exploratory charter and captured findings.
8. Links among requirement, case, automated result, bug, build, and release.
9. A sanitized data factory/reset mechanism.
10. A dashboard for pass rate, duration, flake rate, escaped defects, and risk coverage.

Intentionally create one flaky test and one misleading coverage increase. Diagnose both before applying a quality gate.

## Completion criteria

You are ready for Chapter 14 when you can explain why a test belongs at a level, prioritize testing from risk, run and trace Test Plans artifacts, and distinguish meaningful evidence from test volume or coverage vanity.

## Chapter navigation

[← Part V](../../part-05-infrastructure-and-cloud-native-delivery/README.md) · [Chapter 14 →](../chapter-14-azure-devops-security-and-compliance/README.md)
