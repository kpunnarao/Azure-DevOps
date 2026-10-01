# Branch Policies and Build Validation

> Chapter 4 — Azure Repos and Enterprise Branching Strategies

[← Previous](02-pull-request-lifecycle.md) · [Chapter home](README.md) · [Next →](04-reviewers-status-checks-and-comment-resolution.md)

## Purpose

Branch policies protect critical branches by requiring evidence and review before integration. In Azure Repos, enabling branch policy requires pull requests for ordinary updates and prevents deletion of the protected branch.

Policies should address real risks. Excessive or slow controls encourage large batches, bypass requests, and shadow processes.

## Policy types

Azure Repos supports policies such as:

- Minimum number of reviewers
- Linked work items
- Comment resolution
- Allowed merge types
- Build validation
- Status checks
- Automatically included reviewers
- Path-based policy scope where supported

Repository settings can also control file size, path length, case-related behavior, and other repository-wide concerns.

## Build validation

Build validation runs a pipeline against the proposed merge. It should provide fast confidence that source will integrate successfully.

Typical PR validation:

1. Checkout the policy-provided source/merge context
2. Restore locked dependencies
3. Compile or validate
4. Run unit tests
5. Run lint/static analysis
6. Run secret and dependency checks
7. Publish test results
8. Return a clear status

Keep high-value blocking validation fast. Move expensive exhaustive tests to later stages or parallel jobs when risk allows.

## Policy configuration decisions

### Blocking versus optional

A blocking policy prevents completion when unmet. Use it for controls that truly must hold. Optional validation can provide information without stopping delivery.

### Automatic versus manual queue

Automatic validation runs for new changes. Manual or expiration settings can reduce resource use but may allow stale evidence if poorly configured.

### Build expiration

If the target branch changes after validation, decide when the build becomes invalid. High-risk repositories should validate against current target state.

### Path filters

Apply specialized validation only to relevant paths—for example, database checks for schema changes. Test path filters carefully; incorrect exclusions create blind spots.

### Build identity and secrets

PR validation executes contributor-controlled code. Do not expose privileged secrets or production network access to an untrusted PR job. Use isolated agents and least privilege.

## Recommended baseline for main

A context-dependent baseline might include:

- Pull request required
- At least one independent reviewer
- Linked work item or documented exception
- Comments resolved
- Fast build/test validation
- Security/status checks appropriate to risk
- Restricted merge methods
- Automatic specialist reviewers for sensitive paths
- Rare bypass authority

Higher-risk systems may require more; low-risk documentation repositories may require less.

## Troubleshooting

**Policy does not run**

- Verify it targets the correct branch
- Check pipeline permissions and branch filters
- Check path filters
- Confirm pipeline exists and is enabled
- Inspect policy configuration and queue behavior

**Policy result is stale**

- Check expiration settings
- Verify target updates require revalidation
- Confirm source commit matches displayed run
- Check whether a user bypassed policy

**Validation exposes secrets**

- Stop and rotate exposed credentials
- Inspect logs and agent workspace
- Restrict secret availability
- Separate privileged deployment from untrusted validation
- Review repository and pipeline permissions

## Common mistakes

- Slow validation that takes longer than review
- Required reviewers approving their own changes without justification
- Policies only on main while release branches remain unprotected
- Path filters that omit shared files
- Broad secrets available to PR builds
- Non-deterministic flaky checks blocking every change
- Giving many users bypass rights
- Never auditing policy drift across repositories

## Interview preparation

**Q: What happens when a branch policy is configured?**  
Ordinary changes must use pull requests and satisfy configured policies before completion; the protected branch also cannot be deleted through normal operations.

**Q: What should PR build validation include?**  
Fast, reliable checks that determine integration safety: build, unit tests, lint/static analysis, and proportionate security validation, with clear results.

**Q: Why can PR validation be a security risk?**  
The PR controls code executed by the pipeline. If the job can access trusted secrets, privileged agents, or production networks, malicious or accidental code can misuse them.

**Q: How do you prevent policy bypass becoming routine?**  
Limit bypass permissions, require a documented reason and audit, monitor usage, review after emergencies, and fix the policy or workflow causing unnecessary bypass.

## Practical exercise

Configure baseline policies on main, deliberately fail each one, observe the PR behavior, then correct the change. Record which identity ran validation and which resources it could access.

## Further reading

- [Set and manage branch policies](https://learn.microsoft.com/azure/devops/repos/git/branch-policies)
- [Repository settings and policies](https://learn.microsoft.com/en-us/azure/devops/repos/git/repository-settings)
- [Secure repositories and pull requests](https://learn.microsoft.com/en-us/azure/devops/repos/git/secure-repositories-pull-requests)
