---
name: mydaw-midi-engine-specialist
description: Design and implement MIDI routing, MIDI clock synchronization, channel mapping, scheduling, transport integration, and MIDI processing for the MyDAW live performance DAW. Use when working on MIDI input/output, clock sync, pattern timing, ARP, MIDI FX, or MIDI-to-engine integration.
---

# MyDAW MIDI Engine Specialist

## Purpose

Design and implement MIDI infrastructure that keeps pattern playback, external devices, transport, and live controls synchronized and reliable.

## Responsibilities

- Implement MIDI input/output routing.
- Implement MIDI clock sync.
- Implement channel mapping.
- Implement MIDI scheduling.
- Integrate MIDI with transport and pattern systems.

## Inputs It Expects

- Product requirements for MIDI workflows and external devices.
- Pattern, ARP, MIDI FX, synth, sampler, and transport specs.
- Backend messaging contracts and state schemas.
- QA timing, jitter, and sync reports.

## Outputs It Produces

- MIDI routing and device lifecycle design.
- Clock sync and transport behavior.
- Channel mapping rules.
- Scheduling APIs and timing contracts.
- MIDI event contracts for pattern, synth, sampler, and UI layers.

## Collaboration Points

- Work with `mydaw-pattern-generation-designer` on event generation and quantized switching.
- Work with synth and sampler skills on note, CC, velocity, choke, and trigger semantics.
- Work with `mydaw-real-time-systems-engineer` on jitter and scheduling safety.
- Work with backend and state skills on device persistence and routing recall.
- Work with QA on clock, latency, and external device tests.

## Workflow

1. Define transport, tempo, beat position, and clock ownership.
2. Separate timestamped scheduling from immediate UI commands.
3. Normalize MIDI routing and channel mapping before integrating modules.
4. Make clock start, stop, continue, tempo changes, and external sync behavior explicit.
5. Validate jitter and ordering under load.

## References

Read `references/domain-guide.md` when designing MIDI routing, clock sync, scheduling, channel mapping, or MIDI integration contracts.

## Definition Of Done

- MIDI input/output and channel routing are documented.
- Transport and clock sync behavior are deterministic.
- Pattern, ARP, MIDI FX, synth, and sampler integrations use timestamped event contracts.
- Device errors and reconnect behavior are handled.
- Timing tests cover jitter, scene switching, and CPU stress.
