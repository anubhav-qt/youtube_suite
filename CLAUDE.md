# Session guide

## What we are building

`youtube_suite` is a reproducible YouTube production system. It includes a professional video editor with our own UI, an agent editing harness, SVG animation asset generation, and later direction, animation, and 3D workflows. Style and content will be decided later.

## Current state

- Planning only. No application code, dependencies, tests, or profiling tools yet.
- No upstream editor, framework, renderer, or model provider has been chosen.
- Next: define one small end-to-end video workflow, then evaluate an editor foundation against it.
- Read `docs/tasks.md` for current work and `docs/roadmap.md` for the sequence.

## Working rules

- Keep this file brief and current as code, docs, and major decisions change. Link details instead of copying them here.
- Keep detailed documentation in `docs/`. Use ADRs only for major decisions. Small changes belong in tasks and todos.
- Follow `docs/engineering.md` and `docs/testing.md`.
- Use simple, readable code. Prefer clear names and ordinary if/else blocks over clever one-liners or dense conditions.
- Speak in short, simple one-line sentences. No emojis or em dashes. Ask the human about one topic at a time.
- Finish work carefully. Passing tests, browser checks, and measured performance matter more than speed.
- Test every user-facing input and button in a real browser, including applicable edge cases.
- After tests pass, introduce one deliberate code mistake at a time. Tests must catch it. Add a missing test when needed, undo the mistake, and rerun the tests.
- Commit messages use `feat: short message`, `fix: short message`, or another suitable conventional type. One line only. Extended descriptions belong in PRs. Never add a Claude Code co-author trailer.
- Never claim code was tested, profiled, or production ready without evidence. Record blocked checks honestly.

## Where to look

- `docs/project-brief.md`: product scope and intended behavior.
- `docs/architecture.md`: proposed boundaries and reproducibility rules.
- `docs/engineering.md`: implementation and documentation standards.
- `docs/testing.md`: browser, end-to-end, profiling, and mutation checks.
- `docs/tasks.md`: completed work, next work, and open questions.
- `docs/adr/README.md`: when and how to record major decisions.
