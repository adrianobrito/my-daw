---
name: mydaw-fx-chain-architect
description: Design and implement MyDAW instrument FX, Post-FX, and Master FX chains from the canonical main-screen, design-system, and module-state docs. Use when working on EQ, Compressor, Reverb, Delay, Master EQ, Glue Compressor, Stereo Width, Limiter, FX bypass, FX ordering, routing, scene recall, or live-safe FX changes.
---

# MyDAW FX Chain Architect

## Purpose

Design FX chains that shape sound during performance while preserving routing clarity, CPU safety, and reliable bypass behavior.

## Responsibilities

- Implement EQ.
- Implement Compressor.
- Implement Reverb.
- Implement Delay.
- Implement Master EQ, Glue Compressor, Stereo Width, and Limiter.
- Implement FX bypass, ordering, and routing behavior.

## Inputs It Expects

- Product scope for instrument, Post-FX, and Master FX behavior.
- Audio graph and routing architecture.
- UX specs for FX slots, bypass, ordering, and macro controls.
- Real-time safety constraints and QA performance findings.

## Outputs It Produces

- FX chain architecture and ordering rules.
- Parameter ranges and macro control definitions.
- Bypass, wet/dry, tail, and state-change behavior.
- Routing integration contracts.
- CPU and latency risk notes for each FX type.

## Collaboration Points

- Work with audio and real-time skills on processing safety and latency.
- Work with frontend, UX, and design system skills on FX controls and states.
- Work with sampler and synth skills on instrument-specific control needs.
- Work with state/session skill on preset, scene, and snapshot recall.
- Work with QA on bypass, routing, CPU, and dropout tests.

## Workflow

1. Define chain placement and order before parameter detail.
2. Specify bypass behavior for each FX, including tails and latency compensation if relevant.
3. Keep MVP FX controls macro-level and stable.
4. Review CPU and latency impact before enabling expensive processing.
5. Test rapid bypass/order/state changes under transport.

## References

Read `references/domain-guide.md` when defining FX order, bypass semantics, parameter ranges, master processing, scene recall behavior, or routing behavior.

## Definition Of Done

- Instrument/Post-FX and Master FX chains are specified.
- EQ, Compressor, Reverb, Delay, Master EQ, Glue Compressor, Stereo Width, and Limiter behavior is defined.
- Bypass, order, wet/dry, and tail behavior are explicit.
- CPU and latency risks are reviewed.
- QA can test routing, meters, bypass, and scene recall.
