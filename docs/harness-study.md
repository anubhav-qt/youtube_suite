# Harness study: what youtube_suite takes from Bloop and Jello

Written 2026-11-12.
It collects what the youtube_suite harness learns from the creator's other harnesses.
A session designing the harness can read this file instead of the sources.
Decisions are recorded as ADRs, asked one at a time ([open questions](#open-questions)).

## Sources

| Source | What it is | Read at |
| --- | --- | --- |
| Bloop ([anubhav-qt/bloop](https://github.com/anubhav-qt/bloop/tree/dev), branch `dev`) | Trading agents on a hard fork of pi, with models through CLIProxyAPI. Moving to dodo in Mojo. Its [HARNESS-STUDY.md](https://github.com/anubhav-qt/bloop/blob/dev/docs/HARNESS-STUDY.md) holds findings `F1` to `F44`. | `0bd8025` |
| Jello ([anubhav-qt/jello](https://github.com/anubhav-qt/jello)) | The music studio youtube_suite is modelled on. One agent on pi, with a Rust MCP tool server. | `d9bb80a` |
| pi ([earendil-works/pi](https://github.com/earendil-works/pi), MIT) | The agent toolkit both forks come from. | Through Bloop's fork |
| YouTube Data API docs | `videos.insert` and the quota page. | 2026-11-12 |

Bloop ids (`F12`, ADR `0018`) refer to Bloop's own docs.

## How Bloop's harness is made

- **A hard fork of pi.** `packages/pi-ai` and `packages/pi-agent`, MIT licence kept, trimmed to four model APIs and the agent loop. Not kept in sync with upstream (Bloop ADR 0005).
- **Models through CLIProxyAPI.** A local proxy on 127.0.0.1 serves the creator's subscriptions.
- **Roles as long-running chats.** Trader chats, Trade Master, Lessons, and Auditor. Each role gets only its own tools (`systemToolsFor`).
- **Two kinds of tools.** Data tools reach outside and are designed with the owner. System tools touch only Bloop's records and are designed freely (ADR 0015).
- **Gateways.** One door each for records, models, data, and money (F17 to F20).
- **Everything stored.** Versioned, append-only records and an action log with each call's reasoning, arguments, and result (ADRs 0023, 0024).
- **JSON handovers.** Agents never read each other's raw output. Code checks and stores each handover, then renders it through a template (ADR 0018).
- **Prompts as templates.** No prompt text in code (ADR 0020).
- **A scheduler.** It wakes each chat from a queue of schedules, records, and plans coming due (ADR 0027).
- **Grades and lessons.** Code grades every decision after a fixed time. A Lessons chat learns from the results (ADRs 0010, 0030).
- **Compaction.** Each chat keeps a summary with exact values checked by code (ADR 0032).
- **Moving to dodo.** The TypeScript fork is now the specification for a Mojo port (ADR 0031). It is ported piece by piece, and each piece must pass the same tests.

## How Jello's harness is made

- **One agent, several jobs.** Tutor, Composer, and Producer share tools and the conversation. Only the instructions change.
- **One tool server in Rust.** It runs the same code as the UI's commands, so a new command reaches the agent too. It is MCP over HTTP on 127.0.0.1, behind a password made at launch, with no shell or file tools.
- **pi in a small Node host.** It keeps the session, swaps job instructions, and connects to the tool server.
- **Acting freely, with undo.** The whole song is saved before each reply, and that reply's Undo puts it back. The agent asks only before a few overwrites.
- **Listening by measuring.** A second engine renders offline and returns measurements, so the agent checks its work without guessing.

## What youtube_suite takes

Each item gets an id (`Y1`, `Y2`, ...) so ADRs and tasks can cite it.

### Y1. One harness, long-running chats, one per video `[decided, ADRs 0003 and 0006]`

- From Bloop's roles.
- Chats: one Video chat per video, as Bloop keeps one trader chat per stock, plus Analyst, Lessons, and Auditor ([ADR 0006](adr/0006-long-running-chats-and-the-improvement-loop.md)).
- One tool server, one store, and one set of gateways.
- Bloop's scheduler, handovers, grades, lessons, memory, compaction, and model gateway all carry over.

### Y2. Fat tools, thin calls

- From Bloop F10, F11, and Jello's tools by area.
- Tools carry the code. The model picks the arguments.
- Each tool answers with the fresh state of its area. A refusal says what is allowed.
- Exact values come from code, never the model: timecodes, durations, loudness, file sizes, and analytics changes.
- No generic shell, SQL, or HTTP tool.

### Y3. One tool server, shared with the UI

- From Jello and Bloop F16.
- Rust, MCP on 127.0.0.1 behind a launch password.
- The editor UI and the agent use the same commands, so they cannot drift.
- Outside agents, like Claude Code, can connect later.

### Y4. Gateways

- From Bloop F17 to F20.
- **Project gateway.** Every edit goes through Kdenlive's project and undo operations (ADR 0002).
- **Channel gateway.** The only door to YouTube. It holds the tokens, enforces approvals, sets privacy to private on upload, tracks quota, and paces calls (ADR 0003).
- **Records gateway.** Every write is checked, versioned, and logged.
- **Model gateway.** Later, when several jobs share accounts.

### Y5. Approvals live in the channel gateway, not the prompt `[decided, ADR 0003]`

- Bloop has no approvals (its ADR 0011). Its hard limits live in the engine, out of the agent's reach.
- youtube_suite uses the same idea for approvals. The gateway refuses a public action without a matching approval.
- An approval is single use and tied to the exact change.
- In pi's loop, `beforeToolCall` shows the approval request early. The gateway still decides.

### Y6. Durable runs

- From Bloop F1 to F5.
- A tool call's intent is stored before it runs.
- Each tool declares whether it is safe to replay. An upload is never replayed. It carries an idempotency key and uses YouTube's resumable upload.
- Waiting for an approval or a schedule is a stored sleep.

### Y7. Everything stored, versioned, and logged `[decided, ADRs 0006 and 0007]`

- From Bloop ADRs 0023 and 0024.
- Records are append only. A change names the version it came from. Undo copies an old version forward.
- The action log keeps each call's reasoning, arguments, result, and the records it read and wrote.
- One SQLite file per channel, opened only by the Rust records gateway ([ADR 0007](adr/0007-the-store-on-sqlite-in-rust.md)).

### Y8. JSON handovers and prompt templates

- From Bloop ADRs 0018 and 0020.
- Jobs hand over schema-checked JSON with free-text reasoning inside.
- Prompts live in `prompts/` as templates. No prompt text in code.
- Standing instructions come first and what changes per run comes last, so the provider's cache holds (Bloop F25 to F27).

### Y9. Data tools and channel tools are designed with the creator

- From Bloop ADR 0015.
- Tools that reach outside (YouTube, web research, trends) are asked about one choice at a time.
- System tools that touch only our own records are designed freely.

### Y10. Learning from results `[decided, ADR 0006]`

- From Bloop's grades (ADR 0030) and lessons (ADR 0010).
- Code grades each published video at 7 and 28 days after it goes public, against the median of its format's last 10 videos at the same age.
- Each grade links to the video's brief, title, thumbnail, length, style pack, and the model used.
- A Lessons job keeps lessons. The next brief reads them.
- New lessons reach a chat as one appended message, so its cache holds (Bloop F24).

### Y11. Undo per reply

- From Jello.
- The project is saved before each agent reply. That reply's Undo puts it back through the editor's undo.

### Y12. Usage and cache per run

- From Bloop F37.
- Every model call records tokens, cache reads and writes, cost, and model.

### Y13. Compaction

- From Bloop ADR 0032.
- A summary with Durable Facts, Lessons, and Known Unknowns.
- Exact values the job cited, such as video ids, timecodes, and record versions, must survive. Code checks this.

## What youtube_suite does differently

- **Approvals.** Bloop has none. youtube_suite approves every public action (ADR 0003).
- **Desktop on macOS and Windows.** Mojo has no Windows support, so dodo is not an option for the shipped app today.
- **An editor in C++.** The harness reaches Kdenlive through a command bridge that is not designed yet.
- **Scale.** One creator, a chat per video, and three shared chats, not thousands. The model gateway keeps priorities and ladders, but AIMD seats and helpers wait until they are needed.

## Left out for now

- Code mode: the model would write code, against Y2.
- Local models: no task needs one yet.
- Plugin hot reload.

## YouTube facts

Checked on 2026-11-12 against Google's docs. Recheck before building the channel gateway.

- `videos.insert` makes a video **public** when no privacy status is given. Some uploads may default to private from account settings.
- The `videos.insert` page says uploads from unverified API projects are not restricted to private.
- Default quota: 100 `videos.insert` calls a day, 100 `search.list` calls a day, and 10,000 units a day for everything else.
- Costs: `videos.update` 50, `thumbnails.set` 50, `captions.insert` 400, most list calls 1, `captions.list` 50.
- Upload scopes include `youtube.upload`, `youtube`, and `youtube.force-ssl`.
- The Analytics API answers targeted queries. The Reporting API delivers bulk reports to store.
- Analytics data has a 48 to 72 hour processing delay, and query end dates are cut to the last fully processed day (Analytics API revision history, entry of 2026-09-09).
- The Analytics API has `views`, `estimatedMinutesWatched`, `averageViewDuration`, `averageViewPercentage`, `subscribersGained`, `subscribersLost`, `likes`, `shares`, and retention metrics such as `audienceWatchRatio`.
- Thumbnail impressions and their click-through rate are not in the Analytics API. They come from the Reporting API's reach reports (`channel_reach_basic_a1`): `video_thumbnail_impressions` and `video_thumbnail_impressions_ctr`.
- Reporting API reports come from a reporting job. When to create it, and how far back its first reports reach, is to be checked before the channel gateway is built.

Sources: [videos.insert](https://developers.google.com/youtube/v3/docs/videos/insert), [quota costs](https://developers.google.com/youtube/v3/determine_quota_cost), [Analytics and Reporting APIs](https://developers.google.com/youtube/analytics), [Analytics metrics](https://developers.google.com/youtube/analytics/metrics), [Analytics revision history](https://developers.google.com/youtube/analytics/revision_history), [Reporting API channel reports](https://developers.google.com/youtube/reporting/v1/reports/channel_reports).

## Proposed shape

```text
 Editor UI (Qt)      agent panel       outside agents (later)
       \                 |                 /
        +----------------+----------------+
                         v
          Rust tool server (one command list)
      /              |                |              \
 project gateway  channel gateway  records gateway  narration gateway
  (Kdenlive)   (YouTube, approvals) (store, log)    (Kokoro first)
```

## Open questions

Asked one at a time, in this order.

1. ~~The harness foundation.~~ Decided: a TypeScript hard fork of pi, with tools and gateways in Rust ([ADR 0004](adr/0004-harness-on-a-typescript-pi-fork.md)).
2. ~~How the harness reaches models.~~ Decided: pi's own sign-in, as in Jello ([ADR 0005](adr/0005-models-through-pi-sign-in.md)).
3. ~~Jobs.~~ Decided: long-running chats, with Bloop's improvement loop and memory ([ADR 0006](adr/0006-long-running-chats-and-the-improvement-loop.md)).
4. ~~A chat per video.~~ Decided: one Video chat per video edits, publishes, and reflects on its grades ([ADR 0006](adr/0006-long-running-chats-and-the-improvement-loop.md)).
5. ~~First channel functions.~~ Decided: upload and learn ([project brief](project-brief.md#channel-management)).
6. ~~Grade checkpoints and metrics.~~ Decided: 7 and 28 days; views, impression click-through rate, average view percentage, watch time, and net subscribers ([ADR 0006](adr/0006-long-running-chats-and-the-improvement-loop.md)).
7. ~~Where channel work sits in the roadmap.~~ Decided: upload and learn is step 4, after the first complete editing path; the harness core is built in step 3 ([roadmap](roadmap.md)).
8. ~~The store.~~ Decided: one SQLite file per channel, owned by the Rust records gateway ([ADR 0007](adr/0007-the-store-on-sqlite-in-rust.md)).
