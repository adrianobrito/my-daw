# MyDAW Pattern Mode Selector

This document resolves GitHub issue #9: design the pattern mode selector for the MyDAW MVP.

Source inputs:

- `docs/product-definition.md`
- `docs/information-architecture.md`
- `docs/design-system.md`
- `docs/main-performance-screen.md`
- `docs/module-states.md`
- `docs/pattern-mode-selector-component-contract.json`
- `docs/my-daw-prototype.png`

The pattern mode selector is the live control that switches a drum or synth pattern surface between `MIDI Generation` and `MIDI Circular Pattern` without making the current musical state ambiguous.

## Performer Goal

A performer must be able to scan a lane, see the applied pattern mode, request a different mode during playback, understand when the change will land, and recover if the engine cannot apply it. The selector must keep the prior mode visibly active until the MIDI engine confirms the switch.

The design optimizes for:

- Fast mode recognition in Drums and Synth lanes.
- Safe switching while transport is running.
- Stable layout across expanded, compact, and collapsed densities.
- Clear current-vs-requested state.
- MIDI scheduling integrity, including note-off safety.
- Bounded, repeatable pattern variation for scene recall.

## Placement

The selector appears inside every `Pattern Engine Panel`.

Required placements:

- Drums Pattern Engine: beside drum source controls.
- Bass Synth lane: inline in the lane pattern block.
- Poly/Chord Synth lane: inline in the lane pattern block.
- Pluck/Stab Synth lane: inline in the lane pattern block.
- Collapsed lane or section summaries: active mode chip plus pending/error badge when applicable.

The selector is not hidden behind a menu or dropdown. Menus may expose secondary pattern actions such as save preset, duplicate, or reset, but not the primary mode switch.

## Selector Anatomy

The default selector is a two-option segmented control.

Options:

| Option id | Visible label | Compact label | Meaning |
| --- | --- | --- | --- |
| `midiGeneration` | `Generation` | `Gen` | Engine creates or mutates a bounded MIDI pattern from musical controls. |
| `midiCircularPattern` | `Circular` | `Circ` | Engine cycles through a fixed step or event ring with rotation and step-level variation. |

Required visible regions:

- Applied option: filled or strongly bordered state using the lane tone.
- Requested option: pending outline and timing badge when waiting for a boundary.
- Timing badge: compact text such as `Next bar`, `Next step`, or `Scene`.
- Disabled reason: tooltip and compact reason when the owning lane cannot use pattern modes.
- Error badge: compact failed command state at selector and lane scope.

Stable sizing rules:

- Expanded selector reserves two equal segments.
- Compact selector reserves two equal segments and may use compact labels.
- Collapsed summaries show one fixed-width mode chip plus one fixed-width status badge slot.
- Timing and error badges must not resize the selector.

## Modes

### MIDI Generation

Purpose: create performable MIDI material from musical controls while preserving repeatability when scenes recall the same state.

Primary controls:

| Control | Range | Apply timing | Musical meaning |
| --- | --- | --- | --- |
| `density` | `0` to `100` percent | `nextStep` or `quantized` | How often eligible events are produced. |
| `complexity` | `0` to `100` percent | `quantized` | Rhythmic and melodic detail. |
| `variation` | `0` to `100` percent | `quantized` | Bounded departure from the current generated material. |
| `probability` | `0` to `100` percent | `nextStep` | Chance that eligible generated events fire. |
| `length` | `1/2 Bar`, `1 Bar`, `2 Bars`, `4 Bars` | `quantized` | Duration of the repeating phrase. |
| `swing` | `0` to `75` percent | `nextStep` | Timing offset applied to eligible subdivisions. |

Required state:

- `seed`: persisted when repeatable scene recall matters.
- `generationId`: identifies the generated material currently applied.
- `variationLock`: optional lock that allows parameter changes without replacing the core phrase.

### MIDI Circular Pattern

Purpose: cycle through a fixed ring of steps or timestamped events while making position, length, and rotation predictable during performance.

Primary controls:

| Control | Range | Apply timing | Musical meaning |
| --- | --- | --- | --- |
| `steps` | `1` to `64` | `quantized` | Number of addressable positions in the ring. |
| `activeSteps` | Per-step on/off | `nextStep` | Which positions may emit events. |
| `rotation` | `0` to `steps - 1` | `quantized` | Starting offset of the ring. |
| `probability` | `0` to `100` percent | `nextStep` | Chance that eligible step events fire. |
| `length` | `1/2 Bar`, `1 Bar`, `2 Bars`, `4 Bars` | `quantized` | Musical duration of the ring. |
| `swing` | `0` to `75` percent | `nextStep` | Timing offset applied to eligible subdivisions. |

Required state:

- `stepPreview`: fixed-width visual preview of the ring.
- `playheadPosition`: optional UI snapshot, never a UI timing source.
- `patternId`: persisted identifier for the applied circular material.

## State Behavior

Use the shared state vocabulary from `docs/module-states.md`.

