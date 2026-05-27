# MyDAW Design System

This document defines the MVP design system for MyDAW, a stage-ready live-performance DAW for solo electronic performers. It translates the product definition and prototype into reusable UI rules for future frontend implementation and QA.

Source inputs:

- `product-definition.md`
- `information-architecture.md`
- `main-performance-screen.md`
- `main-performance-screen-component-contract.json`
- `my-daw-prototype.png`
- `skills/mydaw-design-system-engineer`
- `skills/mydaw-ux-ui-designer`
- `skills/mydaw-frontend-engineer`
- `skills/mydaw-documentation-writer`

## Design Principles

MyDAW is an operational performance surface, not a landing page or studio timeline. The first screen must let a performer confirm state, start playback, vary patterns, shape FX, recover from failures, and stop safely.

Core principles:

- Keep transport, tempo, time signature, scene, CPU, and master output visible during performance.
- Prefer dense, scan-friendly layouts with stable dimensions over decorative layouts.
- Make live-critical states distinct at a glance: active, pending, muted, soloed, bypassed, loading, missing-resource, and error.
- Keep normal performance changes fast and direct; reserve confirmation only for disruptive or destructive actions.
- Ensure collapsed sections preserve critical status: activity, warnings, mute/solo/bypass, pending recall, and errors.
- Avoid layout shifts from meters, changing values, hover states, loading labels, or error messages.
- Design controls for desktop-first stage use with keyboard and controller accessibility.

## Visual Tokens

Tokens are named by role instead of raw color intent so implementation can adapt values for platform rendering, contrast, and theme tuning.

### Color

The MVP uses a dark operational base with high-contrast text and limited accents. Accent colors must communicate state consistently across Drums, Synths, Post-FX, Master FX, and global controls.

| Token | Purpose | Suggested value |
| --- | --- | --- |
| `color.bg.app` | Application background | `#05080b` |
| `color.bg.surface` | Primary surface | `#0b1218` |
| `color.bg.surfaceRaised` | Module card and control background | `#101922` |
| `color.bg.surfaceInset` | Meter wells and inactive inputs | `#071017` |
| `color.border.subtle` | Standard boundaries | `#1d2a34` |
| `color.border.strong` | Active section and focus boundaries | `#236086` |
| `color.text.primary` | Primary labels and values | `#f2f7fb` |
| `color.text.secondary` | Secondary labels | `#9fb0bd` |
| `color.text.muted` | Low-emphasis metadata | `#6f808d` |
| `color.accent.transport` | Play, active transport, active engine path | `#67d21f` |
| `color.accent.midi` | MIDI and pattern activity | `#1fb9ee` |
| `color.accent.synth` | Synth modules and modulation | `#a87cff` |
| `color.accent.fx` | Post-FX and reverb/delay | `#8c66ff` |
| `color.accent.master` | Master chain and limiter | `#f0c12c` |
| `color.state.pending` | Quantized or deferred change | `#00c7f2` |
| `color.state.muted` | Muted state | `#526171` |
| `color.state.soloed` | Soloed state | `#f3c94c` |
| `color.state.bypassed` | Bypassed processor | `#77818a` |
| `color.state.warning` | Recoverable warning or CPU caution | `#ffb02e` |
| `color.state.error` | Broken route, missing file, failed command | `#ff4d6d` |
| `color.state.clip` | Meter clipping and limiter danger | `#ff5a2f` |
| `color.focus.ring` | Keyboard/controller focus | `#8bdcff` |

Color usage rules:

- Active controls use accent fill or accent border plus primary text.
- Pending controls use a cyan outline or pulse, never the same treatment as applied active state.
- Muted and bypassed states reduce activity emphasis but keep labels legible.
- Soloed state must be visible even inside collapsed sections.
- Error state overrides local accent color until resolved.
- Meter clipping uses red-orange at the meter edge and must not be confused with normal peak activity.

### Typography

Typography must be readable in dim lighting and must not scale with viewport width.

