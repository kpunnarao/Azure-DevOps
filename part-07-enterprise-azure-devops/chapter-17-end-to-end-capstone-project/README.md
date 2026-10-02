# Chapter 17 — End-to-End Capstone Project

[← Part VII — Enterprise Azure DevOps](../README.md)

The capstone is a production-style evidence portfolio, not a screenshot tutorial. You will design and implement a delivery system for a small cloud-native application, demonstrate normal and failure paths, and defend every important decision.

## Scenario

A team owns an Orders API and a small web client. Changes originate in Azure Boards, source is stored in Azure Repos Git, CI produces an immutable container and optional shared package, IaC provisions ACR/AKS/supporting Azure resources, and a multi-stage pipeline deploys progressively. Production requires independent checks, least privilege, supply-chain evidence, SLO monitoring, and rehearsed recovery.

Use sandbox Azure/Azure DevOps resources with cost limits and synthetic data.

## Capstone sections

| # | Section | Evidence |
|---:|---|---|
| 1 | [Requirements and Architecture](01-capstone-requirements-and-architecture.md) | Scope, NFRs, ADRs, diagrams, threat/risk model |
| 2 | [Boards, Repos, and Branch Policies](02-boards-repos-and-branch-policies.md) | Traceable work and protected source flow |
| 3 | [CI, Quality, and Artifact Design](03-ci-quality-and-artifact-design.md) | Reproducible tested immutable output |
| 4 | [Infrastructure and Environment Provisioning](04-infrastructure-and-environment-provisioning.md) | Reviewed modular IaC and recovery |
| 5 | [Multi-stage Deployment and Approvals](05-multi-stage-deployment-and-approvals.md) | Protected progressive promotion |
| 6 | [Security, Testing, and Compliance](06-security-testing-and-compliance.md) | Defense-in-depth controls and evidence |
| 7 | [Monitoring, Rollback, and Operations](07-monitoring-rollback-and-operations.md) | SLOs, alerts, incidents, recovery |
| 8 | [Documentation, Demo, and Assessment](08-documentation-demo-and-assessment.md) | Reproducible portfolio and expert defense |

## Required constraints

- One immutable artifact/digest is promoted; no environment rebuild.
- YAML templates are versioned and consumer references pinned.
- Workload/service identities use federation/managed identity where supported.
- PR code receives no production authority.
- Secrets are external and never committed/logged.
- Infrastructure change is previewed and destructive action guarded.
- Production uses resource-owned checks and concurrency control.
- Deployment begins with limited blast radius.
- Database/configuration changes remain compatible during overlap.
- Health thresholds and recovery are defined before release.
- Every critical requirement/risk has test/control/telemetry evidence.
- The final demo includes at least five intentionally injected failures.

## Repository deliverables

```text
capstone/
  README.md
  architecture/
  decisions/
  threat-model/
  app/
  tests/
  pipelines/
  templates/
  infrastructure/
  kubernetes/
  operations/
  evidence/
  demo/
```

Store no live secrets, tenant IDs that should remain private, exported tokens, sensitive logs, or proprietary project content.

## Assessment gates

A section is complete only when configuration works, failure behavior is demonstrated, evidence is retained, security implications are explained, and another person can reproduce it. A diagram or YAML fragment without execution/evidence is partial credit.

## Chapter navigation

[← Chapter 16](../chapter-16-governance-scaling-and-platform-engineering/README.md) · [Chapter 18 →](../chapter-18-az-400-and-career-development/README.md)
