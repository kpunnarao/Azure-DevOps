# Azure Boards Process Models

> Chapter 2 — Agile Planning with Azure Boards

[Chapter home](README.md) · [Next →](02-work-item-types-and-hierarchy.md)

## Purpose

A process defines the vocabulary and workflow used to track work. It determines default work-item types, fields, states, backlog levels, and behaviors. Choose the simplest process that represents how the team works and satisfies governance needs.

Azure Boards provides four default processes: Basic, Agile, Scrum, and CMMI.

## Comparison

| Process | Primary backlog item | Typical states | Best starting fit |
|---|---|---|---|
| Basic | Issue | To Do, Doing, Done | Small teams needing minimal structure |
| Agile | User Story | New, Active, Resolved, Closed | Agile teams using story terminology and separate development/test detail |
| Scrum | Product Backlog Item | New, Approved, Committed, Done | Teams following Scrum terminology and sprint planning |
| CMMI | Requirement | Proposed, Active, Resolved, Closed | Formal change control, risk, review, and audit needs |

All processes support higher-level planning, tasks, and defects, but their default terminology and fields differ.

## Process, process model, and process template

- A **process** defines work tracking for projects using the inherited model.
- A **process model** determines how work tracking can be customized.
- A **process template** is used in XML-based models, particularly supported Azure DevOps Server scenarios.

Do not assume instructions for Azure DevOps Services inherited processes apply unchanged to an on-premises XML model.

## Selection guidance

### Choose Basic when

- The team is new to structured work tracking
- Issue, Task, and Epic are sufficient
- Minimal fields and states are preferred
- The project does not require richer default portfolio hierarchy

### Choose Agile when

- The team uses User Stories
- Story Points and Agile workflow terminology fit
- Bugs and test-related work need distinct tracking
- The organization wants a flexible general-purpose process

### Choose Scrum when

- Product Backlog Item terminology is preferred
- The team uses commitment-oriented sprint workflow
- Scrum's backlog language fits existing practice

Choosing Scrum does not make a team Scrum. Roles, goals, events, inspection, and adaptation must exist in practice.

### Choose CMMI when

- Formal requirements and change management are necessary
- Risks, reviews, and audit history need strong representation
- The organization uses a maturity or compliance-oriented lifecycle

Do not choose CMMI merely to appear controlled. Unused fields and states create noise.

## Customization strategy

Prefer an inherited process based on a default process. Customize only when a stable business requirement cannot be represented with existing fields, tags, links, states, or work-item types.

Before adding a field or state, answer:

1. What decision will this information support?
2. Who owns the value?
3. When is it updated?
4. Can it be derived?
5. Does it expose sensitive data?
6. How will historical and existing work items behave?
7. Will reporting and integrations understand it?

Avoid per-project process variants without governance. They make reporting, automation, training, and migration more difficult.

## Changing a process

Changing a project process can map work-item types and states, but it requires preparation:

- Inventory customizations, queries, dashboards, integrations, rules, and reports
- Define mappings and exceptions
- Test in a representative non-production project
- Update board column mappings
- Validate permissions and automation
- Communicate terminology changes
- Recheck historical reporting

## Common mistakes

- Choosing by name instead of actual workflow
- Using custom states for temporary labels
- Adding mandatory fields that users cannot answer when work is created
- Mixing competing estimation fields across teams
- Using state names to represent every technical handoff
- Changing the process without checking queries and dashboards
- Assuming a process enforces good Agile practice

## Interview preparation

**Q: Agile process versus Scrum process?**  
Agile uses User Stories and states such as New, Active, Resolved, and Closed. Scrum uses Product Backlog Items and Scrum-oriented states. Choose based on the team's vocabulary and workflow, not the belief that one is inherently more Agile.

**Q: When would you choose CMMI?**  
When formal requirements, reviews, risks, change management, and auditability justify the additional structure.

**Q: How do you govern process customization?**  
Require a decision-oriented use case, an owner, reporting impact analysis, security review, compatibility assessment, and testing. Prefer shared inherited processes and minimize variants.

## Practical exercise

Create one example project in Basic and one in Agile. Model the same feature in both. Compare hierarchy, fields, states, bug handling, effort tracking, and user experience. Write a short decision record selecting one.

## Further reading

- [Default processes and process templates](https://learn.microsoft.com/en-us/azure/devops/boards/work-items/guidance/choose-process)
- [Plan and track work in Azure Boards](https://learn.microsoft.com/en-us/azure/devops/boards/get-started/plan-track-work)
