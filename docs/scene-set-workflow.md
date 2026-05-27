# MyDAW Scene And Set Workflow

This document resolves GitHub issue #10: design the scene/set workflow for the MyDAW MVP.

Source inputs:

- `docs/product-definition.md`
- `docs/information-architecture.md`
- `docs/design-system.md`
- `docs/main-performance-screen.md`
- `docs/module-states.md`
- `docs/scene-set-workflow-component-contract.json`
- `docs/my-daw-prototype.png`

The scene/set workflow lets a solo electronic performer prepare a set, load it, recall scenes during playback, recover from failed recalls, and preserve changes without making the current musical state ambiguous.

## Performer Goal

A performer must be able to load a prepared set, confirm the current scene, select the next scene, see when recall will apply, understand which parts of the system will change, and recover if a scene cannot be applied. Scene recall is a performance action, not an arrangement timeline.

The design optimizes for:

- Fast current-scene recognition in the persistent shell.
- Quantized recall without hiding the applied scene.
- Visible pending, blocked, failed, and partially recoverable recall states.
- Validation before live systems receive state.
- Recovery for missing samples, presets, devices, routes, and unsupported session versions.
- Autosave and crash recovery without blocking audio or MIDI paths.

## Terminology

| Term | Meaning |
| --- | --- |
| `set` | Ordered performance plan inside a session, made of scenes and set-level metadata. |
| `session` | Saved project file containing set order, scenes, presets, resources, routing, devices, and global defaults. |
| `scene` | Recallable performance state for patterns, mutes, solos, presets, FX, macros, routes, and optional tempo/time signature. |
| `snapshot` | Runtime capture of the current performance state that can become a scene or recovery point after validation. |
| `preset` | Reusable module, synth, sampler, FX, or pattern configuration referenced or embedded by a scene. |
| `current scene` | Last scene confirmed as applied by the engine and state layer. |
| `target scene` | Scene selected by the performer and waiting for validation or a musical boundary. |
| `pending recall` | Accepted recall command that has not yet reached its apply boundary or engine confirmation. |

## Screen Placement

Scene/set controls are distributed by live decision, not placed on a separate arrangement screen.

Required placement:

- Performance shell: current scene chip, target scene when pending, recall timing, failed recall marker, and scene selector trigger.
- Scene selector panel: ordered set list, scene status, target selection, recall boundary, and compact affected-scope summary.
- Affected sections and lanes: pending, loading, missing-resource, muted, soloed, bypassed, preset, route, and FX badges caused by scene recall.
- Recovery surface: scoped failed-recall details with retry, skip blocked changes, resolve resource, or keep current scene actions.
- Session menu: load, save, save as, recent sessions, autosave recovery, duplicate scene, capture scene, rename scene, and reorder set.

The shell remains visible while the scene selector, recovery surface, or session menu is open.

## Set Structure

A set is an ordered list of scenes within one session. The MVP set workflow is list-based and performance-first; it is not a linear arrangement editor.

Required set metadata:

| Field | Purpose |
| --- | --- |
| `setId` | Stable id for the performance set. |
| `name` | Performer-facing set name. |
| `sceneOrder` | Ordered stable scene ids. |
| `defaultRecallBoundary` | Default timing such as `nextBar` or `sceneBoundary`. |
| `currentSceneId` | Last confirmed applied scene when saved. |
| `lastSavedAt` | Save timestamp for session surfaces. |
| `schemaVersion` | Migration and validation gate. |

Set-level actions:

- Open set selector from the shell or session menu.
- Move previous or next scene from the shell when implemented as controller shortcuts.
- Open scene selector and choose any scene in the set.
- Capture the current runtime state into a new scene after validation.
- Duplicate, rename, reorder, or delete scenes outside active recall.
- Save or autosave without blocking playback.

Destructive set actions such as delete scene, overwrite scene, and discard recovery require confirmation. Recall, next scene, previous scene, stop, and panic do not require confirmation.

## Scene Anatomy

