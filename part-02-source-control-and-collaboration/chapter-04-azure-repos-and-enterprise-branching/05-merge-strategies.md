# Merge Strategies

> Chapter 4 — Azure Repos and Enterprise Branching Strategies

[← Previous](04-reviewers-status-checks-and-comment-resolution.md) · [Chapter home](README.md) · [Next →](06-repository-and-branch-permissions.md)

## Purpose

Azure Repos supports several pull-request completion strategies. The choice affects commit identities, topology, rollback, bisecting, attribution, and how one PR appears in history.

## Azure Repos strategies

### Basic merge — no fast-forward

Creates a merge commit whose parents are target and source tips. Source commit identities remain.

**Strengths:** preserves branch topology and individual commits; one merge commit identifies integration.  
**Tradeoffs:** history can become busy; poor feature commits remain visible.

### Squash merge

Combines source changes into one new commit on the target. Original feature commits are not target ancestors.

**Strengths:** one clean commit per PR; easy PR-level revert; feature fixup noise removed.  
**Tradeoffs:** individual commit history is not retained on target; source commit references can confuse later cherry-picks or branch reuse.

### Rebase and fast-forward

Replays source commits onto the target and advances target without a merge commit.

**Strengths:** linear history; preserves logical commit sequence as new commits.  
**Tradeoffs:** commit identities change; PR boundary is less visible in graph; feature commits must already be clean.

### Rebase with merge commit

Replays source commits onto target, then creates a merge commit.

**Strengths:** semi-linear history plus visible PR boundary.  
**Tradeoffs:** creates new source commit identities and an additional merge commit.

## Comparison

| Strategy | Linear target | Keeps source commit IDs | One commit per PR | Visible merge boundary |
|---|---:|---:|---:|---:|
| Basic merge | No | Yes | No | Yes |
| Squash | Yes | No | Yes | Through commit/PR metadata |
| Rebase + fast-forward | Yes | No | No | Limited |
| Rebase + merge | Semi-linear | No | No | Yes |

## Selection guidance

Choose squash when:

- Feature-branch commits are work-in-progress detail
- One reversible commit per PR is valuable
- PRs are cohesive
- Commit-level authorship loss on target is acceptable

Choose basic merge when:

- Original commits and topology matter
- Branch commits are curated
- Merge boundaries aid audit or release reasoning

Choose rebase methods when:

- A linear or semi-linear history is important
- Contributors understand rewritten commit identities
- Commit sequences are meaningful and clean

Enforce permitted strategies for consistency. Allowing every method without guidance produces mixed history and confusing recovery.

## Reverting completed PRs

A squash PR can often be reverted by reverting one commit. A merge commit requires merge-aware revert semantics. A rebased PR may require reverting a range or creating a corrective PR.

No strategy replaces testing. Select based on how the team investigates and reverses change.

## PR metadata and work-item links

Azure Repos can associate PR and work-item information with completion commits. Retain PR records and pipeline evidence; Git graph alone does not carry all review context.

## Common mistakes

- Choosing squash to hide poorly scoped PRs
- Rebasing source commits after approvals without revalidation
- Reusing a source branch after squash and creating duplicate-history confusion
- Mixing strategies with no working agreement
- Expecting a linear graph to show PR boundaries automatically
- Reverting merge commits without selecting and understanding the mainline parent
- Treating merge method as a substitute for atomic design

## Interview preparation

**Q: What does squash merge do?**  
It creates one new target commit containing the aggregate source changes. Original source commits do not become ancestors of the target.

**Q: Which strategy preserves original commit IDs?**  
Basic no-fast-forward merge. Rebase methods and squash create new commits.

**Q: Why choose rebase with merge commit?**  
It produces semi-linear history by replaying commits onto the target while retaining a merge commit that marks the PR boundary.

**Q: Which strategy is best?**  
There is no universal best. Choose based on audit needs, commit quality, PR cohesion, rollback, bisecting, and team comprehension, then apply consistently.

## Practical exercise

Complete the same sample change with all four strategies in disposable branches. Compare git log --graph, parent relationships, commit IDs, work-item/PR evidence, and revert procedure.

## Further reading

- [Set and manage branch policies — merge types](https://learn.microsoft.com/azure/devops/repos/git/branch-policies)
- [Complete pull requests](https://learn.microsoft.com/en-us/azure/devops/repos/git/complete-pull-requests)
