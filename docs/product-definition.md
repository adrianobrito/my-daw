# MyDAW Product Definition

This document resolves the local product-definition work for:

- GitHub issue #1: Define target user
- GitHub issue #2: Define MVP scope
- GitHub issue #3: Define live-performance workflow
- GitHub issue #4: Define supported platforms
- GitHub issue #5: Define non-negotiables

The source visual reference is `my-daw-prototype.png`, a 1672 x 941 prototype of the main MyDAW live-performance screen.

## Product Position

MyDAW is a cross-platform desktop DAW for solo electronic live performers who need a reliable, stage-ready performance surface for drums, synths, MIDI pattern workflows, post-processing, master effects, and scene/session recall.

The MVP favors immediate control, visible state, and predictable performance behavior over deep studio editing. It should help a performer rehearse and play a prepared set without relying on a full arrangement timeline.

## #1 Target User

The primary target user is a solo electronic performer who:

- Performs prepared sets with drums, bass, chord/poly synths, pluck/stab synths, MIDI patterns, FX, and scene changes.
- Needs fast control under live conditions: dim lighting, time pressure, limited attention, and little tolerance for ambiguous state.
- Uses internal pattern generation, circular MIDI patterns, ARP, MIDI FX, and effect macros to create variation during playback.
- May connect MIDI controllers or external MIDI devices, but does not require deep controller mapping in the MVP.
- Can tolerate incomplete editing depth during private alpha, but cannot tolerate unstable audio, unclear state, lost sessions, stuck notes, or silent routing failures.

Secondary users are producers who perform prepared material live and MIDI-focused performers who want generative or circular pattern workflows without building a linear arrangement.

Acceptance criteria:

- A performer can identify the app as a live-performance DAW from the first screen.
- The main workflow supports live control of drums, synths, patterns, FX, transport, and scenes without opening a separate arrangement view.
- Any non-MVP feature request can be evaluated against its value for solo live performance.

## #2 MVP Scope

The MVP includes the smallest coherent live-performance workflow visible in the prototype.

Core performance surface:

- Main screen with persistent transport, tempo, time signature, scene, CPU, and master output status.
- Drums section with MIDI and Sampled source modes.
- Synths section with Bass Synth, Poly/Chord Synth, and Pluck/Stab Synth lanes.
- Post-FX section with instrument/post layer and master FX layer.
- Master FX chain with Master EQ, Glue Compressor, Stereo Width, Limiter, master level, and output metering.

Live controls:

- Play, stop, tap tempo, tempo display, and scene selection/status.
- Mute, solo, bypass, collapse, and expanded/collapsed section state.
- MIDI vs Sampled mode selection where relevant.
- MIDI Generation and MIDI Circular Pattern modes.
- Pattern controls for density, complexity, variation, length, steps, and swing where applicable.
- Basic ARP controls and MIDI FX controls.
- Essential meters for source, post-FX, master pre-limiter, and master output where useful.

State and reliability:

- Scene/session save, load, and recall.
- Quantized scene recall and pattern-mode changes where musical timing requires it.
- Panic/all-notes-off for stuck notes.
- Visible loading, pending, bypassed, muted, soloed, unavailable, and error states.

Post-MVP:

- Full linear timeline arrangement.
- Advanced sample editing.
- Third-party plugin hosting.
- Cloud collaboration.
- Deep modulation matrix workflows.
- Complex controller-mapping editors unless required by alpha users.
- Advanced external hardware routing beyond the MVP MIDI routing contract.

Acceptance criteria:

- A private-alpha performer can load a prepared session, start transport, vary patterns, mute/solo lanes, adjust FX, recall scenes, and stop safely.
- Drums, synths, Post-FX, Master FX, transport, scenes, CPU, and master output are visible or quickly reachable from the main screen.
- Bypassed, muted, soloed, pending, and error states are visually distinct.
- Post-MVP features are documented as deferred and do not block the MVP workflow.

## #3 Live-Performance Workflow

The main workflow is rehearsal-to-performance:

1. Load a session containing scenes, instrument lanes, pattern settings, FX chains, routing, and device preferences.
2. Confirm device, CPU, transport, scene, and master output status from the header.
3. Start transport and perform from the main screen.
4. Use Drums and Synths sections to switch source or pattern modes, adjust pattern variation, mute/solo lanes, and confirm activity through meters.
5. Use ARP and MIDI FX for live harmonic, rhythmic, or scale-constrained variation.
6. Use Post-FX and Master FX controls to shape the current mix while preserving signal continuity.
7. Recall scenes at quantized boundaries when timing safety matters.
8. Recover from device, routing, sample, or MIDI issues without blocking playback when possible.
9. Save the updated session or recover from autosave after failure.

Behavior requirements:

- Transport state, tempo, beat position, scene, CPU, and master output must remain visible during performance.
- Scene recall displays pending and applied states.
- Pattern changes state whether they apply immediately, at the next step, or at a quantized boundary.
- Mute and bypass provide immediate UI feedback even when the audio change is quantized.
- Collapse preserves critical status indicators.
- Error states identify what is broken and what still works.

