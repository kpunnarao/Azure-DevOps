# Capstone Requirements and Architecture

[← Chapter 17](README.md) · [Next: Boards, Repos, and Branch Policies →](02-boards-repos-and-branch-policies.md)

## Establish the problem before tools

Write a one-page product brief: users, jobs, business outcome, in/out of scope, dependencies, owners, support hours, data classification, expected load, and success metrics.

Functional minimum:

- Create/read/update an order.
- Idempotent submission.
- Authentication/authorization.
- Web client or API consumer.
- Health endpoints and telemetry.
- Database persistence.
- Feature-flagged enhancement.

## Nonfunctional requirements

Define measurable targets:

- Availability and latency SLO.
- RTO/RPO and data-integrity requirement.
- Security/threat assumptions.
- Privacy/retention.
- Peak throughput/capacity.
- Deployment frequency/lead-time goal.
- Maximum initial release blast radius.
- Audit/evidence retention.
- Cost budget.
- Supported regions/browsers/API compatibility.

Avoid “highly available” without a measurement/window.

## Architecture package

Create context, container/component, deployment, identity/trust-boundary, CI/CD flow, and telemetry diagrams. Show Azure DevOps, Azure, ACR, AKS/application platform, data, Key Vault/configuration, identities, network, GitOps/pipeline control, and external users/dependencies.

Create ADRs for:

- Azure DevOps topology/repository strategy.
- Azure Pipelines/agent choice.
- Bicep versus Terraform.
- Runtime platform.
- Deployment strategy.
- Secretless identity.
- Database migration.
- Test strategy.
- Observability/SLO.
- Backup/recovery.
- Template/GitOps ownership.

Each ADR includes context, options, decision, consequences, risks, and revisit trigger.

## Risk and threat model

Identify assets, actors, entry points, trust boundaries, data flows, abuse cases, operational failure modes, likelihood/impact, control/test/telemetry/recovery, owner, and residual risk.

Mandatory threats: malicious PR, dependency compromise, agent persistence, stolen credential, artifact substitution, overprivileged service connection, destructive IaC, secret/log leak, insecure container, unapproved production change, telemetry failure, data migration incompatibility.

## Acceptance evidence

- Reviewed product brief and NFR table.
- Versioned diagrams/ADRs.
- Risk-control-test matrix.
- Cost estimate.
- RACI/on-call/escalation.
- Assumption and open-question log.

## Expert review questions

Why this project/repository boundary? Which identity could cause the greatest damage? What fails if Azure DevOps, ACR, Key Vault, or region is unavailable? Which decision is hardest to reverse? What evidence proves the SLO?

## Practical checkpoint

Run a 30-minute architecture review with a developer, platform, security, and operations perspective. Record decisions and update at least one design from feedback.

[Next: Boards, Repos, and Branch Policies →](02-boards-repos-and-branch-policies.md)
