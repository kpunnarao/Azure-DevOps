# Trunk-Based Development and GitFlow

> Chapter 4 — Azure Repos and Enterprise Branching Strategies

[Chapter home](README.md) · [Next →](02-pull-request-lifecycle.md)

## Purpose

A branching strategy coordinates integration, release, and support. The best strategy is the simplest one that satisfies delivery cadence, validation, release, and maintenance requirements.

## Trunk-based development

Developers integrate small changes into a single primary branch—often main—at least daily. Short-lived feature branches and pull requests may be used, but they exist briefly.

Key practices:

- High-quality protected main branch
- Small changes
- Fast pull-request validation
- Feature flags for incomplete user exposure
- Automated testing and deployment
- Frequent integration
- Rapid repair when main fails

Benefits:

- Less merge debt
- Faster feedback
- Easier continuous delivery
- Fewer long-running parallel histories

Requirements:

- Strong automation
- Ability to slice work
- Safe feature-flag lifecycle
- Team discipline around broken builds
- Architecture that tolerates incremental change

## GitFlow

Traditional GitFlow uses long-lived main and develop branches, feature branches, release branches, and hotfix branches.

It can help when:

- Releases are infrequent and packaged
- Several versions require active support
- A formal stabilization period exists
- Production source differs from ongoing development for legitimate reasons

Costs include:

- More merges and policy surfaces
- Delayed integration
- Risk of fixes missing one branch
- Complex release provenance
- Longer feedback loops

Do not adopt GitFlow by habit for a continuously delivered service.

## Practical middle ground

Many teams use:

- Protected main
- Short-lived feature and bug-fix branches
- Pull requests
- Tags for released commits
- Release branches only for actively supported versions
- Fixes applied to main first, then ported to release branches where feasible

This keeps the mainline authoritative while supporting maintenance.

## Feature branches and feature flags

A branch isolates source history. A feature flag separates deployment from user exposure. Long-lived branches are usually a poor substitute for flags because they accumulate integration risk.

Feature flags require:

- Owner
- Default behavior
- Testing of relevant states
- Observability
- Security review for privileged features
- Removal date and cleanup work

## Decision factors

| Factor | Favor trunk-based | Favor release-oriented branching |
|---|---|---|
| Release frequency | Frequent | Infrequent or packaged |
| Supported versions | One current service | Multiple maintained versions |
| Automation | Strong | Limited but improving |
| Change size | Small | Large planned releases |
| Feature exposure | Flags available | Coupled to release |
| Compliance | Automated evidence | Formal stabilization may be required |

Compliance does not automatically require GitFlow. Controls can often be implemented with protected main, immutable artifacts, environment approvals, and traceability.

## Branch naming

Use a predictable convention, for example:

- feature/<work-item>-<description>
- bugfix/<work-item>-<description>
- release/<version>
- hotfix/<work-item>-<description>
- users/<alias>/<description>

Avoid personal data or sensitive incident detail in branch names.

## Common mistakes

- Long-lived “feature” branches
- One branch per environment
- Permanent develop branch with no clear purpose
- Release branches never retired
- Fixing only the release branch and losing the correction from main
- Feature flags with no removal plan
- Measuring productivity by branch or commit count
- Choosing GitFlow because a diagram looks comprehensive

## Interview preparation

**Q: Trunk-based development versus GitFlow?**  
Trunk-based development integrates small changes frequently into a protected mainline. GitFlow uses additional long-lived development and release branches. Choose based on delivery and support needs, accepting GitFlow's integration overhead.

**Q: Why avoid environment branches?**  
They mix code promotion with source divergence. Prefer one immutable artifact promoted through environments with environment-specific configuration.

**Q: How do you manage incomplete features on main?**  
Slice them into safe increments and use well-governed feature flags where user exposure must be delayed.

**Q: When are release branches justified?**  
When a released line requires ongoing fixes independently from current main development, such as multiple supported product versions.

## Practical exercise

Design strategies for two systems: a continuously deployed API and a quarterly desktop product supporting two versions. Draw their flows and explain branch lifetime, validation, tagging, and hotfix propagation.

## Further reading

- [Azure Repos Git branching guidance](https://learn.microsoft.com/en-us/azure/devops/repos/git/git-branching-guidance)
- [About branches and policies](https://learn.microsoft.com/en-us/azure/devops/repos/git/branch-policies-overview)
