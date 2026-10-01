# Reset, Restore, Revert, and Cherry-pick

> Chapter 3 — Git Fundamentals

[← Previous](05-merge-conflict-resolution.md) · [Chapter home](README.md) · [Next →](07-atomic-commits-and-ignore-rules.md)

## Purpose

Git provides several “undo” tools because mistakes can exist in different states. First determine whether the change is uncommitted, staged, committed locally, or shared.

## Decision table

| Situation | Typical safe tool |
|---|---|
| Discard unstaged changes in a path | restore |
| Remove a path from the next commit but keep edits | restore --staged |
| Move an unshared branch and preserve changes | reset --soft or --mixed |
| Discard unshared commits and files | reset --hard, with extreme care |
| Undo a shared commit without rewriting history | revert |
| Apply one existing commit elsewhere | cherry-pick |
| Recover a moved/deleted reference | reflog plus a new branch |

## Restore

Restore changes working-tree or index content.

```bash
git restore path/to/file
git restore --staged path/to/file
git restore --source=<commit> path/to/file
```

The first form discards unstaged edits in that path. The staged form unstages while retaining working-tree content. Inspect status and diff first.

## Reset

Reset moves the current branch reference and optionally updates index and working tree.

- soft: move branch; keep changes staged
- mixed: move branch; keep changes unstaged
- hard: move branch; make index and working tree match, discarding affected uncommitted content

```bash
git reset --soft HEAD~1
git reset HEAD~1
```

Do not reset shared history unless a coordinated recovery specifically requires rewriting it. Never use hard reset casually.

## Revert

Revert creates a new commit that applies the inverse of an existing commit.

```bash
git revert <commit>
```

It is usually appropriate for a bad commit already shared on main because it preserves history. Reverting a merge requires selecting a mainline parent and understanding future merge implications.

A revert may conflict when later changes depend on the original change.

## Cherry-pick

Cherry-pick recreates the change introduced by selected commit(s) on the current branch.

```bash
git cherry-pick <commit>
```

Use it for isolated fixes that genuinely belong on another line of development, such as porting a production fix to a supported release branch. It creates a new commit identity and can duplicate logical changes if the branches later merge.

## Reflog recovery

Reflog records recent local reference movements.

```bash
git reflog
git switch -c recovery/<name> <commit>
```

It can recover work after reset, rebase, amended commits, or deleted local branches while objects remain available. Reflog is local and retention is not a backup strategy.

## Recovery routine

1. Stop issuing destructive commands.
2. Copy or stash uncommitted files if appropriate.
3. Capture status and the commit graph.
4. Inspect reflog.
5. Create a new recovery branch rather than moving more references.
6. Compare recovered content.
7. Decide on the final repair.
8. Communicate if shared history was affected.

## Common mistakes

- Using reset to undo a shared main commit
- Confusing unstage with discard
- Running reset --hard without preserving uncommitted work
- Cherry-picking a long chain instead of fixing branch strategy
- Reverting a merge without understanding parent selection
- Assuming revert restores the repository to an exact old snapshot
- Continuing after lost work without checking reflog
- Force-pushing a recovery before teammates are informed

## Interview preparation

**Q: Reset versus revert?**  
Reset moves a reference and may rewrite visible history; it is mainly for local unshared correction. Revert adds a new inverse commit, making it appropriate for shared history.

**Q: Restore versus reset?**  
Restore targets working-tree or index content. Reset primarily moves the current branch and can also update index and working tree based on mode.

**Q: What does cherry-pick do?**  
It applies the change introduced by selected commits to the current branch by creating new commits.

**Q: How do you recover a commit after a reset?**  
Inspect reflog, identify the prior commit, and create a recovery branch pointing to it before making further changes.

## Practical exercise

In a disposable repository, demonstrate: unstage without discard, restore one file, soft reset, mixed reset, revert a shared-style commit, cherry-pick a fix, delete a branch, and recover it with reflog. Record state before and after every command.

## Further reading

- [Undo changes in Azure Repos Git](https://learn.microsoft.com/en-us/azure/devops/repos/git/undo)
- [git-restore](https://git-scm.com/docs/git-restore)
- [git-reset](https://git-scm.com/docs/git-reset)
- [git-revert](https://git-scm.com/docs/git-revert)
- [git-cherry-pick](https://git-scm.com/docs/git-cherry-pick)
