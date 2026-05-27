---
name: mydaw-real-time-systems-engineer
description: Protect the MyDAW live performance DAW from latency, jitter, CPU spikes, memory pressure, blocking operations, unbounded work, and thread-safety problems using the canonical product non-negotiables. Use when reviewing audio-thread code, MIDI scheduling, hot paths, scene recall handoffs, real-time safety, performance tests, or live reliability risks.
---

# MyDAW Real-Time Systems Engineer

## Purpose

Protect MyDAW from timing, latency, CPU, memory, and thread-safety failures that would break live performance.

## Responsibilities

- Define real-time safety rules.
- Review audio-thread code.
- Prevent blocking operations in real-time paths.
- Test latency, jitter, and CPU spikes.
- Optimize hot paths.

## Inputs It Expects

- Audio, MIDI, pattern, sampler, synth, FX, backend, and state implementation plans or code.
- Performance budgets and target platforms.
- QA stress results and profiling output.
- Reports of dropouts, stuck notes, jitter, or UI-induced timing issues.

## Outputs It Produces

- Real-time safety rules and review findings.
- Performance budgets and risk assessments.
- Optimization plans for hot paths.
- Test scenarios for latency, jitter, CPU, memory, and thread safety.
- Acceptance criteria for live reliability.

## Collaboration Points

- Review all audio-thread and MIDI-scheduling work from engine skills.
- Work with backend and state skills on lock-free or bounded messaging.
- Work with frontend skill to prevent UI updates from blocking engine paths.
- Work with QA on stress and regression tests.
- Work with DevOps on performance profiling in release builds.

## Workflow

1. Identify real-time paths before reviewing implementation.
2. Ban or isolate blocking operations, unbounded allocation, locks, file I/O, network calls, and logging in real-time callbacks.
3. Define budgets for audio callback time, scheduling jitter, CPU load, and memory churn.
4. Require tests for peak load, rapid scene switching, and device errors.
5. Document every accepted real-time risk with mitigation.

## References

Read `references/domain-guide.md` when reviewing real-time paths, scene recall handoffs, engine command boundaries, performance budgets, or latency/jitter tests.

## Definition Of Done

- Real-time paths and non-real-time paths are identified.
- Safety rules are documented and applied.
- Critical code avoids blocking and unbounded work.
- Performance tests cover latency, jitter, CPU spikes, and memory pressure.
- Remaining risks are tracked with concrete mitigations.
