# CLAUDE.md — Agent Instructions

You are working in a codebase that uses Vikas's skills repo for engineering discipline.

## How to take a task

**Step 1.** When a new task arrives, run the [`orchestrator`](skills/orchestrator/SKILL.md) skill **first**. It classifies the task as a small change, medium feature, or large project, and routes you to the right workflow.

**Step 2.** Follow the workflow end-to-end. Do not interleave workflows. If you discover mid-task that the classification was wrong, **stop**, announce it, re-classify, and re-route.

**Step 3.** The workflows compose atomic skills (TDD, diagnose, grill-with-docs, etc.). Use the atomic skill the workflow asks for. Don't skip phases.

## Project-specific context

Before any non-trivial work:

- Read `CONTEXT.md` at the project root. This is the project's domain glossary.
- If the project has multiple bounded contexts, read `CONTEXT-MAP.md` and the relevant per-module `CONTEXT.md`.
- Skim `docs/adr/` — these are decisions you must honour. If your plan contradicts an ADR, surface it before proceeding.

## Hard rules

These are non-negotiable. Override them only when the user explicitly asks you to and you say so out loud.

1. **No horizontal slicing.** When implementing, write one test → one impl → repeat. Never queue all tests, then all code.
2. **Never refactor while red.** Get tests green, then refactor.
3. **One commit / PR per slice.** Each slice should be revertible independently.
4. **Don't push without approval.** No `git push`, no force-resets, no `git clean` autonomously.
5. **Don't touch the "no-go" list.** Auth, migrations, payments, secrets — surface a question, don't act.
6. **Update CONTEXT.md inline** when a new domain term is decided. Don't batch.
7. **Write ADRs sparingly** — only when hard-to-reverse + surprising + a real trade-off.
8. **Tag debug logs** with a unique prefix like `[DEBUG-a4f2]` so cleanup is one grep.

## Output style

- Use the project's domain language from `CONTEXT.md`. Concise > verbose.
- When you're done with a phase, state explicitly that you've finished it and what's next.
- Show your classification at task start. Show your handoff at task end.

## When in doubt

- One bucket smaller, not larger. Cheaper to escalate than to deflate.
- Read the code before guessing. Read `CONTEXT.md` before reading the code.
- Ask one tight question. Don't ask three.
