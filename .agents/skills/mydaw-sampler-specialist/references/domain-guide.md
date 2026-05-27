# Sampler Guide

## Canonical Sources

Use `docs/product-definition.md`, `docs/main-performance-screen.md`, `docs/module-states.md`, and `docs/scene-set-workflow.md` for Sampled mode, missing resources, state visibility, and recall behavior.

## Sampled Mode Scope

Drums exposes MIDI and Sampled source modes. Sampled mode activates Sample Control and Sample FX; MIDI mode may keep sampled preview visible as disabled or preview-only when performer expectations require it.

Sample-related controls should stay stage-friendly: sample identity, loading/missing state, preview availability, tune/transpose, decay/envelope, drive/EQ where supported, level, velocity, and humanize when present.

## Lifecycle

Define sample lifecycle explicitly:

`reference -> load -> decode -> prepare -> trigger -> stop -> unload`

File I/O and decode never run on audio or MIDI real-time paths. Scene recall validates and prepares required resources before live application.

## States And Recovery

Use `loading` for sample preparation, `missing-resource` for absent samples, and `error` for failed decode, route validation, or playback setup. Missing samples appear on the affected source module, section summary, and scene/session recovery surfaces when relevant.

Failed sample load must not destroy the current live session or silently silence unrelated modules.

## QA Focus

Test rapid triggering, sample changes during transport, missing files, failed decode, scene recall, voice limits, CPU/memory pressure, and dropout risk.
