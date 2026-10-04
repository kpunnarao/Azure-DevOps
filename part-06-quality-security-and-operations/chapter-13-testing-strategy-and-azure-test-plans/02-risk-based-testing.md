# Risk-based Testing

[← Test Levels](01-test-levels-and-the-test-pyramid.md) · [Chapter 13](README.md) · [Next: Manual, Exploratory, and Automated →](03-manual-exploratory-and-automated-testing.md)

## Test what can hurt

Risk-based testing allocates depth and frequency using likelihood and impact. Impact includes safety, security, financial loss, privacy, compliance, customer trust, availability, and recovery cost.

A simple score can begin discussion:

```text
risk exposure = likelihood × impact
```

Do not treat the number as objective truth. Add detectability, change complexity, usage, novelty, dependency volatility, incident history, and reversibility.

## Workflow

1. Identify assets, user journeys, threats, and failure modes.
2. Estimate likelihood and consequence with product/engineering/security/operations.
3. Define prevention, detection, and recovery controls.
4. Map each important risk to tests at suitable levels.
5. State residual risk and owner.
6. Reassess after architecture, usage, dependency, or threat changes.
7. Use escaped defects and incidents to update the model.

Example:

| Risk | Evidence |
|---|---|
| Duplicate payment | unit invariants, idempotency integration test, reconciliation monitor |
| Unauthorized access | authorization tests, SAST, threat model, audit alert |
| Schema incompatibility | contract tests, migration rehearsal, canary telemetry |
| Slow checkout | load test, latency SLI, capacity alert |
| Accessibility failure | automated rules plus manual assistive-tech testing |

## Prioritization

High-impact low-frequency risks still require assurance. Negative paths, concurrency, time boundaries, retries, permissions, and recovery are often more valuable than more happy-path cases.

Use change-impact analysis to select fast PR tests, but keep authoritative scheduled/release suites for risks the selector might miss.

## Exit criteria

A release decision should state which critical risks have acceptable evidence, which tests are incomplete/unavailable, known defects, residual risk, exception owner, and monitoring/recovery plan. “95% tests passed” hides which 5% failed.

## Interview preparation

**How prioritize when time is short?**  
Protect high-impact/high-likelihood and irreversible risks first, critical user journeys and recent change next, then use exploratory work for uncertainty. Make residual risk explicit.

**Who owns product risk?**  
Cross-functional product, engineering, security, and operations stakeholders. Testers provide evidence; they do not alone accept business risk.

**Risk coverage versus requirements coverage?**  
Requirement links show specified behavior was considered; risk coverage also includes threats, failures, quality attributes, and recovery not captured as features.

## Practical exercise

Run a risk workshop for login or checkout. Create ten risks, score and challenge them, map tests/telemetry/recovery, then remove half the test budget while preserving the best risk reduction.

## Official references

- [Azure Test Plans traceability](https://learn.microsoft.com/azure/devops/test/overview)
- [Microsoft Security Development Lifecycle practices](https://www.microsoft.com/securityengineering/sdl/practices)

[Next: Manual, Exploratory, and Automated Testing →](03-manual-exploratory-and-automated-testing.md)
