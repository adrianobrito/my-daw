# Pattern Generation Guide

## Canonical Sources

Use `docs/pattern-mode-selector.md` and `docs/pattern-mode-selector-component-contract.json` first. Use `docs/scene-set-workflow.md` for scene recall of pattern state and `docs/module-states.md` for visible state behavior.

## Pattern Modes

MVP pattern surfaces expose both modes without a dropdown:

- `midiGeneration`: visible label `Generation`, compact label `Gen`.
- `midiCircularPattern`: visible label `Circular`, compact label `Circ`.

The applied mode remains active until an engine snapshot confirms a switch. Requested changes render as `pending` with a boundary such as `nextStep`, `nextBar`, or `sceneBoundary`.

## Parameters

Generation controls:

- `density`: 0-100%, applies at `nextStep` or quantized boundary.
- `complexity`: 0-100%, quantized.
- `variation`: 0-100%, quantized.
- `probability`: 0-100%, next step.
- `length`: 1/2 Bar, 1 Bar, 2 Bars, 4 Bars, quantized.
- `swing`: 0-75%, next step.

Circular controls:

- `steps`: 1-64, quantized.
- `activeSteps`: per-step on/off, next step.
- `rotation`: 0 to `steps - 1`, quantized.
- `probability`: 0-100%, next step.
- `length`: 1/2 Bar, 1 Bar, 2 Bars, 4 Bars, quantized.
- `swing`: 0-75%, next step.

Persist seeds, pattern ids, generation ids, and step previews when repeatable scene recall matters.

## Engine Contract

UI requests pattern switches with lane id, requested mode, requested boundary, and command id. The engine publishes snapshots with active mode, pending mode, boundary, switch id, UI state, and last error.

Scheduling rules:

- Timestamp MIDI events in the engine, never from UI event timing.
- Preserve note-on/note-off pairing through mode switches.
- Schedule old-mode note-offs before or at the switch boundary when needed.
- Keep event ordering deterministic and pending switch queues bounded.
- Keep panic/all-notes-off available for recovery.
