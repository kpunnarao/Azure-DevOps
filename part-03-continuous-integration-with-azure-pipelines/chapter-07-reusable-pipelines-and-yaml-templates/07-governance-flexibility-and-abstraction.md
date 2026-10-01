# Governance, Flexibility, and Abstraction

[← Required Templates](06-required-template-checks.md) · [Chapter 7](README.md)

## Build a paved road

Governance succeeds when the approved path is safer and easier than a custom pipeline. Templates should provide secure defaults, consistent evidence, and a short consumer file while allowing teams to express legitimate application differences.

Think in three layers:

1. **Mandatory guardrails:** identity, credential handling, required scans, provenance, and protected-resource policy.
2. **Supported variation:** language, tool version, test selection, packaging type, and platform matrix through typed parameters.
3. **Team-owned logic:** application-specific build or test steps in clearly bounded hooks.

Do not expose a parameter that disables a mandatory control. If exceptions are legitimate, manage them externally with owner approval, expiration, audit trail, and compensating controls.

## Avoid the mega-template

Warning signs include dozens of booleans, nested objects with undocumented keys, conditions for every repository, deep template stacks, and consumers unable to predict the expanded plan. Split by stable capability or workload type. Share smaller primitives only when their contract is genuinely common.

A useful template is opinionated but observable. Give generated stages and jobs clear names, preserve logs, explain errors, and make the expanded plan debuggable.

## Product operating model

Treat the library as an internal platform product:

- Define users and supported workloads.
- Publish getting-started and migration guides.
- Provide sample repositories.
- Maintain versions and support windows.
- Offer a contribution and decision process.
- Track adoption, failure rate, duration, exceptions, and support burden.
- Conduct security review and threat modeling.
- Gather team feedback before expanding scope.

Measure outcomes, not merely template adoption. A 100% adoption rate with frequent bypass requests or slow pipelines is weak governance.

## Decision framework

Centralize when the behavior is common, high-risk, and stable enough to standardize. Leave local when it is application-specific, rapidly changing, or harmless. Offer an extension point when variation is predictable and can be safely bounded. Create an exception when neither the common path nor extension point fits and the business justification outweighs the risk.

## Common tensions

| Tension | Healthy response |
|---|---|
| Consistency vs autonomy | Mandatory minimum plus supported extension points |
| Rapid central fixes vs reproducibility | New version plus controlled upgrade campaign |
| Simple consumer vs debuggability | Short entry file plus visible expanded plan and docs |
| Broad flexibility vs security | Typed data inputs and constrained executable hooks |
| Innovation vs supportability | Sandbox channel and promotion criteria |

## Interview preparation

**How do you prevent teams from bypassing central templates?**  
Make the path useful, protect relevant resources with required-template checks, restrict alternatives, monitor exceptions, and keep an accountable exception process.

**When should logic stay in the application repository?**  
When it is unique to the application, changes with it, has low policy value, or would make the central contract unstable.

**How do you evaluate a platform template?**  
Adoption, setup time, CI reliability and duration, policy coverage, defect escape rate, upgrade effort, exception frequency, and developer feedback.

## Practical exercise

Review a real pipeline and classify every behavior as mandatory, supported variation, or local. Sketch a template API with no more than ten inputs. Write one exception record containing owner, reason, risk, compensating control, and expiry.

## Official references

- [Security through templates](https://learn.microsoft.com/azure/devops/pipelines/security/templates)
- [Azure Pipelines security overview](https://learn.microsoft.com/azure/devops/pipelines/security/overview)

## Chapter review

Explain your template trust boundary, version policy, resource enforcement, and escape process to another engineer. If they cannot predict the generated pipeline or safely upgrade it, the abstraction needs improvement.

[Chapter 8 — Artifacts and Dependency Management →](../chapter-08-artifacts-and-dependency-management/README.md)
