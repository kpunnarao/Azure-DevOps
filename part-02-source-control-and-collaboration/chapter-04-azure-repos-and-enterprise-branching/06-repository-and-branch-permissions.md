# Repository and Branch Permissions

> Chapter 4 — Azure Repos and Enterprise Branching Strategies

[← Previous](05-merge-strategies.md) · [Chapter home](README.md) · [Next →](07-monorepo-versus-multiple-repositories.md)

## Purpose

Azure Repos permissions control who can read, contribute, create branches or tags, manage policy, rewrite history, delete references, and bypass controls. Effective access can result from several group memberships and inheritance levels.

## Permission scopes

- All repositories in a project
- Individual repository
- Individual branch or branch folder

Repositories inherit from the project-level Git repositories entry. Branches inherit applicable permissions from the repository. More specific assignments and group memberships affect effective permission.

## Permission states

- **Not set:** does not grant or deny; another group or inherited scope can still grant.
- **Allow:** grants unless an applicable Deny overrides.
- **Deny:** generally overrides Allow and should be used carefully.

Prefer group-based Allow and inherited Not set. Use Deny when a deliberate hard boundary must override an otherwise applicable Allow. A Deny assigned to a broad group can unexpectedly affect administrators who are members.

## Important permissions

- Read
- Contribute
- Create branch
- Create tag
- Manage notes
- Edit policies
- Manage permissions
- Force push, which includes history rewriting and branch/tag deletion implications
- Bypass policies when completing pull requests
- Bypass policies when pushing
- Remove others' locks
- Create, delete, or rename repository

Exact availability and defaults depend on scope and product version.

## Bypass distinctions

**Bypass when completing PRs** allows authorized completion despite unmet policies through the supported override workflow.

**Bypass when pushing** permits a direct push that bypasses branch policy, often with less opportunity for an explicit per-action decision. Treat it as highly privileged.

Do not grant bypass to solve slow policies. Fix the underlying control.

## Recommended group model

Example groups:

- Repository Readers
- Repository Contributors
- Code Owners / Required Reviewers
- Repository Administrators
- Emergency Bypass Operators
- Automation Identities

Assign people through groups. Use direct user assignments only for exceptional, documented cases.

Automation identities should have only the actions they need. A build service that reads source does not need force push or policy management.

## Branch folders

Permissions can constrain branch creation under prefixes such as feature/, users/, release/, or hotfix/. This supports naming and ownership conventions. Remember that local branch creation cannot be prevented; the permission controls publishing to Azure Repos.

## Audit procedure

1. Identify critical repositories and branches.
2. Export or review group memberships.
3. Inspect all-repository, repository, and branch assignments.
4. Trace inherited and direct permissions.
5. Review Deny, force push, bypass, and manage-permission grants.
6. Identify dormant users and service identities.
7. Validate access through a test identity where possible.
8. Document intended ownership.
9. Correct drift through groups.
10. Repeat periodically and after incidents.

## Common mistakes

- Assigning permissions individually
- Using Deny without examining nested groups
- Granting Project Administrators for repository-only work
- Giving pipelines human-style broad contributor access
- Treating branch policy as permission
- Granting force push so users can delete feature branches
- Allowing many people to bypass policies
- Forgetting release branches when auditing protection
- Assuming a UI “Not set” means no access

## Interview preparation

**Q: Not set versus Deny?**  
Not set supplies no grant and allows other applicable grants to take effect. Deny generally overrides Allow and should be reserved for explicit boundaries.

**Q: Branch policy versus branch permission?**  
Policy defines conditions for integrating changes. Permission defines which identities can perform actions such as contribute, edit policy, force push, or bypass.

**Q: Why is bypass-when-pushing especially sensitive?**  
It can directly update a protected branch while bypassing policy, potentially without the deliberate override step available during PR completion.

**Q: How do you manage repository access at scale?**  
Use purpose-specific groups, inheritance, minimal exceptions, automated inventory where possible, periodic review, and separate emergency access.

## Practical exercise

Create a permissions matrix, configure Reader, Contributor, Administrator, and Emergency groups in a learning repo, then test effective read, push, policy editing, force-push, and bypass behavior.

## Further reading

- [Set Git repository permissions](https://learn.microsoft.com/en-us/azure/devops/repos/git/set-git-repository-permissions)
- [Set branch permissions](https://learn.microsoft.com/en-us/azure/devops/repos/git/branch-permissions)
- [Permissions and security groups reference](https://learn.microsoft.com/en-us/azure/devops/organizations/security/permissions)
