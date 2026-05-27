# UX UI Design Guide

## Live Interface Principles

- Keep the first screen useful during performance; do not start with a landing page.
- Prefer dense but readable operational layouts over decorative panels.
- Prioritize status visibility: transport, tempo, scene, selected modes, meters, mutes, solos, bypass, and pending recalls.
- Use stable component dimensions so controls do not shift during playback.
- Keep high-risk actions away from high-frequency controls.

## Main Screen Organization

- Drums: show lanes or modules with source mode, pattern mode, mute/solo, meter, and quick sound controls.
- Synths: group Bass, Poly/Chord, and Pluck/Stab with pattern source, preset, mute/solo, and key parameters.
- Post-FX: show per-instrument or bus FX in order, with bypass and key macro controls.
- Master FX: show Master EQ, Glue Compressor, Stereo Width, Limiter, output meter, and safety state.
- Transport and scene/session status must stay visible or quickly reachable.

## Mode Selection

- MIDI vs Sampled selection must show the active engine path and unavailable controls.
- MIDI Generation vs MIDI Circular Pattern selection must show whether changes apply immediately, at next step, or at a quantized boundary.
- Pattern variation controls should be performable with coarse controls: density, complexity, probability, swing, variation, length.

## Stage-Friendly Controls

- Use larger hit targets for live-critical controls.
- Avoid ambiguous toggles; label or icon states must clearly show active vs inactive.
- Use meters and activity indicators to confirm audio/MIDI flow.
- Use confirmation only for actions that can disrupt the set; avoid modal friction for normal performance changes.

## Interaction Behavior

- Scene recall should display pending and applied states.
- Mute and bypass should provide immediate feedback even if audio changes are quantized.
- Collapse should preserve critical status indicators.
- Error states should identify what is broken and what still works.
