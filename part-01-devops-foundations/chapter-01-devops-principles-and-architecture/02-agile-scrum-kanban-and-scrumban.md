# Agile, Scrum, Kanban, and Scrumban

> Chapter 1 — DevOps Principles and Azure DevOps Architecture

[← Previous](01-devops-culture-and-continuous-improvement.md) · [Chapter home](README.md) · [Next →](03-continuous-integration-delivery-and-deployment.md)

## Purpose

Agile is a set of values and principles for learning through incremental delivery. Scrum and Kanban are ways to organize that work. Scrumban is a pragmatic combination. They support DevOps but do not replace engineering automation, operational ownership, or production feedback.

## Relationship among the approaches

| Approach | What it is | Cadence | Primary control | Common measures |
|---|---|---|---|---|
| Agile | Values and principles | Adaptive | Feedback and learning | Outcomes and responsiveness |
| Scrum | Framework with roles, events, and artifacts | Fixed-length sprints | Sprint goal and backlog | Sprint goal, velocity, burndown |
| Kanban | Flow-management method | Continuous | WIP limits and pull | Cycle time, throughput, WIP |
| Scrumban | Scrum cadence plus Kanban flow practices | Hybrid | Goals plus WIP limits | Goal progress and flow |

## Agile

Agile favors incremental delivery, collaboration, customer feedback, and responding to change. It does not mean the absence of architecture, documentation, deadlines, or governance. It changes planning from a one-time prediction into a continuous activity.

Healthy Agile delivery includes:

- A prioritized backlog connected to outcomes
- Small vertical slices of value
- Frequent integration and validation
- Feedback from stakeholders and production
- Regular inspection and adaptation
- Technical excellence that keeps change affordable

## Scrum

Scrum organizes work into time-boxed sprints, commonly one to four weeks. Core accountabilities are Product Owner, Scrum Master, and Developers. Core events include Sprint Planning, Daily Scrum, Sprint Review, and Sprint Retrospective.

Use Scrum when a team benefits from a stable cadence, a clear Sprint Goal, and regular stakeholder review. Do not turn Scrum into a ticket factory. A sprint should produce a usable increment and learning.

Azure Boards supports Scrum through Product Backlog Items, sprint backlogs, taskboards, capacity, burndown, velocity, and the Scrum process.

## Kanban

Kanban focuses on flow:

1. Visualize the workflow.
2. Limit work in progress.
3. Manage flow.
4. Make policies explicit.
5. Establish feedback loops.
6. Improve collaboratively.

Work is pulled when capacity is available rather than pushed according to a sprint commitment. Kanban fits support, platform, operations, and other work with variable arrival patterns. It can also improve a product-development workflow.

In Azure Boards, map columns to meaningful workflow states, set WIP limits, use explicit exit criteria, and inspect cumulative flow, cycle time, and blocked work.

## Scrumban

Scrumban commonly retains a Scrum planning or review cadence while managing day-to-day execution with Kanban practices. A team might keep a two-week goal but use WIP limits and pull work across the board.

Use a hybrid intentionally. Do not call an inconsistent process “Scrumban” merely because some Scrum ceremonies and a board exist.

## Choosing an approach

Ask:

- Is work planned around coherent goals or does it arrive continuously?
- How predictable is demand?
- Does the team need a regular stakeholder cadence?
- How frequently do priorities change?
- Are bottlenecks and multitasking the primary problems?
- Can work be sliced small enough to finish regularly?

| Situation | Likely starting point |
|---|---|
| Product team delivering increments toward a goal | Scrum |
| Operations or support with unpredictable requests | Kanban |
| Scrum team with excessive WIP and spillover | Scrumban |
| New team learning iterative delivery | Simple Scrum or Kanban; avoid heavy customization |

## Common mistakes

- Treating story points as hours
- Comparing teams by velocity
- Starting more work whenever someone is free
- Carrying unfinished work through many sprints without root-cause analysis
- Using board columns that reflect job roles rather than value flow
- Holding ceremonies without decisions or learning
- Changing sprint scope silently
- Ignoring urgent work instead of designing an explicit expedite policy

## Practical exercise

Take ten recent work items. Plot when each entered active work and when it completed. Identify:

- Average and variation in cycle time
- How many items were active simultaneously
- How often work was blocked
- How much work crossed sprint boundaries
- Whether the team completed goals or merely closed unrelated tickets

Recommend one process change and define how its effect will be evaluated.

## Interview preparation

**Q: What is the difference between Agile and Scrum?**  
Agile is a set of values and principles. Scrum is one framework that implements Agile through defined accountabilities, events, artifacts, and time-boxed sprints.

**Q: Scrum or Kanban—how do you choose?**  
Choose based on the nature of demand and the improvement needed. Scrum supports goal-oriented increments and cadence. Kanban supports continuous flow and variable demand. Both require explicit policies and feedback.

**Q: Why limit WIP?**  
Excess WIP increases context switching, queues, and cycle time. WIP limits encourage finishing, expose bottlenecks, and make flow problems visible.

**Q: Can Azure Boards enforce an Agile culture?**  
No. It can represent backlogs, workflows, policies, and metrics. Culture depends on how people prioritize, collaborate, learn, and respond to evidence.

## Further reading

- [What is Agile development?](https://learn.microsoft.com/en-us/devops/plan/what-is-agile-development)
- [What is Scrum?](https://learn.microsoft.com/en-us/devops/plan/what-is-scrum)
- [What is Kanban?](https://learn.microsoft.com/en-us/devops/plan/what-is-kanban)
