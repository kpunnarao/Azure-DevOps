# Core Azure DevOps Services

> Chapter 1 — DevOps Principles and Azure DevOps Architecture

[← Previous](05-organizations-projects-teams-and-resources.md) · [Chapter home](README.md) · [Next →](07-plan-to-production-traceability.md)

## Purpose

Azure DevOps provides integrated services for planning, source control, automation, testing, and package management. Expertise means understanding each service's purpose, its evidence, and the integration points—not simply knowing its menu.

## Service map

| Service | Primary question | Important artifacts |
|---|---|---|
| Azure Boards | What value and work are we delivering? | Work items, backlogs, boards, sprints, queries, dashboards |
| Azure Repos | What changed, who reviewed it, and why? | Repositories, branches, commits, pull requests, policies |
| Azure Pipelines | How was the change validated and delivered? | YAML, runs, stages, logs, artifacts, environments |
| Azure Test Plans | What was tested and what was the result? | Plans, suites, cases, configurations, runs, defects |
| Azure Artifacts | Which reusable packages are trusted and consumed? | Feeds, packages, versions, upstream sources |

## Azure Boards

Boards supports planning and tracking from portfolio objectives to tasks and defects. Its value is strongest when work-item states reflect the real workflow and links connect work to code, builds, tests, and deployments.

Avoid using Boards as a reporting database that developers update after work is complete. The board should be a living representation of flow.

## Azure Repos

Repos provides Git repositories and pull-request collaboration. Key capabilities include branch policies, build validation, reviewers, comment threads, permissions, and work-item linking.

A protected main branch is a quality boundary. Policies should enforce meaningful review and validation without making small changes unnecessarily slow.

## Azure Pipelines

Pipelines automates build, test, packaging, infrastructure, and deployment across many languages and targets. YAML pipelines place process definition under version control; environments and protected resources can enforce controls outside YAML.

Pipeline success is evidence that configured steps passed—not proof that every business or operational risk was tested.

## Azure Test Plans

Test Plans supports manual, exploratory, and automated test management. It is useful where traceable test cases, configurations, evidence, and user acceptance are important. Automated test results can also be published directly by pipelines without every team using manual Test Plans.

## Azure Artifacts

Artifacts hosts and shares package types such as NuGet, npm, Maven, Python, and Universal Packages. Feeds control visibility and upstream sources. Package versions should be immutable once released so consumers receive predictable dependencies.

Pipeline artifacts and package feeds solve different problems: pipeline artifacts move outputs through a delivery run; packages are versioned reusable dependencies.

## Supporting capabilities

Azure DevOps also includes:

- Wikis and Markdown documentation
- Dashboards and Analytics
- Service hooks and REST APIs
- Extensions and Marketplace integrations
- Environments and approvals
- Agent pools
- Security groups, permissions, access levels, and auditing

## Integrated lifecycle example

1. A User Story defines acceptance criteria in Boards.
2. A branch and commits reference the work item.
3. A pull request runs policy validation in Pipelines.
4. Automated tests publish results.
5. The main build publishes an immutable artifact or package.
6. A deployment job targets an environment.
7. Deployment status appears on the work item where configured.
8. Operational feedback creates a Bug or improvement item.

## Selection guidance

Use a service because it answers an operational question or creates required evidence. Not every team needs every feature. For example, a small automated service may not need manual Test Plans, while a regulated product may depend heavily on traceable test cases.

## Common mistakes

- Treating each service as an isolated product
- Duplicating source in a package feed instead of building packages
- Using pipeline artifacts as a long-term package-versioning strategy
- Adding work-item links manually when integration could create them reliably
- Granting broad service-connection access to simplify pipelines
- Installing extensions without permission and lifecycle review

## Interview preparation

**Q: Name the five core Azure DevOps services.**  
Azure Boards, Azure Repos, Azure Pipelines, Azure Test Plans, and Azure Artifacts.

**Q: Pipeline artifact versus Azure Artifacts package?**  
A pipeline artifact carries build output through pipeline jobs or stages. A package is a reusable, versioned dependency published to a feed for consumers.

**Q: Must every team use all five services?**  
No. Use the capabilities that support the team's delivery and evidence needs. Integration with external tools is valid when ownership, security, and traceability remain clear.

## Further reading

- [Azure DevOps documentation](https://learn.microsoft.com/en-us/azure/devops/)
- [What is Azure DevOps?](https://learn.microsoft.com/en-us/azure/devops/user-guide/what-is-azure-devops)
