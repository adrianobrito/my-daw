# Real-Time Systems Guide

## Safety Rules

Avoid in real-time paths:

- locks that can block
- heap allocation in steady-state processing
- file I/O
- network I/O
- logging
- device enumeration
- synchronous UI calls
- unbounded loops or queues
- waiting on futures, promises, or condition variables

## Path Classification

Real-time paths include:

- audio callback
- sample rendering
- synth voice rendering
- FX processing
- sample-accurate MIDI event consumption

Near-real-time paths include:

- MIDI input handling
- scheduler lookahead
- transport clock updates

Non-real-time paths include:

- UI rendering
- persistence
- sample decoding
- preset loading
- crash reporting

## Budgets

Define targets for:

- audio callback maximum duration
- average and peak CPU
- MIDI jitter
- scene recall duration
- memory allocation rate
- maximum voice and FX counts

Tie budgets to target hardware and buffer size.

## Review Checklist

- Are resources prepared before playback?
- Can any engine command block?
- Can UI updates back up engine queues?
- Are queues bounded?
- Are graph changes applied safely?
- Are note-offs guaranteed during state changes?
- Are errors surfaced without blocking processing?

## Stress Scenarios

- Dense patterns plus multiple synths and FX.
- Rapid scene recall while transport runs.
- Device disconnect and reconnect.
- Large sample load while playback continues.
- Repeated bypass, mute, solo, and pattern mode switching.
