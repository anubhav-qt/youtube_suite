# Project brief

## Goal

Build a system that helps the creator become a good YouTuber through repeatable production workflows. The system should support ideas, research, writing, direction, assets, animation, 3D modelling, editing, sound, review, export, and later publishing and learning from results.

The style and subject of the videos are intentionally undecided. Early technical choices should leave room for them.

## Chosen platforms and stack

Build an installed desktop application for macOS and Windows using Rust, Tauri, Svelte 5, and vanilla CSS.
See [ADR 0001](adr/0001-desktop-platforms-and-stack.md).
The option to retain and restyle an upstream Qt editor UI is being evaluated; it requires a separate decision about the main editor UI.

The current animation scope is 2D only.
The earlier 3D ambition remains part of the later production system.

## Video editing suite

Create a professional video editing suite with our own UI and an agent harness. Evaluate whether to fork or integrate a mature, actively maintained, widely used open-source editor. No upstream has been chosen.

The long-term ambition is broad professional feature coverage. Before promising feature parity, inventory the chosen upstream's features and test which ones our UI and harness can actually expose.

Expected areas include multitrack editing, precise cuts, trimming, transitions, keyframes, compositing, titles, captions, audio mixing, colour adjustments, proxies, preview, and export. These are evaluation requirements, not implemented features.

The harness should translate a user's request into an explicit edit plan, apply it through structured operations, show the result, and support revision or undo. For ambiguous requests, ask about one topic at a time. Exact timing and technical constraints should be verified automatically; creative choices require human review.

## Animation asset generator

Generate editable SVG assets for animated video elements such as diagrams, labels, icons, characters, and explainers. Preserve useful groups, stable element IDs, layers, dimensions, colours, and text choices so animation tools can address them reliably.

Store generation inputs, asset versions, and animation instructions. Preview assets in context and check rendering at the intended export size. SVG generation and animation playback are separate responsibilities; the chosen renderer must support the intended effects.

Support a saved style pack with approved references and explicit visual and motion rules.
Keep assets, scene timing, animation controls, and rendered output separate so the creator can revise them.
Allow raster layers or drawn frames if the chosen 2D style needs them.
See [the animation research](editor-and-animation-research.md) for candidate tools and the proposed workflow.

## Reproducible production

Treat direction, editing, asset generation, animation, and 3D work as reusable systems. Each video should have a versioned brief, source assets, scripts or structured instructions, tool versions, render settings, and review history.

Reuse accepted generated assets on reruns. A model seed alone is not a guarantee that a later generation will produce the same asset.

## Success criteria

- A small complete workflow can produce a video from a saved brief.
- The creator can inspect and change creative decisions without losing the project.
- A saved project can be rendered again with pinned inputs and tools.
- The UI and harness operate on the same editable project state.
- Tests verify technical requirements; the creator reviews creative quality.
- The system can grow to support animation and 3D without rebuilding every workflow.
