# Audio Engine Guide

## Canonical Sources

Use `docs/product-definition.md` for non-negotiables, `docs/information-architecture.md` and `docs/main-performance-screen.md` for routing visibility, and `docs/module-states.md` for meter/error state requirements.

## Audio Graph Model

Use predictable source-to-output flow:

`Instruments -> assigned Post-FX path -> Master FX -> output -> meters`

Drums, Bass Synth, Poly/Chord Synth, Pluck/Stab Synth, sampled sources, Post-FX, Master FX, master level, limiter, and output meters must expose enough state for the shell and section summaries.

## Real-Time Rules

Keep audio callback and sample-accurate paths bounded, non-blocking, and allocation-free in steady state where practical. Exclude file I/O, network I/O, logging, device enumeration, synchronous UI calls, waits, futures, locks that can block, and unbounded queues from real-time paths.

Prepare graph changes outside the callback and apply them through bounded real-time-safe handoffs.

## Device And Meter Behavior

- Treat device loss, reconnect, sample-rate changes, buffer-size changes, underruns, and broken output routes as expected failure modes.
- Publish lightweight meter snapshots to UI at control-rate cadence.
- Master output failures are summarized in the shell and Master FX section.
- Missing resources and route failures surface at affected modules, sections, and global summary when output is affected.
