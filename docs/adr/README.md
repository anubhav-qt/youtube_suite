# Architecture decision records

Use an ADR for a major decision with lasting consequences, such as the upstream editor, project format, rendering architecture, or fork strategy.

Do not create an ADR for every small change. Put routine choices, progress, and todos in `docs/tasks.md` or the relevant working document.

- [0001: Desktop platforms and application stack](0001-desktop-platforms-and-stack.md): macOS and Windows with the application stack amended by ADR 0002.
- [0002: Retain and restyle Kdenlive's editor UI](0002-retain-kdenlive-editor-ui.md): preserve Qt editor controls and use Rust and Svelte where useful.
- [0003: One harness, with the creator approving every public channel action](0003-one-harness-with-channel-approvals.md): one harness for editing and the channel; approvals enforced in the channel gateway.
- [0004: The harness runs on a TypeScript fork of pi, with tools in Rust](0004-harness-on-a-typescript-pi-fork.md): a hard fork of pi for the conversation; tools and gateways in Rust.
- [0005: The harness reaches models through pi's own sign-in, as Jello does](0005-models-through-pi-sign-in.md): sign in inside the app with Claude, ChatGPT, or an API key.
- [0006: Long-running chats, one per video, with Bloop's improvement loop and memory](0006-long-running-chats-and-the-improvement-loop.md): a Video chat per video, plus Analyst, Lessons, and Auditor chats.
- [0007: The store is one SQLite file per channel, owned by the Rust records gateway](0007-the-store-on-sqlite-in-rust.md): append-only records enforced in the database; nothing installed beside the app.
- [0008: Narration is a harness feature, with Kokoro first and more engines later](0008-narration-through-the-harness.md): a Rust narration gateway with one `narrate` tool over pluggable engines.

When needed, use `NNNN-short-title.md` with:

- Title, date, and status.
- The problem and constraints.
- Real alternatives considered.
- The decision and why it was chosen.
- Consequences, tradeoffs, and evidence.

Link accepted decisions from the affected docs and update `CLAUDE.md` if they change the session context.
