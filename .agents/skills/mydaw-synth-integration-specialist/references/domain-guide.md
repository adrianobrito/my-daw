# Synth Integration Guide

## Module Roles

- Bass Synth: monophonic or limited-voice low-end instrument with stable pitch, glide if included, filter, envelope, and drive or tone macro.
- Poly / Chord Synth: harmonic instrument with polyphony, chord handling, voicing constraints, envelope, filter, and tone controls.
- Pluck / Stab Synth: short transient-focused synth for rhythmic hooks, stabs, and arpeggiated material.

## MVP Controls

Prefer performable macro controls:

- preset
- gain
- filter cutoff/resonance
- envelope shape
- tone or brightness
- drive or character
- glide for bass if supported
- chord/voicing control for Poly/Chord
- decay or pluck shape for Pluck/Stab

Avoid exposing deep synthesis pages unless required by the MVP.

## MIDI Integration

- Define playable note ranges and default channels.
- Specify mono, poly, legato, glide, voice stealing, and note priority behavior.
- Handle note-off safety during preset changes, scene recall, mute, and transport stop.
- Support pattern-generated events and external MIDI consistently.

## Presets

- Presets must include synth parameters, macro values, and any module-specific mode.
- Scene recall may reference presets or capture overridden values; define precedence.
- Preset switching during playback should avoid clicks, CPU spikes, and stuck notes.

## Routing And FX

- Synths route to their Post-FX chains, then Master FX.
- Bypass and mute behavior must be consistent with sampler and drums.
- Metering should expose activity even for short plucks and stabs.
