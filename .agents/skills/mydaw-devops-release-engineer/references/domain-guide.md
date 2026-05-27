# DevOps Release Guide

## Canonical Sources

Use `docs/product-definition.md` for supported platforms, release gates, and alpha non-negotiables. Use QA results and documentation skill output for release notes and onboarding.

## Target Platforms

MVP targets cross-platform desktop builds:

- macOS,
- Windows,
- Linux.

Release readiness is gated per platform because audio, MIDI, permissions, packaging, and device behavior differ.

## Release Gates

A platform is not release-ready until:

- install and launch are verified,
- audio/MIDI device access and reconnect behavior are tested,
- no known reproducible audio dropout exists in the intended alpha workflow,
- no known destructive save/load issue exists,
- required permissions/device steps are documented,
- release notes list known limitations and platform-specific caveats.

## Build Metadata

Private-alpha builds should include version, build date, commit or build identifier, and release channel where possible. Artifacts should package required runtime assets, presets, examples, and default sessions that are in MVP scope.

## Diagnostics

Crash reporting and fatal diagnostics must stay off audio/MIDI real-time paths. Collect enough information for alpha bug reports without compromising performance or privacy.

## Release Notes And Onboarding

Include version/date, highlights, fixes, known issues, testing status, upgrade/compatibility notes, platform caveats, setup steps, audio/MIDI device guidance, and bug-report diagnostics.
