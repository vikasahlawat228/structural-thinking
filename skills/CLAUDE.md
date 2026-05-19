# CLAUDE.md — Agent Instructions

You are working in a project that uses Vikas's skills repo for structural thinking and engineering discipline.

## How to take a task

**Step 1.** When a new task arrives, run the [`start`](skills/general/start/SKILL.md) skill **first**. It runs a five-step protocol — Sense, Classify domain, Imagine done, Constraints, Route — and dispatches to the right next skill (workflow or atomic).

**Step 2.** Follow the dispatched workflow or skill end-to-end. Do not interleave workflows. If you discover mid-task that the classification was wrong, **stop**, announce it, re-classify with the user, and re-route.

**Step 3.** Workflows compose atomic skills. Use the atomic skill the workflow asks for. Don't skip phases.

## Project-specific context

Before any non-trivial work, **discover** what the project already has — don't assume any specific layout.

Look for:

- Any of: `README.md`, `CONTEXT.md`, `GLOSSARY.md`, `ARCHITECTURE.md`, `NOTES.md`, `docs/`, `documentation/` at the project root.
- Decision records in any common convention: `docs/adr/`, `docs/decisions/`, `decisions/`, `architecture/`, `rfcs/`, or a single `DECISIONS.md`.
- External knowledge bases linked from any README (Notion, Confluence, Obsidian, GitHub Wiki).
- Per-module READMEs or context files in subdirectories.

Read whatever exists. Use the project's vocabulary. If your plan contradicts a written decision, surface it before proceeding.

If a doc layout doesn't exist yet and the work calls for one, **ask the user where new docs should live**. Never silently invent a layout.

## Hard rules

These are non-negotiable. Override them only when the user explicitly asks and you say so out loud.

1. **No horizontal slicing.** When implementing, write one test → one impl → repeat. Never queue all tests, then all code.
2. **Never refactor while red.** Get tests green, then refactor.
3. **One commit / PR per slice.** Each slice should be revertible independently.
4. **Don't push without approval.** No `git push`, no force-resets, no `git clean` autonomously.
5. **Don't touch the no-go list** for the project — auth, migrations, payments, secrets. Surface a question, don't act.
6. **Update the project's glossary inline** when a new domain term is decided, in whichever artifact already holds the glossary. Don't batch.
7. **Write decision records sparingly** — only when hard-to-reverse + surprising + a real trade-off.
8. **Tag debug logs** with a unique prefix like `[DEBUG-a4f2]` so cleanup is one grep.
9. **Discover, don't impose.** Match the project's existing doc layout, format, and conventions. The skills are written to adapt — apply them that way.

## Output style

- Use the project's domain language wherever it's written down. Concise > verbose.
- When you finish a phase, state explicitly that you've finished it and what's next.
- Show your classification at task start (per `start`). Show your handoff at task end (per `handoff`).

## When in doubt

- One bucket smaller, not larger. Cheaper to escalate than to deflate.
- Read the code before guessing. Read whatever glossary exists before reading the code.
- Ask one tight question. Don't ask three.
