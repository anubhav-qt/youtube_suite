# Engineering standards

## Readability

- Prefer small, explicit functions with clear names and one clear job.
- Use ordinary control flow. Avoid clever one-liners, nested ternaries, and dense conditions.
- Add abstractions when a real repeated need justifies them.
- Explain non-obvious reasons in comments. Keep behavior visible in the code.
- Validate external input at boundaries and return actionable errors.

## Reliability

- Make project changes recoverable and test undo or rollback behavior.
- Handle cancellation, partial failure, and retries for long operations.
- Keep secrets out of source, fixtures, logs, and generated project metadata.
- Pin runtime and dependency versions when implementing the chosen Rust, Tauri, Svelte 5, and vanilla CSS stack.
- If the upstream Qt UI is retained, document that agreed boundary and preserve a small, reviewable fork diff.
- Define reproducible local setup and CI commands with the first implementation.
- Track asset provenance and upstream licence requirements.

## Documentation

- Keep detailed documentation in `docs/`.
- Update docs with the behavior they describe, in the same change.
- Review `CLAUDE.md` after meaningful progress or decisions. Update stale context, remove obsolete material, and link details instead of making it long.
- Use ADRs for major decisions with lasting consequences. Use tasks and todos for small choices and routine work.
- At handoff, record what changed, what was verified, what remains, and the next decision.

## Human communication

- Use short, simple one-line sentences that a child could follow.
- No emojis or em dashes.
- Ask about one topic at a time. Do not make the human decide many things at once.
- Explain the result and any real blocker plainly.

## Commits and PRs

- Use a conventional type and one direct line, such as `feat: add timeline undo` or `fix: preserve caption timing`.
- Other suitable types include `docs`, `test`, `refactor`, `perf`, `build`, `ci`, and `chore`.
- Do not use a commit body or an em dash. Extended descriptions belong in PRs.
- Never add Claude Code as a co-author.
- PRs explain the problem, resulting behavior, verification, and known limits.

## Definition of done

A code change is complete when the implementation is readable, docs are current, relevant checks pass, user-facing behavior has been exercised in a real browser where applicable, performance has been measured for affected paths, and deliberate mistakes have proven that the tests catch likely regressions.

Remove every deliberate mistake and rerun the clean tests before committing. Record unavailable checks as gaps. Do not call an unverified change production ready.

Documentation-only changes need link, consistency, and repository checks; they do not need invented application tests or profiling.
