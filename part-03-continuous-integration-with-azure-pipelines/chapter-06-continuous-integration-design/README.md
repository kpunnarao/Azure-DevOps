# Chapter 6 — Continuous Integration Design

[← Part III — Continuous Integration with Azure Pipelines](../README.md)

A pipeline that merely compiles code is not yet a dependable continuous-integration system. This chapter explains how to design CI as a trustworthy evidence-producing process: every accepted change is built in a controlled environment, tested at appropriate levels, assigned an immutable identity, and published once for later deployment.

## Why this chapter matters

Poor CI design creates slow feedback, flaky gates, unreproducible releases, and “it passed on my machine” incidents. Good CI shortens the time between a defect being introduced and discovered while preserving enough evidence to explain exactly what was built. These practices apply whether the output is a web application, library, infrastructure module, mobile package, or container image.

## What you will learn

- Separate fast pull-request validation from authoritative main-branch integration.
- Build one immutable release candidate and promote it through environments.
- Make builds repeatable by controlling dependencies, tools, inputs, and environment.
- Design a balanced test strategy and enforce meaningful quality gates.
- Improve speed through measurement, caching, parallelism, and selective execution.
- Use coverage and static analysis as decision signals rather than vanity metrics.
- Create collision-free build and package versions.
- Preserve artifact identity, container provenance, and retention evidence.

## Topics

| # | Topic | Practical outcome |
|---:|---|---|
| 1 | [Build Once, Deploy Many](01-build-once-deploy-many.md) | Promote identical bits between environments |
| 2 | [Deterministic and Reproducible Builds](02-deterministic-and-reproducible-builds.md) | Control inputs and explain build variance |
| 3 | [PR Validation and Main-Branch CI](03-pr-validation-and-main-branch-ci.md) | Design fast and authoritative feedback loops |
| 4 | [Test Pyramids and Quality Gates](04-test-pyramids-and-quality-gates.md) | Match tests and gates to risk |
| 5 | [Caching, Parallelism, and Performance](05-caching-parallelism-and-performance.md) | Reduce lead time safely |
| 6 | [Code Coverage and Static Analysis](06-code-coverage-and-static-analysis.md) | Turn analysis into actionable evidence |
| 7 | [Build Numbering and Semantic Versioning](07-build-numbering-and-semantic-versioning.md) | Give every output a durable identity |
| 8 | [Container Builds, Provenance, and Retention](08-container-builds-provenance-and-retention.md) | Trace a deployed image back to source and run |

## Guided chapter lab

Create a CI pipeline for a small application. It should restore from a lock file, compile, execute unit tests, publish test and coverage results even when tests fail, package the application once, and publish an immutable pipeline artifact. Add a main-branch container build tagged with both a human-readable version and an immutable identifier.

Record:

1. Commit SHA, run ID, build number, toolchain version, and dependency-lock hash.
2. Duration of each step before and after caching.
3. The test layers chosen and why.
4. The exact artifact or image digest that would be promoted.
5. A retention decision for ordinary CI runs and released outputs.

Then simulate three failures: stale cache, flaky test, and unavailable package source. Decide whether each failure should retry, fail the run, or require investigation.

## Completion criteria

You are ready to continue when you can explain why rebuilding for production breaks provenance, design distinct PR and main-branch workflows, diagnose nondeterminism, defend a quality gate, and trace a released binary or image to one source revision and pipeline run.

## Chapter navigation

[← Chapter 5](../chapter-05-yaml-pipeline-fundamentals/README.md) · [Chapter 7 →](../chapter-07-reusable-pipelines-and-yaml-templates/README.md)
