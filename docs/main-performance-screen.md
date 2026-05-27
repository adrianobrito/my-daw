# MyDAW Main Performance Screen

This document resolves GitHub issue #7: design the main performance screen for the MyDAW MVP.

Source inputs:

- `docs/product-definition.md`
- `docs/information-architecture.md`
- `docs/design-system.md`
- `docs/pattern-mode-selector.md`
- `docs/scene-set-workflow.md`
- `docs/scene-set-workflow-component-contract.json`
- `docs/my-daw-prototype.png`

The main performance screen is the first usable surface. It is not a landing page, setup wizard, arrangement timeline, or mode hub.

## Performer Goal

The screen must let a solo electronic performer load a prepared session, confirm that the system is ready, start transport, vary drums and synth patterns, shape post and master effects, recall scenes, recover from failures, and stop safely without leaving the main surface.

The design optimizes for:

- Fast scanning in dim stage conditions.
- Stable control placement during playback.
- Clear source-to-output routing.
- Explicit live state for active, pending, muted, soloed, bypassed, loading, missing-resource, and error conditions.
- Direct recovery paths for stop, panic/all-notes-off, output problems, missing resources, and failed recalls.

## Screen Structure

The screen uses one persistent shell and four ordered performance sections:

1. Performance Shell
2. Drums
3. Synths
4. Post-FX
5. Master FX

The sections follow the live signal and decision flow:

`global readiness -> rhythm/source -> melodic/source -> post processing -> master output`

The shell remains visible while any section changes mode, collapses, loads a resource, applies a scene recall, or enters an error state.

## Wide Layout

The wide layout targets 1440 px and above. It preserves the density shown in the prototype while making component responsibilities explicit.

```text
+--------------------------------------------------------------------------------+
| menu | play stop tap | Live Performance DAW | tempo | time | scene | cpu | out |
+--------------------------------------------------------------------------------+
| Drums          | MIDI | Sampled | aggregate state | mute | collapse             |
| Drum Synth     | Drum MIDI     | Pattern Engine             | Sampled Preview   |
| quick macros   | velocity      | generation/circular modes  | sample control    |
+--------------------------------------------------------------------------------+
| Synths         | aggregate state | mute | collapse                              |
| Bass lane      | pattern engine | ARP | MIDI FX | solo | mute | expand           |
| Poly lane      | pattern engine | ARP | MIDI FX | solo | mute | expand           |
| Pluck lane     | pattern engine | ARP | MIDI FX | solo | mute | expand           |
+--------------------------------------------------------------------------------+
| Post-FX        | aggregate bypass/activity/error | mute | collapse             |
| Instrument/Post Layer: source -> compressor -> reverb -> delay -> post level    |
+--------------------------------------------------------------------------------+
| Master FX      | limiter/output safety | mute/protect | collapse                |
| Master EQ -> Glue Compressor -> Stereo Width -> Limiter -> Master Level -> Out  |
+--------------------------------------------------------------------------------+
```

Rules:

- The shell is visually compact and fixed at the top of the app window.
- Drums, Synths, Post-FX, and Master FX are full-width bands, not nested page cards.
- Module cards and FX slots are the repeated card units.
- Meters, mode selectors, lane controls, and value labels reserve fixed dimensions.
- The master output meter remains visible in the shell even when Master FX is collapsed.

## Performance Shell

Purpose: show global readiness, timing, scene state, output safety, and first-level recovery.

Required content:

- Menu trigger.
- Transport controls: play, stop, tap tempo.
- App title: `Live Performance DAW`.
- Tempo value and tap target.
- Time signature.
- Scene status and selector.
- CPU meter and degraded status.
- Master output level, stereo meter, clip or limiter warning.
- Settings/device entry.
- Panic/all-notes-off safety control.

State behavior:

- `idle`: stop is available, play is inactive, meters may show no-signal.
- `playing`: play is active, beat position may be shown if implemented, transport accent is active.
- `pendingScene`: scene chip shows current and target scene plus quantized apply timing.
- `degraded`: CPU, device, route, or output warning appears without hiding transport.
- `error`: global audio, MIDI, or output failure appears in shell and links to the affected section.

Interaction rules:

