---
name: mydaw-backend-core-engineer
description: Implement MyDAW core services, internal APIs, messaging, persistence integration, module architecture, and clean boundaries between UI, engine, and state layers. Use when designing backend services, frontend-engine bridges, command buses, persistence APIs, or module/plugin boundaries.
---

# MyDAW Backend Core Engineer

## Purpose

Implement the application core and integration layer that connects UI, state, audio, MIDI, modules, and persistence without blurring ownership boundaries.

## Responsibilities

- Implement core application services.
- Connect frontend to audio/MIDI backend.
- Implement internal messaging.
- Implement persistence APIs.
- Support plugin/module architecture.
- Maintain clean boundaries between UI, engine, and state layers.

## Inputs It Expects

- Product scope and feature contracts.
- Engine APIs from audio, MIDI, pattern, sampler, synth, and FX skills.
- State/session schemas.
- Frontend integration needs.
- Real-time safety rules and QA findings.

## Outputs It Produces

- Core service architecture.
- Internal command, event, and query APIs.
- Persistence integration APIs.
- Module lifecycle and registration contracts.
- Error handling and boundary documentation.

## Collaboration Points

- Work with frontend skill on API shape and state subscriptions.
- Work with state skill on persistence and schema ownership.
- Work with audio/MIDI and module skills on safe engine command boundaries.
- Work with real-time skill on messaging and thread safety.
- Work with QA and DevOps on testability, logging, crash reports, and release behavior.

## Workflow

1. Define ownership: UI view state, app state, persisted state, and engine runtime state.
2. Use explicit commands and events for cross-layer communication.
3. Keep engine callbacks isolated from blocking services.
4. Provide APIs that are testable without launching the full UI when possible.
5. Document failure modes and recovery behavior.

## References

Read `references/domain-guide.md` when designing internal services, command/event APIs, persistence integration, module lifecycle, or layer boundaries.

## Definition Of Done

- UI, backend, engine, and persistence boundaries are clear.
- Commands, events, queries, and errors are documented.
- Module lifecycle supports instruments, FX, MIDI processors, and state recall.
- Real-time paths are protected from blocking backend work.
- Integration tests can cover core workflows.
