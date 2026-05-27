---
name: mydaw-sampler-specialist
description: Design MyDAW Sampled mode, sample loading, playback, triggering, missing-sample recovery, transpose controls, envelopes, sample control, scene recall, and sample-related FX integration from the canonical MVP docs. Use when working on sampled mode, drum samples, sample playback reliability, envelopes, transpose, sample EQ controls, or sample resource handling.
---

# MyDAW Sampler Specialist

## Purpose

Design and implement sample playback behavior that is responsive, predictable, and safe for live use.

## Responsibilities

- Implement sample loading and playback.
- Implement sample triggering.
- Implement transpose controls.
- Implement sample envelopes.
- Integrate Sample Control with FX such as transpose and 3-band EQ.

## Inputs It Expects

- Product and UX requirements for Sampled mode.
- Audio engine voice, buffering, and routing constraints.
- MIDI engine trigger and scheduling contracts.
- State/session requirements for sample paths and presets.

## Outputs It Produces

- Sample playback and trigger behavior.
- Sample loading and missing-file policy.
- Envelope, transpose, choke, and voice behavior specs.
- Sample control and EQ integration contracts.
- Tests for sample timing, switching, and resource handling.

## Collaboration Points

- Work with `mydaw-audio-engine-architect` on voice rendering and routing.
- Work with `mydaw-midi-engine-specialist` on trigger timing and note semantics.
- Work with `mydaw-state-session-designer` on sample references and preset recall.
- Work with UX/frontend skills on Sampled mode controls and missing sample states.
- Work with QA and real-time systems on loading, CPU, memory, and dropout tests.

## Workflow

1. Define sample lifecycle: load, decode, prepare, trigger, stop, unload.
2. Keep file I/O and decoding out of real-time paths.
3. Specify voice behavior for polyphony, choke groups, one-shot, gated, and looped playback as needed.
4. Define how transpose, envelope, gain, pan, and 3-band EQ apply.
5. Test rapid triggering, sample changes, and scene recall under transport.

## References

Read `references/domain-guide.md` when designing Sampled mode, loading behavior, missing-sample recovery, envelopes, transpose, voice allocation, scene recall, or sample FX integration.

## Definition Of Done

- Sample loading and playback behavior are documented.
- Trigger timing works with MIDI scheduling.
- Missing or invalid sample behavior is visible and non-crashing.
- Transpose, envelope, and EQ controls have clear ranges.
- Real-time and QA reviews cover CPU, memory, and dropout risks.
