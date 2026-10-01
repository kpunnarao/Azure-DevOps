# Atomic Commits and Ignore Rules

> Chapter 3 — Git Fundamentals

[← Previous](06-reset-restore-revert-and-cherry-pick.md) · [Chapter home](README.md) · [Next →](08-tagging-and-semantic-versioning.md)

## Purpose

A good commit is a coherent, reviewable, buildable statement of intent. Ignore rules keep generated, local, and sensitive-by-design files out of ordinary selection—but they are not security controls.

## Atomic commits

An atomic commit contains one logical change. It should be understandable and independently reviewable. Where practical, it should leave the repository in a valid state.

Benefits:

- Easier review
- Useful history and blame
- Safer revert and cherry-pick
- Better bisecting
- Clearer release notes
- Reduced conflict scope

Atomic does not mean “one file.” A behavior change may legitimately require implementation, tests, configuration, and documentation in one commit.

## Separating mixed work

Use:

```bash
git diff
git add -p
git diff --staged
git commit
```

If a file mixes formatting, refactoring, and behavior, consider separate commits or redo the edit more deliberately. Avoid a “misc changes” commit.

## Commit messages

A useful message explains intent.

Recommended structure:

```text
Short imperative summary

Why the change is needed, important constraints,
and any non-obvious consequences.

Work item or issue reference where appropriate.
```

Good: “Reject expired checkout tokens”  
Weak: “Changes”, “Fix stuff”, or a filename.

The diff explains what changed. The message should help explain why.

## Amend and fixup

Amend can correct the most recent unshared commit. Interactive rebase can combine fixup commits before publication. These rewrite commit identity; do not amend commits others already depend on without coordination.

## Ignore rules

.gitignore patterns describe intentionally untracked files:

- Build output
- Local caches
- IDE state
- Temporary files
- Local environment configuration
- Generated files not meant for source control

Commit the .gitignore file so the team shares the policy.

A repository can also have local excludes for personal files that should not become team policy.

## Important limitations

- .gitignore does not affect already tracked paths
- It does not remove history
- It does not encrypt or protect secrets
- Broad patterns can hide important source files
- Negation and directory patterns require careful ordering

Inspect why a path is ignored:

```bash
git check-ignore -v path/to/file
```

To stop tracking a file while keeping it locally, update policy and remove it from the index deliberately. If it contained a secret, rotate the secret and perform an approved history-cleaning process when necessary.

## Secret prevention

Use multiple layers:

- Do not create plaintext production secrets in repository paths
- Provide example configuration without real values
- Use secret stores and workload identity
- Add pre-commit or server-side secret scanning
- Review staged diffs
- Restrict and rotate credentials
- Respond to exposure as an incident

## Common mistakes

- One commit combining refactor, formatting, feature, and dependency updates
- Messages that repeat the filename
- Committing generated output or dependency folders
- Ignoring lock files without understanding language guidance
- Adding a leaked secret to .gitignore and considering it fixed
- Using a global ignore rule that silently hides required project files
- Amending or squashing after review without requiring revalidation

## Interview preparation

**Q: What makes a commit atomic?**  
It represents one coherent logical change, includes necessary tests and documentation, and can be reviewed or reversed without unrelated effects.

**Q: Why are atomic commits operationally useful?**  
They improve review, bisecting, reverts, cherry-picks, auditing, and diagnosis.

**Q: Does .gitignore protect secrets?**  
No. It only influences ordinary handling of untracked files. A committed secret remains in history and must be rotated.

**Q: Should generated files be committed?**  
Usually not when they are reproducibly generated. Commit them only when consumers require them and ownership, review, and regeneration policy are explicit.

## Practical exercise

Create one file with two unrelated changes. Use patch staging to create two coherent commits. Add ignored build output, verify the rule source, then demonstrate that adding a tracked file to .gitignore does not untrack it.

## Further reading

- [gitignore documentation](https://git-scm.com/docs/gitignore)
- [git-commit](https://git-scm.com/docs/git-commit)
- [Work with large files in Azure Repos](https://learn.microsoft.com/en-us/azure/devops/repos/git/manage-large-files)
