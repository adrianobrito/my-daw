# MyDAW Information Architecture

This document is the MVP source of truth for MyDAW's information architecture. It translates the product definition and prototype into screen hierarchy, grouping, navigation, reveal, and state visibility rules for future frontend work.

Source inputs:

- `docs/product-definition.md`
- `docs/design-system.md`
- `docs/main-performance-screen.md`
- `docs/module-states.md`
- `docs/pattern-mode-selector.md`
- `docs/scene-set-workflow.md`
- `docs/scene-set-workflow-component-contract.json`
- `docs/my-daw-prototype.png`

The IA uses the design system's MVP state names and dense, stage-readable layout guidance as implementation constraints.

## IA Principles

- The first screen is the live-performance surface, not a landing page, setup wizard, or arrangement view.
- Global performance state remains visible during transport, mode switching, scene recall, FX changes, loading, and error recovery.
- Drums, Synths, Post-FX, and Master FX stay in one scan path so performers can understand source, pattern, processing, and output state without changing screens.
- Controls that can affect audio, MIDI timing, or output safety must expose active, pending, degraded, and failed states at the level where the performer can act on them.
- Collapsed surfaces preserve enough status to answer whether a section is active, muted, soloed, bypassed, loading, missing resources, or in error.

## MVP Hierarchy

The MVP IA has one primary performance shell with persistent global controls and ordered performance sections.

1. Performance Shell
2. Drums
3. Synths
4. Post-FX
5. Master FX
6. Scene and session surfaces
7. Safety, status, and device surfaces

### Performance Shell

The shell is always visible on the main screen.

Persistent shell elements:

- App navigation entry point.
- Transport: play, stop, and tap tempo.
- Tempo and time signature.
- Current scene selector and scene recall status.
- CPU status.
- Master output level, stereo meter, and clipping or limiter warning state.
- Settings or device entry point.
- Panic/all-notes-off recovery path.

The shell owns global status. Section-level controls must not hide transport, scene, CPU, device, or master output state.

### Drums

Drums is the first performance section because it usually anchors live timing and groove.

Required first-level surfaces:

- Section header with expand/collapse, mute, and activity/error summary.
- Source mode switch: MIDI and Sampled.
- Drum Synth lane.
- Drum MIDI lane.
- Pattern Engine surface.
- Sampled preview and access path when MIDI mode is active.
- Sample Control and Sample FX when Sampled mode is active.

Required controls and state:

- Source labels and MIDI channel where applicable.
- Meters for drum source activity and post-source level where useful.
- MIDI Generation mode.
- MIDI Circular Pattern mode.
- Pattern controls for density, complexity, variation, length, steps, and swing where applicable.
- Sample loading, missing-sample, unavailable, and preview states.
- Quick macros such as tune, decay, drive, level, velocity, and humanize when present in the implementation.

### Synths

Synths follows Drums and groups playable melodic or harmonic lanes.

Required first-level lanes:

- Bass Synth.
- Poly/Chord Synth.
- Pluck/Stab Synth.

Each synth lane exposes:

- Lane identity, preset or sound name, source mode, MIDI channel, meter, and activity state.
- MIDI Generation mode.
- MIDI Circular Pattern mode.
- ARP controls.
- MIDI FX controls.
- Solo, mute, and lane expand controls.

Lane expansion reveals deeper controls for the selected synth, pattern, ARP, or MIDI FX without replacing the global performance shell. Collapsed lanes retain lane identity, active mode, meter or activity state, mute, solo, pending, and error indicators.

### Post-FX

Post-FX follows instrument sources and exposes the ordered processing path before master output.

Required first-level surfaces:

- Section header with expand/collapse, mute, and bypass summary.
- Instrument/Post Layer chain.
- Master FX Layer chain entry, when represented in the Post-FX section.
- Ordered processors with visible routing direction.
- Processor bypass controls.
- Macro controls and meters where useful.
- Post level.

The Post-FX IA must make routing legible: instrument sources feed their assigned post-processing path, then the master chain, then output. Bypassed processors remain visible in chain position and indicate whether signal continuity is preserved.

### Master FX

Master FX is the final processing and output control surface.

