# DevOps Release Guide

## Build Pipeline

- Use the repository’s native build tools and package manager.
- Separate debug, profiling, and release builds.
- Preserve symbols or source maps for crash diagnosis.
- Cache dependencies carefully without hiding clean-build failures.
- Run unit and integration tests before packaging.

## Packaging

- Define target OS versions and CPU architectures.
- Include required runtime assets, presets, examples, and default sessions if in scope.
- Handle audio/MIDI permissions where the platform requires them.
- Keep installer/update behavior simple for private alpha.

## Versioning

- Use consistent semantic or date-based versioning.
- Embed version, commit, build date, and channel in the app where possible.
- Tag release artifacts or otherwise make builds traceable.

## Crash Reporting

- Capture crashes, fatal errors, and relevant diagnostics.
- Avoid collecting sensitive user content unless explicitly approved.
- Ensure reporting does not run on audio or MIDI real-time paths.
- Provide a way to correlate alpha user reports with builds.

## Alpha Release Gate

- QA acceptance complete or documented exceptions approved.
- No known reproducible audio dropout in intended use.
- No known destructive save/load issue.
- Installation and launch verified on target platform.
- Release notes and onboarding notes available.
