# Tasks

## Current state

Planning repository created on 2026-11-06. No application code exists.
macOS and Windows are selected.
Keep and restyle Kdenlive's Qt/C++ editor, with Rust and Svelte 5 plus vanilla CSS where useful.
Tauri can host a separate workspace if needed; no source has been imported or built yet.

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

## Next

- [ ] Define one short video workflow and measurable acceptance criteria.
- [ ] Pin and build an unchanged Kdenlive stable baseline on macOS and Windows.
- [ ] Qualify the baseline against the first workflow and measure performance.
- [ ] Prove a harness command bridge that preserves the editor's project state and undo.
- [ ] Pin stack versions and document setup, test, lint, and profiling commands.
- [ ] Build and verify the first complete editing workflow.
- [ ] Generate and animate one SVG asset in a rendered video.

## Open decisions

The editor foundation and retained Qt UI are decided; do not ask the creator to choose them again.
Define the first test workflow next, one topic at a time.
The animation engine, process bridge, any separate production workspace, model providers, and asset storage remain open.
Channel style and content remain deferred; current animation work is limited to 2D.

## Verification

Initial checks on 2026-11-06 passed: 11 Markdown files, 12 local links, no em dashes, and a session guide under 60 lines. The staged Git diff was checked for whitespace errors before the initial commit.

Application tests, browser checks, profiling, and mutation checks do not apply yet because no executable code exists.

The editor and animation recommendations are based on documentation and source inspection only.
No candidate has been built, exercised end to end, or profiled for this project.

Research update checks on 2026-11-06 passed: 13 Markdown files, 21 local links, no em dashes, no stale claims that the stack is undecided, and a 37-line session guide.
The staged diff was checked for whitespace errors before committing.

Qt decision update checks on 2026-11-06 passed: 14 Markdown files, 29 local links, and a 38-line session guide.
No stale statements leave the editor UI choice open; no executable code changed.
