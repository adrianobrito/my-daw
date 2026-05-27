# Frontend Engineering Guide

## Implementation Priorities

- Render the actual performance interface first.
- Use existing framework, styling, state management, and component patterns.
- Keep controls stable in size during meter updates and state changes.
- Avoid visible instructional copy; design controls to be understandable.
- Prefer explicit state indicators over hidden side effects.

## Screen Structure

- Top-level performance shell: transport, tempo, scene/session status, global safety indicators.
- Drums section: lanes or modules with source mode, pattern mode, controls, and meters.
- Synths section: Bass, Poly/Chord, Pluck/Stab modules.
- Post-FX section: ordered FX slots with bypass and macro controls.
- Master FX section: master processors, output meter, limiter state.

## Engine Binding

- UI commands should call backend or engine APIs, not mutate engine internals directly.
- Treat commands as fallible and show recoverable error states.
- Use throttling or subscription cadence for meters and high-frequency values.
- Do not let UI rendering rate drive MIDI or audio timing.

## Controls

- Mute, solo, bypass, collapse, and mode switching must have clear active states.
- Pattern changes should show pending state when quantized.
- Disabled controls should explain state through affordance or tooltip when needed.
- Preserve keyboard/controller accessibility for critical actions.

## Verification

- Check layout at compact and wide sizes.
- Verify text does not overflow controls.
- Verify meters update without layout shift.
- Verify scene recall and pattern switching states are visible.
