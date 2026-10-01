# Merge, Rebase, and History

> Chapter 3 — Git Fundamentals

[← Previous](03-clone-fetch-pull-and-push.md) · [Chapter home](README.md) · [Next →](05-merge-conflict-resolution.md)

## Purpose

Merge and rebase integrate histories differently. Neither is universally superior. Select based on collaboration, audit needs, readability, and whether history has already been shared.

## Merge

Merge finds common ancestry and combines histories.

### Fast-forward

If the target has not diverged, its reference can move directly to the source tip. No merge commit is required.

### Three-way or no-fast-forward merge

When histories diverge, Git compares both tips with a merge base and creates a commit with multiple parents after resolution.

Advantages:

- Preserves existing commit identities
- Shows where histories joined
- Safe for shared branches when normal policy is followed

Tradeoffs:

- Frequent synchronization merges can make history noisy
- Review intent can be harder to summarize if branch discipline is weak

## Rebase

Rebase identifies commits unique to the current branch and recreates them on a new base.

Before:

```text
A---B---C main
     \
      D---E feature
```

After rebasing feature onto main:

```text
A---B---C main
         \
          D'---E' feature
```

D' and E' contain equivalent changes but are new commit objects.

Advantages:

- Produces a linear feature history
- Removes synchronization merge commits
- Can clean unpublished commits before review

Tradeoffs:

- Rewrites commit identities
- Requires force updating a previously pushed feature branch
- Can repeatedly surface conflicts when commits are replayed
- Is dangerous when collaborators depend on the old history

## Golden safety rule

Avoid rebasing shared history. Rebasing a personal or coordinated feature branch can be appropriate. Rebasing protected main or a branch consumed by others requires extraordinary coordination and normally should not occur.

## Interactive rebase

Interactive rebase can reorder, edit, combine, or remove unpublished commits.

Use it to improve reviewability before publication, not to hide material review history after approval.

## Merge strategy versus PR merge method

Local merge/rebase operations update local history. Azure Repos also offers PR completion methods such as basic merge, squash, rebase and fast-forward, and rebase with merge commit. Chapter 4 covers their governance implications.

## Choosing a local integration approach

Use merge when:

- Commit identities must remain stable
- The branch is shared
- Preserving topology is valuable
- Team policy prefers explicit joins

Use rebase when:

- Commits are unpublished or coordinated
- A linear branch is preferred
- You are preparing a clean PR
- The team understands rewritten-history implications

## Common mistakes

- Rebasing main because the graph “looks messy”
- Force-pushing after rebase without checking remote updates
- Resolving repeated rebase conflicts mechanically
- Squashing unrelated changes into one commit
- Merging the wrong base branch into a feature
- Assuming a linear history proves high quality
- Treating history aesthetics as more important than audit and collaboration

## Interview preparation

**Q: Merge versus rebase?**  
Merge combines existing histories, often with a multi-parent commit, preserving commit identities. Rebase recreates selected commits on a new base, producing new identities and often a linear history.

**Q: Why not rebase shared history?**  
Collaborators may have commits based on the old graph. Rewriting forces them to reconcile duplicate or replaced history and risks lost work.

**Q: Does rebase lose authorship?**  
Normally it preserves author information but creates new commit identities and new committer metadata.

**Q: When would you use interactive rebase?**  
To clean unpublished local history—for example, combine fixup commits or improve messages—before sharing or requesting review.

## Practical exercise

Create the same divergent graph twice. Integrate one with a no-fast-forward merge and the other with rebase. Compare commit IDs, parent relationships, log graph, diff of final trees, and required push behavior.

## Further reading

- [Update branches with merge or rebase](https://learn.microsoft.com/en-us/azure/devops/repos/git/pulling)
- [git-merge](https://git-scm.com/docs/git-merge)
- [git-rebase](https://git-scm.com/docs/git-rebase)
