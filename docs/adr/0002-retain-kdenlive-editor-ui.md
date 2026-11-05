# 0002: Retain and restyle Kdenlive's editor UI

Date: 2026-11-06

Status: Accepted by the creator.

## Context

The creator wants to reuse a mature editor's features and avoid rebuilding its controls.
Kdenlive is the selected foundation for that approach, based on the source research.
The creator chose to keep and restyle its Qt controls and use Rust and Svelte where useful.

## Decision

- Keep Kdenlive's Qt Widgets, Qt Quick, and C++ editor implementation as the starting point.
- Retain the timeline, monitors, effect controls, project handling, and undo system.
- Customize branding, appearance, menus, and workspace layouts.
- Hide unwanted tools before considering removal of their code.
- Use Rust for the new harness and supporting services where it helps.
- Use Svelte 5 and vanilla CSS for useful new web UI surfaces.
- Use Tauri where a separate desktop workspace is useful; it is not required to host the Qt editor.
- Route agent edits through the editor's project and undo operations.
- Keep the fork's changes small so upstream fixes can be merged.

This amends the UI boundary in [ADR 0001](0001-desktop-platforms-and-stack.md).
The main editor does not need to be rebuilt in Svelte.
Do not add a second shell, a second timeline model, or a second window without a concrete need.

## Alternatives considered

Building a complete Svelte editor over MLT would reuse media processing but require substantially more editor UI work.
Retaining the existing Qt controls preserves more application behavior and matches the creator's chosen priority.
A separate Tauri/Svelte production workspace remains possible, but its need and integration are not decided here.

## Consequences

The project includes upstream C++ and QML alongside new Rust and any Svelte code.
Existing Qt controls use Qt styling; vanilla CSS applies to Svelte surfaces.
The harness command bridge still needs to be designed and tested.
Qt controls require native application testing; browser tests apply to web UI surfaces.
Fork maintenance, upstream licence obligations, packaging, and cross-platform checks remain necessary.

This is a foundation decision, not a claim that the fork has been built or qualified.
No upstream source has been imported yet.
Pin and build a stable baseline on macOS and Windows before changing its behavior.
The animation backend, content, and visual style remain open; current animation scope is 2D.

## Evidence

The creator explicitly selected: "Keep and restyle Qt controls; use Rust and Svelte where useful".
See [the source-based research](../editor-and-animation-research.md) for the editor comparison and UI reuse details.
