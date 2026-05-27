# MyDAW Module States

This document defines the MVP module-state contract for MyDAW. It extends the state model in `design-system.md` with concrete visual, interaction, and frontend behavior rules for instrument lanes, source modules, pattern panels, Post-FX slots, and Master FX processors.

Source inputs:

- `docs/product-definition.md`
- `docs/information-architecture.md`
- `docs/design-system.md`
- `docs/my-daw-prototype.png`

## Purpose

Module states let a performer answer four live questions without opening another screen:

- Is this module producing, receiving, or processing signal?
- Is it intentionally silent, isolated, bypassed, deferred, unavailable, or broken?
- What can still be changed safely while transport is running?
- Will a command apply immediately or at a quantized boundary?

The design goal is fast scan and safe action under stage conditions. State must be visible on the module itself, summarized on collapsed rows, and elevated to the section or shell only when it affects wider playback, routing, output, or recovery.

## State Vocabulary

Use the same state names across UX specs, design tokens, component props, QA notes, and frontend tests.

| State | Performer meaning | Primary surface |
| --- | --- | --- |
| `inactive` | Available but not currently selected or producing meaningful signal | Module body |
| `active` | Selected, enabled, receiving input, producing output, or processing signal | Module body and meter |
| `armed` | Ready to trigger, record, receive, or apply, but not yet active | Module header marker |
| `pending` | Command accepted and waiting for async completion or a musical boundary | State badge and affected control |
| `muted` | Signal intentionally silenced | Mute control and module header |
| `soloed` | Module is isolated or prioritized in audible output | Solo control and section summary |
| `bypassed` | Processor skipped while routing continues | Bypass control and processor body |
| `disabled` | Unavailable because of mode, platform, route, or dependency | Disabled control or module body |
| `loading` | Resource or state is being loaded or applied | Module body and affected controls |
| `missing-resource` | Required sample, preset, device, route, or dependency is absent | Module header and compact reason |
| `error` | Command, route, device, load, or processor failed | Module header, affected control, and section summary |

## State Precedence

When multiple states apply, the highest priority state owns the dominant visual treatment. Lower priority states remain visible as compact badges or control states when they affect live decisions.

1. `error`
2. `missing-resource`
3. `loading`
4. `pending`
5. `soloed`
6. `muted`
7. `bypassed`
8. `armed`
9. `active`
10. `inactive`
11. `disabled`

Examples:

- A muted Bass Synth with a failed preset load renders as `error`, with a secondary muted badge.
- A pending bypass command on a Reverb slot renders as `pending`, with the bypass control showing the requested target.
- A disabled Sample Preview in MIDI mode renders as `disabled`, unless the selected sample is missing, in which case it renders as `missing-resource`.

## Module Anatomy

Every module card or lane must reserve stable regions for state. Labels, meters, badges, and changing values must not resize the card.

Required anatomy:

- Identity: icon or module type, module name, source or preset label.
- Scope metadata: MIDI channel, route, device, source mode, or processor order when applicable.
- State strip: dominant state marker plus secondary badges for mute, solo, bypass, pending, warning, or error.
- Activity: audio, MIDI, route, or CPU meter when the module can produce or process activity.
- Live controls: mute, solo, bypass, expand/collapse, mode selector, or menu as musically valid.
- Recovery affordance: compact entry point for missing resources, failed commands, or broken routes.

Stable sizing rules:

- Module header height remains unchanged across all states.
- State badges reserve a fixed slot count in expanded and compact density.
- Meters keep fixed width and height, including `noSignal`, `muted`, `disabled`, and `error`.
- Long preset, sample, route, or scene names truncate with tooltip support.
- Error and missing-resource text uses one compact line in the module body; deeper details open from the recovery affordance.

## Visual Treatment

| State | Border/fill | Text and icon | Meter behavior | Control behavior |
| --- | --- | --- | --- | --- |
| `inactive` | Standard surface and subtle border | Secondary text | `noSignal` or low activity | Available controls normal |
| `active` | Tone accent border or filled active control | Primary text | Live activity visible | Controls enabled |
| `armed` | Armed marker distinct from active accent | Primary text plus armed badge | Shows input readiness if available | Trigger/apply controls enabled |
| `pending` | Cyan outline or slow pulse | Pending badge names target state | Continue current-state meter until applied | Affected control protected from duplicate command |
| `muted` | Muted surface treatment | Mute icon/control active; labels remain legible | Reduced meter; pre-mute signal only if implementation labels it | Unmute available |
| `soloed` | Yellow solo marker | Solo icon/control active | Active meter if signal exists | Unsolo available |
| `bypassed` | Processor body subdued; chain position preserved | Bypass icon/control active | Route activity may continue | Re-enable available |
| `disabled` | Inset or subdued surface | Disabled icon/label plus compact reason | Disabled meter state | Unavailable controls disabled with tooltip |
| `loading` | Protected surface with compact loader | Loading badge names resource or command | Last known meter may continue if playback is unaffected | Duplicate commands disabled |
| `missing-resource` | Warning/error marker by scope | Missing item label, such as `Sample missing` | No dependent activity | Resolve or fallback entry point visible |
| `error` | Error border and marker override tone | Concise cause, such as `Route failed` | Error state or last safe meter if playback continues | Retry, bypass, reconnect, or fallback path visible |

Color is never the only signal. Each state needs at least one shape, icon, label, badge, or control-state difference in addition to color.

## Interaction Rules

### Immediate Commands

Mute, solo, transport stop, panic, and local UI collapse should provide immediate UI feedback. If audio or engine application is quantized, the module still shows the requested control state plus a `pending` marker until engine confirmation arrives.

