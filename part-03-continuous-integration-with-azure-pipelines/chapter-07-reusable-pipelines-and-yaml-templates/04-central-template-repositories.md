# Central Template Repositories

[← Conditional Insertion](03-conditional-insertion-and-each-loops.md) · [Chapter 7](README.md) · [Next: Template Versioning →](05-template-versioning-and-compatibility.md)

## Share standards without copying

A central repository gives platform owners one reviewed source for pipeline templates. Consumers declare it as a repository resource and reference templates by alias.

```yaml
resources:
  repositories:
  - repository: pipelineTemplates
    type: git
    name: Platform/azure-pipeline-templates
    ref: refs/tags/v2.3.1

stages:
- template: stages/ci.yml@pipelineTemplates
  parameters:
    applicationType: dotnet
```

The `ref` is critical. Pin consumers to an approved version instead of silently following a moving default branch. Template expansion fetches YAML; it does not automatically check out the template repository as a workspace for scripts. If a template needs files from that repository at runtime, design an explicit checkout and understand the trust impact.

## Ownership model

A production library needs:

- Named maintainers and code owners.
- Protected branches and mandatory review.
- Automated consumer/contract tests.
- Release notes and version tags.
- A vulnerability-response path.
- Deprecation and support windows.
- Usage telemetry that avoids exposing secrets.
- A local or sandbox testing path for consumers.

Treat template repository access as part of the build trust boundary. Restrict who can change it more strongly than ordinary application code, because one change can affect many pipelines.

## Resource authorization

The consuming pipeline identity needs permission to read the repository, and the resource may require explicit authorization. Cross-project repositories also depend on project scope and job authorization settings. Do not solve access failures by broadly granting collection-wide rights; grant the minimum required identity and repository access.

## Development workflow

A safe release flow is:

1. Change a feature branch.
2. Run template self-tests.
3. Test representative sample consumers.
4. Review security and compatibility impact.
5. Merge to the protected branch.
6. Create an immutable release reference.
7. Publish migration notes.
8. Update consumers in controlled batches.

For development, a test consumer can temporarily point to a feature branch in a non-production context. Never leave that mutable development reference in a production pipeline.

## Common mistakes

- Following `main` in every consumer.
- Assuming template files are present during task execution.
- Granting broad project-collection permissions for convenience.
- Changing central templates without testing old parameter combinations.
- Coupling templates to one repository's path conventions.
- Hiding breaking changes inside a “minor cleanup.”

## Interview preparation

**Why use a central repository?**  
It creates a controlled distribution channel for tested build patterns and policy, reducing drift while preserving versioned consumer choice.

**How do you test a central-template change?**  
Validate the library itself, compile and run representative consumers, test expected failures, review security, and release through a pinned version.

**Why not reference the default branch?**  
A mutable reference lets an unrelated merge change many pipelines without a consumer review or predictable rollback.

## Practical exercise

Create a small template repository, tag a version, consume it from another repository, and verify authorization. Introduce a second version, compare expanded plans, then roll the consumer backward by changing only the pinned reference.

## Official references

- [Use templates from another repository](https://learn.microsoft.com/azure/devops/pipelines/process/templates)
- [Repository resource schema](https://learn.microsoft.com/azure/devops/pipelines/yaml-schema/resources-repositories-repository)
- [Pipeline security for repositories](https://learn.microsoft.com/azure/devops/pipelines/security/secure-access-to-repos)

[Next: Template Versioning and Compatibility →](05-template-versioning-and-compatibility.md)
