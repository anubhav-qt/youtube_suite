# 0006: Long-running chats, one per video, with Bloop's improvement loop and memory

Date: 2026-11-12

Status: Accepted by the creator.

## Context

The harness is one system for editing and the channel ([ADR 0003](0003-one-harness-with-channel-approvals.md)).
Three shapes were offered: one conversation with jobs (Jello), a hybrid, or a long-running chat per job (Bloop).
The creator chose long-running chats on 2026-11-12, with "the same improvement loop, memories, everything like in Bloop".
The creator then chose one chat per video, as Bloop keeps one trader chat per stock.
Bloop's design is summarised in [the harness study](../harness-study.md). Bloop ids below refer to [Bloop's ADRs](https://github.com/anubhav-qt/bloop/tree/dev/docs/adr) and its [HARNESS-STUDY.md](https://github.com/anubhav-qt/bloop/blob/dev/docs/HARNESS-STUDY.md).

## Decision

**Chats** (Bloop ADR 0013)

Each chat is long-running, with its own conversation and only its own tools.

| Chat | Does | Bloop's version |
| --- | --- | --- |
| Video chat, one per video | Carries its video from brief to results: script, assets, timeline edits, preview, render, then title, description, tags, chapters, captions, thumbnail, and playlists. Uploads privately and asks the creator for every public action. Reflects on its grades. Works with the creator in the app. | Trader chat, one per stock |
| Analyst | Reads analytics across all videos, finds patterns, and briefs the Video chats and Lessons. | Trade Master |
| Lessons | Studies every video that did worse than expected, and every Auditor undo, and keeps the lessons every chat follows. | Lessons chat (ADR 0010) |
| Auditor | Reads every run, undoes bad changes to our records, hands bad work back to its chat, and tells Lessons why. | Auditor chat (ADR 0012) |

- A Video chat is made when its video's brief starts. One run at a time per chat.
- Only a Video chat writes to YouTube, only for its own video, and only through the channel gateway. Approvals still apply ([ADR 0003](0003-one-harness-with-channel-approvals.md)).
- The Auditor can undo our own records. It cannot touch YouTube. For a public video it writes a finding to that video's chat, which asks the creator.
- Nothing forces a chat to act. It can read more, ask another chat, plan and wait, or pass (Bloop ADR 0022).

**Handovers** (Bloop ADR 0018)

- Chats never read each other's raw output.
- Every handover is schema-checked JSON with free-text reasoning inside: a brief, a pattern, a finding, a lesson, a question, an answer, a grade reflection.
- Code checks it, stores it as a versioned record, and renders it into the next chat's input from a template.

**Scheduler** (Bloop ADR 0027, F6)

- One always-on process wakes each chat from a queue: its schedules, records written for it, and its own plans coming due.
- A chat wakes once with everything due for it. A failed run is tried again, up to a limit.
- Each record kind either steers a running chat or follows up with the next run.
- Steer: the creator's message to that chat, an approval or refusal, an Auditor finding or work handed back, and the answer to a question the running chat asked.
- Follow up: briefs, patterns, grades, and questions from other chats.

**Grades and lessons** (Bloop ADRs 0010, 0030)

- Code grades every published video at 7 and 28 days after it goes public (the creator, 2026-11-12). The 28-day grade is the main one.
- A grade is computed once that day's analytics are fully processed, about 3 days later.
- A grade compares the video with the channel's own recent videos at the same age, the way Bloop compares a trade with the index.
- Metrics: views, impression click-through rate, average view percentage, watch time, and net subscribers.
- Each metric is shown raw and against the median of the last 10 videos of the same format at the same age. Shorts and long videos have separate baselines.
- Test uploads are marked as tests and never count in a baseline.
- Until 10 videos of a format exist, the grade says "no baseline yet" and shows raw numbers only.
- Every grade records the brief, title, thumbnail, length, style pack, and models that made the video.
- The video's chat writes a short reflection for Lessons, since it knows why it made each choice.
- Click-through rate judges the packaging (title and thumbnail). Average view percentage judges the content.
- Click-through rate comes from the Reporting API's reach reports, not the Analytics API ([harness study](../harness-study.md#youtube-facts)).

**Memory** (Bloop ADR 0032 items 2 and 3, F22 to F24)

- A memory is a versioned record with a key, a kind (fact, rule, preference, open question), a scope, the run it came from, when it became known, and its evidence.
- Scopes: everyone, a kind of chat, a series or topic, and a video. A Video chat writes for its own video, its series, or its topic.
- A read returns only each key's active version, never a retired one. Every scope has a Known Unknowns list.
- Any chat writes memories within its own scope. The Analyst and Lessons may write for everyone. The Auditor can retire any memory.
- Lessons and memories are frozen in a chat's instructions at its last compaction. Changes since arrive as one message in the next run's opening, and fold in at the next compaction. The cache holds.

**Compaction** (Bloop ADRs 0014, 0026, 0032 item 6)

- At a token limit, a chat saves what is not stored yet, then keeps only a summary, in a compaction run of its own.
- A run that crosses the limit pauses, compacts, and a continuation run carries on.
- The summary has prose plus Durable Facts, Lessons, and Known Unknowns. A previous summary is consolidated, not copied forward.
- Code checks that exact values the chat cited survive: video ids, timecodes, record versions, and approval ids. Missing values get one rewrite, then code adds them to Durable Facts. Compaction never loops.

**Model gateway** (Bloop ADR 0032 item 4, F18)

- Every model call goes through one gateway, over the accounts signed in through pi ([ADR 0005](0005-models-through-pi-sign-in.md)).
- Priority order: (1) a Video chat the creator is talking to, or one holding an approval; (2) the Auditor and the Analyst; (3) other runs; (4) Lessons, compaction, and research.
- Each kind of chat has a ladder of models, best first. A chat stays on its last model while that model has room, so its cache stays warm.

**Everything stored** (Bloop ADRs 0023, 0024, 0028)

- Records, transcripts, and the action log are append only. A change names the version it came from. Undo copies an old version forward.
- The action log keeps each call's reasoning, arguments, result, and the records it read and wrote.
- Every fetched fact keeps when it was fetched. YouTube revises analytics, so a grade cites the numbers it used.
- Every tool call's intent is stored before it runs. An upload is never replayed (F1 to F4).

## Alternatives considered

- One conversation with jobs, as in Jello: simplest, but nothing studies results while the creator is away.
- One Editor and one Publisher chat for all videos: fewer chats, but each context mixes every video, and the reflection is not written by the chat that made the choices.
- An Editor chat and a Publisher chat per video: focused, but one video's work is split across two chats that must hand over.
- A hybrid: one conversation for editing and publishing, background chats for learning. The creator wanted Bloop's whole system instead.

## Consequences

- The harness carries Bloop's TypeScript design as its specification: chats, runs, the scheduler, records, handovers, memory, compaction, and grades.
- Bloop's code and tests are a reference. Code that came from pi keeps pi's MIT licence and says what changed.
- The app needs views for chats, runs, approvals, grades, lessons, and findings.
- Bloop has no approvals. Here a Video chat's runs pause on stored waits for the creator.
- The store must be decided before the scheduler is built. It is SQLite owned by Rust ([ADR 0007](0007-the-store-on-sqlite-in-rust.md)).
