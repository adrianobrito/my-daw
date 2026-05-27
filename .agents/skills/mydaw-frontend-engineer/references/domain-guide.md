# Frontend Engineering Guide

## Canonical Sources

Use `docs/main-performance-screen.md`, `docs/design-system.md`, and `docs/module-states.md` first. Use the JSON component contracts for pattern selector and scene/set workflow when binding commands and snapshots.

## Implementation Priorities

- Render the actual performance surface first: shell, Drums, Synths, Post-FX, Master FX.
- Use existing project framework, styling, state management, and component patterns.
- Keep UI state, persisted state, backend state, and engine runtime state separate.
- Treat engine/backend commands as asynchronous and fallible unless a narrower contract exists.
- Render from snapshots/subscriptions; do not infer applied audio, MIDI, pattern, or scene state from click timing.

## Core UI Contracts

- Pattern selector consumes `activeMode`, `pendingMode`, `pendingBoundary`, `uiState`, `disabledReason`, and `lastError`; pending target never looks applied until engine confirmation.
- Scene status consumes current scene, target scene, `sceneRecallState`, `applyBoundary`, affected scopes, and recall errors; current scene remains authoritative until an applied snapshot.
- Module cards derive dominant `uiState` through documented precedence and keep secondary states visible as badges/control states.
- Meters update at UI/control cadence and never drive audio or MIDI timing.
- Session load/save/autosave uses lifecycle states and must not replace the live session until validation succeeds or recovery is accepted.

## Layout Rules

- Preserve shell visibility during mode switching, scene recall, loading, errors, and settings/device panels.
- Preserve section order at all widths.
- Avoid layout shift from meters, changing values, hover states, pending labels, errors, and long names.
- Collapsed summaries keep active mode, meter/activity, mute, solo, bypass, pending, loading, missing-resource, degraded, and error indicators.

## Verification

Check wide, standard, compact, and minimum layouts. Verify pattern switching, scene recall, collapse, mute/solo/bypass, missing resources, loading, and error states visually and through tests where feasible.