### Quantized Commands

Pattern mode changes, scene-driven module updates, and timing-sensitive route changes may apply at the next step, bar, scene boundary, or other quantized point.

Rules:

- The current applied state remains visually distinct from the requested target.
- The pending badge states the target where space allows, such as `Circular pending`.
- If the command fails, pending is replaced by `error` or `missing-resource` at the affected module.
- Clearing a pending command restores the last applied state.

### Async Commands

Sample loads, preset loads, device reconnects, route validation, and session-driven updates render `loading` while protected from duplicate commands.

Rules:

- Loading does not hide transport, meters, panic, or master output.
- Playback-safe loading keeps activity meters visible.
- Playback-blocking loading is elevated to the section header and shell if it affects output.

### Collapse

Collapsed modules and sections preserve live-critical state.

Minimum collapsed summary:

- Module name or abbreviated identity.
- Dominant state marker.
- Mute, solo, bypass, pending, warning, or error badges when active.
- Activity meter or activity dot.
- Active source or pattern mode where it changes performer decisions.

Collapse must not hide the only indication of muted, soloed, bypassed, pending, missing-resource, or error state.

## Module Type Rules

### Instrument Lanes

Applies to Drum Synth, Bass Synth, Poly/Chord Synth, and Pluck/Stab Synth.

Required states:

- `active` when receiving pattern/MIDI input or producing output.
- `muted` and `soloed` as primary lane controls.
- `pending` for quantized pattern, scene, preset, mute, or solo changes.
- `loading`, `missing-resource`, and `error` for preset, route, MIDI device, or synth engine failures.

Instrument lanes keep preset, MIDI channel, pattern mode, meter, mute, and solo visible in compact density.

### Source Modules

Applies to Drum MIDI, Sampled source, Sample Preview, and future external input sources.

Required states:

- `active` for selected source path.
- `disabled` for controls unavailable in the current MIDI/Sampled mode.
- `loading` for sample and source setup.
- `missing-resource` for absent samples, MIDI devices, or routes.
- `error` for failed decode, failed route validation, or device failure.

Unavailable source controls remain visible when their absence affects performer expectations. For example, Sample Preview is disabled in MIDI mode, not hidden.

### Pattern Panels

Applies to MIDI Generation, MIDI Circular Pattern, ARP, and MIDI FX blocks.

Required states:

- `active` for the applied mode.
- `pending` for mode or parameter changes waiting on next step or quantized boundary.
- `disabled` when the owning source mode cannot use the pattern block.
- `error` when pattern generation or scheduling fails.

Pattern panels distinguish current mode from requested target. Step previews and parameter controls reserve stable dimensions while values change.

### FX Slots

Applies to Instrument/Post Layer processors and Master FX processors.

Required states:

- `active` for processors in the audible chain.
- `bypassed` for skipped processors while route continuity remains intact.
- `pending` for quantized or async bypass/reorder/apply commands.
- `missing-resource` for absent IR, preset, route, or dependency.
- `error` for processor failure or unsafe route state.

Bypassed FX stay in their chain position. Bypass never looks like deletion. Master limiter, clipping, and output errors override normal master accent treatment.

### Master Output Modules

Applies to Master Level, Limiter, master output meter, and output protection.

Required states:

- `active` for normal output.
- `muted` for intentional master mute or output protect.
- `pending` for output device or scene-driven level changes.
- `warning` treatment through `missing-resource` or `error` paths for clipping, limiter danger, device degradation, or broken output.
- `error` for failed audio output or unsafe routing.

Master output failures are always summarized in the shell and Master FX section header.

## Frontend Contract

Recommended component fields:

| Field | Values | Purpose |
| --- | --- | --- |
| `uiState` | State vocabulary names | Dominant module state |
| `secondaryStates` | Array of state names | Badges for lower priority live states |
| `activityState` | `noSignal`, `active`, `peak`, `clipping`, `muted`, `disabled`, `error` | Meter rendering |
| `applyTiming` | `immediate`, `nextStep`, `quantized`, `async` | Pending copy and timing indicator |
| `density` | `expanded`, `compact`, `collapsed` | Layout variant |
| `tone` | `neutral`, `midi`, `synth`, `fx`, `master`, `warning`, `error` | Accent family before state override |
| `isCommandPending` | boolean | Protects duplicate commands |
| `pendingTarget` | string or enum | Names requested target mode/state when pending |
| `recoveryAction` | optional command descriptor | Opens retry, resolve, fallback, reconnect, or details |

Frontend rules:

- Derive the dominant `uiState` through the precedence list before rendering.
- Keep controls optimistic only where UX specifies immediate feedback; otherwise show requested state as pending.
- Do not mutate engine internals from module components.
- Treat every engine command as async or fallible until a narrower contract exists.
- Assert visible state in tests rather than private engine implementation.

## Acceptance Checklist

- Module states use the shared vocabulary and precedence order.
- Instrument lanes expose active, muted, soloed, pending, loading, missing-resource, and error states.
- Source modules expose active, disabled, loading, missing-resource, and error states.
- Pattern panels expose active, pending, disabled, and error states with current vs requested mode distinction.
- FX slots expose active, bypassed, pending, missing-resource, and error states while preserving chain position.
- Master output errors are visible in the shell and Master FX section.
- Collapsed summaries preserve dominant state, mute, solo, bypass, pending, warning, error, and activity.
- Meters, badges, controls, and changing labels keep stable dimensions.
- State is not communicated by color alone.
- Pending commands do not look applied until engine confirmation arrives.
- Missing resources identify the affected module and provide a recovery path.
