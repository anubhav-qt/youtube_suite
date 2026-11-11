# 0007: The store is one SQLite file per channel, owned by the Rust records gateway

Date: 2026-11-12

Status: Accepted by the creator.

## Context

The harness keeps Bloop's records, transcripts, action log, scheduler queue, grades, lessons, and memory ([ADR 0006](0006-long-running-chats-and-the-improvement-loop.md)).
They need one store, decided before the scheduler is built.
youtube_suite is a desktop app installed on macOS and Windows ([ADR 0001](0001-desktop-platforms-and-stack.md)).
Bloop first used one SQLite file through `node:sqlite` in its TypeScript harness ([Bloop ADR 0024](https://github.com/anubhav-qt/bloop/blob/dev/docs/adr/0024-storage-and-the-run-loop.md)).
That ADR rejected Postgres as a server to run and look after for one PC.
Bloop later moved to Postgres in Docker only because it is being rebuilt on dodo ([Bloop ADR 0033](https://github.com/anubhav-qt/bloop/blob/dev/docs/adr/0033-the-store-on-postgres.md)).
The creator chose SQLite owned by Rust on 2026-11-12.

## Decision

- One SQLite file per channel, in the app's data folder, outside the video project folders.
- Only the records gateway in the Rust tool server opens it ([ADR 0004](0004-harness-on-a-typescript-pi-fork.md)).
- The TypeScript harness, the editor UI, and the channel gateway read and write through the tool server, never the file.
- WAL mode, so the app's views can read while a run writes.
- Tables change only through numbered migrations, applied in order. A database newer than the code is refused, not changed.
- The database enforces Bloop's rules with triggers and constraints:
  - Records, transcripts, and the action log are append only.
  - A record's versions go 1, 2, 3 in order; two writers racing for one version collide, and the loser reads again.
  - One Video chat per video, one chat for each shared role, and one running run per chat.
  - Scheduler wakes are the one table whose rows change, and only their status, due time, tries, and error.
- Each row keeps its whole checked JSON object, plus typed columns for what is queried or constrained.
- Every time is written by the harness's clock, never by the database, so replays store the same times.
- Approvals are records here too. The channel gateway checks them in the same Rust code that stores them.
- Backups use SQLite's online backup to a folder the creator picks.

The Rust SQLite library and its version are pinned when the store is built.

## Alternatives considered

- SQLite owned by the TypeScript harness, as in Bloop's first version: closer to Bloop's code, but the Rust gateways would reach the store through the harness, and approvals would be checked away from where they are stored.
- Postgres in Docker, as Bloop does now: a stronger database, but every machine needs Docker and a running database server beside the app.

## Consequences

- Nothing is installed beside the app. One file per channel is copied to back up or move.
- Bloop's `@bloop/store` and its tests are the specification. Its rules are ported to Rust and SQLite triggers.
- The records gateway is built in roadmap step 3, before the scheduler.
- One writer process means no cross-process write locking, but a slow write blocks other writes. Measure this once runs exist.