Required chain:

- Master EQ.
- Glue Compressor.
- Stereo Width.
- Limiter.
- Master Level.
- Master output metering.

Master FX may appear as a dedicated lower section or as the second lane inside the Post-FX area, but it must remain visually and semantically distinct from instrument Post-FX. Limiter, clipping, master mute, and output degradation warnings must be visible without opening a secondary screen.

### Scene And Session Surfaces

Scenes and sessions are supporting surfaces for performance continuity.

The detailed scene/set behavior, persistence boundaries, recall states, and recovery rules are defined in `docs/scene-set-workflow.md`. This IA owns where those surfaces appear in the hierarchy; the scene/set workflow spec owns how recall, save/load, autosave, and recovery behave.

Required scene IA:

- Current scene remains visible in the shell.
- Scene selector is reachable from the shell.
- Ordered set scene list is reachable without replacing the performance shell.
- Pending scene recall is visible before apply.
- Applied scene is visible after quantized recall.
- Failed or partially blocked recall is visible and structured.
- Recall timing communicates immediate, next-step, or quantized-boundary behavior where relevant.
- Affected Drums, Synths, Post-FX, and Master FX scopes show scene-driven pending and failure states.

Required session IA:

- Load, save, save-as, autosave recovery, and recent sessions are reachable from first-level navigation or settings.
- Session load cannot replace the main screen until validation has either succeeded or produced actionable errors.
- Missing samples, devices, presets, or routes are shown on the affected section or lane and summarized globally.
- Autosave recovery is distinguishable from intentional saved sessions.

## Main-Screen Ordering

The MVP main screen order is:

`Shell -> Drums -> Synths -> Post-FX -> Master FX`

The detailed screen-level behavior, responsive layout, interaction flows, and component inventory are defined in `docs/main-performance-screen.md`. This IA remains the hierarchy source of truth; the main screen spec is the implementation-ready artifact for issue #7.

The shell remains fixed or otherwise persistently available. Drums, Synths, Post-FX, and Master FX follow a top-to-bottom signal and performance flow:

- Global readiness and transport.
- Rhythm and source generation.
- Melodic and harmonic source generation.
- Instrument and post-processing.
- Final master processing and output.

The MVP does not include a separate arrangement screen, timeline-first view, landing page, or mode hub. Any setup, device, session, or recovery surface must return to the main performance screen through a first-level path.

## Navigation And Reveal Rules

### Section Expand And Collapse

- Each main section can expand or collapse.
- Collapse preserves critical state: active playback, meter/activity, mute, solo, bypass, pending, loading, missing-resource, error, and degraded status.
- Collapsed sections remain in the main ordering and cannot move to a hidden navigation drawer.
- Expanding a section restores the last meaningful subsection or lane context when possible.

### Mode Switching

- Drums exposes MIDI and Sampled source modes at the section level.
- Pattern surfaces expose MIDI Generation and MIDI Circular Pattern modes at the lane or pattern level.
- The pattern mode selector contract is defined in `docs/pattern-mode-selector.md`; this IA only defines where the selector appears.
- Active mode is always visually distinct from available modes.
- Pending mode changes are visible when they apply at a step, bar, scene, or other quantized boundary.
- Unavailable modes remain discoverable only when the performer can understand why they are unavailable.

### Lane Expansion

- Synth lanes and any future drum lanes may expand inline.
- Expanded lanes reveal deeper controls but do not navigate away from the shell.
- Lane expansion must not hide lane mute, solo, active pattern mode, or meter state.
- Only the level needed for performance should be exposed by default; deeper editing remains secondary for MVP.

### First-Level Recovery Paths

The performer must be able to recover without searching through deep navigation.

Required first-level recovery paths:

- Panic/all-notes-off for stuck notes.
- Stop transport.
- Master output mute or level access.
- Audio/MIDI device status and reconnect path.
- Missing sample or preset resolution path.
- Failed scene recall details and retry or fallback path.

### Settings And Device Entry Points

Settings and devices are secondary surfaces entered from the shell. They may open a panel, modal, or dedicated view, but they must preserve a clear return path to the main performance screen.

Settings/device surfaces may contain:

