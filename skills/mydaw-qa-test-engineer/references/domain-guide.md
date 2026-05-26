# QA Test Guide

## Test Categories

- Unit tests: pure logic, schemas, pattern generation, parameter mapping, reducers, command validation.
- Integration tests: UI-to-backend commands, persistence flows, engine command boundaries, module lifecycle.
- Timing tests: MIDI scheduling, clock sync, quantized switching, note-off safety.
- Audio reliability tests: dropouts, routing, bypass, mute/solo, device changes.
- Stress tests: CPU load, dense patterns, rapid scene recall, sample loading, FX toggles.
- Regression tests: every fixed live-critical bug gets a targeted test.

## MVP Acceptance Scenarios

- Create a session with Drums, Synths, Post-FX, and Master FX.
- Switch MIDI vs Sampled mode where supported.
- Switch MIDI Generation and MIDI Circular Pattern modes during transport.
- Recall scenes while transport runs.
- Save, load, autosave, and recover sessions.
- Toggle mute, solo, bypass, and collapse without losing state.

## Timing And Reliability

- Measure jitter under normal and stressed CPU conditions.
- Test external MIDI clock if supported.
- Verify no stuck notes on stop, panic, scene recall, route change, and preset change.
- Check audio callback stability while UI meters update.

## Reporting

Bug reports should include:

- expected behavior
- actual behavior
- reproduction steps
- severity for live performance
- logs, screenshots, timing data, or audio observations where relevant

## Release Gate

Do not recommend alpha release if there are known reproducible crashes, audio dropouts in expected use, destructive save/load bugs, or uncontrolled stuck notes.
