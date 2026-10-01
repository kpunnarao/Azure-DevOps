# Agent Pools, Capabilities, and Demands

> Chapter 5 — YAML Pipeline Fundamentals

[← Previous](04-hosted-and-self-hosted-agents.md) · [Chapter home](README.md) · [Next →](06-variables-parameters-and-variable-groups.md)

## Purpose

Pools organize agents and define which pipelines can request their execution. Capabilities describe self-hosted agents; demands constrain job scheduling. Parallel-job entitlement limits how many jobs can run concurrently even when many agents exist.

## Pools and queues

An organization-level pool contains agents. Projects receive access through the relevant queue/resource authorization model. Pool permissions determine who can use and administer the resource.

A pool is a trust boundary. If one pool reaches production, do not authorize every pipeline to use it.

## Capabilities

Capabilities are name/value pairs:

- System capabilities discovered by the agent
- User capabilities configured by administrators

Examples include operating system, tool versions, or a custom marker such as InternalNetwork.

Environment variables can appear as capabilities and become job environment data. Prevent sensitive or mutable variables from being captured through supported ignore configuration.

Capabilities describe availability; they do not install software or prove it is healthy.

## Demands

A self-hosted job can request capabilities:

```yaml
pool:
  name: InternalLinux
  demands:
  - Agent.OS -equals Linux
  - Docker
```

Supported demand matching is primarily existence or equality. Some tasks add demands automatically.

If no agent satisfies demands, the job can wait or fail depending on whether matching agents exist and availability. Inspect agent capabilities before adding more demands.

## Parallelism

Three constraints matter:

1. Dependency graph: are jobs ready simultaneously?
2. Parallel-job entitlement: may the organization run them simultaneously?
3. Matching agents: are suitable agents online and idle?

Adding agents does not increase entitlement. Purchasing capacity does not help if only one matching agent exists.

## Pool design

Possible separation:

- linux-ci
- windows-ci
- untrusted-pr
- internal-network
- nonprod-deploy
- prod-deploy

Avoid a single pool with every capability and broad network access. Also avoid one pool per pipeline without operational justification.

## Capability drift

Self-hosted software changes can invalidate deterministic builds. Manage agents as versioned images:

- Define image configuration as code
- Rebuild rather than manually patching indefinitely
- Validate expected tool versions
- Monitor deprecated agents
- Drain and replace unhealthy capacity
- Record image version in logs
- Keep user capabilities minimal and meaningful

## Troubleshooting queued jobs

1. Inspect the pool selected after template expansion.
2. Check project/pipeline authorization.
3. Confirm parallel capacity.
4. Check agents are enabled and online.
5. Compare demands with capabilities exactly.
6. Check task-added demands.
7. Check agent version.
8. Review pool maintenance or network failures.

## Common mistakes

- Treating capabilities as package installation
- Putting secrets in capabilities
- Depending on mutable PATH or tool versions
- Adding agents while parallel entitlement remains the bottleneck
- Using demands to select one named machine permanently
- Authorizing all pipelines to a sensitive pool
- Mixing untrusted validation and deployment
- Manually modifying agents until they are unique and unreproducible

## Interview preparation

**Q: Capability versus demand?**  
A capability describes an agent. A demand is a job requirement used to match a self-hosted agent.

**Q: Why are jobs queued when agents are idle?**  
They may lack matching capabilities, pipeline authorization, or parallel-job entitlement, or dependencies may not be ready.

**Q: Do more agents always increase concurrency?**  
No. Parallel-job licensing/entitlement and dependency structure also limit concurrency.

**Q: How should pools map to security?**  
Separate material trust zones and network reach, authorize only required pipelines, and use least-privileged agent identities.

## Practical exercise

Create two mock agents or reason from capability lists. Add a demand no agent meets, diagnose the queue, correct it, then document why selecting by durable capability is better than machine name.

## Further reading

- [Azure Pipelines agents](https://learn.microsoft.com/azure/devops/pipelines/agents/agents)
- [Demands](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/demands)
