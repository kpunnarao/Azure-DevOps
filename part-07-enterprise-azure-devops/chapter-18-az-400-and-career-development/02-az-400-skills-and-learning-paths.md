# AZ-400 Skills and Learning Paths

[← Certification Path](01-certification-path-and-prerequisites.md) · [Chapter 18](README.md) · [Next: Practice and Gap Analysis →](03-practice-assessments-and-gap-analysis.md)

## Use the study guide as the source of truth

Download/review the official AZ-400 study guide on the day you build your plan and again before scheduling. Microsoft updates the English exam first and localized versions may follow later. Track the “skills measured as of” date and change log.

Current domains map to this repository:

| AZ-400 domain | Primary Parts |
|---|---|
| Processes and communications | I, II, VI, VII |
| Source-control strategy | II |
| Build/release pipelines | III, IV, V, VI, VII |
| Security and compliance | II–VII |
| Instrumentation | IV, VI, VII |

The exam outline also expects both GitHub and Azure DevOps solution experience. Add GitHub Issues/Projects, branch protection, Actions, runners, environments, packages, authentication/GITHUB_TOKEN/OIDC, Advanced Security/Dependabot, and insights labs where this Azure DevOps-centered repository does not provide equivalent hands-on work.

## Objective-to-evidence matrix

For every bullet in the official outline record:

- Confidence 0–3.
- Official documentation link.
- Hands-on lab.
- Failure/troubleshooting example.
- Design tradeoff.
- Capstone evidence.
- Last review date.
- Next action.

A read article counts as exposure, not capability. Level 3 means implement, break, diagnose, compare, and explain.

## Weight study intelligently

Build/release pipelines currently carry 50–55%, so they deserve the largest study time. Still integrate every domain: a pipeline scenario may test identity, package version, approval, database migration, and monitoring simultaneously.

Suggested cycle:

- 50% hands-on build/break.
- 20% official documentation/study modules.
- 15% scenario questions.
- 10% teaching/explanation.
- 5% flash review of exact product distinctions.

Adjust based on measured gaps.

## Documentation habits

Keep notes as decision tables rather than copied paragraphs: when to use, how it works, security boundary, failure modes, alternatives, limits, and verification. Date volatile facts such as licensing, agent types, exam weights, deprecations, and feature support.

## Interview preparation

**How study a broad objective like pipelines?**  
Decompose into triggers, graph/data, agents, templates, artifacts, packages, testing, deployment, security, performance, retention, and troubleshooting; create integrated scenarios.

**What if Microsoft changes the outline?**  
Diff the new guide, classify new/changed/removed objectives, update the matrix/repository, and run targeted labs before the exam.

## Practical exercise

Convert every current study-guide bullet into a spreadsheet/Markdown matrix. Link this repository's pages and capstone evidence, then create GitHub-specific labs for unmapped objectives.

## Official references

- [Official AZ-400 study guide](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-400)
- [AZ-400 certification training page](https://learn.microsoft.com/training/courses/az-400t00)

[Next: Practice Assessments and Gap Analysis →](03-practice-assessments-and-gap-analysis.md)
