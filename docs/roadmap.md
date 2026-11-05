# Roadmap

## 1. Define the first workflow

Choose one short video task with explicit inputs and acceptance criteria. Keep the channel's long-term content and style open. Resolve human decisions one topic at a time.

## 2. Qualify the selected editor foundation

The source-based shortlist is in [the research](editor-and-animation-research.md).
The creator selected retaining and restyling Kdenlive's Qt UI in [ADR 0002](adr/0002-retain-kdenlive-editor-ui.md).
Pin and build an unchanged stable release on macOS and Windows.
Use the first workflow to prove editing, preview, save/load, export, and the feasibility of a harness command bridge.
Record feature coverage, performance, and fork costs.

## 3. Build one complete editing path

Restyle the retained editor controls and connect the editing harness to the editor's project and undo operations.
Support an instruction, a visible timeline change, preview, undo, save/reopen, and final export. Add checks and profiling before expanding scope.

## 4. Add SVG assets and animation

Generate an editable SVG, animate named elements using a saved style pack, place it on the timeline, and verify the final render.
Current animation scope is 2D only. Save enough inputs and metadata to reproduce the accepted result.

## 5. Grow professional editing coverage

Expand features against a tracked upstream capability matrix. Improve performance and reliability with representative production projects.

## 6. Connect the wider production system

Add repeatable direction, writing, sound, 3D, animation, and review workflows as real videos need them. Decide channel style and content with the creator. Add publishing and feedback workflows when requested.
