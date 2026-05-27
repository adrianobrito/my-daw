# MyDAW Documentation Guide

## Canonical Sources

Use the repo `docs/` folder as the source of truth:

- `product-definition.md`: product position, target user, MVP scope, workflow, platforms, and non-negotiables.
- `information-architecture.md`: screen hierarchy, section order, navigation, reveal, and state visibility.
- `main-performance-screen.md`: implementation-ready shell, Drums, Synths, Post-FX, Master FX, interaction flows, and responsive behavior.
- `design-system.md`: tokens, component inventory, state vocabulary, accessibility, and QA checklist.
- `module-states.md`: module-state precedence, visual treatment, interaction rules, and frontend fields.
- `pattern-mode-selector.md` and `pattern-mode-selector-component-contract.json`: Generation/Circular selector behavior, parameters, commands, snapshots, and MIDI scheduling constraints.
- `scene-set-workflow.md` and `scene-set-workflow-component-contract.json`: set/session/scene terminology, recall states, persistence boundaries, validation, autosave, and recovery.

## Writing Rules

- Write from the solo electronic performer's live task first, then add reference detail.
- Keep the first screen as the usable performance surface. Do not describe a landing page, setup wizard, arrangement timeline, or mode hub as MVP behavior.
- Mark deferred work explicitly: full timeline, advanced sample editor, third-party plugin hosting, cloud collaboration, deep modulation matrix, and complex controller mapping.
- Use shared terms exactly: Drums, Synths, Post-FX, Master FX, MIDI Generation, MIDI Circular Pattern, Sampled, scene, set, session, snapshot, preset.
- Use shared states exactly: `inactive`, `active`, `armed`, `pending`, `muted`, `soloed`, `bypassed`, `disabled`, `loading`, `missing-resource`, `error`.
- For scene docs, use scene recall states exactly: `current`, `selected`, `armed`, `pending`, `queued`, `applying`, `applied`, `blocked`, `failed`, `partialRecoverable`.

## Documentation Patterns

- Product docs should state MVP scope, post-MVP boundaries, supported platforms, and acceptance criteria.
- UX and design docs should preserve shell -> Drums -> Synths -> Post-FX -> Master FX ordering and collapsed critical-state visibility.
- Developer docs should identify ownership, data flow, command/snapshot APIs, state boundaries, and real-time constraints.
- Component-contract docs should name required data, variants, controls, commands, snapshots, and acceptance criteria.
- Alpha docs and release notes should include setup, audio/MIDI device notes, known limitations, platform caveats, QA status, and diagnostics/reporting guidance.

## Definition Of Done

- Docs align with current canonical specs and component contracts.
- Active vs pending behavior remains unambiguous.
- Real-time safety constraints are visible wherever engine, MIDI, audio, state, or persistence behavior is described.
- Known limitations and deferred features are explicit.