| Token | Use | Size | Weight | Line height |
| --- | --- | --- | --- | --- |
| `type.shellTitle` | App title and main surface title | 18 px | 600 | 24 px |
| `type.sectionTitle` | Drums, Synths, Post-FX headers | 18 px | 600 | 24 px |
| `type.moduleTitle` | Module and lane names | 14 px | 600 | 20 px |
| `type.controlLabel` | Knob, selector, meter labels | 11 px | 600 | 14 px |
| `type.value` | Numeric performance values | 14 px | 500 | 18 px |
| `type.caption` | Metadata and secondary state | 12 px | 400 | 16 px |
| `type.status` | Scene, CPU, output status | 13 px | 500 | 18 px |

Typography rules:

- Use sentence case for module labels and title case only where already established by product terms.
- Keep labels short and domain-specific: `Density`, `Swing`, `Length`, `Scene`, `CPU`, `Master Out`.
- Do not use visible instructional copy inside the performance surface except compact state text needed to identify a failure.
- Long names truncate with a tooltip; they must not resize a module or control.

### Spacing And Layout

The interface is dense but must remain touchable and keyboard navigable.

| Token | Value | Use |
| --- | --- | --- |
| `space.2xs` | 4 px | Icon-label gaps, meter segment gaps |
| `space.xs` | 6 px | Tight internal grouping |
| `space.sm` | 8 px | Standard control padding |
| `space.md` | 12 px | Module internal spacing |
| `space.lg` | 16 px | Section gutters |
| `space.xl` | 24 px | Major layout separation |

Layout rules:

- Use an 8 px base grid for major layout and a 4 px grid for dense controls.
- Minimum pointer target for live-critical controls is 36 x 36 px.
- Minimum pointer target for secondary compact controls is 28 x 28 px.
- Cards and controls must keep fixed or bounded dimensions while values, meters, and states change.
- Page sections are full-width bands or unframed regions; repeated modules and FX slots may be cards.
- Do not put UI cards inside other UI cards.

### Radius, Border, And Elevation

| Token | Value | Use |
| --- | --- | --- |
| `radius.control` | 6 px | Buttons, tabs, inputs |
| `radius.card` | 6 px | Module cards, FX slots |
| `radius.section` | 8 px | Section containers |
| `border.width.default` | 1 px | Standard boundaries |
| `border.width.active` | 1 px | Active boundaries |
| `shadow.none` | none | Default operational UI |
| `shadow.raised` | subtle inner/outer contrast | Active overlays or menus only |

Elevation rules:

- Use borders, fills, and state accents before shadows.
- Avoid decorative glow except for focused, active, or pending controls.
- Keep radius at 8 px or less.

### Motion

Motion must communicate state without distracting the performer.

- Meter movement may update at control-rate cadence and must not affect layout.
- Pending scene recall may use a slow pulse on the scene chip or section marker.
- Loading states may use a compact spinner or shimmer inside the affected control.
- Disable non-essential animation when reduced motion is requested.
- Do not use decorative background animation.

## State Model

Use these state names consistently in specs, component props, QA, and future frontend tests.

| State | Meaning | Required treatment |
| --- | --- | --- |
| `inactive` | Available but not currently producing or selected | Muted surface, secondary text, no activity accent |
| `active` | Selected, enabled, or currently producing signal | Accent border/fill, primary text, activity meter when relevant |
| `armed` | Ready to apply, record, trigger, or receive input | Distinct armed marker, not identical to active |
| `pending` | Command accepted but waiting for quantized boundary or async apply | Cyan pending outline/pulse plus explicit pending marker |
| `muted` | Signal intentionally silenced | Muted control active, reduced meter, collapsed indicator |
| `soloed` | Signal isolated or prioritized | Solo control active in yellow, section aggregate marker |
| `bypassed` | Processor skipped while signal continues | Bypass control active, processor card subdued, routing still visible |
| `disabled` | Unavailable due to mode, platform, or dependency | Disabled affordance, tooltip or compact reason |
| `loading` | Resource or state in progress | Compact loader, controls protected from duplicate commands |
| `missing-resource` | Sample, preset, device, or route missing | Warning/error marker with recoverable label |
| `error` | Command, route, device, or load failed | Error color, concise cause, affected scope visible |

State precedence from highest to lowest:

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

If multiple states apply, render the highest priority state as the dominant visual treatment and keep secondary states as small badges or control states.

## Core Components

Component contracts describe behavior and required data. They do not prescribe framework-specific APIs.

