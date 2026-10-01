# Template Versioning and Compatibility

[← Central Repositories](04-central-template-repositories.md) · [Chapter 7](README.md) · [Next: Required Templates →](06-required-template-checks.md)

## Templates need a release lifecycle

A central template is a dependency. Changing a parameter name, job name, output variable, default pool, task version, permissions assumption, or condition can break consumers even when YAML still compiles.

Use a documented scheme such as:

- Major version: breaking contract or behavior.
- Minor version: backward-compatible feature.
- Patch version: compatible correction.
- Prerelease: evaluation before broad adoption.

Pin consumers to a reviewed tag or commit. A Git tag can technically be moved, so repository policy must make release tags immutable in practice; a commit SHA gives the strongest source identity but is less readable.

## Define compatibility

Document the public surface:

- Template paths and types.
- Parameter names, types, defaults, and allowed values.
- Generated stage/job/step names used by consumers.
- Output variables and artifact names.
- Required service connections, variable groups, permissions, and agent capabilities.
- Supported Azure DevOps environment and task versions.
- Behavioral guarantees such as whether tests always publish results.

If it is not documented, consumers will still discover and depend on it. Minimize accidental surface area.

## Change workflow

1. Classify the change.
2. Add tests before implementation.
3. Test representative consumers.
4. Publish a prerelease when risk is material.
5. Write migration examples.
6. Release a new immutable version.
7. notify owners and measure adoption.
8. Maintain the previous major version for the promised window.
9. Remove it only after consumers migrate or accept an explicit exception.

A compatibility test should compile old examples, run key paths, and verify expected failures. Golden expanded-YAML snapshots can help, but avoid brittle snapshots that fail on irrelevant formatting.

## Deprecation versus emergency remediation

Routine breaking changes deserve notice and migration time. A critical security issue may justify an accelerated update or revocation. Define that exception before an incident, including authority, communications, supported overrides, and audit evidence.

Do not silently modify a released version to “fix everyone.” That destroys reproducibility. Publish a new version and use governance mechanisms to require migration when necessary.

## Interview preparation

**What counts as a breaking template change?**  
Anything that invalidates a supported consumer or materially changes behavior: removed/renamed inputs, changed defaults, names, outputs, permissions, agent requirements, or execution ordering.

**Tag, branch, or commit?**  
A protected immutable tag balances readability and stability; a commit is strongest for exact identity; a branch is useful for opt-in development but unsafe as a production pin.

**How do you know who will break?**  
Maintain representative contract tests, inventory consumers/versions, publish prereleases, and test expanded plans and real runs before rollout.

## Practical exercise

Release a `v1` template. Add an optional parameter in a compatible release, then rename a required parameter in a new major version. Write migration guidance and demonstrate rollback by repinning the consumer.

## Official references

- [YAML template security](https://learn.microsoft.com/azure/devops/pipelines/security/templates)
- [Template usage and repository references](https://learn.microsoft.com/azure/devops/pipelines/process/templates)

[Next: Required Template Checks →](06-required-template-checks.md)