| State | Selector behavior | Performer meaning |
| --- | --- | --- |
| `active` | Applied option is selected; mode controls are enabled. | This mode is currently driving the pattern stream. |
| `pending` | Requested option has pending treatment; applied option remains active. | The command was accepted and is waiting for a musical boundary or engine confirmation. |
| `disabled` | Both options remain visible but unavailable with a compact reason. | This lane cannot currently use the pattern engine. |
| `loading` | Selector is protected from duplicate commands while session or scene pattern state loads. | Pattern state is being restored. |
| `missing-resource` | Selector remains visible; affected lane shows missing route, preset, or device. | The pattern path depends on a missing resource. |
| `error` | Requested switch clears; last valid option remains active with an error badge. | The engine rejected or failed the switch. |

Pending never replaces active. The applied mode remains the source of truth until the engine snapshot reports the new mode.

## Interaction Flow

### Switch While Stopped

1. Performer selects `Generation` or `Circular`.
2. UI sends `requestPatternModeSwitch`.
3. If the engine can apply immediately, the selected option becomes active.
4. If validation is async, the selected option shows `pending` until confirmed.
5. Failed validation restores the last active mode and shows an error on the selector and lane.

### Switch While Playing

1. Performer selects the inactive mode.
2. UI sends `requestPatternModeSwitch` with the lane id, requested mode, and requested boundary.
3. Engine returns an accepted command snapshot with `pendingMode` and `switchId`.
4. Selector shows the old mode as active and the requested mode as pending.
5. At the boundary, the engine schedules note-offs for any old-mode notes that must end, applies the new mode, and publishes an applied snapshot.
6. Selector clears pending and marks the new mode active.
7. If the switch fails, selector restores the previous applied mode and shows scoped error state.

Default playing behavior:

- Mode switches apply at the next bar unless the lane contract explicitly narrows this to `nextStep`.
- Parameter changes that do not restructure the pattern may apply at `nextStep`.
- Scene recall may override timing with the scene boundary, but affected selectors still show pending state.

### Replace Pending Target

When a switch is already pending, selecting the other mode replaces the pending target only if the engine confirms cancellation or replacement. Until then, the first pending target remains visible and the selector protects against duplicate commands.

### Collapse During Pending

Collapsed summaries show:

- Lane or section identity.
- Applied mode chip.
- Pending target badge, such as `Circ next bar`.
- Error or missing-resource badge if the switch cannot apply.

Collapse never hides the only pending mode indicator.

## MIDI Engine Contract

The UI does not own musical timing. It requests mode changes and renders snapshots from the MIDI or backend engine.

Required command fields:

| Field | Values | Purpose |
| --- | --- | --- |
| `laneId` | Stable lane id | Selects the affected drum or synth lane. |
| `requestedMode` | `midiGeneration`, `midiCircularPattern` | Target mode. |
| `requestedBoundary` | `immediate`, `nextStep`, `nextBar`, `sceneBoundary` | Preferred apply boundary. |
| `commandId` | Stable id | Correlates UI request and engine snapshots. |

Required snapshot fields:

| Field | Values | Purpose |
| --- | --- | --- |
| `activeMode` | `midiGeneration`, `midiCircularPattern` | Applied mode currently scheduling events. |
| `pendingMode` | Mode id or `null` | Requested mode waiting to apply. |
| `pendingBoundary` | Boundary id or `null` | Boundary where the switch will apply. |
| `switchId` | Stable id or `null` | Engine switch operation id. |
| `uiState` | Shared state vocabulary | Dominant selector state. |
| `lastError` | Structured error or `null` | Scoped failure reason. |

Scheduling rules:

- Pattern events are timestamped by the engine, not by UI event timing.
- Note-on and note-off pairs remain paired through mode switches.
- Old-mode note-offs must be scheduled before or at the switch boundary when needed.
- Simultaneous events preserve deterministic ordering.
- Scheduler lookahead and pending switch queues stay bounded.
- Panic/all-notes-off remains available from the shell when pattern switching fails or a route changes.

## Visual And Accessibility Rules

- Active uses lane tone and a selected shape, not color alone.
- Pending uses cyan outline or pulse plus text or icon state, never the same treatment as active.
- Disabled options remain visible when the performer expects the mode to exist.
- Error state uses an error marker and compact cause, with details available from the lane recovery affordance.
- Keyboard focus enters the selector as one control group, then moves between options.
- Accessible names use full mode names: `MIDI Generation` and `MIDI Circular Pattern`.
- Tooltips explain disabled reasons and pending boundaries.
- Reduced motion disables pending pulse but keeps the pending badge and outline.

## Acceptance Criteria

This design is ready for frontend implementation when:

- Every Drums and Synth pattern surface exposes both pattern modes without using a dropdown.
- The selector clearly distinguishes active, pending, disabled, loading, missing-resource, and error states.
- Pending mode switches keep the previous applied mode visibly active until engine confirmation.
- The default playing switch applies at a quantized boundary and shows the boundary to the performer.
- Collapsed summaries preserve applied mode, pending target, and scoped error state.
- Generation and Circular mode controls have documented ranges, meaning, and apply timing.
- UI commands use engine snapshots and do not drive MIDI scheduling directly.
- Note-off safety, deterministic event ordering, bounded pending work, and panic recovery are represented in the contract.
