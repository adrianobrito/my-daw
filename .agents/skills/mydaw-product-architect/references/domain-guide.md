# Product Architecture Guide

## Canonical Sources

Start with `docs/product-definition.md`. Use `docs/information-architecture.md`, `docs/main-performance-screen.md`, `docs/pattern-mode-selector.md`, and `docs/scene-set-workflow.md` when product decisions affect screens, pattern behavior, or session/scene workflows.

## Product Position

MyDAW is a cross-platform desktop DAW for solo electronic live performers. It prioritizes a stage-ready performance surface for drums, synths, MIDI pattern workflows, Post-FX, Master FX, and scene/session recall over deep studio editing.

The first screen is the usable live-performance surface. It is not a landing page, setup wizard, arrangement timeline, or mode hub.

## MVP Scope

Include:

- Persistent shell with transport, tempo, time signature, scene, CPU, master output, settings/device entry, and panic/all-notes-off.
- Drums with MIDI and Sampled source modes.
- Synths with Bass Synth, Poly/Chord Synth, and Pluck/Stab Synth.
- Post-FX instrument/post layer and distinct Master FX chain.
- MIDI Generation and MIDI Circular Pattern modes with visible active and pending state.
- Scene/session save, load, recall, autosave recovery, and missing-resource recovery.
- Visible loading, pending, bypassed, muted, soloed, unavailable, degraded, and error states.

Defer full timeline arrangement, advanced sample editing, third-party plugin hosting, cloud collaboration, deep modulation matrix, complex controller mapping, and advanced external hardware routing unless promoted by explicit product decision.

## Non-Negotiables

- Reliability and timing are product features.
- No known reproducible audio dropout ships in the intended alpha workflow.
- Scene/session data must not be destroyed by failed load or recall.
- Stuck-note recovery must be first-level through panic/all-notes-off.
- Missing devices, samples, presets, routes, and failed recalls must be visible and recoverable where possible.

## Acceptance Criteria Pattern

Frame acceptance around live performance tasks: load a prepared session, start transport, vary drums/synth patterns, mute/solo lanes, adjust FX, recall scenes, recover from problems, and stop safely from the main screen.
