# MIDI Engine Guide

## Canonical Sources

Use `docs/product-definition.md` for reliability constraints, `docs/pattern-mode-selector.md` for pattern switching, and `docs/scene-set-workflow.md` for scene recall timing. Use JSON contracts when defining command and snapshot fields.

## Core MIDI Responsibilities

- Maintain one clock authority at a time: internal clock or external MIDI clock.
- Integrate transport, tempo, beat position, start/stop/continue, and clock sync deterministically.
- Route MIDI input/output with explicit channel mapping and reconnect behavior.
- Schedule pattern, ARP, MIDI FX, synth, sampler, and scene-driven events with timestamps or sample-accurate offsets.

## Pattern Switching

Pattern switches use `requestPatternModeSwitch` with lane id, requested mode, requested boundary, and command id. While playing, default mode switching applies at `nextBar` unless a lane contract narrows it to `nextStep`.

Do not let UI timing schedule musical events. Render active and pending state from engine snapshots.

## Scene Recall Timing

Scene recall may affect source modes, pattern modes, presets, mutes, solos, bypass, routes, tempo, and time signature. The MIDI layer must support prepared, bounded handoffs at `immediate`, `nextStep`, `nextBar`, `nextPhrase`, or `sceneBoundary`.

## Safety Requirements

- Preserve note-on/note-off pairing through mode switches, route changes, transport stop, and scene recall.
- Preserve deterministic ordering for simultaneous events.
- Keep scheduler lookahead and pending queues bounded.
- Provide panic/all-notes-off for stuck-note recovery.
- Treat device disconnect/reconnect and clock loss as expected failure modes.
