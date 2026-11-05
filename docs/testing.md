# Testing and profiling standards

## Test real behavior

Use focused unit tests for rules, integration tests for adapters and persistence, and end-to-end tests for full user workflows. Assert visible outcomes and output properties rather than duplicating implementation details.

For media workflows, inspect actual rendered output. Cover timing, frame rate, dimensions, duration, audio presence and sync, missing media, save/reopen, and undo when relevant. Use visual or audio comparisons with explicit tolerances where exact bytes are unreliable.

## Browser checks

The implementing agent must exercise every user-facing input and button in a real browser. Use Claude's browser when working in Claude, or the available browser automation in another agent environment. If no browser is available, record that gap and do not claim the UI is fully verified.

Build an edge-case matrix for each control. Cover every applicable class, including:

- Empty, whitespace-only, valid, invalid, boundary, and oversized values.
- Unicode, pasted text, malformed paths or files, and unsupported media.
- Keyboard access, focus, disabled states, and validation messages.
- Repeated clicks, double submission, loading, cancellation, and retry.
- Network or render failures, stale state, unsaved changes, and recovery.

Use the real UI for the complete happy path and important failure paths. Keep automation for repeatable coverage. Record tested cases and remaining gaps.

## End-to-end acceptance

For the first implementation, verify brief or instruction -> project operation -> preview -> save/reopen -> export. Verify undo and failures without losing the original project.

Add reusable fixture media with known duration, frame rate, audio properties, and provenance. Keep test assets small and outputs outside source control.

## Profiling

Measure affected workflows on a documented machine with a representative project. Record the input, command, tool versions, runtime, and relevant CPU, memory, frame responsiveness, or rendering measurements.

Find the bottleneck, fix it, and measure again with the same workload. Agree performance budgets when the first real workload exists. Avoid unsupported claims based only on how the code looks.

## Deliberate mistake checks

After the clean tests pass:

1. Choose a meaningful risk in the changed code, such as a timing boundary, validation check, or condition.
2. Introduce exactly one deliberate mistake locally.
3. Run the relevant tests. A behavioral assertion must fail; a syntax error or broken test setup is not useful evidence.
4. If the tests pass, add a test for the missing behavior and prove it fails with the mistake still present.
5. Undo the mistake and prove the tests pass again.
6. Repeat for the other important risks, one mistake at a time.
7. Inspect the final diff and run the required clean checks before committing.

Never commit a deliberate mistake. Record the mutation, the test that caught it, and the clean result. Apply this to meaningful executable behavior, not prose-only changes.

## Evidence

Record commands, results, browser coverage, profiling measurements, mutation results, and gaps in the task record or PR. Add reusable validation instructions to these docs as tools are introduced.