- Play and stop do not require confirmation.
- Panic/all-notes-off is reachable from the shell and does not require confirmation.
- Tap tempo remains available during transport.
- Scene selection opens a compact selector; selecting a scene creates a pending recall when quantized.
- Device/settings surfaces return to this screen without resetting section expansion state.
- Detailed scene/set recall, save/load, autosave, and recovery behavior follows `docs/scene-set-workflow.md`.

## Drums Section

Purpose: control rhythm source mode, drum source modules, and drum pattern behavior.

Default expanded content:

- Section header with title, activity, pending, muted, missing-resource, and error summary.
- Source mode selector: `MIDI` and `Sampled`.
- Drum Synth module.
- Drum MIDI module.
- Pattern Engine panel.
- Sampled Preview area.

MIDI mode:

- Drum Synth and Drum MIDI modules are active or available.
- Pattern Engine defaults to the last selected pattern mode.
- Sampled Preview stays visible as disabled or preview-only when sampled controls are unavailable.
- MIDI channel, kit/source name, activity meter, and quick macros remain visible.

Sampled mode:

- Sample Control and Sample FX become active.
- Missing sample, loading sample, and preview unavailable states appear on the sampled module and in the section summary.
- MIDI source controls that do not apply are disabled, not removed, when the performer still needs to understand the inactive path.

Pattern Engine:

- Exposes `MIDI Generation` and `MIDI Circular Pattern`.
- Uses the selector behavior and engine contract in `docs/pattern-mode-selector.md`.
- Generation controls: density, complexity, variation, length.
- Circular controls: steps, step activity preview, swing.
- Mode or parameter changes show `applyTiming`: `immediate`, `nextStep`, `quantized`, or `async`.

Collapsed state:

- Shows section name, active source mode, activity meter, mute state, pending mode or scene effects, missing-resource, and error summary.
- Does not hide a missing sample or failed drum route.

## Synths Section

Purpose: control melodic and harmonic lanes from one scan path.

Default lanes:

- Bass Synth.
- Poly/Chord Synth.
- Pluck/Stab Synth.

Each lane contains:

- Lane identity and synth type.
- Preset or source label.
- Source mode or MIDI route label.
- MIDI channel where applicable.
- Activity meter.
- Pattern mode selector or active pattern display.
- ARP block.
- MIDI FX block.
- Solo, mute, and expand controls.

Lane behavior:

- Expanded lanes show deeper synth, pattern, ARP, or MIDI FX controls inline.
- Collapsed lanes keep identity, active pattern mode, meter or MIDI activity, solo, mute, pending, and error states visible.
- Solo is yellow and must remain visible at lane and section summary level.
- Muted-by-solo and manually muted states must be distinguishable in copy, badges, or control state.
- Pattern changes follow the same active vs pending rules as Drums.

Synth lane defaults:

| Lane | Primary use | Required quick controls |
| --- | --- | --- |
| Bass Synth | Low-end pattern and groove support | Preset, pattern mode, density or steps, ARP rate, MIDI FX, solo, mute |
| Poly/Chord Synth | Harmonic bed and chord movement | Preset, pattern mode, chord/scale MIDI FX, ARP rate, solo, mute |
| Pluck/Stab Synth | Short melodic or rhythmic accents | Preset, pattern mode, swing or steps, ARP rate, MIDI FX, solo, mute |

## Post-FX Section

Purpose: show instrument/post processing order before the master chain.

Default content:

- Section header with activity, bypass, muted, warning, and error summary.
- Instrument/Post Layer chain.
- Ordered FX slots with flow direction.
- Per-slot bypass.
- Macro controls or compact graphs.
- Post level and post output meter.

Chain rules:

- Routing is shown left-to-right.
- Bypassed processors remain in position and show signal continuity.
- Processor order is visible through explicit position, flow connector, or both.
- Slot meters and value labels do not resize the chain.
- Post level remains available without opening a secondary panel.

Collapsed state:

- Shows layer name, aggregate bypass, activity, post level, warning/error summary, and whether output continues.

## Master FX Section

Purpose: show final output processing and safety-critical output state.

Required chain:

- Master EQ.
- Glue Compressor.
- Stereo Width.
- Limiter.
- Master Level.
- Master output metering.

Rules:

