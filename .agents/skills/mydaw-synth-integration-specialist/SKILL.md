---
name: mydaw-synth-integration-specialist
description: Design and integrate MyDAW Bass Synth, Poly/Chord Synth, Pluck/Stab Synth, preset handling, scene recall, routing, and synth connections to MIDI and pattern engines from the canonical MVP docs. Use when working on synth modules, instrument engines, synth presets, playable ranges, voice behavior, or synth-pattern integration.
---

# MyDAW Synth Integration Specialist

## Purpose

Design and integrate synth modules that are musically useful, performable, and cleanly connected to MIDI, pattern, FX, and session systems.

## Responsibilities

- Implement Bass Synth module.
- Implement Poly / Chord Synth module.
- Implement Pluck / Stab Synth module.
- Define preset handling for synths.
- Integrate synth modules with MIDI and pattern engines.

## Inputs It Expects

- Product scope for synth modules and MVP sound design.
- MIDI and pattern event contracts.
- Audio engine voice and routing architecture.
- UX and design-system component specs.
- State/session requirements for presets and scenes.

## Outputs It Produces

- Synth module behavior and parameter specs.
- Preset schema and recall behavior.
- MIDI mapping and playable range rules.
- Audio routing and FX send requirements.
- Tests for note handling, presets, and CPU behavior.

## Collaboration Points

- Work with audio and FX skills on voice rendering and routing.
- Work with MIDI and pattern skills on note, chord, ARP, and modulation behavior.
- Work with state/session skill on preset save/load and scene recall.
- Work with frontend and UX skills on module controls and preset selection.
- Work with real-time and QA skills on CPU load and stuck-note prevention.

## Workflow

1. Define the musical role of each synth before parameter depth.
2. Keep MVP controls macro-level and stage-friendly.
3. Specify MIDI response, voice allocation, envelopes, filter behavior, and preset recall.
4. Route each synth through the shared Post-FX and Master FX model.
5. Test dense MIDI, preset switching, scene recall, and CPU load.

## References

Read `references/domain-guide.md` when defining synth modules, presets, MIDI integration, pattern inputs, scene recall, voice behavior, or performance controls.

## Definition Of Done

- Bass, Poly/Chord, and Pluck/Stab synth roles are defined.
- Preset behavior is clear and safe during transport.
- MIDI and pattern inputs produce deterministic note behavior.
- Parameters are scoped for MVP and documented.
- QA covers stuck notes, preset recall, voice limits, and CPU stress.
