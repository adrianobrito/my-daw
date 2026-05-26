---
name: mydaw-state-session-designer
description: Design session, scene, preset, snapshot, autosave, crash recovery, and persistence behavior for the MyDAW live performance DAW. Use when defining project schemas, save/load, scene recall, preset recall, state migration, or live-safe state changes.
---

# MyDAW State Session Designer

## Purpose

Design persistence and recall behavior that lets performers trust sessions, scenes, presets, and snapshots during live use.

## Responsibilities

- Define project/session schema.
- Implement save/load sessions.
- Implement scene recall.
- Implement preset save/load.
- Implement autosave and crash recovery.
- Ensure state changes are safe during live performance.

## Inputs It Expects

- Product requirements for sessions, scenes, presets, and alpha workflows.
- Engine state contracts from audio, MIDI, pattern, sampler, synth, and FX skills.
- Backend API constraints and storage format preferences.
- UX requirements for save/load, pending recall, and error states.

## Outputs It Produces

- Session, scene, preset, and snapshot schemas.
- Save/load and recall behavior.
- Migration, validation, and missing-resource policies.
- Autosave and crash recovery design.
- State-change safety rules for live performance.

## Collaboration Points

- Work with product and UX skills on recall workflows and visible states.
- Work with backend skill on persistence APIs and state ownership.
- Work with engine skills on safe application of state changes.
- Work with QA on load/save, scene switching, and crash recovery tests.
- Work with documentation skill on session and preset workflows.

## Workflow

1. Define state ownership and serialization boundaries.
2. Separate project/session, scene, preset, and runtime-only state.
3. Specify when state changes apply during transport.
4. Validate loaded state before applying it to live systems.
5. Design recovery for missing samples, devices, unsupported versions, and crashes.

## References

Read `references/domain-guide.md` when defining schemas, recall semantics, autosave, migration, or live-safe state changes.

## Definition Of Done

- Session, scene, preset, and snapshot boundaries are explicit.
- Save/load behavior is validated and recoverable.
- Scene recall is safe during transport and communicates pending state.
- Autosave and crash recovery do not block real-time paths.
- QA can test persistence, missing resources, migration, and rapid recall.
