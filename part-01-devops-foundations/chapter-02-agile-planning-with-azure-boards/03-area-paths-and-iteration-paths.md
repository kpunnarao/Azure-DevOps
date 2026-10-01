# Area Paths and Iteration Paths

> Chapter 2 — Agile Planning with Azure Boards

[← Previous](02-work-item-types-and-hierarchy.md) · [Chapter home](README.md) · [Next →](04-backlogs-boards-sprints-and-queries.md)

## Purpose

Area Paths and Iteration Paths are frequently confused because both filter team views.

- **Area Path answers:** which product area or team owns this work?
- **Iteration Path answers:** in which time period, sprint, release, or planning interval does it belong?

## Area Paths

Area Paths organize work by product, feature area, business domain, or durable team ownership. Teams select one or more Area Paths, and these selections help determine which work appears on their backlogs and boards.

Area Paths can also:

- Support portfolio rollups
- Filter queries and reports
- Restrict work-item permissions at an area node
- Represent product/component ownership

Example:

```text
OnlineStore
├── Checkout
├── Catalog
├── Fulfillment
└── Platform
```

Prefer durable product domains over temporary project phases or employee names. Keep the tree shallow unless reporting and permissions require hierarchy.

## Iteration Paths

Iteration Paths represent time. They can be flat or hierarchical and usually have start and end dates.

Example:

```text
OnlineStore
└── 2027
    ├── Sprint 01
    ├── Sprint 02
    └── Sprint 03
```

Each team selects the iterations it uses. Iteration Paths enable sprint backlogs, capacity, burndown, velocity, forecasting, and Delivery Plans.

A Kanban team that does not plan with sprints can retain a backlog iteration and use continuous flow rather than forcing every item into a sprint.

## Team settings

A team has:

- Selected Area Paths
- A default Area Path
- Selected Iteration Paths
- A backlog Iteration Path
- A default Iteration Path

The **backlog iteration** defines the iteration hierarchy included in product and portfolio backlogs. The **default iteration** is assigned to new items created in team context. They are related but not identical.

Items created from different interfaces may receive different defaults. Always verify path values when work is missing.

## Visibility logic

A work item typically appears in a team's view when its:

- Area Path matches a selected team area
- Iteration Path fits the relevant team scope
- Work-item type belongs to an enabled backlog level
- State is eligible for the view
- Hierarchy position is a visible leaf where applicable

This means “I have permission to open the work item” does not guarantee “the item appears on my team's board.”

## Multi-team design

For feature teams:

- Give each a clear default Area Path
- Minimize overlapping ownership
- Use a portfolio or management team that selects several areas for rollup
- Define shared iteration dates when cross-team planning needs alignment
- Avoid assigning one work item to several teams through overlapping paths

A work item has one Area Path. Cross-team work should be split into owned items and connected with dependency or related links rather than using ambiguous ownership.

## Permissions warning

Area-level permissions can restrict viewing or editing work items, but they require careful inheritance analysis. Do not treat Area Paths as the only security boundary for highly sensitive data. Work-item history and identity references may have visibility implications.

## Common troubleshooting

**Item missing from board**

1. Confirm the selected team.
2. Check Area Path.
3. Check backlog and selected Iteration Paths.
4. Check work-item type and backlog configuration.
5. Check workflow state.
6. Check parent-child nesting and leaf behavior.
7. Refresh team configuration and filters.

**Item appears on two team boards**

The teams probably select overlapping Area Paths or include subareas. Define one owning team and use portfolio views for aggregation.

## Common mistakes

- Using Iteration Path for team ownership
- Using Area Path as a release version
- Creating an Area Path per sprint
- Selecting the root Area Path for every team
- Renaming paths without checking queries and integrations
- Deleting old iterations instead of archiving or retaining history appropriately
- Assuming defaults apply identically in every creation interface

## Interview preparation

**Q: Area Path versus Iteration Path?**  
Area Path represents product or ownership classification. Iteration Path represents time, such as a sprint or release interval.

**Q: Why is a work item missing from a team board?**  
Check selected team, Area Path, backlog Iteration Path, work-item type, state, hierarchy position, and filters. Permission to view is not the same as inclusion in the team view.

**Q: How do you support portfolio visibility across teams?**  
Give feature teams distinct Area Paths, then configure a management team to select those areas and focus on higher backlog levels such as Features and Epics.

## Further reading

- [About teams and Agile tools](https://learn.microsoft.com/en-us/azure/devops/organizations/settings/about-teams-and-settings)
- [Define Iteration Paths and team iterations](https://learn.microsoft.com/en-us/azure/devops/organizations/settings/set-iteration-paths-sprints)
- [Manage and configure team tools](https://learn.microsoft.com/en-us/azure/devops/organizations/settings/manage-teams)
