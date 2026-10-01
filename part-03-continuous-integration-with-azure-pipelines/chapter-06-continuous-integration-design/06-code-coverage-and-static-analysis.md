# Code Coverage and Static Analysis

[← CI Performance](05-caching-parallelism-and-performance.md) · [Chapter 6](README.md) · [Next: Build Numbering →](07-build-numbering-and-semantic-versioning.md)

## Evidence, not certainty

Coverage reports which code was executed by tests; it does not prove that assertions were correct. Static analysis finds patterns without executing the application; it cannot prove the absence of every defect. Both are valuable signals when tied to risk and developer action.

## Code coverage

Azure Pipelines can publish coverage generated in supported formats. Keep generation separate from publication: the language-specific tool creates the report and the pipeline makes it visible.

```yaml
- task: PublishCodeCoverageResults@2
  condition: succeededOrFailed()
  inputs:
    summaryFileLocation: '$(System.DefaultWorkingDirectory)/**/coverage.cobertura.xml'
```

Verify the current task inputs for your report format and toolchain.

Useful policies include:

- Require tests for changed business-critical logic.
- Monitor changed-line or diff coverage.
- Prevent unexplained regression from an agreed baseline.
- Exclude only generated or structurally untestable code with review.
- Combine percentage with mutation testing or critical-path review where warranted.

Branch coverage can reveal untested decisions that line coverage hides. High coverage with weak assertions is still weak testing.

## Static analysis categories

- Compiler warnings and type checks.
- Formatting and lint rules.
- Maintainability and complexity findings.
- Security-oriented static application security testing.
- Secret and credential scanning.
- Dependency and license analysis.
- Infrastructure-as-code and container configuration checks.

Place fast checks early. For larger security analysis, preserve the report and provide a clear path from finding to source line, owner, severity, and remediation.

## Introducing a gate safely

A legacy codebase may contain thousands of existing findings. Enabling a “zero findings” gate immediately often causes broad suppression. Establish a trusted baseline, prevent new high-risk issues, assign remediation work, and tighten policy over time. Expire suppressions and require a reason.

Distinguish tool failure from finding failure. If the scanner crashes or cannot reach its service, decide whether the risk requires fail-closed behavior. A silent skipped scan should never appear as a passing security gate.

## Diagnostic checklist

When coverage is missing:

1. Confirm tests actually generated a report.
2. Inspect the resolved file path and format.
3. Run publication even after test failure when safe.
4. Check multi-module merging rather than overwriting.
5. Verify source paths map to checked-out files.
6. Confirm containers mounted the report into the agent workspace.

## Interview preparation

**Is 100% coverage a good target?**  
Rarely as a universal gate. It can be useful for a small critical component, but overall confidence also depends on assertions, branch behavior, integration, and risk.

**How do you introduce static analysis to a legacy repository?**  
Baseline existing debt, fail new or worsened high-severity issues, assign ownership, track remediation, and ratchet standards without mass suppression.

**Should scanner outages fail CI?**  
Choose based on risk and policy. High-assurance release paths may fail closed; lower-risk PR feedback may mark the result unavailable and block only later promotion. Never report an unavailable scan as passed.

## Practical exercise

Generate unit-test coverage, publish it, and inspect uncovered branches. Add one valuable test rather than chasing a percentage. Introduce a lint or SAST rule, create a baseline, then prove a newly introduced violation fails validation.

## Official references

- [Review code coverage results](https://learn.microsoft.com/azure/devops/pipelines/test/review-code-coverage-results)
- [PublishCodeCoverageResults@2](https://learn.microsoft.com/azure/devops/pipelines/tasks/reference/publish-code-coverage-results-v2)

[Next: Build Numbering and Semantic Versioning →](07-build-numbering-and-semantic-versioning.md)
