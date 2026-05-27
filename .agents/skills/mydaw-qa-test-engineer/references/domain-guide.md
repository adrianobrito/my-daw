# QA Test Guide

## Canonical Sources

Use all `docs/` acceptance criteria. Prioritize `product-definition.md`, `main-performance-screen.md`, `module-states.md`, `pattern-mode-selector.md`, and `scene-set-workflow.md`.

## MVP Acceptance Scenarios

Test that a performer can:

- load a prepared session,
- confirm device, CPU, transport, scene, and master output status,
- start and stop transport,
- switch Drums between MIDI and Sampled modes,
- switch pattern surfaces between Generation and Circular with visible pending state,
- mute/solo lanes,
- bypass FX while preserving chain visibility,
- recall scenes with current/target/boundary visibility,
- recover from stuck notes with panic/all-notes-off,
- handle missing samples, presets, devices, routes, and failed recalls without losing current session state.

## UI And State Tests

- Verify shell -> Drums -> Synths -> Post-FX -> Master FX ordering.
- Verify collapsed summaries preserve activity, active mode, mute, solo, bypass, pending, loading, missing-resource, degraded, and error states.
- Verify active and pending states are visually and semantically distinct.
- Verify meters, changing values, labels, and errors do not shift layout.
- Verify color is not the only state signal and focus order follows performance workflow.

## Engine And Reliability Tests

- Dense MIDI and pattern generation under transport.
- Rapid pattern switches and scene recalls.
- Note-off safety on stop, route change, pattern switch, and recall.
- Rapid FX bypass/order/state changes.
- Device disconnect/reconnect, clock loss, missing resources, and failed save/load.
- CPU stress, memory pressure, latency, jitter, and dropout tests.

## Release Evidence

Report pass/fail status, platform coverage, known limitations, residual risks, repro steps for failures, and whether any issue blocks private alpha.
