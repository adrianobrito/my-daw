# Product Architecture Guide

## MVP Product Principles

- Optimize for a performer using the app under time pressure, dim lighting, and limited attention.
- Favor immediate control over deep editing in the MVP.
- Treat reliability, low latency, predictable scene recall, and clear state visibility as product features.
- Keep Drums, Synths, Post-FX, pattern generation, circular patterns, ARP, MIDI FX, and session persistence in one coherent performance workflow.
- Avoid adding studio-production workflows unless they directly support live use.

## Target Users

- Electronic live performers who need a compact session view for drums, synths, FX, and patterns.
- Producers who perform prepared sets and need safe variation controls.
- MIDI-focused performers who want generative and circular pattern workflows without building a full arrangement.
- Alpha users who can tolerate incomplete depth but not unstable audio or unclear state.

## MVP Boundaries

MVP includes:

- Main performance screen with Drums, Synths, Post-FX, Master FX, and transport context.
- MIDI vs Sampled mode selection where relevant.
- MIDI Generation and MIDI Circular Pattern modes.
- Basic ARP and MIDI FX behavior.
- Scene/session save, load, and recall.
- Essential meters, mute, solo, bypass, collapse, and pattern switching.
- Reliability and performance tests for live use.

Post-MVP includes:

- Full timeline arrangement.
- Advanced sample editing.
- Third-party plugin hosting.
- Cloud collaboration.
- Deep modulation matrix workflows.
- Complex controller-mapping editors unless needed for alpha users.

## Prioritization Rubric

Score features higher when they:

- Reduce live-performance risk.
- Make critical state visible.
- Enable fast musical variation.
- Simplify routing or scene recall.
- Have low real-time safety risk.
- Support a complete alpha rehearsal workflow.

Defer features when they:

- Require long configuration during performance.
- Add hidden state that is hard to inspect.
- Increase audio-thread complexity before core reliability exists.
- Duplicate functionality already covered by a simpler control.

## Acceptance Criteria Pattern

Write requirements as observable behavior:

- "A performer can switch a drum lane from Sampled to MIDI without stopping transport."
- "Scene recall updates selected pattern modes at the next quantized boundary."
- "Bypassed FX remain visible and cannot silently affect audio."

Avoid requirements that only describe implementation preference unless the implementation choice protects latency, reliability, or maintainability.