Acceptance criteria:

- A performer can move from loaded session to active performance without leaving the main screen.
- A performer can switch a drum lane between MIDI and Sampled modes without stopping transport when both modes are available.
- A performer can switch between MIDI Generation and MIDI Circular Pattern modes and see the active mode clearly.
- A performer can recall a scene during playback and see pending state before the recall is applied.
- A performer can trigger panic/all-notes-off from a reliable recovery path.

## #4 Supported Platforms

The MVP targets cross-platform desktop builds:

- macOS
- Windows
- Linux

The product behavior should be shared across platforms, but release readiness is gated per platform because audio, MIDI, permissions, packaging, and device behavior differ.

Platform requirements:

- Use native or platform-appropriate low-latency audio and MIDI device access.
- Support explicit audio device selection, sample rate, buffer size, and channel configuration where the platform allows it.
- Support MIDI input/output routing and reconnect behavior.
- Package builds with required runtime assets, presets, examples, and default sessions that are in MVP scope.
- Embed version, build date, commit or build identifier, and release channel in private-alpha builds where possible.
- Capture crashes and fatal diagnostics off the audio/MIDI real-time paths.

Release gates:

- Installation and launch verified on each target platform.
- No known reproducible audio dropout in the intended alpha workflow.
- No known destructive save/load issue.
- Required audio and MIDI permissions or device access steps are documented for each platform.
- Release notes list known limitations and platform-specific caveats.

Acceptance criteria:

- A build can be traced to version and channel metadata.
- Each target platform has a reproducible build and package path before private alpha.
- QA can run the same core live-performance scenario on macOS, Windows, and Linux.
- A platform is not considered release-ready until its audio/MIDI device tests pass.

## #5 Non-Negotiables

Reliability and timing are product features. The following constraints are non-negotiable for MVP work.

Real-time safety:

- No locks that can block on audio-thread or sample-accurate MIDI event paths.
- No heap allocation in steady-state audio processing.
- No file I/O, network I/O, logging, device enumeration, synchronous UI calls, unbounded queues, unbounded loops, or waits on futures/promises/condition variables in real-time paths.
- Graph changes are prepared outside the audio callback and applied through a bounded, real-time-safe handoff.
- UI updates, persistence, sample decoding, preset loading, crash reporting, and device enumeration stay off real-time paths.

Audio behavior:

- Audio graph shape is predictable: sources to instrument buses, Post-FX, Master FX, output, and meters.
- Instruments route to their assigned Post-FX path, then Master FX, then output.
- Bypass preserves signal continuity and avoids pops.
- Mute and solo behavior is deterministic and testable.
- Meter taps publish lightweight snapshots to the UI at control-rate cadence and never block audio processing.

MIDI behavior:

- One clock authority is active at a time: internal clock or external MIDI clock.
- MIDI scheduling uses timestamps or sample-accurate offsets when integrating with audio.
- Note-on and note-off pairs are scheduled safely.
- Event ordering is preserved for simultaneous events.
- Event queues and scheduler lookahead are bounded.
- Panic/all-notes-off is available for stuck-note recovery.

State and recovery:

- Scene/session save, load, autosave, recover, preset save/load, and scene recall are validated before applying loaded data.
- Scene recall cannot silently partially fail; failures must be visible and structured.
- Missing devices, samples, presets, or routes surface actionable error states.
- Device disconnect and reconnect are expected failure modes.
- Session data must not be destroyed by a failed load or recall.

Acceptance criteria:

- Real-time paths and non-real-time paths are identified before engine implementation.
- Stress testing covers dense patterns, multiple synths and FX, rapid scene recall, device disconnect/reconnect, large sample load during playback, and repeated bypass/mute/solo/pattern switching.
- UI event floods cannot create unbounded engine work.
- Note-offs are guaranteed during transport stop, route changes, and scene recall.
- No private-alpha release ships with a known reproducible audio dropout in the intended MVP workflow.

## Prototype Alignment

The prototype establishes the first-screen product direction:

- Header: hamburger, play/stop/tap, `Live Performance DAW`, tempo, time signature, scene, CPU, master output meter, and settings.
- Drums: MIDI and Sampled tabs, drum synth, drum MIDI, pattern engine, sample preview, meter activity, source mode labels, and quick macros.
- Synths: Bass Synth, Poly/Chord Synth, Pluck/Stab Synth, MIDI Generation, MIDI Circular Pattern, ARP, MIDI FX, solo, mute, and collapse controls.
- Post-FX: instrument/post layer and master FX layer with ordered processors, bypass, macro controls, meters, and level controls.
- Master behavior: Master EQ, Glue Compressor, Stereo Width, Limiter, master level, and output metering.

Implementation should keep this operational density and stage-readability. The first screen should be the usable performance surface, not a landing page or setup wizard.