- Audio device selection.
- Sample rate and buffer size where supported.
- MIDI input and output routing.
- Clock source.
- Diagnostics and build information.
- Session defaults.

Device changes must show pending, applying, success, degraded, and failed states when those states affect performance reliability.

## State Visibility Rules

State must be visible where the performer can act and summarized where it affects global safety.

`module-states.md` defines the module-level state contract used by these visibility rules. The IA owns where state appears in the screen hierarchy; the module-state contract owns how module states behave, collapse, and map to frontend component fields.

### Active

- Active transport, active scene, active source mode, active pattern mode, active lane, active processor, and active output are visually distinct.
- Meters or activity indicators confirm live signal or MIDI activity where useful.

### Pending

- Pending scene recall, pending pattern mode change, pending device change, and pending route change must show what is waiting and when it will apply.
- Pending state should not be confused with failure or disabled state.

### Muted

- Muted sections and lanes show muted state at the control and at collapsed summary level.
- Muted state remains visible during transport and scene recall.

### Soloed

- Soloed lanes show solo state at the lane and section summary level.
- When any solo changes audible output elsewhere, affected muted-by-solo surfaces must be distinguishable from manually muted surfaces.

### Bypassed

- Bypassed processors remain in their chain position.
- Bypassed Post-FX and Master FX surfaces show bypass state at processor, lane, and collapsed summary levels where relevant.
- Bypass must not be represented as deletion or removal from routing.

### Loading

- Loading samples, presets, sessions, scenes, and devices show progress or busy state at the affected surface.
- Loading state must communicate whether playback can continue.
- Long-running loads must not hide transport, master output, CPU, or panic controls.

### Missing Resource

- Missing samples, presets, devices, or routes appear at the affected lane, section, and global summary where they can affect output.
- Missing-resource state should describe what is missing and whether there is a fallback.

### Error

- Errors are visible at the point of failure and summarized globally if they affect transport, routing, output, scene recall, session integrity, or device reliability.
- Error state distinguishes broken, bypassed, unavailable, and degraded behavior.
- Error recovery actions are first-level when the error affects live performance.

### Degraded CPU Or Device

- CPU degradation shows in the shell and, where possible, identifies the likely affected section or processor.
- Device degradation covers disconnects, reconnect attempts, buffer underruns, clock loss, unavailable MIDI routes, and audio output changes.
- Degraded state must communicate what still works.

### Scene Recall

- Scene recall states include current, selected, pending, applying, applied, blocked, failed, and partially recoverable.
- Scene recall cannot silently partially fail.
- Scene changes that affect mute, solo, bypass, pattern, preset, sample, route, or FX state must update the visible state at the affected section or lane.

## MVP Acceptance Criteria For Frontend Work

Future frontend work satisfies this IA when:

- A performer can scan transport, tempo, scene, CPU, device, and master output state without leaving the main screen.
- A performer can identify the active Drums source mode and switch between MIDI and Sampled modes when both are available.
- A performer can identify each Synth lane, its MIDI channel or source identity, its active pattern mode, and its mute/solo state.
- A performer can switch between MIDI Generation and MIDI Circular Pattern modes and understand whether the change is immediate or pending.
- A performer can follow source-to-Post-FX-to-Master-FX routing from the main screen.
- A performer can see processor bypass state without confusing bypass with deletion or unavailable routing.
- A performer can recall scenes, see pending recall, and confirm applied or failed recall.
- A performer can recover from stuck notes through a visible panic/all-notes-off path.
- A performer can understand loading, missing-resource, error, degraded CPU, and degraded device states without leaving the main screen.
- A collapsed Drums, Synths, Post-FX, or Master FX section still communicates live-critical state.
- The MVP does not require a separate arrangement view, landing surface, or setup hub to perform a prepared session.

## Deferred IA

The following IA surfaces are deferred until they are promoted by product scope:

- Full linear arrangement or timeline editing.
- Advanced sample editor.
- Third-party plugin browser and plugin hosting surface.
- Deep modulation matrix editor.
- Complex controller-mapping editor.
- Cloud collaboration or account surfaces.
- Advanced external hardware routing beyond MVP device and MIDI routing.
