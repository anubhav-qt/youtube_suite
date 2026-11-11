# 0003: One harness, with the creator approving every public channel action

Date: 2026-11-12

Status: Accepted by the creator.

## Context

The creator wants the system to run their YouTube channel end to end.
That means publishing, reading analytics, and learning from results.
The creator wants one harness for this and for editing, built the way Bloop's harness is.
See [the harness study](../harness-study.md).

Three levels of autonomy were offered:

- A: the system prepares and reads freely, and the creator approves each public or hard to reverse action.
- B: the system acts on its own within rules the creator sets.
- C: the system only advises and never writes to YouTube.

The creator chose A on 2026-11-12.

## Decision

**One harness.**

- One harness serves editing, assets, publishing, analytics, and learning.
- Its jobs share one tool server, one store, and the same gateways.
- Only a job's instructions and tool set differ.

**What the harness does without asking.**

- Read channel data and analytics.
- Research, draft, and revise briefs, scripts, titles, descriptions, tags, chapters, captions, and thumbnails.
- Edit projects through the editor's project and undo operations.
- Upload a video as private, with its metadata, thumbnail, and captions.
- Write reports, grades, lessons, and suggestions for the next video.

**What needs the creator's approval, every time.**

- Making a video public or unlisted, now or on a schedule.
- Changing anything viewers can see on a public or unlisted video, such as its title, description, thumbnail, captions, or playlists.
- Deleting anything on YouTube.
- Replying to, hiding, or moderating comments.
- Community posts.
- Channel settings and branding.

The rule behind the list: anything viewers can see, or anything that cannot be undone, waits for the creator.

**Where the rule is enforced.**

- A channel gateway in Rust is the only code that talks to YouTube or holds its tokens.
- The gateway enforces approvals in code. A prompt, a model, or an outside agent cannot skip it.
- An approval shows the exact change, before and after.
- An approval is used once and is tied to that exact change. If the change differs, the approval does not apply.
- Every upload sets its privacy status to private explicitly. The API makes a video public when no status is given (verified 2026-11-12).
- While a job waits for an approval, the wait is stored. Nothing is held open.

**Loosening it later.**

- The creator can let one kind of action run without approval at a time.
- Each change amends this ADR.

## Alternatives considered

- B, autonomous within rules: faster, but a bad title or an early publish reaches viewers before anyone sees it. Bloop works this way ([its ADR 0011](https://github.com/anubhav-qt/bloop/blob/dev/docs/adr/0011-fully-autonomous-agent.md)) because trades need speed and it has hard limits. A channel does not need that speed.
- C, advisory only: safest, but the creator does every upload by hand. That is not end to end.
- Separate harnesses for editing and the channel: two stores and two sets of tools that can drift apart. The creator asked for one.

## Consequences

- The app needs an approvals view: what will change, why, and approve or decline.
- The gateway needs OAuth scopes for upload, managing videos, and reading analytics. The exact scopes are set when it is built.
- Tokens live in the operating system's keychain. They never enter projects, logs, prompts, or the model's context.
- The gateway tracks YouTube's daily quota and refuses work that would pass it.
- Every action is stored with its reasoning, so the creator can review what was done.
