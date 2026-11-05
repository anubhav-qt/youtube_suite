# Proposed architecture

This is a planning outline, not an adopted implementation or an ADR. Choose concrete interfaces after the first workflow and upstream evaluation.

## Parts

| Part | Responsibility |
| --- | --- |
| Production project | Store the brief, scripts, asset references, timeline, tool versions, render settings, and review history. |
| Editor UI | Let the creator inspect, edit, preview, compare, and undo work. |
| Editing harness | Convert instructions into a reviewable plan and validated project operations. |
| Editor adapter | Map those operations to the selected editor or media engine. |
| Asset generator | Create versioned SVG assets and metadata for animation. |
| Animation and 3D workflows | Produce reusable scenes and motion with saved parameters and tool versions. |
| Render pipeline | Produce previews and final videos from saved project state. |
| Verification | Check project validity, output properties, regressions, and performance. |

## Intended flow

Brief -> plan -> assets and scenes -> timeline edits -> preview -> human review -> final render.

The UI and harness should share one project model. A harness operation should identify its target, inputs, expected preconditions, and result. Apply edits transactionally or preserve a recoverable project version before changing state.

## Reproducibility

- Use stable asset and clip identifiers and an explicit timeline timebase.
- Record source hashes, tool versions, fonts, codecs, render settings, and applicable random seeds.
- Save generated assets and the inputs that produced them.
- Distinguish repeatable project operations from exact byte-for-byte renders, which may depend on platform and codec behavior.
- Keep large source media and generated renders outside Git; version their references and provenance.
- Make long renders resumable or safely restartable where supported.

## Editor foundation evaluation

Evaluate candidates with the same small real editing task. Record maintenance evidence, usage evidence, licence obligations, platform support, project format, programmatic control, custom UI feasibility, supported features, render quality, performance, and the cost of maintaining a fork.

Decide between a fork, an integration, or an engine adapter only after the spike. Record that major decision in an ADR.