- Master FX may be visually adjacent to Post-FX, but it remains a distinct semantic section.
- Limiter activity, clipping, master mute, output protect, and route errors override normal master accent.
- Master output status is duplicated in summary form in the shell.
- Master level is a live-critical control and keeps a stable hit target.
- Master mute or output protect is first-level when implemented.

Collapsed state:

- Shows chain enabled/bypassed state, limiter or clip warning, master level, and output meter summary.

## State Matrix

Use the shared state names from the design system. The table below defines screen-level placement.

| State | Shell | Section header | Module/lane | Control treatment |
| --- | --- | --- | --- | --- |
| `inactive` | No global warning | Optional low activity | Subdued card | Secondary text, no activity accent |
| `active` | Transport, scene, output active | Accent strip or activity marker | Accent border or active meter | Filled or bordered active state |
| `armed` | Optional global marker | Armed badge if section-wide | Armed badge | Distinct marker, not same as active |
| `pending` | Scene or device pending | Pending badge and pulse | Pending badge on affected lane | Cyan outline/pulse plus timing |
| `muted` | Only if global mute applies | Muted summary | Muted button active | Reduced activity, label remains legible |
| `soloed` | Solo summary if needed | Solo aggregate marker | Solo button active | Yellow marker and non-color indicator |
| `bypassed` | Master bypass only if global | Bypass summary | Bypassed processor subdued | Bypass active, routing visible |
| `disabled` | Device-dependent only | Disabled summary if section blocked | Disabled controls visible | Disabled affordance and tooltip/reason |
| `loading` | Session/device loading summary | Loading badge | Loader on affected item | Prevent duplicate commands |
| `missing-resource` | Global summary if output affected | Warning/error badge | Affected module marked | Recover action where available |
| `error` | Global safety banner/chip | Dominant section state | Dominant item state | Error color plus concise cause |

State precedence follows `docs/design-system.md`. The highest-priority state gets the dominant treatment; secondary states remain visible as badges or control states.

## Interaction Flows

### Start Performance

1. Performer confirms session, device, CPU, scene, and master output in the shell.
2. Performer presses play.
3. Shell enters `playing`.
4. Drums and Synths meters show activity as sources produce MIDI or audio.
5. Any route, device, or output issue appears globally and on the affected section.

### Switch Drum Source Mode

1. Performer selects `MIDI` or `Sampled`.
2. If safe immediately, selected mode becomes `active`.
3. If quantized or async, selected mode becomes `pending` with apply timing.
4. On apply, active source modules update and unavailable modules become disabled.
5. If apply fails, the mode selector returns to last valid active mode and shows error on the affected source.

### Switch Pattern Mode

1. Performer selects `MIDI Generation` or `MIDI Circular Pattern`.
2. The selected mode shows `pending` if waiting for next step, bar, or scene boundary.
3. The prior mode remains visually active until the engine confirms the switch.
4. On apply, controls for the new mode become active and the timing marker clears.
5. On failure, the lane or panel shows error and preserves the last valid pattern mode.

### Recall Scene

1. Performer opens scene selector from the shell.
2. Performer selects target scene.
3. Shell shows current scene, target scene, and apply timing.
4. Affected sections show pending markers when their state will change.
5. On quantized apply, shell shows the new current scene and affected sections update.
6. On blocked or failed recall, shell shows failed recall and affected sections show scoped errors.

### Bypass FX

1. Performer toggles bypass on an FX slot or layer.
2. UI feedback is immediate on the bypass control.
3. If audio change is quantized or async, the slot shows pending until applied.
4. The processor remains in the chain and routing remains visible.
5. Failed bypass shows an error on the slot and does not imply the processor was removed.

### Collapse Section

1. Performer presses collapse in a section header.
2. Section body collapses but header stays in the main order.
3. Header keeps critical state: activity, meter summary, mute, solo, bypass, pending, loading, missing-resource, error, and degraded status.
4. Re-expanding restores the last meaningful lane or panel context when possible.

### Recover From Failure

1. Global failures appear in the shell.
2. Scoped failures appear on affected section and module/lane.
3. Performer can use stop, panic/all-notes-off, output protect, device reconnect, retry scene recall, or resource resolution from first-level paths.
4. Playback controls remain reachable unless the app is in a fatal state.

