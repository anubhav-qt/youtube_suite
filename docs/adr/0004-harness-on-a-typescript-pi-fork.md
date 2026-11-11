# 0004: The harness runs on a TypeScript fork of pi, with tools in Rust

Date: 2026-11-12

Status: Accepted by the creator. Amends [ADR 0001](0001-desktop-platforms-and-stack.md) and [ADR 0002](0002-retain-kdenlive-editor-ui.md).

## Context

ADR 0003 asks for one harness for editing and the channel.
ADR 0001 said Rust for the harness.
The creator's other harnesses run on pi ([the harness study](../harness-study.md)).
Bloop hard forked pi's model and agent packages. Jello runs pi in a small Node process beside a Rust tool server.

Three foundations were offered: a pi fork with tools in Rust, a loop written in Rust, or dodo in Mojo.
The creator chose the TypeScript harness on 2026-11-12.

## Decision

**The conversation runs on a hard fork of pi, in TypeScript.**

- Fork pi's model and agent packages fresh from upstream ([earendil-works/pi](https://github.com/earendil-works/pi), MIT) at a pinned commit.
- Keep pi's LICENSE and mark our changes.
- Trim it the way Bloop did: keep only the model APIs we use and the agent loop.
- Which model APIs stay follows [ADR 0005](0005-models-through-pi-sign-in.md): pi's own sign-in, as in Jello.
- It is a hard fork, not a dependency kept in sync. Upstream stays the reference.
- The harness runs as its own Node process. Node and every package are pinned.

**Tools and gateways stay in Rust.**

- One Rust tool server holds every tool, shared with the editor UI ([harness study](../harness-study.md) Y3).
- The project, channel, and records gateways live behind it.
- The channel gateway enforces approvals ([ADR 0003](0003-one-harness-with-channel-approvals.md)). The TypeScript harness never holds YouTube tokens.
- The harness reaches the tool server over MCP on 127.0.0.1, behind a password made at launch.

## Alternatives considered

- A loop written in Rust: one language, but we would rebuild pi's model clients and loop first.
- dodo in Mojo: shares Bloop's future kit, but Mojo has no Windows support and the kit is not built.
- pi as an ordinary dependency, as Jello does: less code to own, but harder to trim and change. The creator asked for a fork like Bloop's.

## Consequences

- The project has TypeScript for the harness, Rust for tools and gateways, and C++ and QML in the editor.
- A crash or bug in the harness cannot publish anything. Approvals and tokens sit in Rust.
- The shipped app carries its own pinned Node runtime, as Jello does.
- Harness tests run with Node's test runner. Exact commands are set with the first code.
