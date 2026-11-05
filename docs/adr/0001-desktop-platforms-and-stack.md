# 0001: Desktop platforms and application stack

Date: 2026-11-06

Status: Accepted; the editor UI boundary is amended by [ADR 0002](0002-retain-kdenlive-editor-ui.md).

## Context

The creator wants an installed desktop application for macOS and Windows.
The creator has chosen Rust, Tauri, Svelte 5, and vanilla CSS.

## Decision

- Target macOS and Windows as first-class desktop platforms.
- Use Rust for new backend and harness code.
- Use Svelte 5 with vanilla CSS for useful new web UI surfaces; Tauri can host a separate desktop workspace when needed.
- Keep the current animation scope to 2D; the earlier 3D vision belongs to the later production system.
- Keep video style and content open.

The creator subsequently chose to retain and restyle Kdenlive's Qt editor controls.
ADR 0002 records that decision; the editor itself does not require a Tauri shell or a Svelte rewrite.

## Alternatives

Separate native macOS and Windows UI implementations were not chosen; the creator specified a shared stack.
A complete Qt editor fork was selected to preserve existing editor behavior.
An engine integration under a custom Svelte UI was considered but would require rebuilding more controls.

## Consequences

Tauri uses a system webview for the Svelte UI; it does not convert Svelte into AppKit or WinUI controls.
Native media work can run outside the webview.
Third-party engines may be written in C or C++; a Rust adapter does not rewrite them.
Exact versions, supported OS versions, CPU architectures, and distribution packaging remain to be set during implementation.

Kdenlive is the selected editor foundation; the animation backend remains unselected.
See [the research](../editor-and-animation-research.md) for evidence and proposed paths.

Source: [Tauri webview documentation](https://v2.tauri.app/reference/webview-versions/).
