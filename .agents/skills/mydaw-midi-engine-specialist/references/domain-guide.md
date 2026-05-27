# MIDI Engine Guide

## MIDI Routing

- Represent MIDI devices separately from logical routes.
- Allow routes from physical input, internal pattern generators, ARP, and MIDI FX to instruments or external outputs.
- Keep channel mapping explicit and visible to state/session persistence.
- Define behavior when an output device is missing during session load.

## Clock And Transport

- Define one clock authority at a time: internal clock or external MIDI clock.
- Support start, stop, continue, tempo, beat position, and quantized boundaries.
- Document how tempo changes affect scheduled notes and pattern generation.
- Avoid UI-thread timing as a source of musical scheduling.

## Scheduling

- Use timestamps or sample-accurate offsets when integrating with audio.
- Schedule note-on and note-off pairs safely.
- Preserve event ordering for simultaneous events.
- Bound lookahead and avoid unbounded event queues.

## MIDI Processing

- ARP and MIDI FX should transform event streams without hiding source state.
- Probability, swing, humanization, and variation must be deterministic enough for recall when required.
- Panic/all-notes-off must be available for stuck notes.
- Channel pressure, CC, pitch bend, and program changes should be scoped to MVP requirements.

## Reliability Checks

- Test clock drift against external sources.
- Test dense note streams and rapid scene changes.
- Test device disconnect and reconnect.
- Test stuck-note prevention during transport stop, route changes, and scene recall.
