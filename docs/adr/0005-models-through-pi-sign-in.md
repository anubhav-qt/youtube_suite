# 0005: The harness reaches models through pi's own sign-in, as Jello does

Date: 2026-11-12

Status: Accepted by the creator.

## Context

The harness is a TypeScript hard fork of pi ([ADR 0004](0004-harness-on-a-typescript-pi-fork.md)).
It needs a way to reach models.
Bloop runs CLIProxyAPI, a local proxy that serves the creator's subscriptions.
Jello uses pi's own sign-in inside its agent panel.
The creator chose Jello's way on 2026-11-12.

## Decision

- The creator signs in inside the app, through pi's sign-in.
- Two ways to sign in: in the browser (OAuth), or by pasting an API key.
- Providers offered first, as in Jello: Anthropic (Claude Pro or Max sign-in), OpenAI (Sign in with ChatGPT), and Google (an AI Studio API key). Others can come later.
- Google's Antigravity sign-in is left out. Jello's notes say pi dropped it and Google bans accounts that use it from other tools.
- The fork keeps pi's sign-in code: the OAuth flows and credential store in pi's `ai` package, and a small runtime like pi's `ModelRuntime`.
- The fork keeps only the model APIs those providers need. The exact list is set when pi is forked.
- Sign-ins are kept in the app's settings folder, readable by the creator's account only.
- Model tokens stay in the harness process. The UI never sees them. They never enter projects, logs, prompts, or the model's context.
- A model picker lets the creator choose the model.

## Alternatives considered

- CLIProxyAPI, as in Bloop: one proxy could serve Bloop and youtube_suite, and the fork would be smaller. But it is one more process to run, and proxying subscriptions may break providers' terms (Bloop ADR 0005).
- API keys only: the clearest terms, but every token is paid for. Pasting a key stays possible under this decision.

## Consequences

- The fork is larger than Bloop's, because Bloop dropped pi's sign-in code.
- No proxy process runs beside the app.
- Usage limits come from each provider's own sign-in, not from a shared proxy.
- Sign-in flows need tests with a fake provider, and one real sign-in per provider checked by hand.

## Evidence

- pi's sign-in lives in `packages/ai/src/auth` (OAuth for Anthropic and OpenAI ChatGPT, among others) and `ModelRuntime` in `packages/coding-agent/src/core/model-runtime.ts`, at upstream `2a64f90` (2026-11-11).
- Jello signs in through `ModelRuntime.login` in its agent host (Jello `d9bb80a`).