### Performance Shell

Purpose: Persistent container for global performance status and recovery actions.

Required content:

- Menu or navigation trigger.
- Transport controls: play, stop, tap tempo.
- Product title: `Live Performance DAW`.
- Tempo, time signature, active scene, CPU, master output meter, settings.
- Panic/all-notes-off recovery path when engine integration exists.

Variants:

- `idle`: transport stopped, session loaded or empty.
- `playing`: transport active, play highlighted, beat/scene state visible.
- `pendingScene`: scene chip shows pending recall state.
- `degraded`: CPU, device, route, or master output warning visible.
- `error`: global audio/MIDI failure visible without hiding transport.

Rules:

- The shell remains visible during all main performance workflows.
- Tempo and scene values reserve enough width for expected values.
- Panic/all-notes-off must be reachable without opening deep navigation.
- Master output meter shows no-signal, activity, peak, and clipping.

### Section Header

Purpose: Group and summarize Drums, Synths, Post-FX, and Master FX areas.

Required content:

- Section icon and name.
- Collapse/expand control.
- Aggregate status indicators for activity, muted, soloed, bypassed, pending, warning, and error.
- Optional section-level mute or bypass where musically valid.

Variants:

- `default`
- `active`
- `collapsed`
- `pending`
- `warning`
- `error`

Rules:

- Collapsed state keeps section name, aggregate meter/activity, and highest priority state visible.
- Header height is stable across variants.
- Section headers may use a subtle accent band, but error and warning states override the accent.

### Module Card

Purpose: Represent an instrument lane, source module, or processor module with live controls.

Required content:

- Drag or order handle when ordering applies.
- Icon or module symbol.
- Name and preset/source label.
- Source type or mode label.
- MIDI channel, route, or device label when applicable.
- Activity meter.
- Primary state controls: mute, solo, bypass, collapse, or menu as applicable.

Variants:

- `instrument`: Bass Synth, Poly/Chord Synth, Pluck/Stab Synth, Drum Synth.
- `source`: Drum MIDI, sampled source, sample preview.
- `processor`: Compressor, Reverb, Delay, EQ, Stereo Width, Limiter.
- `compact`: Used in dense rows or collapsed summaries.
- `expanded`: Shows primary parameters and macro controls.

Rules:

- Module cards use one card level only.
- Meters and changing values must not resize the card.
- Missing presets, samples, devices, or routes show `missing-resource` on the module, not only globally.
- Menu controls must not hide live-critical state.

### Mode Selector

Purpose: Switch between mutually exclusive modes such as MIDI/Sampled or MIDI Generation/MIDI Circular Pattern.

Required content:

- All available options.
- Active option.
- Disabled/unavailable options.
- Pending state if change is quantized or deferred.

Variants:

- `tabs`: Section-level mode selector, such as MIDI vs Sampled.
- `segmented`: Inline mode selector, such as MIDI Generation vs MIDI Circular Pattern.
- `compact`: Narrow row variant for lane-level mode.

Rules:

- Active and pending must be visually different.
- If a mode is unavailable, keep it visible but disabled when its absence affects performer expectations.
- Quantized changes display pending state until applied.
- Do not use hidden dropdowns for high-frequency performance mode switching.

### Parameter Control

Purpose: Control values such as density, complexity, variation, tune, decay, drive, level, ARP rate, swing, length, and macro amounts.

Types:

- `knob`: Continuous or stepped parameter with radial feedback.
- `slider`: Horizontal or vertical continuous parameter.
- `stepper`: Discrete values where exact selection matters.
- `select`: Small set of named values such as `2 Bars` or ARP rate.
- `toggle`: Binary setting such as bypass, mute, solo, or enabled.

Required content:

- Label.
- Current value or state.
- Range or available options when applicable.
- Default marker when useful for reset.
- Disabled or pending state.

Rules:

- Coarse controls should be usable under live conditions.
- Numeric labels reserve fixed width.
- Toggles must show active/inactive state without relying on color alone.
- High-frequency controls should not open large popovers.

### Meter

Purpose: Confirm audio, MIDI, CPU, or output activity.

Types:

- `audioLevel`: RMS/peak segmented level.
- `midiActivity`: Event or note activity.
- `cpu`: System load status.
- `clip`: Clipping/limiter warning.
- `routeActivity`: Source/post/master signal presence.

