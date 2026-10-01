# Work Item Links and Traceability

> Chapter 2 — Agile Planning with Azure Boards

[← Previous](07-flow-metrics-and-dashboards.md) · [Chapter home](README.md)

## Purpose

Work-item links express meaning. They support hierarchy, dependency management, test coverage, defect relationships, cross-organization work, and connections to development artifacts. Good links make navigation and analysis easier; careless links create an unreadable network.

## Link families

### Hierarchical links

- Parent
- Child

Use these for Epic → Feature → Story/PBI/Requirement → Task. A child should have one clear parent.

### Dependency links

- Predecessor
- Successor

Use these when one item must precede or enable another. Do not use parent-child links merely because two items are sequential.

### Associative and defect links

- Related
- Duplicate / Duplicate Of
- Tests / Tested By
- Other process-supported test or defect relationships

Use Related only when no more specific semantic relationship fits.

### Development and build links

Supported connections can include:

- Branch
- Commit
- Pull Request
- Build
- Found in Build
- Integrated in Build
- Integrated in Release Environment

Availability depends on platform, repository, pipeline, and integration configuration.

### External and remote links

Azure Boards can link URLs, supported GitHub artifacts, and work items in other organizations through supported link types.

## Linking strategy

Define which relationships are required and why:

| Relationship | Why it matters | Preferred creation |
|---|---|---|
| Story to Feature | Portfolio rollup | Backlog mapping or explicit parent |
| Task to Story | Execution context | Sprint/backlog task creation |
| Dependency | Sequencing and risk | Explicit predecessor/successor |
| Commit/PR to work | Change intent and review | Development integration |
| Build/test to source | Validation evidence | Pipeline |
| Deployment to work | Release evidence | Pipeline environment integration |
| Bug to build/version | Impact and diagnosis | Test/deployment workflow |

Automate reliable links. Manual linking remains useful for semantic relationships requiring human judgment.

## Querying linked work

Azure Boards queries can return:

- Flat lists
- Work items and direct links
- Trees of work items

Use tree queries for hierarchy and direct-link queries for dependencies or associations. Keep shared queries named by purpose and document expected link direction.

## Traceability quality checks

Create queries or reviews for:

- Orphan stories without a Feature where portfolio policy requires one
- Tasks without a parent
- Active dependencies past their due or target iteration
- Completed items without development links
- Bugs without affected build/version evidence
- Test cases not linked to requirements where required
- Features with no active child delivery
- Cross-team items with ambiguous ownership

Not every item needs every link. Apply traceability proportionate to risk and decision needs.

## Common mistakes

- Using Related for every relationship
- Reversing predecessor and successor
- Building same-level parent-child hierarchies
- Linking a work item to a branch but not to the actual pull request or commit
- Assuming the link proves acceptance
- Duplicating work instead of linking it
- Creating dependencies with no owner or review process
- Retaining links to deleted or inaccessible artifacts without detection

## Interview preparation

**Q: Parent-child versus predecessor-successor?**  
Parent-child represents decomposition or containment. Predecessor-successor represents execution dependency or sequence.

**Q: How do links support auditing?**  
They connect intent, implementation, review, validation, artifact, and deployment evidence so an auditor or engineer can reconstruct the change.

**Q: Would you require a work item for every commit?**  
Not necessarily. Set policy based on risk and workflow. Protected production changes normally need meaningful intent and traceability, while documentation or automated maintenance may use appropriately scoped items or policies.

**Q: How do you find orphaned backlog items?**  
Use a tree or direct-link query designed to identify items without the expected parent relationship, then review whether the policy genuinely applies.

## Practical exercise

For the Guest Checkout feature:

1. Create the hierarchy.
2. Add a predecessor dependency on an identity-policy decision.
3. Link a test case to a story.
4. Link a bug to the relevant story and build.
5. Associate a branch, commit, and pull request.
6. Create a query that displays the hierarchy.
7. Create a query for unresolved dependencies.
8. Explain the meaning of every link.

## Review checklist

- [ ] Link types communicate intent
- [ ] Hierarchy crosses appropriate backlog levels
- [ ] Dependencies have an owner and are reviewed
- [ ] Development links identify exact changes
- [ ] Test and defect evidence is connected where required
- [ ] Queries detect important gaps
- [ ] Retention preserves necessary evidence
- [ ] Sensitive information is not exposed through links

## Further reading

- [Link work items to objects](https://learn.microsoft.com/en-us/azure/devops/boards/backlogs/add-link)
- [Link types reference](https://learn.microsoft.com/en-us/azure/devops/boards/queries/link-type-reference)
- [About work items](https://learn.microsoft.com/en-us/azure/devops/boards/work-items/about-work-items)
