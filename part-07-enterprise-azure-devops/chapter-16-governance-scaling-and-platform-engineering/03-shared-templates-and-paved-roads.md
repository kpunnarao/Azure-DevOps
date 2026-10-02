# Shared Templates and Paved Roads

[← Platform Ownership](02-platform-teams-and-product-team-ownership.md) · [Chapter 16](README.md) · [Next: Agent Pools →](04-agent-pool-architecture-and-security.md)

## A paved road is a supported journey

A paved road combines scaffolding, versioned templates, approved tasks, agent/image choices, identity, artifact patterns, deployment strategy, telemetry, documentation, and support. It should be easier than building a custom path while enforcing mandatory minimum controls.

## Layer the design

1. **Mandatory guardrails:** provenance, secret handling, scans, protected deployment.
2. **Workload profiles:** language/runtime/container/IaC-specific jobs.
3. **Supported parameters:** typed, documented safe variation.
4. **Local logic:** bounded application-owned hooks.
5. **Exception:** risk owner, compensating controls, expiry, remediation.

Never provide a boolean that disables a mandatory control. Keep checks on protected resources when consumer YAML must not bypass them.

## Distribution and versioning

Store central templates in a protected repository with code owners, contract tests, representative consumers, tags/releases, changelog, migration guides, support windows, and emergency-security process. Pin consumers to reviewed immutable references.

Roll out via pilot, prerelease channel, opt-in cohort, measured adoption, then policy deadline where justified. Do not mutate an existing released reference silently.

## Avoid abstraction failure

Warning signs: dozens of booleans, arbitrary script strings, deep nesting, unreadable expanded YAML, central conditions for individual repos, all consumers broken by one update, and teams copying templates to regain control.

Split by stable capability, expose data rather than executable strings, preserve clear job/stage names, and offer a local test/validation path.

## Governance metrics

Track version distribution, outdated/vulnerable consumers, compile/run failures, queue/duration, policy coverage, exception age, setup time, support volume, upgrade effort, and satisfaction. Measure product outcomes, not line-count reduction.

## Interview preparation

**Template versus paved road?**  
A template is one implementation component; a paved road includes the end-to-end supported experience, identity, infrastructure, controls, docs, and lifecycle.

**How ship a breaking change?**  
New major version, compatibility tests, prerelease/pilot, migration guide/tooling, support window, adoption inventory, and rollback.

**How enforce adoption?**  
Deliver value first, protect high-risk resources with checks/required templates, restrict unsafe alternatives, and run an accountable exception process.

## Practical exercise

Design a paved road for a container API from repository creation to AKS deployment. Limit the public template API to ten inputs, identify mandatory controls, implement one extension point, and write a version migration.

## Official references

- [Azure Pipelines templates](https://learn.microsoft.com/azure/devops/pipelines/process/templates)
- [Template security](https://learn.microsoft.com/azure/devops/pipelines/security/templates)
- [Approvals and required-template checks](https://learn.microsoft.com/azure/devops/pipelines/process/approvals)

[Next: Agent-pool Architecture and Security →](04-agent-pool-architecture-and-security.md)
