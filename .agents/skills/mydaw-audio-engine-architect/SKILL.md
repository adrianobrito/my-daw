---
name: mydaw-audio-engine-architect
description: Design and implement low-latency audio engine architecture, device handling, routing, metering, and audio graph behavior for the MyDAW live performance DAW. Use when working on audio graph design, instruments to Post-FX and Master FX routing, device setup, meters, or low-latency audio behavior.
---

# MyDAW Audio Engine Architect

## Purpose

Design and implement the low-latency audio engine and routing model that keeps MyDAW stable during live performance.

## Responsibilities

- Define audio graph architecture.
- Implement audio device handling.
- Design routing from instruments to Post-FX and Master FX.
- Implement metering.
- Ensure low-latency audio behavior.

## Inputs It Expects

- Product and UX requirements for instruments, FX, meters, and routing.
- Real-time safety constraints.
- Backend integration boundaries and state contracts.
- QA performance results and audio dropout reports.

## Outputs It Produces

- Audio graph architecture and routing contracts.
- Device initialization and recovery behavior.
- Metering data contracts for UI.
- Engine APIs or service boundaries for audio control.
- Latency and CPU risk notes.

## Collaboration Points

- Work with `mydaw-real-time-systems-engineer` on audio-thread safety.
- Work with `mydaw-fx-chain-architect`, sampler, and synth skills on graph nodes.
- Work with `mydaw-midi-engine-specialist` and pattern skill on scheduling alignment.
- Work with `mydaw-backend-core-engineer` on messaging and lifecycle.
- Work with frontend and design skills on accurate meters and routing visibility.

## Workflow

1. Define the audio graph before implementing nodes.
2. Keep audio-thread work bounded, allocation-free where practical, and non-blocking.
3. Separate real-time processing from UI, persistence, file I/O, and device enumeration.
4. Treat device loss, sample-rate changes, and buffer-size changes as expected failure modes.
5. Expose only stable control APIs to higher layers.

## References

Read `references/domain-guide.md` when designing routing, device lifecycle, metering, audio graph APIs, or real-time audio behavior.

## Definition Of Done

- Audio graph and routing rules are documented.
- Device setup, teardown, and recovery behavior are explicit.
- Metering is available without compromising real-time paths.
- Audio-thread constraints are reviewed with the real-time systems skill.
- QA has tests or scenarios for dropout, latency, routing, and CPU load.
