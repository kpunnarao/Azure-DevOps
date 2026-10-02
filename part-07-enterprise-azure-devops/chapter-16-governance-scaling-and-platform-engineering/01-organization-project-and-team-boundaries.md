# Organization, Project, and Team Boundaries

[← Chapter 16](README.md) · [Next: Platform and Product Ownership →](02-platform-teams-and-product-team-ownership.md)

## Boundaries have consequences

An Azure DevOps organization is the top cloud-service administration/billing/identity boundary. Projects contain Boards, Repos, Pipelines, Test Plans, Artifacts, teams, and permissions. Teams provide backlog/board/iteration views through area and iteration configuration. Repositories and protected resources add object-level boundaries.

Do not create one project per application automatically. Projects increase isolation and administrative independence but make cross-project queries, artifacts, permissions, templates, service identities, and portfolio reporting more complex.

Microsoft documentation describes both single-project scaling and multiple-project designs and publishes current service/object limits. Limits are capacity constraints, not architecture targets.

## Decision criteria

Create separate organizations when tenant, geography/data residency, acquisition, contractual isolation, billing/administration, or policy autonomy demands it. Cross-organization collaboration and migration are materially harder.

Create separate projects for strong security/administration/process boundaries, external collaboration isolation, business-unit autonomy, or scale. Use teams within a project when products share process, identity, reporting, and resources but need distinct backlogs/iterations.

Keep repositories aligned to independently versioned/deployed ownership. A monorepo can simplify atomic cross-component change while increasing CI selection and permission complexity.

## Area/iteration ownership

Teams select area paths for backlogs and iteration paths for cadence. Overlapping team ownership can double-count work and confuse reporting. Define one primary owning team and deliberate portfolio rollups.

Process customization applies broadly. Govern fields/states so teams do not create incompatible reporting vocabularies.

## Security

Use private projects by default. Manage membership through Entra groups. Limit project/collection administrators and visibility. Object-level isolation inside one project may not satisfy hard tenant/data boundaries. Evaluate guests, service identities, feeds, pools, service connections, and analytics—not repositories alone.

## Migration cost

Moving work items, repos, pipelines, histories, permissions, Test Plans, artifacts, dashboards, and links between projects/organizations is not equally supported. Choose boundaries deliberately and document exit/migration before scale.

## Interview preparation

**One project or many?**  
One improves shared reporting/collaboration and reduces administration; many strengthen autonomy/isolation. Decide from ownership, process, security, scale, and migration—not team count alone.

**Team versus project?**  
A team is a planning/configuration view within a project; a project is a broader resource, permission, and process boundary.

**What is hardest to change later?**  
Cross-organization/project identities, links/history, pipelines/resources, artifacts and process/reporting relationships. Test migration early.

## Practical exercise

Model three teams with shared services and one regulated workload. Produce two topology options, permission/tracing impact, limits, cost, and migration tradeoffs; select one with an ADR.

## Official references

- [About projects and enterprise scaling](https://learn.microsoft.com/azure/devops/organizations/projects/about-projects)
- [Plan organizational structure](https://learn.microsoft.com/azure/devops/user-guide/plan-your-azure-devops-org-structure)
- [Work tracking and project limits](https://learn.microsoft.com/azure/devops/organizations/settings/work/object-limits)

[Next: Platform Teams and Product-team Ownership →](02-platform-teams-and-product-team-ownership.md)