Each scene stores only recallable performance state. Runtime meters, current playhead position, transient errors, UI hover/focus, and in-flight command ids are not persisted as scene state.

Required scene fields:

| Field | Purpose |
| --- | --- |
| `sceneId` | Stable scene id used by commands, snapshots, and persistence. |
| `name` | Short performer-facing label. |
| `index` | Current set order position. |
| `recallBoundary` | Optional override for the set default. |
| `tempoBpm` | Optional scene tempo override. |
| `timeSignature` | Optional scene meter override. |
| `moduleStates` | Drums, Synths, Post-FX, and Master FX recall state. |
| `patternStates` | MIDI Generation or MIDI Circular Pattern ids and parameters. |
| `presetRefs` | Referenced or embedded preset ids. |
| `resourceRefs` | Samples, devices, routes, and external resources needed for recall. |
| `macroValues` | Scene-level performance macro values where implemented. |
| `createdAt` | Audit metadata. |
| `updatedAt` | Audit metadata. |

Scene state can affect:

- Drums MIDI/Sampled source mode.
- Drum and synth active pattern modes and pattern parameters.
- Synth presets, ARP state, and MIDI FX state.
- Lane mute, solo, and level state.
- Post-FX and Master FX bypass, macro, and level state.
- Master output protection and limiter-safe values where supported.
- Route assignments when the state layer can prepare them safely outside real-time paths.

Scene recall must not directly load or decode samples on the audio thread. Required resources are validated and prepared before the scene becomes recallable.

## Scene Status

Scene status uses a dedicated scene recall state vocabulary so the UI can be more specific than the shared module state model.

| State | Performer meaning | Primary surface |
| --- | --- | --- |
| `current` | Scene is applied and driving visible performance state. | Shell scene chip and scene list row. |
| `selected` | Scene is highlighted in the selector but no recall is armed. | Scene selector. |
| `armed` | Scene is ready to recall on the selected boundary. | Scene selector and shell chip. |
| `pending` | Recall command is accepted and waiting for validation, async preparation, or a boundary. | Shell chip and affected scopes. |
| `queued` | Recall is behind another accepted scene command. | Scene selector row and shell queue marker. |
| `applying` | Boundary reached and state handoff is in progress. | Shell chip and affected scopes. |
| `applied` | Target scene has become current and pending markers can clear. | Shell scene chip. |
| `blocked` | Recall cannot be accepted until the performer resolves a validation problem. | Shell chip, selector row, and recovery surface. |
| `failed` | Accepted recall did not apply and current scene remains authoritative. | Shell chip and recovery surface. |
| `partialRecoverable` | Safe subset applied or can apply, but one or more scoped changes failed. | Shell chip, affected sections, and recovery surface. |

State precedence for scene status:

1. `failed`
2. `blocked`
3. `partialRecoverable`
4. `applying`
5. `pending`
6. `queued`
7. `armed`
8. `selected`
9. `applied`
10. `current`

The current scene remains visible during `armed`, `pending`, `queued`, `applying`, `blocked`, `failed`, and `partialRecoverable` states.

## Recall Timing

Scene recall timing is explicit and visible.

| Boundary | Use |
| --- | --- |
| `immediate` | Safe while stopped, or for non-musical UI-only changes. |
| `nextStep` | Small pattern-safe changes that the MIDI engine can apply at the next scheduler step. |
| `nextBar` | Default playing recall for the MVP. |
| `nextPhrase` | Longer musical boundary when a scene owns multi-bar pattern state. |
| `sceneBoundary` | Engine-defined boundary used when several sections must switch together. |
| `manual` | Scene is selected or armed but waits for an explicit recall command. |

Default behavior:

- While stopped, scene recall can apply immediately after validation.
- While playing, scene recall defaults to `nextBar`.
- Scenes that change phrase length, routes, presets, or multiple pattern engines may use `sceneBoundary`.
- Resource preparation and validation are async and complete before any real-time handoff.
- The UI renders snapshots from the state layer and engine; it does not drive musical timing.

