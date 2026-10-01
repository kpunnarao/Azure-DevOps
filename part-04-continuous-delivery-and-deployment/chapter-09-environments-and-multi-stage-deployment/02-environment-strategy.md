# Environment Strategy

[← CI and CD Separation](01-ci-and-cd-separation.md) · [Chapter 9](README.md) · [Next: Deployment Jobs →](03-deployment-jobs-and-deployment-history.md)

## Model risk, not tradition

Development, Test, Staging, and Production are common names, but an environment earns its place only when it provides a distinct validation, isolation, ownership, or release function. Too few environments hide risk; too many create delay, cost, drift, and false confidence.

An Azure Pipelines environment is a logical deployment target that can contain supported resources such as Kubernetes or virtual machines, or remain empty solely to record deployment history and enforce checks.

## Design dimensions

For every environment define:

- Purpose and exit criteria.
- Data classification and privacy constraints.
- Infrastructure ownership and Azure subscription/resource group.
- Network boundary and private connectivity.
- Deployment and operational identities.
- Allowed pipelines and branches.
- Approvals/checks and concurrency policy.
- Configuration and secret sources.
- Monitoring, support, retention, and recovery.
- Whether infrastructure is persistent or ephemeral.

Preproduction should resemble production where a difference could hide a failure, but duplicating production scale is rarely necessary. Preserve topology, policies, runtime versions, identity model, networking behavior, and deployment mechanism.

## Environment topology options

- **Shared Development:** low cost and fast, but subject to contention.
- **Ephemeral per change:** isolated and reproducible, but requires automated lifecycle and cost controls.
- **Integration/Test:** validates service contracts and shared dependencies.
- **Staging/Preproduction:** rehearses production-like configuration and deployment.
- **Production rings/stamps:** limits exposure by geography, tenant, or capacity unit.

Do not use environment names as the only access control. Authorize pipelines and users explicitly and apply Azure RBAC to the actual target.

## Drift management

Create infrastructure through version-controlled IaC, compare deployed configuration continuously, and eliminate undocumented portal changes. If staging and production intentionally differ, record why, owner, review date, and how the risk is otherwise tested.

Test data must respect privacy. Never clone sensitive production data into lower environments without approved masking, minimization, access, and retention controls.

## Azure Pipelines considerations

Create important environments explicitly rather than relying on automatic creation. Limit pipeline permissions to selected pipelines. Environment roles control who can administer, use, create, or view the resource; they do not replace permissions inside Azure or Kubernetes.

Use specific environment resources when deployment history must identify the actual VM or Kubernetes target.

## Interview preparation

**How many environments should a team have?**  
Enough to validate materially different risks, and no more. Each requires a clear purpose, automated creation/configuration, owner, exit criteria, and cost justification.

**Should staging be identical to production?**  
It should be equivalent in behavior relevant to deployment risk. Capacity and data often differ, but runtime, topology, identity, networking, policy, and deployment method should be representative.

**What is an empty Azure Pipelines environment useful for?**  
It can record deployment history and carry approvals/checks even without registered VM or Kubernetes resources.

## Practical exercise

Create an environment decision record for one workload. List the risk each environment detects, differences from production, data policy, permissions, checks, cost, and destruction/retention plan. Remove any environment with no unique function.

## Official references

- [Create and target Azure Pipelines environments](https://learn.microsoft.com/azure/devops/pipelines/process/environments)
- [Pipeline resource security](https://learn.microsoft.com/azure/devops/pipelines/security/resources)

[Next: Deployment Jobs and Deployment History →](03-deployment-jobs-and-deployment-history.md)
