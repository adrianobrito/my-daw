# UX UI Design Guide

## Canonical Sources

Use `docs/information-architecture.md` and `docs/main-performance-screen.md` first. Pull states from `docs/module-states.md`, tokens/components from `docs/design-system.md`, pattern behavior from `docs/pattern-mode-selector.md`, and scene behavior from `docs/scene-set-workflow.md`.

## Screen Model

Design one primary performance shell with ordered sections:

`Performance Shell -> Drums -> Synths -> Post-FX -> Master FX`

The shell always preserves transport, tempo, time signature, current/target scene, CPU, master output, settings/device entry, and panic/all-notes-off.

## Live Interaction Rules

- Group controls by live decision: rhythm/source, melodic/source, post processing, master output, and safety.
- Keep Drums, Synths, Post-FX, and Master FX in one scan path.
- Keep active and pending states visually distinct. Pending never replaces the applied active state until confirmed.
- Collapsed sections preserve activity, mute, solo, bypass, pending, loading, missing-resource, degraded, and error summaries.
- High-frequency actions such as recall, mute, solo, bypass, stop, and panic should avoid modal confirmation; destructive edits require confirmation.

## Required Surfaces

- Drums exposes MIDI/Sampled mode, Drum Synth, Drum MIDI, Pattern Engine, and Sampled Preview/Sample Control states.
- Synths exposes Bass, Poly/Chord, and Pluck/Stab lanes with preset/source, MIDI route, meter, pattern mode, ARP, MIDI FX, solo, mute, and expand controls.
- Post-FX shows ordered instrument/post processing, bypass, macro controls, meters, and post level.
- Master FX shows Master EQ, Glue Compressor, Stereo Width, Limiter, Master Level, master output meter, limiter/clipping warnings, and output safety state.
- Scene selector shows ordered set scenes without replacing the shell.

## Responsive Guidance

Optimize for desktop live performance. At narrower widths, preserve section order and shell status first; reduce graph detail before hiding controls. Below 768 px, support inspection and basic operation only.
