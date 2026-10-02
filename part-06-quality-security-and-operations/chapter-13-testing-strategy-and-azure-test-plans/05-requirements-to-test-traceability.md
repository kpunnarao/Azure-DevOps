# Requirements-to-test Traceability

[← Test Plans Artifacts](04-test-plans-suites-cases-and-runs.md) · [Chapter 13](README.md) · [Next: Test Data →](06-test-data-and-environment-management.md)

## Evidence chain

Traceability connects why a change exists to how it was verified:

```text
requirement/risk → test case/automated test → result/run
       → bug → source change → build artifact → deployment
```

It supports impact analysis, audit, release decisions, and investigation. It does not prove requirement quality or product correctness.

## Azure DevOps mechanisms

Requirement-based suites connect requirement work items to cases. Cases can link bugs and automated test methods. Pipeline results connect tests to builds, and deployments can connect commits/work items to environments. Dashboards and reports can show requirement quality when links and results are maintained.

Define which work-item types count as requirements for your process. Use intentional link types rather than arbitrary “Related” links when semantic traceability matters.

## Coverage questions

For each critical requirement/risk ask:

- Is it testable and unambiguous?
- Which positive, negative, security, and quality-attribute behavior matters?
- Which tests provide evidence?
- What is the latest relevant result on the release candidate?
- Which configurations were covered?
- Are failures/blocks/exceptions resolved and owned?
- What production signal will detect escaped failure?

A link to a case that has never run—or ran against another build—does not establish release evidence.

## Automation association

Associate business-significant automated tests with cases where requirement reporting benefits. Avoid creating a work item for every unit test. Preserve the exact automated-test identity; refactoring names/namespaces can break association and historical interpretation.

## Change impact

When a requirement, architecture, configuration, or dependency changes, query linked tests and risks, then review whether the relationship remains valid. Traceability should reduce analysis time, not become a ceremonial matrix maintained after release.

## Anti-patterns

- Chasing 100% requirement links while ignoring failure risks.
- Linking one vague test to many unrelated requirements.
- Marking coverage based on case existence, not current result.
- Using production bugs without feeding them into regression/risk analysis.
- Manually duplicating evidence in spreadsheets.
- Deleting work items/results needed for audit.

## Interview preparation

**Traceability versus coverage?**  
Traceability shows relationships and evidence lineage; coverage assesses how sufficiently behavior/risk was exercised. Links alone do not measure depth.

**What makes evidence current?**  
The result must apply to the exact relevant candidate, configuration, environment, and test version within the release policy's time window.

**How handle defects?**  
Link bug to failed result/case/requirement/build, add regression evidence, and preserve resolution/retest history.

## Practical exercise

Create a requirement-based suite, execute manual and associated automated tests against a known build, file/link a bug, fix and retest, then produce an evidence report. Identify one important risk not represented by a requirement and add it.

## Official references

- [Azure Test Plans traceability](https://learn.microsoft.com/azure/devops/test/overview)
- [Associate automated tests with test cases](https://learn.microsoft.com/azure/devops/test/associate-automated-test-with-test-case)

[Next: Test Data and Environment Management →](06-test-data-and-environment-management.md)
