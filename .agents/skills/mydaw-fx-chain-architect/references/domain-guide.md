# FX Chain Guide

## Chain Model

- Instrument or bus source feeds Post-FX.
- Post-FX feeds Master FX.
- Master FX order defaults to Master EQ, Glue Compressor, Stereo Width, Limiter unless product requirements choose otherwise.
- Meter taps should support source, post-FX, and master output visibility.

## MVP FX

- EQ: gain bands, frequency controls if exposed, and bypass.
- Compressor: threshold, ratio or amount, attack/release or macro, makeup gain if needed.
- Reverb: size/decay, wet/dry, pre-delay if needed.
- Delay: time, feedback, wet/dry, sync mode if supported.
- Master EQ: broad tone correction.
- Glue Compressor: bus cohesion with conservative defaults.
- Stereo Width: master width with mono-safe considerations.
- Limiter: final protection with visible gain reduction or clip state.

## Bypass And Ordering

- Bypass should avoid clicks and preserve predictable signal flow.
- Reverb and delay tail behavior must be specified.
- Ordering changes must be prepared outside real-time paths.
- Disabled FX should not consume unnecessary CPU where avoidable.
- UI must show active order and bypass state clearly.

## Live Safety

- Limit controls that can cause sudden extreme level changes.
- Provide sane defaults and bounded ranges.
- Test rapid bypass toggling and scene recall.
- Master Limiter should protect output but not hide gain staging problems.

## Persistence

- Save FX order, enabled/bypassed state, parameters, and macro values.
- Define how missing or unsupported FX types load.
- Scene recall should update chains at safe boundaries when needed.
