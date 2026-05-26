---
name: mydaw-qa-test-engineer
description: Create and run correctness, integration, performance, MIDI timing, scene switching, audio dropout, CPU stress, and regression tests for the MyDAW live performance DAW. Use when defining test plans, acceptance tests, reliability tests, or alpha release validation.
---

# MyDAW QA Test Engineer

## Purpose

Create and run tests that prove MyDAW works correctly and remains reliable under live-performance pressure.

## Responsibilities

- Write unit tests.
- Write integration tests.
- Test MIDI timing.
- Test scene switching.
- Test audio dropouts.
- Test CPU stress.
- Maintain regression test suites.

## Inputs It Expects

- Product acceptance criteria.
- UX flows and design-system state expectations.
- Engine, backend, and persistence contracts.
- Real-time performance budgets.
- Bug reports and release criteria.

## Outputs It Produces

- Unit, integration, performance, and regression test plans.
- Automated tests where feasible.
- Manual live rehearsal test scripts where automation is insufficient.
- Bug reports with reproduction steps and severity.
- Release readiness recommendations.

## Collaboration Points

- Work with product skill to convert requirements into acceptance tests.
- Work with real-time skill on latency, jitter, CPU, and dropout tests.
- Work with engine and backend skills on integration test seams.
- Work with frontend and UX skills on workflow and state-visibility tests.
- Work with DevOps on CI, artifacts, and alpha validation.

## Workflow

1. Start from live failure modes, not only code coverage.
2. Map every MVP feature to at least one acceptance scenario.
3. Add stress cases for transport running, dense MIDI, scene recall, FX changes, and device errors.
4. Preserve regressions with targeted tests.
5. Report residual risk clearly before release.

## References

Read `references/domain-guide.md` when designing test suites, reliability scenarios, timing tests, or release validation.

## Definition Of Done

- MVP workflows have acceptance tests.
- Unit and integration tests cover core state and engine contracts.
- MIDI timing, scene switching, dropouts, and CPU stress are tested.
- Known risks and gaps are documented.
- Release candidate has clear pass/fail evidence.
