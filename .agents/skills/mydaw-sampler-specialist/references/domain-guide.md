# Sampler Guide

## Sample Lifecycle

- Load file metadata and decode audio outside the audio callback.
- Prepare playback buffers before triggering.
- Report loading, ready, missing, invalid, and error states.
- Keep session references portable where possible.
- Avoid blocking scene recall on slow disk access during performance.

## Triggering Behavior

- Define one-shot, gated, retrigger, choke, and polyphony behavior per module type.
- Preserve note-off handling for gated samples.
- Provide panic or stop behavior for stuck or runaway voices.
- Keep rapid triggering bounded by voice limits.

## Controls

- Gain, pan, transpose, tune, start offset, envelope attack/decay/sustain/release, and 3-band EQ should have documented ranges.
- Transpose must define quality/CPU tradeoffs and whether it is semitone, cents, or continuous.
- Envelopes should avoid clicks on trigger, release, mute, and scene recall.

## Integration

- Sampled mode should share routing with other instrument sources through Post-FX and Master FX.
- MIDI triggers should use the MIDI engine’s scheduling contract.
- Scene recall should not silently swap to a missing sample.
- UI should expose missing-resource state and allow recovery.

## Reliability Tests

- Rapid triggering under CPU load.
- Switching samples while transport runs.
- Scene recall with loaded, missing, and large samples.
- Transpose and envelope changes during playback.
- Mute, solo, and bypass interaction with sample voices.
