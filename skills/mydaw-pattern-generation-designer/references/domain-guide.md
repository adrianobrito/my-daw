# Pattern Generation Guide

## Pattern Modes

MIDI Generation mode should create or mutate note events from musical controls such as density, complexity, variation, probability, length, scale, register, and rhythm emphasis.

MIDI Circular Pattern mode should cycle through a defined set of steps or events, supporting rotation, length changes, probability, and variation without losing the performer’s sense of position.

## Parameter Semantics

- Density: how often events occur.
- Complexity: rhythmic and melodic detail.
- Variation: controlled departure from the current pattern.
- Swing: timing offset applied to defined subdivisions.
- Probability: chance that eligible events fire.
- Length: musical duration of the repeating phrase.
- Humanization: bounded timing, velocity, or note variation that does not break sync.

## Live-Safe Switching

- Prefer applying structural changes at quantized boundaries.
- Show pending state in UI before the change lands.
- Preserve note-off integrity when switching patterns.
- Avoid sudden unbounded density or CPU changes.
- Provide clear behavior for transport stop, restart, and scene recall.

## Randomness And Recall

- Persist seeds or generated material when repeatability matters.
- Define whether scene recall restores exact output or parameter state.
- Keep randomization bounded by musical scale, range, and density constraints.
- Separate "generate new" from "vary current" to avoid accidental destructive changes.

## ARP And MIDI FX

- ARP behavior should define order, octave range, rate, gate, latch, and sync.
- MIDI FX should expose transformations such as transpose, scale constrain, velocity shape, delay, probability, and humanization.
- Transformations must preserve event ordering and note-off safety.
