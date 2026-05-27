# Synth Integration Guide

## Canonical Sources

Use `docs/product-definition.md`, `docs/main-performance-screen.md`, `docs/module-states.md`, `docs/pattern-mode-selector.md`, and `docs/scene-set-workflow.md` for synth lanes, pattern inputs, visible states, and scene recall.

## MVP Synth Lanes

Synths contains:

- Bass Synth for low-end pattern and groove support.
- Poly/Chord Synth for harmonic beds and chord movement.
- Pluck/Stab Synth for short melodic or rhythmic accents.

Each lane exposes identity, preset/source label, MIDI channel or route, activity meter, pattern mode, ARP, MIDI FX, solo, mute, and expand/collapse.

## Integration Rules

- Connect synth note behavior to timestamped MIDI/pattern events, not UI timing.
- Route each synth through assigned Post-FX, then Master FX, then output.
- Keep MVP controls macro-level and stage-friendly before adding deep synth editing.
- Preset and scene recall must preserve note-off safety and avoid unsafe real-time work.

## States

Instrument lanes expose `active`, `muted`, `soloed`, `pending`, `loading`, `missing-resource`, and `error`. Collapsed lanes preserve preset/source, MIDI channel, active pattern mode, meter/activity, mute, solo, pending, and errors.

## QA Focus

Test dense MIDI, stuck-note prevention, preset switching, pattern mode switching, scene recall, voice limits, CPU load, and routing through FX.
