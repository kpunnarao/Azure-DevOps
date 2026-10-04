# Flaky Tests, Code Coverage, and Effectiveness

[← Test Data](06-test-data-and-environment-management.md) · [Chapter 13](README.md)

## Signal quality

A flaky test passes and fails without a relevant product change. It destroys confidence, consumes investigation time, and can hide real intermittent defects. Treat it as test or product reliability debt—not normal noise.

Common causes include timing assumptions, uncontrolled concurrency, shared state, test order, unstable selectors, external services, resource pressure, clocks/time zones, random data, and cleanup failures.

## Response

1. Preserve logs, seed, environment, version, timing, and artifacts.
2. Reproduce through focused repeated execution.
3. Classify product race, test defect, or environment failure.
4. Assign owner and severity.
5. Fix isolation/synchronization/assertion.
6. Verify repeated stability.
7. Remove any quarantine.

Quarantine only to protect the main signal when necessary. Keep the test visible, assign an owner and expiry, run it separately, and prevent critical-risk coverage from disappearing. Blind retry turns intermittent failure into false success; report the initial failure and retry count.

## Coverage

Line coverage shows executed lines, branch coverage shows exercised decisions, and changed-code coverage focuses on new work. None proves assertion quality. High coverage can execute code without checking results; low coverage may still protect the most important invariants.

Use coverage to find unexamined code and prevent meaningful regression. Avoid universal targets that encourage trivial tests or exclusion abuse. Combine with mutation testing, risk coverage, review, defect escapes, and production evidence.

## Effectiveness metrics

Useful portfolio measures:

- Time to actionable feedback.
- Defect detection by stage and escaped-defect severity.
- Flake rate and rerun rate.
- Failure diagnostic time.
- Critical-risk/requirement coverage.
- Mutation score where practical.
- Test duration and maintenance cost.
- Environment-caused failure.
- Bugs reopened and regression recurrence.

Pass rate alone is easily gamed. A suite that never fails may be irrelevant.

## Interview preparation

**How handle a flaky required test?**  
Preserve evidence, assign owner, diagnose root cause, time-limit any quarantine, keep critical risk covered, and never silently convert failure to success through retries.

**Is 80% coverage enough?**  
It is context-free. Evaluate changed critical code, branches, assertions, risks, mutation results, and escaped defects.

**How measure test effectiveness?**  
How early and reliably important defects are detected, diagnostic speed, risk coverage, escapes, flakiness, and cost—not test count.

## Practical exercise

Create three flaky failure modes: fixed sleep, shared record, and random seed. Diagnose and correct them. Add tests that increase coverage without assertions, then replace them with one invariant-rich test and compare mutation/defect detection.

## Official references

- [Flaky test management](https://learn.microsoft.com/azure/devops/pipelines/test/flaky-test-management)
- [Review code coverage](https://learn.microsoft.com/azure/devops/pipelines/test/review-code-coverage-results)
- [Analyze test results](https://learn.microsoft.com/azure/devops/pipelines/test/test-analytics)

## Chapter review

Present a release test strategy that connects product risk, test level, human exploration, Test Plans traceability, data/environment controls, and production signals. Explain every omitted test and every quality exception.

[Chapter 14 — Azure DevOps Security and Compliance →](../chapter-14-azure-devops-security-and-compliance/README.md)
