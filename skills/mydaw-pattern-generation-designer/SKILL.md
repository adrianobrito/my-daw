---
name: mydaw-pattern-generation-designer
description: Design MIDI Generation, MIDI Circular Pattern, ARP, randomness, variation, swing, probability, humanization, and live-safe pattern switching for the MyDAW live performance DAW. Use when working on pattern generation behavior, circular patterns, pattern controls, variation, or quantized pattern changes.
---

# MyDAW Pattern Generation Designer

## Purpose

Design musical pattern systems that provide useful variation during performance while staying predictable, controllable, and safe to switch live.

## Responsibilities

- Implement MIDI Generation mode.
- Implement MIDI Circular Pattern mode.
- Define density, complexity, variation, swing, probability, and length behavior.
- Design pattern switching rules.
- Support live-safe pattern changes.

## Inputs It Expects

- Product requirements for generative and circular workflows.
- MIDI engine timing contracts.
- UX specs for pattern mode selection and live controls.
- Synth, sampler, ARP, MIDI FX, and state constraints.

## Outputs It Produces

- Pattern behavior specs and parameter definitions.
- Event generation contracts for MIDI scheduling.
- Pattern switching and quantization rules.
- Persistence requirements for pattern state and seeds.
- Test scenarios for musical timing and recall.

## Collaboration Points

- Work with `mydaw-midi-engine-specialist` on scheduling and event contracts.
- Work with `mydaw-state-session-designer` on scene recall, seeds, snapshots, and persistence.
- Work with synth and sampler skills on playable ranges and trigger behavior.
- Work with UX, design system, and frontend skills on clear controls and pending states.
- Work with QA on switching, timing, and determinism tests.

## Workflow

1. Define musical intent and parameter ranges before implementation.
2. Make random behavior controllable through seeds, locks, or bounded variation.
3. Specify when changes apply: immediate, next step, next bar, or scene boundary.
4. Keep generated output schedulable by the MIDI engine.
5. Test under rapid mode switching and scene recall.

## References

Read `references/domain-guide.md` when defining generation parameters, circular pattern rules, ARP behavior, randomness, or live-safe switching.

## Definition Of Done

- MIDI Generation and MIDI Circular Pattern modes are specified.
- Pattern controls have clear ranges and musical meaning.
- Switching behavior is quantized and safe.
- Persistence requirements for parameters, seeds, and active patterns are clear.
- QA can test timing, variation, recall, and no-stuck-note behavior.
