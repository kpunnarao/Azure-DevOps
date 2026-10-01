# Merge Conflict Resolution

> Chapter 3 — Git Fundamentals

[← Previous](04-merge-rebase-and-history.md) · [Chapter home](README.md) · [Next →](06-reset-restore-revert-and-cherry-pick.md)

## Purpose

A conflict means Git cannot safely infer the intended result. Resolution is a design decision, not a command-selection exercise. The goal is a correct integrated system, not merely the removal of conflict markers.

## Why conflicts happen

Common cases:

- Both histories change overlapping lines
- One side edits a file the other deletes
- Both sides rename or add the same path differently
- File mode or case changes conflict
- Generated files differ
- Rebase replays a commit against a changed context
- Multiple merge bases or long-lived divergence create ambiguity

## Resolution workflow

1. Stop and identify the operation: merge, rebase, cherry-pick, or revert.
2. Inspect status and the conflicting paths.
3. Understand the base, ours, and theirs in that operation's context.
4. Read the surrounding code and acceptance intent.
5. Combine the behavior intentionally.
6. Remove conflict markers.
7. Stage the resolved result.
8. Continue or complete the operation.
9. Build and test the integrated behavior.
10. Review the final diff and graph.

Useful commands:

```bash
git status
git diff
git diff --name-only --diff-filter=U
git add <resolved-path>
git merge --continue
git rebase --continue
git cherry-pick --continue
```

Abort when the integration strategy is wrong:

```bash
git merge --abort
git rebase --abort
git cherry-pick --abort
```

## Ours and theirs warning

The meaning of “ours” and “theirs” depends on the operation. During rebase, the mental perspective can surprise users because commits are replayed onto another base. Do not choose a whole side merely from the label. Inspect content and intent.

## Conflict markers

A textual conflict includes sections similar to:

```text
<<<<<<< HEAD
current side
=======
incoming side
>>>>>>> branch
```

The correct result may use one side, both sides, or entirely new content. Delete the markers after composing the intended version.

## Semantic conflicts

Git may merge text successfully while behavior is wrong—for example:

- One branch renames a method while another adds calls to the old name
- Two changes independently assign the same route
- A schema change invalidates code merged elsewhere
- Configuration keys conflict logically without overlapping text

Automated build, tests, static analysis, and human review are required after every meaningful integration.

## Reducing conflict frequency

- Keep branches short-lived
- Integrate main frequently
- Make small cohesive changes
- Coordinate high-contention files
- Separate generated files from hand-edited sources
- Use stable formatting rules
- Avoid broad refactors mixed with feature behavior
- Assign clear code ownership
- Use feature flags rather than long-running branches

## Azure Repos pull-request conflicts

Azure Repos detects whether the source can merge into the target. Resolve with a suitable client or supported web experience. After resolution, ensure branch policies and build validation rerun against the current proposed merge.

Never bypass validation simply because resolution was “only a conflict fix.” Resolution can change behavior significantly.

## Common mistakes

- Accepting all incoming or current content without analysis
- Removing markers but failing to compile and test
- Resolving generated files instead of regenerating them
- Committing unrelated cleanup during conflict resolution
- Continuing a rebase repeatedly without checking each commit's intent
- Forgetting that the target branch advanced after approval
- Assuming a clean textual merge is semantically correct

## Interview preparation

**Q: What causes a merge conflict?**  
Git cannot automatically combine changes safely based on the histories and merge base. Human intent is required.

**Q: How do you resolve a conflict safely?**  
Identify the operation and versions, understand intended behavior, compose the result, stage it, continue, then run validation and review the final diff.

**Q: What is a semantic conflict?**  
A behavior-level incompatibility that may not produce textual conflict markers. Tests and review must detect it.

**Q: How do you reduce conflicts?**  
Use small short-lived branches, integrate frequently, coordinate high-contention areas, separate refactoring from behavior, and automate formatting and validation.

## Practical exercise

Create three conflict types: same-line edit, modify/delete, and rename/edit. Resolve each, explain the intended outcome, run tests, and record which evidence was needed.

## Further reading

- [Review pull requests and resolve changes](https://learn.microsoft.com/en-us/azure/devops/repos/git/review-pull-requests)
- [git-merge](https://git-scm.com/docs/git-merge)
