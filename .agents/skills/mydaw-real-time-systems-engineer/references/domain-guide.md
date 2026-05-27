# Real-Time Systems Guide

## Canonical Sources

Use `docs/product-definition.md` non-negotiables first. Use `docs/pattern-mode-selector.md` and `docs/scene-set-workflow.md` when reviewing pattern switching and scene recall handoffs.

## Real-Time Path Rules

On audio-thread and sample-accurate MIDI paths, avoid:

- locks that can block,
- heap allocation in steady state,
- file I/O,
- network I/O,
- logging,
- device enumeration,
- synchronous UI calls,
- unbounded queues,
- unbounded loops,
- waits on futures/promises/condition variables.

Prepare graph, route, preset, sample, session, and scene changes outside real-time paths. Apply through bounded, real-time-safe handoffs.

## Live Reliability Risks

Review these as first-class product risks:

- audio dropouts,
- MIDI jitter,
- stuck notes,
- lost note-offs,
- CPU spikes,
- memory churn,
- device disconnect/reconnect,
- route changes during playback,
- rapid scene recall,
- rapid bypass/mute/solo/pattern switching.

## Required Safeguards

- Meter taps publish lightweight UI snapshots and never block processing.
- Scheduler lookahead and pending command queues stay bounded.
- Pattern switching preserves note-on/note-off pairing.
- Scene recall validates and prepares resources before real-time handoff.
- Panic/all-notes-off remains available for recovery.

## Acceptance Criteria

No private-alpha release ships with a known reproducible audio dropout in the intended MVP workflow. Remaining timing or performance risks must have concrete mitigation and test coverage.
