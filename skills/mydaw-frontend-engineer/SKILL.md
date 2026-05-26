---
name: mydaw-frontend-engineer
description: Implement the MyDAW live performance UI and connect it to audio, MIDI, state, and backend services. Use when building Drums, Synths, Post-FX, Master FX, module cards, selectors, meters, knobs, mute, solo, bypass, collapse, or pattern mode switching.
---

# MyDAW Frontend Engineer

## Purpose

Implement the live-performance interface and connect UI state to engine state without compromising clarity or real-time reliability.

## Responsibilities

- Build the main performance screen.
- Build Drums, Synths, Post-FX, and Master FX UI sections.
- Build module cards, selectors, meters, knobs, and controls.
- Connect UI state to audio/MIDI engine state.
- Implement mute, solo, bypass, collapse, and pattern mode switching.

## Inputs It Expects

- Product requirements and UX specs.
- Design-system components and tokens.
- Backend APIs, state schemas, and engine command contracts.
- QA findings and browser/runtime constraints.

## Outputs It Produces

- Implemented UI screens and components.
- State bindings and engine command integrations.
- Frontend tests for critical workflows.
- Handling for loading, errors, pending states, and unavailable controls.

## Collaboration Points

- Work with UX and design-system skills on interaction and component fidelity.
- Work with backend and state skills on API contracts and persistence flows.
- Work with audio, MIDI, pattern, sampler, synth, and FX skills on accurate control mapping.
- Work with real-time skill to prevent UI behavior from blocking engine paths.
- Work with QA on responsive, workflow, and regression tests.

## Workflow

1. Build from existing project patterns and component architecture.
2. Keep UI state, persisted state, and engine state boundaries explicit.
3. Treat engine commands as asynchronous or fallible unless contracts say otherwise.
4. Render active, pending, bypassed, muted, soloed, loading, and error states clearly.
5. Verify desktop and mobile/responsive layouts where relevant.

## References

Read `references/domain-guide.md` when implementing performance screen structure, module components, UI-engine binding, or live control behavior.

## Definition Of Done

- Main performance screen is usable with Drums, Synths, Post-FX, and Master FX.
- Controls reflect engine state and handle pending/error states.
- Mute, solo, bypass, collapse, and pattern mode switching are implemented.
- UI does not create layout shifts or block engine paths.
- Tests or manual verification cover key live workflows.