States:

- `noSignal`
- `active`
- `peak`
- `clipping`
- `muted`
- `disabled`
- `error`

Rules:

- Meters update visually without driving audio or MIDI timing.
- Meters reserve fixed size and never shift neighboring controls.
- Clipping must be distinct from normal high-level activity.
- Muted meters may show reduced pre-mute signal only if labeled by implementation.

### Pattern Engine Panel

Purpose: Present pattern generation and circular pattern controls in a performable way.

Required content:

- Mode selector for `MIDI Generation` and `MIDI Circular Pattern`.
- Generation controls: density, complexity, variation, length.
- Circular controls: steps, step activity preview, swing.
- Apply timing indicator when mode or parameter changes are quantized.

States:

- `active`
- `pending`
- `disabled`
- `error`

Rules:

- The active pattern mode is always visible.
- Step previews remain fixed width as steps change.
- Pending mode switches do not look applied until confirmed by engine state.
- Pattern controls use coarse values suitable for live adjustment.

### FX Slot

Purpose: Represent an ordered processor in the Post-FX or Master FX chain.

Required content:

- Processor name.
- Order position or flow connector.
- Bypass control.
- Macro controls or compact graph.
- Meter or activity indicator when useful.
- Routing hint where needed.

Variants:

- `instrumentPost`: Instrument/Post Layer processors.
- `master`: Master EQ, Glue Compressor, Stereo Width, Limiter, Master Level.
- `compact`: Collapsed or narrow chain.
- `empty`: Available slot, if future implementation supports adding processors.

Rules:

- Bypass preserves routing visibility.
- Processor order must be clear from left-to-right flow or explicit numbering.
- Master limiter warnings override normal accent treatment.
- FX slots must keep enough contrast for stage readability.

### Scene Status

Purpose: Show current scene and recall state.

Required content:

- Current scene number and name.
- Recall menu or selector.
- Pending scene indicator.
- Failed recall state when applicable.

States:

- `current`
- `pending`
- `applied`
- `failed`
- `disabled`

Rules:

- Pending recall shows both current scene and target scene where space allows.
- Failed recall identifies affected scope when available.
- Scene status must remain visible while transport is playing.

### Safety Control

Purpose: Provide reliable recovery from stuck notes, unsafe output, or broken routing.

Controls:

- Panic/all-notes-off.
- Stop transport.
- Master mute or output protect when implemented.
- Device/routing warning entry point.

Rules:

- Safety controls must be reachable from the shell or first-level UI.
- Panic must be visually distinct from normal performance toggles.
- Destructive or session-changing safety actions may require confirmation; panic and stop should not require modal confirmation.

## Section Guidance

### Drums

The Drums section supports MIDI and Sampled source modes with visible source status.

Required patterns:

- Section-level MIDI/Sampled selector.
- Drum Synth and Drum MIDI module cards when MIDI mode is active.
- Pattern Engine panel beside drum source controls.
- Sample preview area visible or disabled when not in Sampled mode.
- Meters for source activity and output activity.

State guidance:

- Unavailable sampled controls are disabled, not hidden, when MIDI mode is active and the prototype expects a sampled path.
- MIDI channel and kit/source labels remain visible in compact cards.
- Pattern mode pending state must be visible while transport is running.

### Synths

The Synths section contains Bass Synth, Poly/Chord Synth, and Pluck/Stab Synth rows.

Required patterns:

- One module row per synth lane.
- Lane-level MIDI channel and preset/source label.
- Pattern mode selector or active mode display.
- ARP and MIDI FX blocks per lane.
- Solo, mute, and collapse controls.
- Activity meter per lane.

State guidance:

- Solo and mute must remain visible in collapsed lane summaries.
- ARP rate and MIDI FX labels use compact, stable controls.
- Pattern previews reserve stable widths for generation and circular modes.

### Post-FX And Master FX

Post-FX presents ordered processing with clear bypass and signal flow.

Required patterns:

- Instrument/Post Layer chain.
- Master FX Layer chain.
- Ordered FX slots connected by flow arrows or equivalent structure.
- Per-layer bypass or enable control where valid.
- Level control and output meter at the end of each chain.

