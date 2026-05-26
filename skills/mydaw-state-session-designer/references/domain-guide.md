# State And Session Guide

## State Categories

- Project/session state: global tempo, routing, modules, devices, scenes, presets, references.
- Scene state: performance recall state such as active patterns, mutes, selected presets, FX states, macro values.
- Preset state: reusable module settings for synths, sampler controls, FX chains, or pattern configurations.
- Snapshot state: captured runtime configuration for fast recall.
- Runtime-only state: meters, transient pending UI state, current audio buffer data, temporary errors.

## Schema Requirements

- Include schema version.
- Keep IDs stable for modules, routes, scenes, presets, and resources.
- Store external resource references with enough information for recovery.
- Validate before applying to engine state.
- Provide migration paths for alpha changes when practical.

## Scene Recall

- Define quantization boundary for musical changes.
- Preserve note-off safety.
- Make pending and applied states visible.
- Avoid file I/O and decoding in the real-time path.
- Define partial failure behavior when a scene references missing resources.

## Autosave And Crash Recovery

- Autosave must never block audio or MIDI scheduling.
- Use atomic writes or equivalent safe persistence.
- Keep recovery files distinguishable from intentional saves.
- Surface recovery options clearly on next launch.

## Presets

- Define whether presets are embedded, referenced, or both.
- Define precedence between scene overrides and preset defaults.
- Ensure preset recall is safe during playback.
