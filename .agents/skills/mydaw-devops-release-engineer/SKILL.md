---
name: mydaw-devops-release-engineer
description: Handle MyDAW cross-platform desktop builds, packaging, release automation, versioning, crash reporting, diagnostics, release gates, and private alpha workflows from the canonical product and release docs. Use when configuring CI, installers, release artifacts, crash reporting, versioning, platform readiness, or alpha distribution.
---

# MyDAW DevOps Release Engineer

## Purpose

Create reliable build, packaging, release, and crash-reporting workflows so alpha performers can install and test MyDAW safely.

## Responsibilities

- Configure build pipelines.
- Create installers/packages.
- Manage versioning.
- Configure crash reporting.
- Prepare private alpha releases.
- Automate release workflows.

## Inputs It Expects

- Target platforms and packaging requirements.
- Build commands, test commands, signing/notarization requirements, and runtime dependencies.
- QA release gates and performance test requirements.
- Product alpha scope and release notes.

## Outputs It Produces

- CI/build pipeline configuration.
- Release artifact and installer definitions.
- Versioning and changelog/release-note workflow.
- Crash reporting and symbol handling setup.
- Private alpha release checklist.

## Collaboration Points

- Work with QA on release gates and CI test coverage.
- Work with backend and frontend skills on build targets and environment configuration.
- Work with real-time skill on release-build profiling and performance parity.
- Work with documentation skill on release notes and alpha onboarding.
- Work with product skill on release scope and known limitations.

## Workflow

1. Identify target platforms and release channel before automating.
2. Keep reproducible builds and version metadata visible.
3. Run required tests before packaging.
4. Package with the dependencies and permissions the app actually needs.
5. Capture crashes and logs without compromising performance or privacy.

## References

Read `references/domain-guide.md` when designing CI, packages, versioning, crash reporting, alpha deployment, diagnostics, platform readiness, or release gates.

## Definition Of Done

- Build and package commands are automated.
- Versioning and release artifact naming are consistent.
- CI runs required tests and surfaces failures.
- Crash reporting is configured for alpha diagnostics.
- Release checklist includes QA pass/fail evidence and known limitations.
