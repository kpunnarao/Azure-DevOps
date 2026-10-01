# Separation of Duties

[← Approvals and Locks](07-approvals-checks-and-exclusive-locks.md) · [Chapter 9](README.md)

## Prevent unilateral high-risk change

Separation of duties divides the ability to propose, approve, execute, and administer sensitive changes. It reduces fraud and error, but an excessive process can create queues, shared accounts, rubber-stamp approvals, and emergency bypasses.

A practical model distinguishes:

- Application developer: proposes code and deployment logic.
- Peer reviewer/code owner: approves protected changes.
- Platform owner: maintains templates, agents, and resource policy.
- Resource/environment owner: controls production checks and permissions.
- Automated pipeline identity: performs the approved deployment.
- Operations/on-call: observes, stops, and recovers service.
- Auditor/security: reviews evidence and exceptions.

One person may hold multiple roles in a small team, but the highest-risk action should not be unreviewed and unaudited.

## Control matrix

| Capability | Recommended control |
|---|---|
| Modify application code | Protected branch and peer review |
| Modify production template | Separate owners and stronger review |
| Use production connection | Selected pipeline permission and checks |
| Change Azure RBAC | Privileged identity process |
| Approve own deployment | Disabled where independent review is required |
| Bypass a check | Rare role, reason, alert, after-action review |
| Rotate secrets | Dual control for critical credentials |
| Emergency recovery | Break-glass identity with monitoring and expiry |

Avoid shared user accounts. The deployment should run as a service identity so logs identify the human decision and automated executor separately.

## Evidence

Preserve who proposed and reviewed the change, artifact/source identity, resource and pipeline permissions, approver list captured at check start, approval/rejection comments, bypass events, effective cloud identity, deployment output, and health/recovery decisions.

Approval alone does not prove correctness. Automate repeatable verification and reserve human judgment for business risk, exceptional conditions, or regulatory accountability.

## Break-glass design

An emergency path must be faster, not invisible. Predefine eligible responders, strong authentication, time-limited privilege, scoped authority, mandatory reason, alerting, session/activity records, and retrospective review. Test it before an incident.

## Common mistakes

- Developers administer the production service connection they consume.
- A release manager manually performs every technical deployment.
- An approver sees no artifact or health evidence.
- Open access is enabled on protected resources.
- A shared production account destroys attribution.
- Emergency access is either nonexistent or permanently privileged.

## Interview preparation

**Does DevOps conflict with separation of duties?**  
No. DevOps removes handoff waste and automates evidence; it does not require one person to hold every privilege. Independent control can be embedded in protected resources and review.

**How do you avoid approval bottlenecks?**  
Automate objective gates, use group coverage and timeouts, provide concise evidence, approve risk rather than commands, and continuously remove low-value approvals.

**What should a bypass trigger?**  
Authentication, reason capture, immediate notification, audit preservation, time-limited authority, and a required review.

## Practical exercise

Create a RACI and permission matrix for code, templates, environments, service connections, Azure RBAC, secrets, approval, and break-glass. Identify any identity capable of an unreviewed production change and add a proportional control.

## Official references

- [Security through Azure Pipelines](https://learn.microsoft.com/azure/devops/pipelines/security/overview)
- [Approvals and checks](https://learn.microsoft.com/azure/devops/pipelines/process/approvals)
- [Service connection security](https://learn.microsoft.com/azure/devops/pipelines/library/service-endpoints)

## Chapter review

Trace a production deployment from pull request through approval and service identity. Name which party can alter each control and prove that no ordinary pipeline edit can silently grant itself production authority.

[Chapter 10 — Deployment Strategies, Validation, and Rollback →](../chapter-10-deployment-strategies-validation-and-rollback/README.md)
