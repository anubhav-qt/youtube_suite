# Editor and 2D animation research

Research date: 2026-11-06.

This is a source-based comparison, not a completed build, benchmark, or production qualification.
Recommendations below are engineering judgments; no editor or animation engine has been adopted yet.

## Recommendation

Kdenlive is the strongest complete editor candidate for this project's needs.
MLT is the first engine to test if the complete editing UI must be written in Svelte.
Keeping and restyling Kdenlive's existing Qt UI is a valid alternative that preserves much more finished behavior.
The choice between those paths is still open.

For 2D animation, test Glaxnimate first for the SVG and vector workflow.
Evaluate Friction for richer motion design, Synfig for puppet animation, and OpenToonz for drawn animation if the chosen style needs them.
Do not integrate all four before there is a real need.

None of this research establishes complete Resolve or Premiere feature parity.
Professional output is possible without matching every feature; reliability and performance still need hands-on proof.

## Editor comparison

| Candidate | Evidence and strengths | Fit for this project |
| --- | --- | --- |
| Kdenlive | Multitrack workflows, nested timelines, titles, subtitles, and Glaxnimate integration; Qt/C++ application using MLT. | Best complete-app fork candidate; keeping its UI saves substantial work. |
| Shotcut | MLT-based Qt application with multitrack editing, keyframes, proxies, 10-bit processing, HDR, and audio tools. | Strong second candidate and an important reference for MLT integration and colour processing. |
| OpenShot / libopenshot | A separate C++ editing library with compositing, animation curves, time mapping, and language bindings. | Worth a small API comparison; documentation alone does not prove it is the strongest professional editing foundation. |
| Olive | Cross-platform editor whose own README labels it highly unstable alpha software. | Do not use as the production baseline at this stage. |

