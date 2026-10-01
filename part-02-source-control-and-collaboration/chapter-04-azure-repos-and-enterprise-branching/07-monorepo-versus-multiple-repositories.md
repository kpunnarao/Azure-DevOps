# Monorepo versus Multiple Repositories

> Chapter 4 — Azure Repos and Enterprise Branching Strategies

[← Previous](06-repository-and-branch-permissions.md) · [Chapter home](README.md) · [Next →](08-hotfixes-emergency-changes-and-git-lfs.md)

## Purpose

Repository boundaries influence ownership, atomic change, pipeline performance, permissions, dependency management, discoverability, and release independence. They should follow architecture and operating needs, not fashion.

## Monorepo

A monorepo stores multiple components or services in one repository.

### Benefits

- Atomic cross-component changes
- Consistent tooling and policies
- Easier discovery and shared refactoring
- One pull request can update API and consumers
- Unified dependency and standards management

### Costs

- Large checkout and history
- Complex path-based pipelines
- Broad visibility if repository permissions cannot isolate enough
- High review load without code ownership
- Tooling investment for selective build/test
- Accidental coupling
- One policy surface may not fit all components

A monorepo is not one deployable unit. Components can still build and release independently if pipelines and versioning support it.

## Multiple repositories

Each service, component, or bounded context has a repository.

### Benefits

- Clear ownership and permission boundaries
- Independent pipelines and release cadence
- Smaller clones and focused histories
- Policies tailored to the component

### Costs

- Cross-repository changes require coordination
- Shared tooling can drift
- Dependency versions need explicit management
- Discovery and onboarding can become harder
- Too many tiny repositories increase administration

## Decision factors

| Factor | Monorepo tendency | Multiple-repo tendency |
|---|---|---|
| Changes often span components | Strong | Weak |
| Strict code-access isolation | Weak | Strong |
| Independent release cadence | Possible with tooling | Natural |
| Shared standards/refactoring | Easier | Requires automation |
| Repository scale/tooling | Requires investment | Distributed |
| Ownership | Path-based | Repository-based |
| External contributor isolation | Harder | Easier |

## Avoid repository-per-micro-detail

A repository boundary creates lifecycle and governance overhead. Do not create a repository for every library or folder unless independent ownership, release, access, or reuse justifies it.

Conversely, do not keep unrelated systems in one repository merely for convenience.

## Branches versus forks

Use branches when contributors share repository access and policy. Use forks when contributors need an isolated repository boundary, experimentation is intentionally separate, or upstream write access is inappropriate.

In Azure Repos, a fork does not automatically copy permissions, policies, or build pipelines. Changes return through a pull request. Plan how forks are secured, synchronized, validated, and retired.

## Pipeline design

For monorepos:

- Detect affected components carefully
- Use path triggers as optimization, not the only correctness mechanism
- Maintain dependency-aware validation
- Apply path-based reviewers
- Cache selectively
- Run periodic full validation

For multiple repositories:

- Version shared packages
- Use reusable pipeline templates
- Automate dependency updates
- Define compatible API and schema evolution
- Coordinate cross-repo releases
- Maintain centralized discoverability

## Common mistakes

- Choosing monorepo only to avoid package management
- Splitting repos only because teams are separate today
- Using Git submodules as a default dependency solution
- Path filters that skip shared dependency changes
- Giving every monorepo contributor access to sensitive paths
- Duplicating pipeline logic across hundreds of repos
- Treating forks as backups
- No owner or retirement policy for experimental repositories

## Interview preparation

**Q: How do you choose monorepo versus multiple repos?**  
Evaluate change coupling, ownership, access isolation, release independence, scale, tooling, and dependency management. Choose the boundary that matches durable architecture and governance.

**Q: Can a monorepo deploy services independently?**  
Yes. Repository and deployment boundaries are separate. Dependency-aware pipelines can build and release affected services independently.

**Q: Branch versus fork?**  
A branch shares repository permissions and policy. A fork is a separate repository with isolated changes and returns contributions through a PR; its policies and pipelines require separate consideration.

**Q: Main monorepo risk?**  
Without path ownership, selective validation, and scalable tooling, changes become slow, access broadens, and unrelated components become operationally coupled.

## Practical exercise

Model five services with two shared libraries. Score monorepo and multi-repo options against access, change coupling, releases, build time, ownership, discovery, and governance. Write an architecture decision record.

## Further reading

- [Fork an Azure Repos repository](https://learn.microsoft.com/en-us/azure/devops/repos/git/forks)
- [Optimize repository performance](https://learn.microsoft.com/en-us/azure/devops/repos/git/optimize-repository-performance)
- [Azure Repos Git documentation](https://learn.microsoft.com/en-us/azure/devops/repos/git/)
