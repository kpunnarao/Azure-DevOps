# Documentation, Demo, and Assessment

[← Monitoring and Operations](07-monitoring-rollback-and-operations.md) · [Chapter 17](README.md)

## Documentation package

The capstone README explains problem, architecture, prerequisites, safe sandbox setup, cost warning, repository map, quick start, normal delivery, failure labs, cleanup, evidence, and limitations. Link deeper:

- Product/NFR/risk documentation.
- Diagrams and ADRs.
- Developer contribution/onboarding.
- Platform/template contracts.
- Operations and incident runbooks.
- Security/threat/evidence model.
- Testing strategy and Test Plans.
- Cost/retention/continuity.
- Teaching notes and glossary.

Use sanitized examples; never publish tenant identifiers, endpoints, credentials, customer data, private logs, or proprietary screenshots.

## Reproducibility test

Give the repository to another engineer. They should deploy a sandbox using only documented prerequisites, observe CI/CD and telemetry, execute one failure/recovery, and clean up. Record every unclear or manual step and improve it.

## Demo script (30–45 minutes)

1. Business problem, NFRs, architecture, and risks.
2. Board item to branch and protected PR.
3. CI graph, tests/scans, SBOM, artifact digest.
4. IaC plan and environment identity.
5. Development/Test promotion.
6. Production checks and canary.
7. Telemetry and deployment annotation.
8. Injected failure, automatic halt, incident/recovery.
9. Trace evidence from requirement to restored production.
10. Enterprise governance/cost/DX lessons.

Keep a pre-recorded/screenshot fallback that contains no secrets, but demonstrate live evidence when practical.

## Assessment rubric (100)

| Area | Points |
|---|---:|
| Requirements, architecture, risk, ADR quality | 15 |
| Boards/Repos traceability and protection | 10 |
| CI/reproducibility/artifacts/templates | 15 |
| IaC/environments/deployment | 15 |
| Security/testing/compliance evidence | 15 |
| Observability/SLO/recovery | 15 |
| Enterprise governance/cost/continuity/DX | 10 |
| Documentation/demo/expert defense | 5 |

A critical secret leak, unreviewed production path, environment rebuild, unrecoverable destructive operation, or fabricated evidence prevents “expert-ready” status regardless of score.

## Expert defense

Prepare answers for alternative designs, trust boundaries, failure behavior, scaling, migration, cost, and tradeoffs. Say what you do not know and show how you would verify it.

## Final checklist

- [ ] Clean clone reproduces documented result
- [ ] All diagrams/links current
- [ ] Five failure demonstrations recorded
- [ ] No secret/sensitive data in history
- [ ] Sandbox cleanup validated
- [ ] Evidence indexed by section
- [ ] Independent reviewer scores rubric
- [ ] Gaps become backlog with owners
- [ ] Portfolio summary and demo published safely
- [ ] Review date scheduled

## Official references

- [Azure Architecture Center](https://learn.microsoft.com/azure/architecture/)
- [Azure Well-Architected Framework](https://learn.microsoft.com/azure/well-architected/)
- [Azure DevOps documentation](https://learn.microsoft.com/azure/devops/)

## Chapter review

The capstone is complete when another engineer can reproduce, operate, challenge, and learn from it. Completion is demonstrated capability, not the presence of files.

[Chapter 18 — AZ-400 and Career Development →](../chapter-18-az-400-and-career-development/README.md)
