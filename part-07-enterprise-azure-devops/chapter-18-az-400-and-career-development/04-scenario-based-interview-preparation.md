# Scenario-based Interview Preparation

[← Practice and Gap Analysis](03-practice-assessments-and-gap-analysis.md) · [Chapter 18](README.md) · [Next: Portfolio and Teaching →](05-portfolio-teaching-and-continuous-development.md)

## Answer as an engineer

Use this structure:

1. Clarify outcome, scale, risk, constraints, and current state.
2. State assumptions.
3. Offer viable options.
4. Recommend one and explain tradeoffs.
5. Describe implementation and trust boundary.
6. Define failure/recovery.
7. Define evidence/metrics.
8. State rollout/migration and revisit trigger.

Avoid reciting features without connecting them to the problem.

## Core scenarios

Practice aloud:

- Design Azure DevOps topology for regulated and ordinary teams.
- Secure PR validation while deploying privately.
- Choose hosted, Managed DevOps Pools, or self-hosted agents.
- Reduce 25-minute pipeline without weakening evidence.
- Version and migrate central YAML templates.
- Prevent artifact substitution across environments.
- Choose Bicep/Terraform and protect state/destruction.
- Design canary plus compatible database migration.
- Respond to leaked PAT and compromised build agent.
- Build SLO alerts and decide rollback/roll-forward.
- Migrate classic releases or Azure DevOps organization.
- Balance platform standardization and team autonomy.
- Diagnose skipped stage, queued agent, 403 feed, empty artifact, noisy probe.

## Behavioral evidence

Prepare STAR/CAR stories for a production incident, security improvement, pipeline performance improvement, conflict/tradeoff, failed design, mentoring/teaching, migration, cost reduction, and ambiguity.

Use sanitized facts and quantify baseline, action, outcome, guardrails, and learning. Never disclose employer secrets.

## Whiteboard artifacts

Practice drawing:

- CI/CD trust chain and identities.
- Azure DevOps organization/project/team/resource model.
- Build-once promotion.
- Agent trust zones/network.
- IaC plan/state flow.
- AKS image-to-pod chain.
- Progressive deployment health loop.
- Observability correlation and SLO.
- Incident timeline.
- Platform product/exception flow.

A diagram should show ownership, boundaries, data/credential flow, failure, and evidence—not only boxes.

## Interviewer follow-ups

Expect: Why not alternative? How scale? What fails? How secure? How migrate? What costs? How verify? What would change your decision? Answer uncertainty honestly and explain verification.

## Practical exercise

Record ten 8-minute scenario answers and two 30-minute designs. Score clarity, constraints, alternatives, security, operations, metrics, and concision. Ask a peer to challenge assumptions.

## Official references

- [Azure Architecture Center](https://learn.microsoft.com/azure/architecture/)
- [Azure Well-Architected Framework](https://learn.microsoft.com/azure/well-architected/)
- [Microsoft Cloud Adoption Framework](https://learn.microsoft.com/azure/cloud-adoption-framework/)

[Next: Portfolio, Teaching, and Continuous Development →](05-portfolio-teaching-and-continuous-development.md)
