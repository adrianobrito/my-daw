# Design System Guide

## Token Priorities

- Use a restrained operational palette with clear accent colors for active, armed, warning, error, bypassed, muted, soloed, and pending states.
- Define spacing that supports dense scan-friendly layouts.
- Keep typography legible at stage distance; avoid viewport-scaled text.
- Use consistent focus rings for keyboard and controller workflows.
- Keep card radius at 8px or less unless the application later establishes a different system.

## Core Components

- Module card: title, type, active state, meter, mute/solo/bypass controls, collapse affordance, and primary parameters.
- Section header: section name, aggregate state, add/select actions, and collapse state.
- Mode selector: mutually exclusive state, disabled states, and pending transition state.
- Knob/slider: value display, range, default marker, automation/modulation indicator if needed.
- Meter: peak/RMS or activity mode, clipping state, and no-signal state.
- FX slot: name, bypass, order, macro controls, routing hint.

## State Model

Design visible states for:

- inactive
- active
- armed
- pending
- muted
- soloed
- bypassed
- disabled
- loading
- missing-resource
- error

Use state names consistently across UX specs, frontend props, tests, and docs.

## Component Rules

- Do not make nested cards for page sections; reserve cards for modules and repeated items.
- Avoid layout shifts from meters, labels, changing values, or hover states.
- Ensure text fits inside controls at narrow widths.
- Keep control labels short and domain-specific.
- Provide compact variants for collapsed sections that still expose status.

## Documentation Expectations

For each component, document:

- purpose
- required props or data
- variants
- state behavior
- accessibility notes
- examples of valid placement
