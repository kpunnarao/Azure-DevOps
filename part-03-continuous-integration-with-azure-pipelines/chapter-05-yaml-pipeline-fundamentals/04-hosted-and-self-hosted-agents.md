# Hosted and Self-Hosted Agents

> Chapter 5 — YAML Pipeline Fundamentals

[← Previous](03-stages-jobs-steps-tasks-and-scripts.md) · [Chapter home](README.md) · [Next →](05-agent-pools-capabilities-and-demands.md)

## Purpose

An agent executes pipeline code. Agent choice is therefore a security, networking, reliability, performance, and cost decision—not only a machine-size choice.

## Options

| Model | Operations | Isolation | Custom network/software |
|---|---|---|---|
| Microsoft-hosted | Microsoft manages images | Fresh VM per job | Limited/customize during run |
| Self-hosted | Customer manages | Depends on design; state persists | Full control |
| VM scale-set agents | Customer infrastructure with scaling integration | Image/design dependent | Full control |
| Managed DevOps Pools | Managed service for custom scalable pools | Configurable managed model | Broader customization |

Availability varies between Azure DevOps Services and Server. Microsoft-hosted agents are not available for Azure DevOps Server.

## Microsoft-hosted agents

Advantages:

- Fresh VM per job
- Automatic image maintenance
- Simple start
- Reduced persistent compromise and cleanup risk
- Broad preinstalled tooling

Considerations:

- Files do not persist to later jobs
- Image software changes over time
- Limited private-network reach
- Fixed available machine profiles and storage
- Tool installation adds run time
- Hosted image deprecations require monitoring

Use explicit tool installers or controlled containers when reproducibility requires a version not guaranteed by the image.

## Self-hosted agents

Advantages:

- Private-network connectivity
- Custom hardware and software
- Persistent caches
- Control over images and regional placement

Responsibilities:

- Patch OS, agent, and tools
- Secure registration and service identity
- Isolate workloads
- Clean workspace and credentials
- Monitor capacity and health
- Rotate/rebuild images
- Restrict outbound and internal access
- Investigate compromise

A reused agent can retain malicious files, credentials, processes, or modified tools. Do not run untrusted PR code on a privileged persistent agent.

## Trust zones

Separate pools for materially different trust levels:

- Untrusted PR validation
- Normal CI
- Internal package publishing
- Non-production deployment
- Production deployment

Do not rely solely on pipeline conventions. Use pool permissions, protected resources, network boundaries, ephemeral compute, and least-privileged identities.

## Agent communication

The agent typically initiates communication to Azure DevOps and receives job payloads. Deployment connectivity must exist from the agent to targets. A self-hosted agent behind a firewall needs outbound control-plane access and target line of sight according to design.

## Decision framework

Consider:

- Source trust
- Required network reach
- Sensitive data
- Toolchain
- Hardware/performance
- Workload duration
- Isolation and cleanup
- Autoscaling
- Cost and queue time
- Platform team operational capability

Start with Microsoft-hosted when it satisfies needs. Choose custom agents for clear network, compliance, hardware, or performance requirements.

## Common mistakes

- Using a production-network agent for PR builds
- Installing several agents on one machine without resource isolation
- Relying on undeclared software from a persistent machine
- Giving agent service accounts administrator rights
- Failing to rebuild compromised agents
- Allowing arbitrary pipelines to use privileged pools
- Storing permanent secrets on disk
- Assuming hosted image “latest” is immutable

## Interview preparation

**Q: Why prefer hosted agents initially?**  
They reduce maintenance and provide fresh per-job isolation. Move to custom agents when network, toolchain, compliance, performance, or hardware requirements justify the responsibility.

**Q: Primary self-hosted risk?**  
Persistent state and privileged network reach can allow one job to affect later jobs or sensitive systems.

**Q: How do you secure production agents?**  
Use a separate restricted pool, minimal pipeline authorization, ephemeral or clean images, least-privileged identities, controlled network access, patching, monitoring, and no untrusted code.

**Q: Can sequential jobs rely on the same self-hosted agent?**  
No. The scheduler may choose any matching available agent. Share files explicitly.

## Further reading

- [Azure Pipelines agents](https://learn.microsoft.com/azure/devops/pipelines/agents/agents)
- [Microsoft-hosted agents](https://learn.microsoft.com/azure/devops/pipelines/agents/hosted)
- [Agent pools](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/pools-queues)
