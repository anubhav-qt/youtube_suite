# Roadmap

## 1. Define the first workflow

Choose one short video task with explicit inputs and acceptance criteria. Keep the channel's long-term content and style open. Resolve human decisions one topic at a time.
Done on 2026-11-12: a 60 to 90 second narrated explainer in 16:9 with generated inputs, a fixed edit list, exact checks, and a review checklist ([first workflow](first-workflow.md)).

## 2. Qualify the selected editor foundation

The source-based shortlist is in [the research](editor-and-animation-research.md).
The creator selected retaining and restyling Kdenlive's Qt UI in [ADR 0002](adr/0002-retain-kdenlive-editor-ui.md).
Pin and build an unchanged stable release on macOS and Windows.
Use the first workflow to prove editing, preview, save/load, export, and the feasibility of a harness command bridge.
Build the first workflow's edit list by hand here; its export must pass every check ([first workflow](first-workflow.md#two-builds)).
Record feature coverage, performance, and fork costs.

## 3. Build one complete editing path

Restyle the retained editor controls and connect the editing harness to the editor's project and undo operations.
Build the harness core here, since editing needs it: the pi fork, the store, the scheduler, and records.
The Video chat then builds the same first workflow from one instruction, and its export must pass the same checks.
Support an instruction, a visible timeline change, preview, undo, save/reopen, and final export. Add checks and profiling before expanding scope.

## 4. Upload and learn

Add the channel gateway, approvals, private upload with metadata, analytics, grades, and lessons ([project brief](project-brief.md#channel-management)).
Create the YouTube reach reporting job before the first upload, so click-through rate is collected from the start.
Publish soon after the first full edit, because grades need 10 videos of a format before comparisons mean anything.
The first test is a private upload of the first workflow's export. It never goes public and never counts in a baseline.

## 5. Add SVG assets and animation

Generate an editable SVG, animate named elements using a saved style pack, place it on the timeline, and verify the final render.
Current animation scope is 2D only. Save enough inputs and metadata to reproduce the accepted result.

## 6. Grow professional editing coverage

Expand features against a tracked upstream capability matrix. Improve performance and reliability with representative production projects.

## 7. Connect the wider production system

Add repeatable direction, writing, sound, 3D, animation, and review workflows as real videos need them. Decide channel style and content with the creator.

## Channel management

The creator asked for publishing, analytics, and learning on 2026-11-12.
One harness serves it and editing, and every public action waits for approval ([ADR 0003](adr/0003-one-harness-with-channel-approvals.md)).
It is step 4, after the first complete editing path (the creator, 2026-11-12). Later channel functions follow as grades show what is worth adding.
