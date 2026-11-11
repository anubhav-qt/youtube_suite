# Project brief

## Goal

Build a system that helps the creator become a good YouTuber through repeatable production workflows. The system should support ideas, research, writing, direction, assets, animation, 3D modelling, editing, sound, review, export, and later publishing and learning from results.

The style and subject of the videos are intentionally undecided. Early technical choices should leave room for them.

## Chosen platforms and stack

Build an installed desktop application for macOS and Windows by retaining and restyling Kdenlive's Qt/C++ editor.
The harness is a TypeScript fork of pi, with tools and gateways in Rust ([ADR 0004](adr/0004-harness-on-a-typescript-pi-fork.md)). Use Svelte 5 with vanilla CSS where useful; use Tauri for a separate production workspace only if needed.
See [ADR 0001](adr/0001-desktop-platforms-and-stack.md) and [ADR 0002](adr/0002-retain-kdenlive-editor-ui.md).

The current animation scope is 2D only.
The earlier 3D ambition remains part of the later production system.

## Video editing suite

Create a professional video editing suite with our own appearance and workflows, using Kdenlive as the selected editor foundation.
Retain its existing controls and editing behavior, then add an agent harness.
No source has been imported or built yet.

The long-term ambition is broad professional feature coverage. Before promising feature parity, inventory the chosen upstream's features and test which ones our UI and harness can actually expose.

Expected areas include multitrack editing, precise cuts, trimming, transitions, keyframes, compositing, titles, captions, audio mixing, colour adjustments, proxies, preview, and export. These are evaluation requirements, not implemented features.

The harness should translate a user's request into an explicit edit plan, apply it through structured operations, show the result, and support revision or undo. For ambiguous requests, ask about one topic at a time. Exact timing and technical constraints should be verified automatically; creative choices require human review.

## Narration

Narration is a harness feature: someone who wants a narrated video can do everything through the harness ([ADR 0008](adr/0008-narration-through-the-harness.md)).
The harness writes the script, makes speech with a text-to-speech engine, places it on the timeline, and cuts pictures to its sentences.
Kokoro-82M, running locally, is the first engine. More engines come later, each chosen with the creator.
Each narration is a versioned asset with its script, engine, voice, and settings, and the creator hears it before it reaches viewers.

## Animation asset generator

Generate editable SVG assets for animated video elements such as diagrams, labels, icons, characters, and explainers. Preserve useful groups, stable element IDs, layers, dimensions, colours, and text choices so animation tools can address them reliably.

Store generation inputs, asset versions, and animation instructions. Preview assets in context and check rendering at the intended export size. SVG generation and animation playback are separate responsibilities; the chosen renderer must support the intended effects.

Support a saved style pack with approved references and explicit visual and motion rules.
Keep assets, scene timing, animation controls, and rendered output separate so the creator can revise them.
Allow raster layers or drawn frames if the chosen 2D style needs them.
See [the animation research](editor-and-animation-research.md) for candidate tools and the proposed workflow.

## Channel management

The system runs the creator's YouTube channel end to end: publishing, analytics, and learning from results.
One harness does this and the editing work ([ADR 0003](adr/0003-one-harness-with-channel-approvals.md)).

- **Publishing.** Prepare and upload each video privately with its title, description, tags, chapters, captions, thumbnail, and playlists.
- **Analytics.** Collect each video's and the channel's results on a schedule and keep them over time.
- **Learning.** Grade each video against its brief and production choices, keep lessons, and feed them into the next brief.
- **Approvals.** Anything viewers can see, or that cannot be undone, waits for the creator's approval.

**First version: upload and learn** (the creator, 2026-11-12).

- In: private upload with title, description, tags, chapters, captions, and thumbnail.
- In: the creator approves going public, now or on a schedule.
- In: analytics, grades, lessons, and the Analyst's patterns.
- Later: comments, community posts, playlists, channel settings, and changes to videos already public.
- Next after that: changing a public video's title or thumbnail, once grades exist to judge it.
See [the harness study](harness-study.md) for the design and its open questions.

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
- Nothing reaches viewers without the creator's approval.
- Each published video's results are linked back to the choices that made it.
