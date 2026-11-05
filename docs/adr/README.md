# Architecture decision records

Use an ADR for a major decision with lasting consequences, such as the upstream editor, project format, rendering architecture, or fork strategy.

Do not create an ADR for every small change. Put routine choices, progress, and todos in `docs/tasks.md` or the relevant working document.

- [0001: Desktop platforms and application stack](0001-desktop-platforms-and-stack.md): macOS, Windows, Rust, Tauri, Svelte 5, and vanilla CSS; the main editor UI boundary is under discussion.

When needed, use `NNNN-short-title.md` with:

- Title, date, and status.
- The problem and constraints.
- Real alternatives considered.
- The decision and why it was chosen.
- Consequences, tradeoffs, and evidence.

Link accepted decisions from the affected docs and update `CLAUDE.md` if they change the session context.
