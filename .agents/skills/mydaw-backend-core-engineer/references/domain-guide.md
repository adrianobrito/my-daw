# Backend Core Guide

## Canonical Sources

Use `docs/design-system.md`, `docs/module-states.md`, `docs/pattern-mode-selector-component-contract.json`, and `docs/scene-set-workflow-component-contract.json` when shaping command, event, query, and snapshot APIs.

## Ownership Boundaries

Keep boundaries explicit:

- UI renders view state and sends commands.
- Backend coordinates commands, events, queries, validation, and subscriptions.
- State/session owns persisted session, set, scene, preset, snapshot, autosave, migration, and validation state.
- Audio/MIDI engines own real-time processing, scheduling, clocks, routing handoffs, and engine snapshots.
- UI components do not mutate engine internals.

## Command And Snapshot Patterns

- Treat commands as asynchronous and fallible.
- Correlate command requests and snapshots with stable ids.
- Publish snapshots for active/pending/error state; do not require UI to infer applied state from command dispatch.
- Keep high-frequency meter/activity updates throttled and separate from control/state events.

## Required Contracts

- Pattern switch commands include lane id, requested mode, requested boundary, and command id.
- Scene recall commands include target scene, requested boundary, command id, and optional replacement/partial flags.
- Session load validates before replacing active live session.
- Save/autosave runs off real-time paths and failed save preserves in-memory state.

## Real-Time Safety

Backend messaging must not introduce blocking work, unbounded queues, synchronous UI calls, logging, or file I/O into audio callback or sample-accurate MIDI paths.
