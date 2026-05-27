# Design System Guide

## Canonical Sources

Use `docs/design-system.md` and `docs/module-states.md` first. Use `docs/main-performance-screen.md`, `docs/pattern-mode-selector.md`, and `docs/scene-set-workflow.md` for placement and component contracts.

## Visual Direction

MyDAW is a dense, operational stage interface. Prefer high-contrast dark surfaces, stable dimensions, clear state markers, and scan-friendly section bands. Do not use decorative page sections, nested cards, landing-page composition, or hidden live-critical controls.

## Shared State Vocabulary

Use these names consistently in specs, props, QA, and docs:

`inactive`, `active`, `armed`, `pending`, `muted`, `soloed`, `bypassed`, `disabled`, `loading`, `missing-resource`, `error`.

Precedence:

`error > missing-resource > loading > pending > soloed > muted > bypassed > armed > active > inactive > disabled`.

Scene recall states:

`current`, `selected`, `armed`, `pending`, `queued`, `applying`, `applied`, `blocked`, `failed`, `partialRecoverable`.

## Component Responsibilities

- Performance Shell: global readiness, transport, scene, CPU, master output, device/settings, panic.
- Section Header: aggregate state and collapse for Drums, Synths, Post-FX, Master FX.
- Module Card: identity, metadata, state strip, meter, live controls, recovery affordance.
- Pattern Mode Selector: Generation/Circular options with active, pending, disabled, loading, missing-resource, and error states.
- Scene Status and Scene Selector: current/target scene, boundary, affected scopes, blocked/failed recovery.
- Set Session Surface: load/save/autosave/recovery lifecycle without implying real-time-path work.

## Implementation Constraints

- Reserve fixed dimensions for meters, badges, selectors, values, and long names.
- Color is never the only signal for state.
- Icon-only controls need accessible names and tooltips.
- Critical controls use at least 36 x 36 px targets; secondary compact controls use at least 28 x 28 px.
- Component props should map to `uiState`, `activityState`, `applyTiming`, `sceneRecallState`, `sessionLifecycleState`, `density`, and `tone`.
