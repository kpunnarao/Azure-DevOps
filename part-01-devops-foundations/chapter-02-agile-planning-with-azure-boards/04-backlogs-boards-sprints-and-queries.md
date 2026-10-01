# Backlogs, Boards, Sprints, and Queries

> Chapter 2 — Agile Planning with Azure Boards

[← Previous](03-area-paths-and-iteration-paths.md) · [Chapter home](README.md) · [Next →](05-definition-of-ready-and-definition-of-done.md)

## Purpose

Azure Boards offers several views over the same work-item data. Each is optimized for a different question. Using the wrong view leads to duplicated lists and unnecessary reporting work.

## View comparison

| Tool | Primary question | Best use |
|---|---|---|
| Product backlog | What should we deliver next? | Prioritization, refinement, forecasting |
| Portfolio backlog | How do lower-level items support larger outcomes? | Epics, features, rollup, roadmap |
| Kanban board | Where is work in the workflow? | Flow, WIP, blocking, policies |
| Sprint backlog | What did we select for this iteration? | Sprint planning and scope |
| Taskboard | How is sprint implementation progressing? | Tasks, remaining work, daily coordination |
| Query | Which items match defined criteria? | Search, audit, bulk updates, alerts, charts |
| Dashboard | What does an audience need to decide? | Shared operational and delivery signals |
| Delivery Plan | How does scheduled work align across teams? | Cross-team iteration and dependency view |

## Backlogs

A backlog is a prioritized list. Position matters: higher items should generally be more valuable, better understood, and closer to readiness.

Good refinement:

- Clarifies outcome and acceptance
- Splits oversized work
- Identifies dependencies and risks
- Adds enough estimate for planning
- Removes obsolete items
- Keeps near-term items better prepared than distant ones

A backlog is not a promise that every item will be delivered.

## Boards

A board visualizes workflow states. Configure columns around meaningful flow such as Ready, Development, Review, Validation, and Done. Avoid a column for every job title.

For each column define:

- Entry condition
- Exit condition
- Owner of action
- WIP limit
- Blocked-work policy
- Expected evidence

Split columns can distinguish Doing and Done within a workflow step. WIP limits should trigger a conversation; they are not decorative numbers.

## Sprints and taskboards

A sprint is a time-box selected through Iteration Paths. Sprint tools support capacity, tasks, remaining work, burndown, and daily execution.

Protect the Sprint Goal while allowing learning. If urgent work enters, make the tradeoff visible. Do not hide scope change by silently replacing work.

## Queries

Queries provide reusable selection logic. Useful shared queries include:

- Blocked work
- Active items without recent updates
- Backlog items without parents
- Items missing acceptance criteria
- Bugs by severity and age
- Work without an owner
- Items changed after sprint start
- Completed work without linked development evidence
- Cross-team dependencies

Store personal experiments in My Queries and team-operational queries in Shared Queries with clear ownership.

## Dashboards

A dashboard should serve an audience:

- **Team:** flow, blocked work, build health, sprint goal, defects
- **Product:** outcomes, feature progress, lead time, release confidence
- **Operations:** deployment health, incidents, reliability signals
- **Leadership:** trends, risks, dependencies, outcome progress

Do not combine every metric into one executive dashboard.

## Common mistakes

- Duplicating work items to make them appear in different views
- Ranking backlog by who requested the work rather than value and risk
- Treating every board movement as administrative reporting
- Configuring columns that do not map cleanly to workflow states
- Ignoring board filters when diagnosing missing work
- Creating shared queries with no owner
- Using sprint burndown without examining scope change
- Building dashboards that display activity but support no decision

## Interview preparation

**Q: Backlog versus board?**  
The backlog prioritizes what should be delivered. The board visualizes how selected work flows through the team's process.

**Q: Sprint backlog versus product backlog?**  
The product backlog contains prioritized future work. A sprint backlog is the subset assigned to a selected iteration, along with implementation detail.

**Q: When would you use a query instead of a board?**  
Use a query for precise reusable criteria, audits, cross-team searches, bulk changes, alerts, or chart sources. Use a board for workflow and flow management.

## Practical exercise

Create the Guest Checkout items, prioritize them in a backlog, configure workflow columns and WIP limits, assign one story to a sprint, create tasks, and build queries for blocked and unparented items. Explain what each view reveals.

## Further reading

- [Use backlogs to manage projects](https://learn.microsoft.com/en-us/azure/devops/boards/backlogs/backlogs-overview)
- [Scrum work processes](https://learn.microsoft.com/en-us/azure/devops/boards/sprints/scrum-overview)
- [Azure Boards documentation](https://learn.microsoft.com/en-us/azure/devops/boards/)
