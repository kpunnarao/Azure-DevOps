# Feed Scope, Permissions, and Upstream Sources

[← Feeds and Package Types](02-azure-artifacts-feeds-and-package-types.md) · [Chapter 8](README.md) · [Next: Package Immutability →](04-package-immutability-and-versioning.md)

## Feed scope

A feed can be organization-scoped or project-scoped. Project-scoped feeds are associated with a project and require access to both the project and feed. Organization-scoped feeds can be used across the organization subject to feed permissions. Choose scope based on the real sharing and administration boundary; avoid granting broad access merely to simplify one pipeline.

## Roles

Azure Artifacts feed roles generally progress from reader to collaborator/feed-and-upstream reader, publisher/contributor, and owner. Exact labels can vary in presentation; use the minimum role that supports the job.

- Consumers need package read access.
- An identity that saves packages from an upstream may require the collaborator capability.
- Publishing CI needs publisher capability.
- Feed settings and permissions belong only to a small owner group.

Azure Pipelines can run as a project build-service identity or, depending on job authorization scope, a project-collection build-service identity. Grant the actual runtime identity access; do not grant a user's account and assume the pipeline inherits it.

## Upstream sources

An upstream source lets a feed resolve packages from another source such as a public registry or another Azure Artifacts feed. On first eligible use, Azure Artifacts can save a copy to the feed, improving availability and preserving the consumed version.

Benefits include one configured endpoint, controlled curation, and resilience. Risks include dependency confusion, unexpected source priority, malicious new versions, license exposure, and granting too many identities permission to save packages.

Define:

- Approved upstreams and ordering.
- Whether internal names may ever resolve publicly.
- Who may save upstream packages.
- Vulnerability and license scanning.
- Incident response and package blocking.
- How existing internal packages take precedence.

## Permission troubleshooting

When restore or publish returns 401/403:

1. Identify the pipeline's effective build-service identity.
2. Confirm project membership if the feed is project-scoped.
3. Confirm the feed role.
4. Confirm the pipeline's project/job authorization scope.
5. Check package-manager authentication setup.
6. Check cross-project resource authorization.
7. Avoid escalating to owner as the first fix.

A 404 can also mask authorization or an incorrect feed URL/scope.

## Common mistakes

- Granting owner to every build service.
- Confusing project visibility with feed permissions.
- Allowing untrusted PR pipelines to publish or save upstream packages.
- Adding public upstreams without internal-name protection.
- Assuming a service connection automatically grants feed access.
- Changing collection-wide job scope to solve a local permission issue.

## Interview preparation

**Reader versus collaborator?**  
A reader consumes packages already present; a collaborator can also save packages from configured upstreams. Grant that extra capability only where needed.

**What is dependency confusion?**  
A resolver selects an unintended package—often a public package sharing an internal name or with attractive version precedence. Control sources, names, ordering, and saved packages.

**Which pipeline identity gets permission?**  
The build-service identity that the job actually uses, determined by organization/project settings and job authorization scope.

## Practical exercise

Create read-only consumer and publisher identities in a sandbox feed. Verify read, publish, and settings operations. Add an upstream, restore one approved package, inspect the saved version, then document how an internal namespace is protected.

## Official references

- [Feed permissions](https://learn.microsoft.com/azure/devops/artifacts/feeds/feed-permissions)
- [Project-scoped and organization-scoped feeds](https://learn.microsoft.com/azure/devops/artifacts/feeds/project-scoped-feeds)
- [Upstream sources](https://learn.microsoft.com/azure/devops/artifacts/concepts/upstream-sources)

[Next: Package Immutability and Versioning →](04-package-immutability-and-versioning.md)