State guidance:

- Bypassed processors remain in the chain but subdued.
- Master EQ, Glue Compressor, Stereo Width, Limiter, Master Level, and master output must be easy to scan.
- Limiter, clipping, or output errors override normal master accent.

## Responsive Rules

MVP priority is cross-platform desktop. Responsive behavior exists to preserve usability at narrower desktop or tablet-like widths, not to transform the app into a phone-first layout.

Breakpoints:

- `wide`: 1440 px and above. Full density, multi-column rows.
- `standard`: 1024 px to 1439 px. Preserve all sections; reduce non-critical graphs first.
- `compact`: 768 px to 1023 px. Stack section internals, keep shell and section headers visible.
- `minimum`: below 768 px. Support inspection and basic operation only; avoid claiming full live-performance readiness.

Rules:

- Header status may compress labels but must preserve transport, tempo, scene, CPU, and master output.
- Section internals can stack, but module order must remain Drums, Synths, Post-FX, then Master FX.
- Text never overlaps controls; truncate non-critical names and expose full text through tooltip.
- Meters, mode selectors, and safety controls must preserve fixed minimum dimensions.

## Accessibility

Accessibility is part of reliability for a stage tool.

Requirements:

- Keyboard focus ring uses `color.focus.ring` and is visible on all controls.
- Critical controls have accessible names matching their visible labels or domain terms.
- Color is never the only signal for active, pending, error, muted, soloed, or bypassed states.
- Contrast targets: 4.5:1 for normal text, 3:1 for large text and essential UI boundaries.
- Reduced motion preference disables decorative or non-essential motion.
- Tooltips name unfamiliar icons and explain disabled or unavailable controls.
- Focus order follows the performance workflow: shell, Drums, Synths, Post-FX, Master FX, safety/status.

## Implementation Contracts

Future frontend work should map these design-system concepts to component props or state fields without leaking audio-engine internals into UI components.

Recommended shared concepts:

- `uiState`: one of the state model names.
- `activityState`: `noSignal`, `active`, `peak`, `clipping`, `disabled`, or `error`.
- `applyTiming`: `immediate`, `nextStep`, `quantized`, or `async`.
- `density`: `expanded`, `compact`, or `collapsed`.
- `tone`: `neutral`, `midi`, `synth`, `fx`, `master`, `warning`, or `error`.

Rules:

- UI commands call backend or engine command APIs; components do not mutate engine internals.
- Treat commands as asynchronous and fallible unless a future contract says otherwise.
- High-frequency data such as meters arrives through throttled subscriptions or snapshots.
- UI rendering cadence must not drive audio or MIDI timing.
- Frontend tests should assert visible state, not internal engine implementation.

## QA Checklist

Design-system acceptance for MVP documentation and future implementation:

- Transport, tempo, scene, CPU, and master output are visible in the shell.
- Drums, Synths, Post-FX, and Master FX have consistent section headers and aggregate states.
- MIDI/Sampled and MIDI Generation/MIDI Circular Pattern selectors show active, disabled, and pending states.
- Mute, solo, bypass, collapse, loading, missing-resource, and error states are visually distinct.
- Module cards, meters, selectors, knobs, and FX slots keep stable dimensions while values update.
- Text does not overflow or overlap at wide, standard, compact, and minimum widths.
- Meters show no-signal, activity, peak, clipping, muted, disabled, and error states.
- Collapsed sections preserve critical state indicators.
- Scene recall pending and failed states are visible while transport is playing.
- Panic/all-notes-off has a reliable first-level recovery path.
- Keyboard focus is visible and follows performance order.
- Reduced motion and disabled states remain understandable.

## Do And Do Not

Do:

- Use dense, operational layouts.
- Keep stage-critical state visible.
- Use state names consistently across design, frontend, and QA.
- Reserve fixed dimensions for meters and changing values.
- Prefer icons for common commands, with tooltips where recognition may be weak.

Do not:

- Start the app with a marketing page or setup wizard.
- Hide active engine paths behind menus.
- Make pending changes look applied.
- Use nested cards for sections and modules.
- Depend only on color for state.
- Let hover states, meters, or labels resize controls.
- Add decorative backgrounds that reduce scan speed or contrast.
