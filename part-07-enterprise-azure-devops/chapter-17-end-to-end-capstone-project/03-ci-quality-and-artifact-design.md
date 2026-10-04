# CI, Quality, and Artifact Design

[← Boards and Repos](02-boards-repos-and-branch-policies.md) · [Chapter 17](README.md) · [Next: Infrastructure Provisioning →](04-infrastructure-and-environment-provisioning.md)

## Authoritative CI

Separate PR validation from protected-main CI. Main CI:

1. Checks out exact commit.
2. Installs pinned toolchain.
3. Restores locked dependencies.
4. Builds deterministically.
5. Runs layered tests and analysis.
6. Publishes test/coverage/security evidence.
7. Assigns unique version.
8. Builds final multi-stage non-root image/package.
9. Generates SBOM and provenance record.
10. Scans final content.
11. Pushes to ACR/feed with immutable identity.
12. Publishes deployment metadata.

Use central templates pinned to a release. Show typed parameters and one bounded extension point.

## Test portfolio

Implement unit, component/contract, integration, and a small end-to-end smoke test. Map critical risks to tests. Include negative authorization, idempotency, retry, concurrency, schema compatibility, and performance threshold.

Publish results even after failure where safe. Track flakiness; retries never silently produce success.

## Artifact identity

Record commit, run ID/number, package version, image tags, registry digest, base digest, dependency lock hash, template revision, tool versions, test/scan results, and SBOM reference. CD must select a specific successful producer run/digest.

Never publish official packages/images from an untrusted PR.

## Performance

Measure queue, restore, build, tests, scan, publish, and critical path. Add safe lock-derived cache, parallelize independent tests, cancel superseded PRs, and prove cache-disabled correctness.

## Quality gates

Define failure thresholds for tests, new high-risk security findings, required coverage of changed critical code, contract compatibility, and artifact creation. State exception owner/expiry. Scanner unavailable is not passed.

## Failure demonstrations

- Changed lock file invalidates cache.
- Missing output skips/fails dependent job visibly.
- Flaky test is detected/quarantined with expiry.
- Vulnerable dependency blocks official publication.
- Attempt version overwrite fails.
- Tag is moved but digest deployment remains stable.

## Acceptance evidence

Pipeline graph, expanded template view, timings before/after, result dashboards, artifact manifest, checksum/digest, ACR/feed roles, SBOM/scan, and trace from source/work item to output.

## Expert review questions

Can the build reproduce cleanly? Who can publish? Can a compromised PR poison a privileged cache? Is the scanned digest the deployed digest? How are task/template updates controlled?

[Next: Infrastructure and Environment Provisioning →](04-infrastructure-and-environment-provisioning.md)
