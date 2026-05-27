---
name: mydaw-documentation-writer
description: Create and maintain MyDAW product, UX, component-contract, developer, module, workflow, alpha onboarding, and release documentation from the canonical docs folder. Use when writing or updating docs for product scope, information architecture, main performance screen behavior, design system, module states, pattern mode selection, scene/set workflow, MIDI/FX workflows, onboarding, or release notes.
---

# MyDAW Documentation Writer

## Purpose

Create accurate, practical documentation that helps performers use MyDAW and helps developers maintain it without obscuring MVP limits.

## Responsibilities

- Maintain the docs folder as the source of truth for MyDAW MVP behavior.
- Align docs with product-definition, IA, main-screen, design-system, module-state, pattern-selector, and scene/set workflow specs.
- Write user guide.
- Write developer architecture docs.
- Document module behavior.
- Document MIDI and FX workflows.
- Create private alpha onboarding notes.
- Maintain release notes.

## Inputs It Expects

- Product scope, UX flows, implementation behavior, and known limitations.
- Engine, backend, state, and module contracts.
- QA results and release notes.
- Screenshots, diagrams, or examples when available.

## Outputs It Produces

- User guide and quick-start material.
- Developer architecture documentation.
- Module behavior reference.
- MIDI, pattern, sampler, synth, FX, scene, and session workflow docs.
- Alpha onboarding notes and release notes.

## Collaboration Points

- Work with product skill to keep documentation aligned with MVP scope.
- Work with UX/frontend skills for accurate screen and control descriptions.
- Work with engine, backend, and state skills for architecture accuracy.
- Work with QA and DevOps on known issues, release validation, and alpha instructions.

## Workflow

1. Document actual behavior, not intended behavior that is not implemented.
2. Write from the performer's task first, then provide reference detail.
3. Keep developer docs boundary-focused: ownership, data flow, APIs, and real-time constraints.
4. Make known limitations explicit for alpha users.
5. Update release notes from product scope, merged changes, QA results, and known issues.
6. Preserve the MVP distinction between the first-screen live performance surface and deferred timeline, plugin-hosting, cloud, and deep-editor work.

## References

Read `references/domain-guide.md` before writing or revising docs. It maps the canonical `docs/` files to documentation tasks and terminology.

## Definition Of Done

- User docs explain live workflows clearly.
- Developer docs identify architecture boundaries and real-time constraints.
- Module, MIDI, FX, scene, and session behavior is documented.
- Alpha onboarding includes setup, known limitations, and reporting guidance.
- Release notes match the shipped build.
