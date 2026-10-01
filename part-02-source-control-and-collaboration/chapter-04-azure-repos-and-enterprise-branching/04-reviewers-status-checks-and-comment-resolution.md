# Reviewers, Status Checks, and Comment Resolution

> Chapter 4 — Azure Repos and Enterprise Branching Strategies

[← Previous](03-branch-policies-and-build-validation.md) · [Chapter home](README.md) · [Next →](05-merge-strategies.md)

## Purpose

These controls answer different questions:

- **Reviewer:** does an accountable human judge the change acceptable?
- **Status check:** does an integrated system report a required result?
- **Comment resolution:** have review concerns been explicitly handled?

Using all three appropriately provides stronger evidence than simply increasing reviewer count.

## Reviewer design

### Minimum reviewers

Select a number that provides meaningful independent review without creating unnecessary queues. More reviewers can reduce accountability when everyone assumes someone else examined the change.

Consider:

- Risk and criticality
- Team size and time zones
- Knowledge concentration
- Separation-of-duties requirements
- Self-approval policy
- Resetting votes after changes
- Availability and escalation

### Automatically included reviewers

Add domain owners based on repository paths, such as:

- Identity and authorization
- Infrastructure
- Database schemas
- Shared libraries
- Compliance configuration
- Pipeline templates

Keep ownership current. A departed or overloaded required reviewer can stop delivery.

### Required versus optional

Required reviewers block completion according to policy. Optional reviewers provide expertise or awareness. Do not mark everyone required.

## Quality review

Review dimensions:

- Alignment with requirement
- Correctness and edge cases
- Security and privacy
- Failure and recovery behavior
- Test design
- Maintainability and readability
- Compatibility and migration
- Observability and support
- Performance and cost where relevant
- Documentation

Style rules that can be automated should not consume most review attention.

## Status checks

Status checks allow external or integrated systems to report PR status. Examples:

- Security analysis
- License compliance
- Architecture policy
- Change-management system
- External CI
- Artifact evaluation

A useful blocking check must be deterministic enough to trust, clearly owned, observable, and supported. Define timeout and failure handling. Do not create a control nobody can diagnose.

## Comment lifecycle

Azure Repos comments can be active, pending, resolved, won't fix, or closed. Team policy should clarify who resolves a thread and when.

Good practice:

- Reviewer states the concern and impact
- Author explains the response or asks for clarification
- Material code changes trigger re-review
- Won't fix includes rationale and follow-up where necessary
- Resolution does not erase discussion
- Blocking comments remain active until addressed

A comment-resolution policy ensures active comments do not disappear beneath approval.

## Review service expectations

Define:

- Expected first-response time
- How authors select reviewers
- Backup reviewer paths
- How urgent reviews are handled
- Maximum PR size guidance
- How stale PRs are closed
- When votes reset
- When synchronous discussion is more efficient

Slow review is a flow problem. Measure wait time and workload rather than blaming individuals.

## Common mistakes

- Requiring every specialist on every PR
- Allowing author-selected friendly approval to satisfy all control
- Treating status checks as trusted without securing the posting identity
- Resolving comments without response
- Leaving “won't fix” with no rationale
- Approving before validation finishes
- Failing to reset or reconsider approval after major changes
- Using human review for formatting that tooling can enforce

## Interview preparation

**Q: Build validation versus status check?**  
Build validation runs an Azure Pipeline policy for the proposed change. A status check consumes a status posted by an integrated service or system. Both can be blocking.

**Q: How many reviewers should be required?**  
Enough for independent, accountable review given risk and separation needs, but not so many that ownership diffuses and flow stops. There is no universal number.

**Q: Who should resolve a comment?**  
Team policy should decide. A strong pattern is that the author responds and the reviewer confirms material concerns; automated comment-resolution policy ensures no active thread is forgotten.

**Q: What makes a status check trustworthy?**  
A protected posting identity, clear ownership, deterministic evaluation, secure inputs, observable operation, and a defined failure path.

## Practical exercise

Configure a path-based reviewer and a mock or available status check. Create comments with each lifecycle state. Document which comments block completion and how reviewer votes behave after source updates.

## Further reading

- [Review pull requests](https://learn.microsoft.com/en-us/azure/devops/repos/git/review-pull-requests)
- [Set and manage branch policies](https://learn.microsoft.com/azure/devops/repos/git/branch-policies)
