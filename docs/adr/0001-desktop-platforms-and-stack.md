# 0001: Desktop platforms and application stack

Date: 2026-11-06

Status: Accepted for the platform targets and requested application stack; the main editor UI boundary is under discussion.

## Context

The creator wants an installed desktop application for macOS and Windows.
The creator has chosen Rust, Tauri, Svelte 5, and vanilla CSS.

## Decision

- Target macOS and Windows as first-class desktop platforms.
- Use Rust for new backend and harness code.
- Use Tauri for the planned desktop shell and Svelte 5 with vanilla CSS for its web UI.
- Keep the current animation scope to 2D; the earlier 3D vision belongs to the later production system.
- Keep video style and content open.

The creator subsequently asked about retaining and restyling an upstream editor's controls.
This is being evaluated; it has not yet replaced the chosen stack.
A Qt-based editor would require an explicit update to the UI boundary in this decision.

## Alternatives

Separate native macOS and Windows UI implementations were not chosen; the creator specified a shared stack.
A complete Qt editor fork is now under discussion because it could preserve existing editor behavior.
An engine integration under a custom Svelte UI remains the path that follows the originally chosen stack throughout the editor UI.

## Consequences

Tauri uses a system webview for the Svelte UI; it does not convert Svelte into AppKit or WinUI controls.
Native media work can run outside the webview.
Third-party engines may be written in C or C++; a Rust adapter does not rewrite them.
Exact versions, supported OS versions, CPU architectures, and distribution packaging remain to be set during implementation.

The editor foundation and animation backend are still unselected.
See [the research](../editor-and-animation-research.md) for evidence and proposed paths.

Source: [Tauri webview documentation](https://v2.tauri.app/reference/webview-versions/).
