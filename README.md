# youtube_suite

A reproducible system for making YouTube videos, from ideas and direction to editing, animation, 3D work, and final delivery.

The video editor is one part of this larger system, much like Jello is for music. The aim is professional editing with our own UI and an agent harness that turns a user's instructions into precise, reviewable edits.

Status: planning repository. No editor, generator, harness, or rendering pipeline is implemented yet.

Targets: macOS and Windows. Chosen stack: Rust, Tauri, Svelte 5, and vanilla CSS. Current animation scope is 2D. The option to keep and restyle an upstream Qt editor UI is under discussion; video style, content, and editor foundation remain open.

Start with [the project brief](docs/project-brief.md), [the roadmap](docs/roadmap.md), and [current tasks](docs/tasks.md).

Read [the editor and animation research](docs/editor-and-animation-research.md) for the shortlist and UI reuse tradeoff.

Agents must read [CLAUDE.md](CLAUDE.md). Detailed project documentation lives in [docs/](docs/README.md).