## Component Inventory

The main screen uses these design-system components:

| Component | Placement | Required variants |
| --- | --- | --- |
| Performance Shell | Fixed top shell | idle, playing, pendingScene, degraded, error |
| Transport Control | Shell | play, stop, tap, disabled, error |
| Scene Status | Shell | current, pending, applied, failed, disabled |
| Scene Selector Panel | Shell overlay or side panel | expanded, compact, recovery |
| Set Session Surface | Session menu or first-level panel | ready, loading, validating, saving, autosaved, recoverable, error |
| CPU Meter | Shell | active, warning, error |
| Master Output Meter | Shell and Master FX | noSignal, active, peak, clipping, muted, error |
| Section Header | Every section | default, active, collapsed, pending, warning, error |
| Mode Selector | Drums and pattern panels | tabs, segmented, compact |
| Module Card | Drums and Synth lanes | instrument, source, compact, expanded |
| Pattern Engine Panel | Drums and Synth lanes | active, pending, disabled, error |
| Pattern Mode Selector | Pattern Engine Panel | segmented, compact, collapsedSummary |
| Parameter Control | Modules, ARP, MIDI FX, FX slots | knob, slider, stepper, select, toggle |
| Meter | Modules, lanes, shell, FX | audioLevel, midiActivity, cpu, clip, routeActivity |
| FX Slot | Post-FX and Master FX | instrumentPost, master, compact, empty |
| Safety Control | Shell and output areas | panic, stop, outputProtect, reconnect |

## Responsive Behavior

### Wide, 1440 px and above

- Full multi-column layout.
- Drums shows source modules, Pattern Engine, and Sampled Preview on one row.
- Synth lanes show identity, pattern, ARP, MIDI FX, and lane controls on one row.
- Post-FX and Master FX chains use horizontal flow.

### Standard, 1024 px to 1439 px

- Shell preserves all global controls, with shorter labels where needed.
- Drums keeps Pattern Engine visible; Sampled Preview may stack under source modules.
- Synth lane internals can reduce graph detail before hiding controls.
- FX chains may wrap after logical processors while preserving order.

### Compact, 768 px to 1023 px

- Sections stack internals vertically.
- Shell remains visible and may group settings and panic behind stable icon buttons with accessible names.
- Mode selectors remain visible.
- Meters preserve fixed minimum sizes.

### Minimum, below 768 px

- Supports inspection and basic operation only.
- Transport, scene, CPU, master output, and safety remain reachable.
- Section order is unchanged.
- Full live-performance readiness is not claimed at this width.

## Accessibility And Input

- Focus order follows: shell, Drums, Synths, Post-FX, Master FX, safety/status.
- Every icon-only control has an accessible name and tooltip.
- Color is never the only signal for active, pending, muted, soloed, bypassed, loading, missing-resource, or error.
- Critical controls have at least 36 x 36 px pointer targets.
- Secondary compact controls have at least 28 x 28 px pointer targets.
- Reduced motion disables non-essential pulses and shimmer while preserving visible pending/loading state.
- Long names truncate with tooltip and never resize layout.

## Acceptance Criteria

This screen design is ready for frontend implementation when:

- Transport, tempo, time signature, scene, CPU, master output, settings/device, and panic are visible or first-level in the shell.
- Drums exposes MIDI/Sampled source mode, Drum Synth, Drum MIDI, Pattern Engine, and Sampled Preview/Sample Control states.
- Synths exposes Bass, Poly/Chord, and Pluck/Stab lanes with pattern, ARP, MIDI FX, solo, mute, expand, and activity state.
- Post-FX shows ordered instrument/post processing, bypass, macro controls, meters, and post level.
- Master FX shows Master EQ, Glue Compressor, Stereo Width, Limiter, Master Level, and master output metering.
- Active and pending modes are visually different.
- Muted, soloed, bypassed, loading, missing-resource, degraded, and error states are visible at actionable scope.
- Collapsed sections preserve critical performance state.
- Scene recall shows current, target, pending, applied, blocked, and failed states.
- The performer can stop, panic/all-notes-off, and identify output failure without deep navigation.
- The layout respects design-system tokens, state names, component contracts, and responsive rules.
