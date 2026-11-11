# youtube_suite

A reproducible system for making YouTube videos, from ideas and direction to editing, animation, 3D work, and final delivery.

The video editor is one part of this larger system, much like Jello is for music. The aim is professional editing with our own UI and an agent harness that turns a user's instructions into precise, reviewable edits.

Status: planning repository. No editor, generator, harness, or rendering pipeline is implemented yet.

Targets: macOS and Windows. Keep and restyle Kdenlive's Qt/C++ editor, with a TypeScript fork of pi for the harness, Rust for its tools and gateways, and Svelte 5 with vanilla CSS where useful. Tauri can host a separate production workspace if needed. Current animation scope is 2D; video style, content, and animation backend remain open.

Kdenlive is selected but has not been imported, built, or qualified yet. See [the editor decision](docs/adr/0002-retain-kdenlive-editor-ui.md).

Start with [the project brief](docs/project-brief.md), [the roadmap](docs/roadmap.md), and [current tasks](docs/tasks.md).

Read [the editor and animation research](docs/editor-and-animation-research.md) for the shortlist and UI reuse tradeoff.

Agents must read [CLAUDE.md](CLAUDE.md). Detailed project documentation lives in [docs/](docs/README.md).
