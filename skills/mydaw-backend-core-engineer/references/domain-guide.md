# Backend Core Guide

## Layer Boundaries

- UI layer: renders state and sends commands.
- Application core: validates commands, coordinates services, owns high-level workflow.
- State/session layer: serializes, validates, migrates, and recalls persisted state.
- Engine layer: audio, MIDI, pattern, synth, sampler, and FX runtime behavior.
- Platform layer: devices, file system, crash reporting, packaging integrations.

Avoid direct UI access to real-time engine internals.

## Messaging

- Commands represent intent from UI or controllers.
- Events represent completed or observed changes.
- Queries or subscriptions expose current state.
- High-frequency streams such as meters should use specialized throttled channels.
- Errors must be structured enough for UI and logs.

## Module Architecture

- Define module identity, lifecycle, parameters, routing, presets, and serialization.
- Support instruments, FX, MIDI processors, and pattern sources with consistent contracts where practical.
- Avoid plugin flexibility that exceeds MVP needs.
- Ensure modules can be restored from session state.

## Persistence APIs

- Provide save, load, autosave, recover, preset save/load, and scene recall operations.
- Validate before applying loaded data.
- Keep persistence work off real-time threads.

## Integration Risks

- UI event floods overwhelming engine queues.
- Unbounded command queues.
- Ambiguous ownership of derived state.
- Blocking file or device work inside engine callbacks.
- Silent partial failures during scene recall.