## Interaction Flows

### Load A Session

1. Performer opens recent sessions or load from the session menu.
2. State layer reads the session off the real-time path.
3. Session enters `validating`.
4. Missing resources, unsupported schema, unavailable devices, and broken routes are reported before application.
5. If valid, the set order, current scene, modules, routing, presets, and resource references become ready.
6. If recoverable, the shell shows a warning and affected scenes or modules show scoped recovery states.
7. If invalid, the current live session remains untouched.

### Select Scene While Stopped

1. Performer opens scene selector from the shell.
2. Performer selects a scene row.
3. Scene row becomes `selected` and shows affected scopes.
4. Recall validates required resources.
5. If valid, scene applies immediately and becomes `current`.
6. If blocked, selector shows the reason and keeps the previous current scene.

### Recall Scene While Playing

1. Performer opens selector or uses next/previous scene command.
2. Target scene becomes `armed` or `pending` depending on command style.
3. Shell shows current scene, target scene, and boundary such as `Next bar`.
4. Affected sections show pending markers for mutes, solos, presets, patterns, FX, routes, and macros that will change.
5. At the boundary, the engine applies a prepared state handoff and publishes an applied snapshot.
6. Shell updates target scene to current and clears pending markers.
7. If recall fails, shell keeps the previous current scene and opens or marks recovery.

### Replace Pending Scene

1. Performer selects a different scene while one recall is pending.
2. UI requests replacement using the active recall command id.
3. State layer confirms replacement, queues it, or rejects it.
4. Until confirmation, the original pending scene remains visible.
5. On replacement, shell and selector show the new target and boundary.

### Blocked Recall

1. Validation detects a blocking issue such as missing sample, missing preset, unsupported session version, unavailable route, or unsafe output change.
2. Target scene becomes `blocked`.
3. Shell names the target scene and marks recall as blocked.
4. Affected sections show scoped missing-resource or error state.
5. Performer can resolve resource, choose fallback, skip blocked change when safe, or keep current scene.

### Partially Recoverable Recall

1. Scene recall validates, but one or more non-critical changes cannot apply.
2. State layer identifies safe changes and blocked changes.
3. UI shows `partialRecoverable` before or after recall depending on whether the safe subset has applied.
4. Affected scopes show exact failed changes.
5. Performer can accept the safe subset, retry failed scopes, or return to the prior current scene when possible.

Partial recall cannot be silent. If the system cannot communicate the subset clearly, recall is blocked instead.

### Capture Scene

1. Performer chooses capture scene from the session menu or scene selector.
2. State layer creates a snapshot from recallable state only.
3. Snapshot validation removes runtime-only state and checks resource references.
4. Performer names the scene.
5. New scene is inserted after the current scene by default.
6. Session marks unsaved and autosave schedules a non-blocking write.

### Save And Autosave

1. Save writes the current session state through an atomic or equivalent safe persistence path.
2. Autosave writes a distinguishable recovery file on a debounce or meaningful-change boundary.
3. Save and autosave never run on audio or MIDI real-time paths.
4. Failed save keeps the in-memory session unchanged and shows a recoverable error.
5. On launch after a crash, recovery is offered before loading a recent session into the live surface.

## State And Persistence Boundaries

### Session State

Persisted in the session:

- Schema version and migration metadata.
- Set order and scene definitions.
- Global tempo, time signature, transport defaults, and device preferences.
- Module definitions for Drums, Synths, Post-FX, and Master FX.
- Routing graph descriptors and stable route ids.
- Presets, preset references, and embedded preset data where supported.
- Resource references for samples, devices, routes, and external files.
- Last known current scene and expansion preferences if safe to restore.

### Scene State

Persisted in each scene:

- Recallable module, pattern, source, preset, mute, solo, bypass, macro, level, route, tempo, and meter-signature targets.
- Scene-level resource requirements and fallback policies.
- Recall boundary override.

### Preset State

