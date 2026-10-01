# Capacity, Estimation, Velocity, and Forecasting

> Chapter 2 — Agile Planning with Azure Boards

[← Previous](05-definition-of-ready-and-definition-of-done.md) · [Chapter home](README.md) · [Next →](07-flow-metrics-and-dashboards.md)

## Purpose

These concepts answer different planning questions. Mixing them creates false precision.

| Concept | Question |
|---|---|
| Capacity | How much working time is realistically available this sprint? |
| Estimate | How large, complex, or uncertain is this work relative to other work? |
| Velocity | How much estimated backlog work has this team completed historically? |
| Forecast | Given ordering and historical evidence, how far might the team progress? |

## Capacity

Azure Boards capacity considers team members, working days, time off, activity, and capacity per day. It is most useful when tasks use remaining work in time units.

Capacity is not the same as velocity:

- Capacity reflects available task time for the current sprint.
- Velocity reflects completed estimated backlog work across prior sprints.

Reduce capacity for support rotation, ceremonies, learning, incidents, and other real responsibilities instead of planning as if every hour is feature development.

## Estimation

Estimates are models, not promises. Common approaches include:

- Relative points
- T-shirt sizes
- Idealized effort
- Time estimates for tasks
- Throughput-based forecasting without story points

Relative estimates commonly consider volume, complexity, uncertainty, and risk. A “5” is useful only relative to the same team's other work under a reasonably stable method.

Do not translate points into individual performance or fixed hours. Doing so encourages gaming and disguises uncertainty.

## Velocity

Azure Boards can sum Story Points, Effort, or Size completed per iteration, depending on process. Use several stable iterations to create a range, not a single target.

Velocity changes when:

- Team membership or availability changes
- Estimation scale changes
- Work slicing changes
- Definition of Done changes
- Interruptions or dependencies change
- Items are completed late or carried over

Therefore, comparing velocities across teams is invalid.

## Forecasting

Forecasting can use historical velocity, item estimates, throughput, or probabilistic methods. Communicate ranges and assumptions.

Example:

- Ordered backlog totals 80 points.
- Recent completed velocity is 16–22 points per sprint.
- A rough forecast is four to five sprints, assuming stable team, slicing, quality policy, and interruption rate.

This is a planning range, not a commitment date.

## Sprint planning workflow

1. Establish a Sprint Goal.
2. Review historical completion and carryover.
3. Set team and member capacity.
4. Select high-priority Ready items.
5. Consider dependencies, skills, and operational obligations.
6. Break down tasks where useful.
7. Check capacity and risk.
8. Remove work until the plan is credible.
9. Make assumptions visible.
10. Inspect and adapt during the sprint.

## Common mistakes

- Filling capacity to 100 percent with planned feature tasks
- Treating velocity as a target that must rise
- Comparing teams using story points
- Averaging estimates from incompatible scales
- Changing estimates after completion to make reporting look accurate
- Counting partially completed stories as delivered
- Forecasting from too little history
- Ignoring scope change and carryover
- Giving a single date without confidence or assumptions

## Interview preparation

**Q: Capacity versus velocity?**  
Capacity estimates available task time for the current sprint. Velocity summarizes completed estimated backlog work from prior sprints.

**Q: Why not compare team velocity?**  
Points are locally defined and influenced by team composition, slicing, workflow, and Definition of Done. Cross-team comparison rewards inflation rather than value.

**Q: How would you forecast a release?**  
Order the remaining backlog, use a range based on stable historical completion or throughput, account for capacity and dependencies, state assumptions, and update the forecast as evidence changes.

**Q: What if the team completes more work after estimates increase?**  
Check whether actual throughput and outcomes improved. A higher point total alone may reflect scale inflation rather than increased delivery.

## Practical exercise

Using six fictional sprint velocities—18, 22, 17, 21, 20, and 12—investigate why the last sprint differs before forecasting. Create a range, list assumptions, and explain how team leave or an incident would alter it.

## Further reading

- [Key concepts for Sprints and Scrum tools](https://learn.microsoft.com/en-us/azure/devops/boards/sprints/scrum-key-concepts)
- [Set sprint capacity](https://learn.microsoft.com/en-us/azure/devops/boards/sprints/set-capacity)
