# Boards, Repos, and Branch Policies

[← Requirements and Architecture](01-capstone-requirements-and-architecture.md) · [Chapter 17](README.md) · [Next: CI and Artifacts →](03-ci-quality-and-artifact-design.md)

## Work model

Create Epic/Feature/User Story or equivalent hierarchy for the capstone. Stories include business value, acceptance criteria, nonfunctional/security conditions, owner, dependencies, estimate, and definition of done. Add bugs, risks, technical debt, spikes, and incident actions explicitly.

Configure area/iteration paths with one owning team. Define workflow states and WIP limits; show one cumulative-flow/cycle-time view. Link commits/PRs/builds/tests/deployments back to work.

## Repository design

Choose monorepo or multiple repositories through an ADR. Include application, tests, pipeline entry points, documentation, and appropriate IaC/manifest/template boundaries. Establish CODEOWNERS-like reviewer policy through branch policies/path ownership where possible.

Add:

- README/contribution/security guidance.
- `.gitignore`, `.gitattributes`, optional LFS.
- Dependency lock files.
- Version policy and changelog/release notes.
- No secrets or generated build outputs.
- Commit/PR naming and work-item linking.

## Protected main branch

Require:

- Minimum reviewers and resolved comments.
- Build validation bound to the correct YAML.
- Current successful policy result.
- Work-item linkage where valuable.
- Appropriate merge strategy.
- Path-specific reviewers for pipeline/IaC/security ownership.
- Restricted bypass/force push/delete.
- Status checks for required security/quality evidence.

Document self-approval, emergency bypass, stale vote, direct push, and administrator behavior. Test—not assume—each policy.

## Pull-request flow

A PR performs safe untrusted validation: format/lint, compile, unit/component tests, secret/dependency/code/IaC scan, and preview with read-only or isolated credentials. It must not publish official versions or access production.

Keep changes small. Use feature flags for incomplete behavior. Record reviewer intent and rejection/iteration evidence.

## Failure demonstrations

- Attempt direct push.
- Submit PR with failing test.
- Submit secret-like disposable test string and verify push/scan policy.
- Modify production pipeline path and trigger owner review.
- Attempt bypass with ordinary developer.
- Show stale build invalidation after new commit.

## Acceptance evidence

Export/screenshot policy configuration without sensitive details, PR timeline, linked work item, validation run, reviewer decision, merge commit, and branch audit. Include a policy table explaining threat and owner.

## Expert review questions

Can a project administrator bypass? Who reviews YAML that selects a privileged resource? How is fork/untrusted code handled? What happens if validation system is unavailable? How do emergency fixes retain review?

[Next: CI, Quality, and Artifact Design →](03-ci-quality-and-artifact-design.md)
