# Audio Engine Architecture Guide

## Audio Graph Shape

Use a predictable graph:

- sources: sampler voices, synth voices, external audio where supported
- instrument buses: Drums, Bass Synth, Poly/Chord Synth, Pluck/Stab Synth
- Post-FX chains: instrument or bus-level FX
- Master FX: Master EQ, Glue Compressor, Stereo Width, Limiter
- output: device output and meters

Keep graph mutation outside the audio callback. Apply graph changes using prepared commands, double buffering, or lock-free handoff patterns appropriate to the implementation stack.

## Device Handling

- Support explicit device selection, sample rate, buffer size, and channel configuration.
- Provide fallback behavior when a device disappears.
- Rebuild or rebind the graph safely after device changes.
- Surface device errors to UI without blocking audio paths.

## Routing Rules

- Instruments route to their assigned Post-FX path, then to Master FX, then output.
- Bypass must preserve signal continuity and avoid pops.
- Mute and solo behavior must be deterministic and testable.
- Meter taps should be defined at useful points: source, post-FX, master pre-limiter, master output.

## Metering

- Calculate meter values on the audio side using lightweight accumulation.
- Publish meter snapshots to UI at a lower control-rate cadence.
- Include clipping or overload state where needed.
- Do not let UI polling block audio processing.

## Low-Latency Requirements

- Avoid locks, heap allocation, file I/O, logging, network calls, and blocking waits on the audio thread.
- Preload or prepare resources outside real-time paths.
- Use bounded work per buffer.
- Treat denormals, oversampling, and expensive FX as CPU risks.
- Document any unavoidable tradeoff and require real-time review.
