# State Session Guide

## Canonical Sources

Use `docs/scene-set-workflow.md` and `docs/scene-set-workflow-component-contract.json` first. Use `docs/module-states.md` for affected scope visibility and `docs/pattern-mode-selector.md` for persisted pattern state.

## Terminology

- `session`: saved project containing set order, scenes, presets, resources, routes, devices, and defaults.
- `set`: ordered performance plan inside a session.
- `scene`: recallable performance state.
- `snapshot`: runtime capture that can become a scene or recovery point after validation.
- `preset`: reusable module, synth, sampler, FX, or pattern configuration.

## State Boundaries

Persist session, scene, and preset state. Do not persist meters, CPU snapshots, MIDI playhead, scheduler queues, audio buffers, hover/focus/open menu state, in-flight command ids, or resolved transient errors as scene state.

Scene state may own source modes, pattern states, presets, mute/solo/bypass, macros, levels, route targets, tempo/time signature, and recall boundary overrides.

## Recall States And Timing

Use scene recall states exactly:

`current`, `selected`, `armed`, `pending`, `queued`, `applying`, `applied`, `blocked`, `failed`, `partialRecoverable`.

Current scene remains authoritative until an applied snapshot confirms the target. While playing, default recall is `nextBar`; scenes that change phrase length, routes, presets, or several pattern engines may use `sceneBoundary`.

## Validation And Recovery

Validate loaded sessions and scene recalls before live application:

- schema version supported or migrated,
- stable unique ids,
- resources available or recoverable,
- devices/routes available or degraded gracefully,
- FX/routing prepared off real-time paths,
- changes represent bounded engine commands.

Failed load or save preserves current in-memory session state. Failed recall preserves the last confirmed current scene. Partial recall is allowed only when failed changes are scoped and visible.