Sources: [Kdenlive introduction](https://docs.kdenlive.org/en/getting_started/introduction.html), [Kdenlive source README](https://raw.githubusercontent.com/KDE/kdenlive/master/README.md), [Shotcut features](https://shotcut.org/features/), [Shotcut source](https://github.com/mltframework/shotcut), [libopenshot](https://www.openshot.org/libopenshot/), and [Olive README](https://github.com/olive-editor/olive/blob/master/README.md).

## Maintenance and usage evidence

- Kdenlive released 26.08.2 on 2026-10-13; its 2026 report records 38 code contributors and 11,500,714 website downloads during 2025.
- Downloads show reach, not unique active users or production reliability.
- Shotcut published 26.9.27 on 2026-09-27.
- MLT published 7.42.0 on 2026-10-03.
- Glaxnimate published stable 0.6.0 on 2026-03-03.
- Friction's download page currently offers 1.0.0-rc.3, a release candidate that needs qualification.
- Synfig lists stable 1.4.5 and development 1.5.5; keep that distinction when testing.
- OpenToonz has a 1.8 release; do not rely on older search results that still call 1.7.1 current.

Sources: [Kdenlive release](https://kdenlive.org/news/releases/26.08.2/), [Kdenlive annual report](https://kdenlive.org/news/2026/state-2026/), [Shotcut news](https://shotcut.org/blog/), [MLT releases](https://www.mltframework.org/), [Glaxnimate release](https://glaxnimate.org/news/releases/0.6.0/), [Friction downloads](https://friction.graphics/download.html), [Synfig releases](https://www.synfig.org/category/releases/), and [OpenToonz 1.8](https://github.com/opentoonz/opentoonz/releases/tag/v1.8.0).

## Keeping and restyling the existing editor UI

Yes, this avoids rebuilding the editing UI from scratch.
Kdenlive already supports rearranging, showing, hiding, and saving panels as workspace layouts.
Its source uses Qt Widgets and a Qt Quick timeline; these are not Svelte components.
[Workspace customization](https://docs.kdenlive.org/en/user_interface/customizing_interface.html), [main window source](https://github.com/KDE/kdenlive/blob/master/src/mainwindow.cpp), and [timeline widget source](https://github.com/KDE/kdenlive/blob/master/src/timeline2/view/timelinewidget.h).

A sensible fork plan would be:

1. Build a pinned stable release unchanged on macOS and Windows.
2. Keep the timeline, preview monitors, effect controls, project model, and undo system.
3. Change the branding, colours, icons, spacing, menus, and default workspace.
4. Hide unwanted tools first; deleting their implementation can break saved projects or shared dependencies.
5. Add an explicit command bridge for the Rust harness, using the same edit and undo paths as the UI.
6. Keep the fork's changes small and regularly merge upstream fixes.

Basic appearance changes use Qt themes and styling; deeper changes require QML or C++ work.
Vanilla CSS would apply to any Svelte surfaces, not to the existing Qt controls.
Restyling preserves features but still needs regression tests, packaging work, and fork maintenance.
Do not assume Kdenlive exposes a complete stable automation API; prove or add the required commands.

## The stack tradeoff

| Path | What is reused | What must be built |
| --- | --- | --- |
| Kdenlive fork with Qt UI | Existing editor controls and their behavior, project handling, and engine integration. | Restyling, chosen product changes, Rust command bridge, harness, and reliable packaging. |
| Tauri + Svelte editor over MLT | Media processing, composition, effects, and render services exposed by MLT. | Timeline interactions, tool panels, project semantics, undo, preview integration, and harness. |
| Tauri/Svelte production hub plus a Qt editor process | Existing Qt editor and a new web-based production workspace. | Process communication, clear ownership of open projects, and a coherent two-window workflow. |

These are proposed designs, not proven integrations.
Embedding arbitrary Qt widgets directly inside a Tauri webview is not a simple component import.
Avoid promising a single seamless window until a cross-platform prototype proves it.
Tauri supports bundled sidecar executables, but that does not automatically merge their UIs.
[Tauri sidecar documentation](https://v2.tauri.app/develop/sidecar/).

The user's chosen application stack is Rust, Tauri, Svelte 5, and vanilla CSS.
The user has asked whether preserving upstream UI can reduce rebuilding; relaxing the main editor UI requirement remains a separate decision.
The native media dependencies discussed here are C/C++ libraries, even when controlled by Rust.

## MLT integration risks to prove

MLT offers a C API, multitrack composition, project serialization, keyframes, and FFmpeg integration.
Its current documentation lists 10-bit processing and GPU paths; old blanket claims that MLT is only 8-bit or CPU-only are outdated.
These capabilities do not prove every effect or operating system follows the same accelerated path.
[MLT features](https://www.mltframework.org/features/).

For a Svelte implementation, test a narrow Rust adapter around MLT's C API or a native worker process.
Separate small control messages from large video frames; profile the actual preview transport before choosing it.
Keep decoding, composition, and export off the UI thread.
Check Windows native build compatibility and macOS packaging early.

Tauri uses WebView2 on Windows and WKWebView on macOS.
Therefore browser rendering, fonts, colour presentation, and available graphics features require checks on both systems.
[Tauri webview documentation](https://v2.tauri.app/reference/webview-versions/).

## 2D animation candidates

| Candidate | Best reason to evaluate it | Main limit for our integration |
| --- | --- | --- |
| Glaxnimate | Editable vectors, tweening, SVG/Lottie workflows, Python scripting, and existing MLT integration. | Test format coverage, text, masks, and effects; integration into our UI is still work. |
| Friction | Vector and bitmap motion graphics with curves, expressions, morphing, and effects. | Current offered build is a release candidate; SVG export supports only part of its full feature set. |
| Synfig | Vector morphing, bones, cutout characters, and deformation. | Separate authoring model; evaluate only when character rigs are needed. |
| OpenToonz | Traditional drawn animation, ink and paint, and compositing with production roots in Toonz. | A substantial separate application; not a ready-made Svelte animation panel. |
| ThorVG | Native SVG/Lottie renderer with C bindings and a web build. | A renderer, not a complete animation editor or rigging system. |
| Motion Canvas | Scripted diagrams and explainers timed to narration. | TypeScript animation library and editor; it adds a separate authoring/runtime approach. |

Sources: [Glaxnimate](https://glaxnimate.org/), [Glaxnimate scripting](https://glaxnimate.org/contributing/scripting/), [MLT animation producer](https://www.mltframework.org/plugins/ProducerGlaxnimate/), [Friction FAQ](https://friction.graphics/documentation/faq.html), [Friction export limits](https://friction.graphics/documentation/export.html), [Synfig](https://www.synfig.org/), [OpenToonz](https://opentoonz.github.io/e/index.html), [ThorVG](https://github.com/thorvg/thorvg), and [Motion Canvas](https://motioncanvas.io/docs/).

## Making animation follow the chosen style

The following is our proposed workflow, not an existing capability inherited from any one library.

1. Save a versioned style pack with approved reference frames, colours, fonts, line widths, shapes, textures, and character proportions.
2. Save motion rules too: pacing, easing, holds, overshoot, transition choices, and rules for camera movement within a 2D scene.
3. Generate editable assets with named layers, stable IDs, pivots, and reusable character parts where needed.
4. Store scene timing, layers, keyframes, masks, paths, and narration cues in an editable animation document.
5. Let the harness change named objects and parameters through validated commands, with undo and a visible preview.
6. Render at explicit frame times, cache accepted outputs, and place the animation on the video timeline.
7. Check style rules automatically where possible; use human review for composition, acting, rhythm, and overall quality.

SVG is the asset format for suitable vector artwork; it is not the whole animation system.
Drawn or textured styles may need raster layers and image sequences as well.
Character animation may need rigs, poses, mouth shapes, or drawn frames that a static SVG does not supply.
Do not force the user's eventual style into a renderer's narrow feature subset.

Preserve native animation documents alongside rendered outputs.
For unsupported live effects, render transparent image sequences and composite those into the video.
Make transparency, colour space, frame rate, and duration explicit.
Use the same rendering implementation for preview and export where practical; compare frames if different implementations are necessary.
Never rely on real-time browser animation timers to define final frame timing.

## Licensing facts that affect reuse

Kdenlive and Shotcut use GPL licences; copying and distributing their code carries licence obligations.
MLT's framework is LGPL-2.1, while individual tools and modules can use different licences.
In particular, MLT's Glaxnimate producer is GPLv3.
Glaxnimate is GPLv3+; Friction is GPL-3.0-only; Synfig states GPLv3.
ThorVG uses MIT; OpenToonz describes its source licence as New BSD.
libopenshot offers LGPLv3 or commercial licensing and explicitly calls out separate dependency licences.
Review the exact bundled code and build configuration before deciding our distribution licence.
A separate process is an architectural choice, not a blanket exemption from licence terms.

Sources: [Kdenlive source licence header](https://github.com/KDE/kdenlive/blob/master/src/mainwindow.cpp), [Shotcut licence](https://github.com/mltframework/shotcut), [MLT policy](https://raw.githubusercontent.com/mltframework/mlt_web/master/CopyrightPolicy.md), [MLT Glaxnimate module](https://www.mltframework.org/plugins/ProducerGlaxnimate/), [Glaxnimate licence](https://glaxnimate.org/contributing/introduction/), [Friction licence](https://raw.githubusercontent.com/friction2d/friction/main/README.md), [Synfig licence](https://www.synfig.org/), [ThorVG licence](https://github.com/thorvg/thorvg/blob/main/LICENSE), [OpenToonz licence](https://opentoonz.github.io/e/index.html), and [libopenshot licensing](https://www.openshot.org/libopenshot/).

## Qualification before adopting a foundation

Use one saved 30-second test project with two video clips, narration, music, captions, a transition, and an animated SVG overlay.
This is a proposed engineering fixture, not a channel style decision.

- Build and run on macOS and Windows with pinned versions.
- Verify trim, seek, ripple edit, undo/redo, save/reopen, missing media, and export cancellation.
- Render a baseline and check frame count, timing, audio sync, colour, fonts, and transparent edges.
- Repeat with 4K and 10-bit sources; check actual processing rather than trusting the input format.
- Measure preview responsiveness, dropped frames, export time, and peak memory on recorded hardware.
- Test the same harness edit through structured commands and the visible UI.
- Test animation masks, morphs, text, layer order, and narration cues against expected frames.
- Introduce wrong timing, skipped validation, and flipped conditions one at a time; require tests to fail, then restore clean code.

No part of this qualification has been run yet.
The next human decision concerns retaining the Qt editor UI versus requiring the entire editor UI to use Svelte.
