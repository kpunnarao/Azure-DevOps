# Clone, Fetch, Pull, and Push

> Chapter 3 — Git Fundamentals

[← Previous](02-commits-branches-tags-and-head.md) · [Chapter home](README.md) · [Next →](04-merge-rebase-and-history.md)

## Purpose

These commands exchange repository data, but they do not perform the same operation. Correct mental models prevent unexpected merges, rejected pushes, and accidental publication.

## Clone

Clone creates a local repository from another repository, copies reachable objects and references according to its configuration, creates a remote usually named origin, and checks out a branch.

```bash
git clone <repository-url>
```

The origin name is conventional, not magical. Verify it with git remote -v.

## Fetch

Fetch contacts a remote, downloads new objects, and updates remote-tracking references such as origin/main. It does **not** normally modify the current local branch or working files.

```bash
git fetch origin
git log --oneline --graph --decorate HEAD..origin/main
```

Fetch is the safest first step when you want to inspect remote change before integration.

Remote-tracking references are local records of remote state at the last fetch. They are not live network pointers.

## Pull

Pull performs fetch followed by integration into the current branch. Integration may be merge, rebase, or fast-forward only, depending on command options and configuration.

```bash
git pull --ff-only
git pull --rebase
```

Use an explicit team policy. Unconfigured pull behavior can produce surprising merge commits or rewritten local history.

- Fast-forward only refuses divergent histories.
- Rebase replays local unpublished commits on the updated upstream.
- Merge creates a merge when histories diverge.

## Push

Push sends objects and asks the remote to update references.

```bash
git push -u origin feature/guest-checkout
```

The -u option establishes upstream tracking for convenient future push and pull use.

A push may be rejected because:

- The remote branch advanced
- Branch policy or permission blocks direct updates
- The reference update is non-fast-forward
- Authentication or network configuration failed
- Repository policy rejects content, path, or file size
- The remote does not accept the requested ref update

Do not respond to rejection with a force push until the reason and shared-history impact are understood.

## Tracking branches

A local branch can have an upstream reference. Inspect it with:

```bash
git branch -vv
git status
```

Local main and origin/main are distinct references. Fetch moves origin/main; integrating moves local main.

## Force-with-lease

If rewriting an unshared feature branch is allowed and a forced update is necessary, prefer force-with-lease over unconditional force:

```bash
git push --force-with-lease
```

It checks that the remote reference still matches the expected observed value, reducing the chance of overwriting someone else's new work. It is not permission to rewrite protected or shared history.

## Practical collaboration routine

```bash
git switch main
git fetch origin
git merge --ff-only origin/main
git switch -c feature/guest-checkout
# edit, stage, and commit
git push -u origin feature/guest-checkout
```

Teams that rebase feature branches can rebase onto origin/main before push or pull request update, subject to policy and coordination.

## Common mistakes

- Believing fetch updates local main
- Pulling while on the wrong branch
- Force-pushing after a rejection without inspecting remote changes
- Using the same credentials for human work and automation
- Publishing local experimental branches unintentionally
- Forgetting to push tags needed by release automation
- Assuming origin always means the authoritative repository
- Embedding credentials in repository URLs or scripts

## Interview preparation

**Q: Fetch versus pull?**  
Fetch downloads objects and updates remote-tracking references. Pull fetches and then integrates into the current branch.

**Q: Why can push be rejected as non-fast-forward?**  
The remote has commits not represented as ancestors of the proposed new tip. Updating it would discard visible history.

**Q: origin/main versus main?**  
origin/main is a local remote-tracking reference updated by fetch. main is the local branch changed by local commits or integration.

**Q: Why prefer force-with-lease?**  
It refuses the forced update when the remote changed unexpectedly, offering protection against overwriting another contributor's work.

## Further reading

- [Update code with fetch and pull](https://learn.microsoft.com/en-us/azure/devops/repos/git/pulling)
- [git-fetch](https://git-scm.com/docs/git-fetch)
- [git-push](https://git-scm.com/docs/git-push)