Preset state may be embedded, referenced, or both. Scene overrides take precedence over preset defaults only for fields the scene explicitly owns.

### Runtime-Only State

Never persisted as scene state:

- Audio meters and CPU snapshots.
- MIDI playhead and scheduler queues.
- Current audio buffer data.
- Hover, focus, selection, open menu, and tooltip state.
- In-flight command ids after restart.
- Transient device errors after they have been resolved.

## Validation And Recovery Policy

Loaded session and scene state is validated before application.

Validation checks:

- Schema version supported or migrated.
- Scene ids, module ids, route ids, preset ids, and resource ids are stable and unique.
- Required samples and presets exist or have fallback policy.
- Required devices and MIDI routes are available or degraded gracefully.
- FX chain and routing graph can be prepared outside the audio callback.
- Scene changes can be represented as bounded engine commands.

Recovery rules:

- Missing samples mark affected source modules and scene rows as `missing-resource`.
- Missing presets mark affected synth, sampler, FX, or pattern modules.
- Missing devices mark shell device status and affected routes.
- Unsupported schema blocks session application unless migration succeeds.
- Failed scene recall preserves the last confirmed current scene.
- Partial recovery is allowed only when failed changes are scoped and visible.

## Component Inventory

| Component | Placement | Required variants |
| --- | --- | --- |
| Scene Status | Shell | current, armed, pending, queued, applying, blocked, failed, partialRecoverable |
| Scene Selector Panel | Shell overlay or side panel | expanded, compact, recovery |
| Scene List Item | Scene selector | current, selected, pending, queued, blocked, failed |
| Set Session Surface | Session menu or first-level panel | ready, loading, validating, saving, autosaved, recoverable, error |
| Scene Recall Summary | Shell and recovery surface | affectedScopes, timing, blockedChanges, failedChanges |
| Autosave Recovery Prompt | Launch or session menu | available, restored, dismissed, failed |

## Responsive Behavior

### Wide, 1440 px and above

- Scene selector can open as a right-side panel while the performance shell and sections remain visible.
- Rows show scene number, name, status, boundary, affected scopes, and warning summary.
- Recovery details can sit beside the scene list.

### Standard, 1024 px to 1439 px

- Scene selector keeps the ordered scene list and affected-scope badges visible.
- Recovery details may stack below the selected scene.
- Long scene names truncate with tooltip.

### Compact, 768 px to 1023 px

- Scene selector becomes a focused panel.
- Shell remains visible above it.
- Scene rows compress to number, name, status, boundary, and warning badge.

### Minimum, below 768 px

- Supports inspection, current scene, next/previous recall, and blocked/failed status.
- Full set editing is not claimed at this width.

## Accessibility And Input

- Scene selector focus order follows set order.
- Current, selected, pending, failed, and blocked states use labels or icons in addition to color.
- Scene rows expose accessible names with scene number, scene name, status, and recall boundary.
- Keyboard and controller shortcuts may target next scene, previous scene, recall selected scene, and cancel pending recall.
- Reduced motion disables pending pulse but keeps boundary text and pending outline.
- Long names, missing-resource reasons, and failed-change summaries truncate visually with full text available through tooltip or details.

## Acceptance Criteria

This design is ready for frontend and state implementation when:

- Current scene, target scene, and recall boundary are visible from the shell.
- Scene selector shows ordered set scenes without replacing the main performance surface.
- Scene recall distinguishes current, selected, armed, pending, queued, applying, applied, blocked, failed, and partial recovery states.
- While playing, recall defaults to a visible quantized boundary.
- Pending recall keeps the current scene visibly authoritative until confirmation.
- Affected Drums, Synths, Post-FX, and Master FX scopes show scene-driven pending and failure states.
- Session, scene, preset, snapshot, and runtime-only boundaries are explicit.
- Loaded state is validated before live application.
- Missing resources, unsupported versions, unavailable devices, broken routes, and failed recalls have scoped recovery behavior.
- Save, autosave, and recovery are designed off real-time paths and do not destroy the current live session after failure.
