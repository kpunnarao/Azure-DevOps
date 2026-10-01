# Definition of Ready and Definition of Done

> Chapter 2 — Agile Planning with Azure Boards

[← Previous](04-backlogs-boards-sprints-and-queries.md) · [Chapter home](README.md) · [Next →](06-capacity-estimation-velocity-and-forecasting.md)

## Purpose

Definition of Ready and Definition of Done are explicit team policies that improve shared understanding. Azure DevOps can record or visualize them, but the tool does not create agreement or quality.

- **Ready:** the team has enough clarity to begin responsibly.
- **Done:** the increment satisfies the team's complete quality standard.

Ready should enable conversation, not become a bureaucratic approval gate. Done should be strong enough that hidden work is not postponed until release.

## Example Definition of Ready

A backlog item may be Ready when:

- The user or stakeholder and desired outcome are understood
- Acceptance criteria are testable
- The item is small enough for the delivery interval
- Dependencies and significant risks are identified
- Required designs or decisions are available
- Test data and environment needs are understood
- Security, privacy, and operational concerns are identified
- The Product Owner and team share an understanding
- An estimate exists if the team uses estimates

Not every detail must be known. Discovery work may be Ready precisely because uncertainty needs investigation.

## Example Definition of Done

An increment may be Done when:

- Acceptance criteria pass
- Code is reviewed and merged through policy
- Automated tests pass
- Required manual or exploratory tests are complete
- Security and compliance checks pass
- The deployable artifact is versioned and retained
- Configuration and database changes are handled safely
- Documentation and runbooks are updated
- Telemetry and alerts exist where required
- The change is deployed to the team's agreed target
- No unresolved critical defect remains
- Evidence is linked to the work item

Decide whether Done means merged, deployable, deployed to a test environment, or released to production. Ambiguity creates misleading metrics.

## Ready versus acceptance criteria

Ready is a policy for whether work can begin. Acceptance criteria define the behavior or conditions the specific item must satisfy. Do not replace item-specific acceptance with a generic Ready checklist.

## Done versus acceptance criteria

Acceptance criteria prove the requested behavior. Done also covers general quality expectations such as review, security, documentation, deployability, and observability.

## Representing policies in Azure Boards

Options include:

- Markdown in the repository or team wiki
- A linked checklist or template
- Board column entry and exit criteria
- Work-item templates
- Carefully chosen required fields
- Queries that find policy gaps
- Pipeline policies that verify technical requirements

Avoid dozens of required checkboxes. Automate evidence where possible. A pipeline result is more reliable than a manually checked “tests passed” box.

## Evolving the definitions

Review Ready and Done when:

- Work frequently enters a sprint and stalls
- Defects escape
- Operational teams receive undocumented changes
- Items are repeatedly reopened
- Security or compliance evidence is missing
- New delivery capabilities make an old manual requirement obsolete

Changes should strengthen outcomes without adding ritual for its own sake.

## Common mistakes

- Treating Ready as a contract that prevents all change
- Requiring detailed design before discovery
- Defining Done as “developer finished coding”
- Using a different hidden Done standard for production
- Allowing every item to waive quality requirements casually
- Adding manual fields for evidence that automation already produces
- Closing parent items while critical children remain incomplete
- Never revisiting the policy after incidents

## Interview preparation

**Q: Definition of Done versus acceptance criteria?**  
Acceptance criteria are specific to a backlog item. Definition of Done is the team's general quality standard for every increment.

**Q: Is Definition of Ready part of Scrum?**  
It is a commonly used team policy, not a required Scrum artifact. It should improve refinement and flow without becoming a heavy approval gate.

**Q: How would you implement Done in Azure DevOps?**  
Document the policy; map board exit criteria; automate technical evidence through repository and pipeline policies; use templates or minimal fields for non-automated evidence; and audit gaps with queries.

## Practical exercise

Write Ready and Done definitions for the Guest Checkout team. For each criterion mark it as:

- Human judgment
- Automatically verifiable
- Evidence captured elsewhere
- Not necessary

Remove any criterion that supports no risk or decision.

## Further reading

- [Scrum work processes in Azure Boards](https://learn.microsoft.com/en-us/azure/devops/boards/sprints/scrum-overview)
- [Plan and track work](https://learn.microsoft.com/en-us/azure/devops/boards/get-started/plan-track-work)
