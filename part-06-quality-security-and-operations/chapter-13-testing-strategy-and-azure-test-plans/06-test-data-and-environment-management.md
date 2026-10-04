# Test Data and Environment Management

[← Traceability](05-requirements-to-test-traceability.md) · [Chapter 13](README.md) · [Next: Flaky Tests and Effectiveness →](07-flaky-tests-code-coverage-and-effectiveness.md)

## Repeatability needs controlled state

A test result is hard to trust when data, time, dependencies, or environment are unknown. Good test data is representative enough to expose risk, deterministic enough to reproduce, isolated enough for parallelism, and protected according to classification.

## Data strategies

- Builders/factories create scenario-specific data.
- Seeded synthetic datasets provide known baselines.
- Transaction rollback or disposable schemas isolate tests.
- Service virtualization provides controlled dependency behavior.
- Masked/subset production data may be used only through approved privacy governance.
- Property-based/generative data explores input space and invariants.
- Fault data simulates expiration, corruption, delay, duplicates, and partial failure.

Never copy production personal, financial, health, or secret data casually. Masking must resist re-identification and preserve only structures necessary for testing.

## Lifecycle

Each test owns setup, unique identifiers, execution, assertion, and cleanup. Cleanup should be idempotent and must not target shared/production resources. For failed-test diagnosis, retain only approved evidence and expire it.

Control time through injectable clocks; control randomness with recorded seeds. Use correlation IDs to trace created data.

## Environment strategy

Use local/ephemeral environments for fast isolated feedback, shared integration for cross-service behavior, and production-like staging for topology/deployment risk. Version environment configuration and dependencies. Monitor drift, capacity, queue backlog, certificates, identities, and third-party sandboxes.

Shared environments need reservations/ownership, health checks, reset mechanism, change calendar, and visible incidents. Do not blame tests for environment outages; classify infrastructure failure separately.

## Parallel execution

Namespace data by run/test, avoid global accounts, use independent queues/topics, and ensure cleanup cannot affect another worker. If tests mutate shared state, serialize only that risk rather than the entire suite.

## Secrets

Use test-only short-lived credentials with least privilege. Keep them out of cases, screenshots, logs, attachments, and exported CSV files. Redact diagnostics and rotate after suspected exposure.

## Interview preparation

**Can production data be used in testing?**  
Only under explicit legal/security/privacy approval with minimization, masking/tokenization, access, retention, and audit. Synthetic data is preferred.

**How make tests parallel-safe?**  
Unique namespaces/identifiers, isolated state, deterministic setup, idempotent cleanup, and no shared mutable accounts.

**What causes environment-related flakiness?**  
Drift, contention, unstable dependencies, stale data, rate limits, time, certificates, network, capacity, and uncoordinated deployments.

## Practical exercise

Create a data factory with unique run IDs and deterministic random seed. Run tests in parallel, interrupt cleanup, and rerun it safely. Define a retention/redaction policy for failed-run attachments and test a full environment reset.

## Official references

- [Use parameters in test cases](https://learn.microsoft.com/azure/devops/test/repeat-test-with-different-data)
- [Test configurations](https://learn.microsoft.com/azure/devops/test/test-different-configurations)

[Next: Flaky Tests, Code Coverage, and Effectiveness →](07-flaky-tests-code-coverage-and-effectiveness.md)
