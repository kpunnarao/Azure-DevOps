# Agent-pool Architecture and Security

[← Paved Roads](03-shared-templates-and-paved-roads.md) · [Chapter 16](README.md) · [Next: Parallelism and Capacity →](05-parallelism-capacity-and-performance.md)

## Agents execute arbitrary code

An agent receives source/scripts, tokens, service connections, dependencies, and network access. Pool design is a trust-boundary decision.

Agent choices include Microsoft-hosted agents, self-hosted agents, Azure VM Scale Set agents, and Managed DevOps Pools in Azure DevOps Services. Microsoft currently recommends considering Managed DevOps Pools for autoscalable custom-pool scenarios. Validate feature/region/network requirements.

## Trust zones

Use separate pools/images/subnets/identities for:

- Untrusted fork/PR validation.
- Ordinary main-branch CI.
- Sensitive signing.
- Infrastructure provisioning.
- Production deployment.
- Regulated or network-private workloads.

Never run untrusted PR code on a persistent privileged agent. A pipeline with pool access may execute arbitrary commands with the agent account/network context even if no explicit service connection is referenced.

## Ephemeral versus persistent

Microsoft-hosted jobs receive fresh VMs. Custom ephemeral agents reduce residue and simplify patch consistency. Persistent agents improve warm-cache performance but retain workspace, processes, credentials, malware, tool changes, and lateral network access; require cleaning, detection, reimaging, and strong isolation.

Do not store permanent cloud credentials on the machine. Use job-scoped tokens/federation and least-privilege agent service accounts.

## Hardening

- Minimal immutable image with pinned toolchain.
- Automated rebuild/patch and inventory.
- Egress allow lists and segmented network.
- No inbound admin except controlled path.
- Endpoint protection compatible with workloads.
- Encrypted disks and secure boot where applicable.
- Disable interactive use and shared accounts.
- Selected-pipeline authorization; avoid open access.
- Monitor registration, capabilities, jobs, image age, drift, and compromise.
- Destroy/quarantine suspicious agents and rotate exposed credentials.

Capabilities/demands route jobs; they are not a security control by themselves.

## Forensics

Ephemeral destruction can erase evidence. Export central logs, image/version, process/network/security telemetry, run/workspace identifiers, and retain a quarantined instance only through approved incident procedure.

## Interview preparation

**Why is self-hosted higher risk?**  
You own OS/tools/network/cleanup, and persistent agents can retain code/credentials between jobs. They may reach private systems.

**How isolate deployment jobs?**  
Dedicated selected-pipeline pool, narrow network/identity, ephemeral instances, protected environment/service connection, and no untrusted source execution.

**Managed DevOps Pools benefit?**  
Managed scalable customized pool capability reduces infrastructure operations while retaining enterprise configuration options; assess boundaries/features.

## Practical exercise

Threat-model four workloads and assign pools. Attempt an unauthorized pipeline, inspect network reachability and agent residue, rebuild image, and document incident isolation/credential-rotation steps.

## Official references

- [Azure Pipelines agents](https://learn.microsoft.com/azure/devops/pipelines/agents/agents)
- [Manage agent-pool security](https://learn.microsoft.com/azure/devops/pipelines/policies/permissions)
- [Managed DevOps Pools security](https://learn.microsoft.com/azure/devops/managed-devops-pools/configure-security)

[Next: Parallelism, Capacity, and Performance →](05-parallelism-capacity-and-performance.md)
