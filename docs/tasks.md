# Tasks

## Current state

Planning repository created on 2026-11-06. No application code exists.
macOS and Windows are selected.
Keep and restyle Kdenlive's Qt/C++ editor, with Rust and Svelte 5 plus vanilla CSS where useful.
Tauri can host a separate workspace if needed.
Unchanged Kdenlive 26.08.3 is built on Windows outside the repository with KDE Craft ([baseline](kdenlive-baseline.md)); no source is imported into this repository yet.

## Completed

- [x] Capture the larger YouTube production vision.
- [x] Capture the editor, editing harness, and SVG generator requirements.
- [x] Add a short shared agent guide and engineering standards.
- [x] Define browser, end-to-end, profiling, and deliberate mistake checks.
- [x] Outline the roadmap without choosing content, style, or upstream editor.
- [x] Record the chosen platforms and application stack in ADR 0001.
- [x] Research editor foundations and 2D animation options using primary sources.
- [x] Document how retaining and restyling Qt controls differs from rebuilding the editor UI in Svelte.
- [x] Record the creator's choice to retain Kdenlive's Qt controls in ADR 0002.
- [x] Capture channel management: publishing, analytics, and learning.
- [x] Study Bloop's and Jello's harnesses in `docs/harness-study.md`.
- [x] Record one harness with creator approval for every public channel action in ADR 0003.

## Next

- [x] Choose the harness foundation: a TypeScript pi fork with Rust tools (ADR 0004).
- [x] Choose model access: pi's own sign-in, as in Jello (ADR 0005).
- [x] Choose jobs: long-running chats, with Bloop's loop and memory (ADR 0006).
- [x] Choose one Video chat per video (ADR 0006).
- [x] Choose the first channel functions: upload and learn (project brief).
- [x] Choose grade checkpoints and metrics: 7 and 28 days, five metrics (ADR 0006).
- [x] Place upload and learn as roadmap step 4, after the first editing path.
- [x] Choose the store: one SQLite file per channel, owned by Rust (ADR 0007).
- [x] Choose the first test video's shape: a 60 to 90 second narrated explainer in 16:9 ([first workflow](first-workflow.md)). Superseded on 2026-11-13.
- [x] Choose the first workflow's inputs: all generated, with text-to-speech narration. Superseded on 2026-11-13.
- [x] Choose the narration engine: Kokoro-82M, local.
- [x] Choose the first test video's edit: a fixed edit list ([first workflow](first-workflow.md#the-edit)).
- [x] Make narration a harness feature, Kokoro first, in a pinned Python worker (ADR 0008).
- [x] Choose the first workflow's code checks: exact checks against the edit list ([first workflow](first-workflow.md#acceptance-checks)).
- [x] Choose the creator review: a checklist in the app ([first workflow](first-workflow.md#creator-review)).
- [x] Choose the harness role: build by hand in step 2, then by the harness in step 3 ([first workflow](first-workflow.md#two-builds)).
- [x] Choose where the first workflow ends: export, then a private test upload in step 4.
- [x] Choose the channel: cinematic 2D animated stories set to the creator's Jello songs ([project brief](project-brief.md#channel-direction)).
- [x] Decide whether the first workflow changes to fit the channel: yes, an animated piece set to one full Jello song.
- [x] Redecide the first workflow for the channel: full Jello song, generated test scenes, no cut rule, Jello's level kept ([first workflow](first-workflow.md)).
- [ ] Make the test song in Jello (the creator), then fill exact times into the test edit list.
- [ ] Measure that a Jello render's beats land where the song file predicts.
- [ ] Fork and trim pi at a pinned commit, keeping its sign-in code.
- [ ] Measure the narration worker's size and speed on macOS and Windows before building it.
- [x] Pin and build an unchanged Kdenlive 26.08.3 baseline on Windows; it launches and passes its 36 upstream tests ([baseline](kdenlive-baseline.md)).
- [ ] Build the same baseline on macOS. Blocked: no Mac yet.
- [ ] Investigate MLT's `mltopenfx.dll` registration warning on Windows.
- [ ] Qualify the baseline against the first workflow and measure performance.
- [ ] Prove a harness command bridge that preserves the editor's project state and undo.
- [ ] Pin stack versions and document setup, test, lint, and profiling commands.
- [ ] Build and verify the first complete editing workflow, with the harness core.
- [ ] Build upload and learn: channel gateway, approvals, private upload, analytics, and grades.
- [ ] Generate and animate one SVG asset in a rendered video.

## Open decisions

The editor foundation and retained Qt UI are decided; do not ask the creator to choose them again.
The first workflow is defined in [the first workflow](first-workflow.md); do not ask its questions again.
The animation engine, process bridge, any separate production workspace, and asset storage remain open.
The channel is cinematic 2D animated stories set to the creator's Jello songs ([project brief](project-brief.md#channel-direction)).
Its visual style, video length, and format remain open. The first workflow is a song-driven piece set to one full Jello song.
Current animation work is limited to 2D.
Channel autonomy and one shared harness are decided in ADR 0003; do not ask again.
The harness foundation is decided in ADR 0004, model access in ADR 0005, and the chats, one per video, in ADR 0006. The first channel functions are in the project brief. Grade checkpoints and metrics are in ADR 0006, upload and learn is roadmap step 4, and the store is in ADR 0007. The harness study's open questions are all answered ([harness study](harness-study.md#open-questions)).

## Verification

Initial checks on 2026-11-06 passed: 11 Markdown files, 12 local links, no em dashes, and a session guide under 60 lines. The staged Git diff was checked for whitespace errors before the initial commit.

Application tests, browser checks, profiling, and mutation checks do not apply yet because no executable code exists.

The editor and animation recommendations are based on documentation and source inspection only.
No candidate has been built, exercised end to end, or profiled for this project.

Research update checks on 2026-11-06 passed: 13 Markdown files, 21 local links, no em dashes, no stale claims that the stack is undecided, and a 37-line session guide.
The staged diff was checked for whitespace errors before committing.

Qt decision update checks on 2026-11-06 passed: 14 Markdown files, 29 local links, and a 38-line session guide.
No stale statements leave the editor UI choice open; no executable code changed.

Harness study and ADR 0003 checks on 2026-11-12 passed: 16 Markdown files, 40 local links and anchors, no em or en dashes, and a 41-line session guide.
The YouTube facts in the harness study were read from Google's docs that day; no API call was made.
After the upload-and-learn scope and analytics facts, the same checks passed with 19 Markdown files, 66 local links, and a 43-line session guide.
After the grade, roadmap, and store decisions, they passed with 20 Markdown files, 76 local links, and a 44-line session guide.
After the first workflow's shape, inputs, edit, and the narration feature, they passed with 22 Markdown files, 92 local links, and a 46-line session guide.
After the first workflow was fully defined, they passed with 22 Markdown files, 97 local links, and a 46-line session guide.
After the channel direction, the song-driven first workflow, and the Windows Kdenlive baseline on 2026-11-13, they passed with 23 Markdown files, 111 local links, and a 46-line session guide.
