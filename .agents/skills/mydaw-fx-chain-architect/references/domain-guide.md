# FX Chain Guide

## Canonical Sources

Use `docs/product-definition.md`, `docs/main-performance-screen.md`, `docs/design-system.md`, and `docs/module-states.md` for MVP FX scope, routing visibility, state treatment, and UI contracts.

## MVP Chains

Post-FX shows ordered instrument/post processing before master output. Master FX is semantically distinct and includes:

- Master EQ,
- Glue Compressor,
- Stereo Width,
- Limiter,
- Master Level,
- master output metering.

Bypassed processors remain visible in chain position. Bypass never looks like deletion and should preserve signal continuity without pops.

## State And Recall

FX slots use `active`, `bypassed`, `pending`, `missing-resource`, and `error` states. Pending bypass, order, preset, or scene-driven changes remain visually distinct until confirmed.

Scene recall may affect FX bypass, macro values, levels, presets, and routes. Prepare expensive changes off real-time paths and apply through bounded handoffs.

## Design Constraints

- Keep MVP controls macro-level and stage-friendly.
- Preserve left-to-right or explicit order.
- Master limiter, clipping, output protect, and route errors override normal master accent.
- Per-slot meters and changing value labels must not resize the chain.

## QA Focus

Test rapid bypass, scene recall, route changes, limiter/clipping warnings, CPU load, and dropout risk under transport.
