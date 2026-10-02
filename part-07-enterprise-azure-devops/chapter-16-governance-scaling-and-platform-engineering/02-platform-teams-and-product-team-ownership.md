# Platform Teams and Product-team Ownership

[← Enterprise Boundaries](01-organization-project-and-team-boundaries.md) · [Chapter 16](README.md) · [Next: Paved Roads →](03-shared-templates-and-paved-roads.md)

## Platform as a product

A platform team builds self-service capabilities that reduce cognitive load and encode organizational standards. Product teams own service outcomes: code, on-call, risks, tests, dependencies, deployment, telemetry, and improvement. The platform team does not become the deployment ticket desk or absorb every application decision.

## Responsibility split

| Platform team | Product team |
|---|---|
| Agent platform and base images | Application build/test behavior |
| Template library and policies | Correct template parameters/extensions |
| Identity/resource patterns | Least-privilege app/workload access |
| Artifact/registry foundations | Package/image lifecycle |
| Environment/GitOps capability | Application rollout and health |
| Observability platform | Useful service telemetry/SLO |
| Documentation/support model | Service runbooks/on-call |

Security, reliability, FinOps, and compliance specialists contribute controls and coaching; accountability must remain explicit.

## Product operating model

Define users, supported workload “golden paths,” service levels, roadmap, ownership/on-call, documentation, version policy, support channels, contribution model, adoption metrics, exception process, deprecation, and funding.

Measure time-to-first-deployment, adoption, task success, lead time, reliability, support toil, exception rate, upgrade lag, security coverage, and developer sentiment. High adoption alone may reflect mandate, not value.

## Self-service

Self-service needs discoverable documentation, examples, templates/scaffolding, validation, safe defaults, preview, clear errors, bounded customization, and automated lifecycle. A portal is optional; an API/CLI/template repository can be excellent if usable.

Use “thinnest viable platform”: standardize repeated high-risk capabilities, leave application logic local, and avoid abstracting immature needs.

## Interaction modes

- Platform-owned capability.
- Self-service with documented contract.
- Team contribution through reviewed interface.
- Consultation/enabling.
- Time-limited exception.
- Experimental channel before standardization.

Avoid hidden escalation paths through private messages. Track demand and feed repeated requests into roadmap or documentation.

## Interview preparation

**Platform versus operations team?**  
A platform team creates reusable self-service products and guardrails; operations may execute/run processes. Mature platform work enables teams to operate safely.

**Who owns production?**  
Product teams own service outcomes, supported by platform capabilities and organizational controls. Shared components have their own accountable owner.

**How prevent a platform bottleneck?**  
Self-service APIs/templates, product roadmap, clear boundaries, contribution model, observable SLAs, and elimination of ticket-based routine work.

## Practical exercise

Write a platform product brief: users, top journeys, non-goals, APIs/templates, SLOs, ownership, support, metrics, roadmap, and deprecation. Interview one developer persona and revise it.

## Official references

- [Azure Architecture Center: platform engineering](https://learn.microsoft.com/platform-engineering/)
- [DevOps culture and collaboration](https://learn.microsoft.com/devops/what-is-devops)

[Next: Shared Templates and Paved Roads →](03-shared-templates-and-paved-roads.md)
