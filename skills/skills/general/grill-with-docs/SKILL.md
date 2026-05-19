---
name: grill-with-docs
description: Grilling session that challenges your plan against whatever knowledge already lives in the project — code, glossary, decision records, design docs, wiki pages, READMEs — wherever they are. Updates that knowledge inline as decisions land. Adapts to any directory layout. Use when stress-testing a plan against an existing project where prior thinking already exists in some form, and you want new decisions to compound with the old.
category: both
---

# Grill With Docs

Same shape as [`grill-me`](../grill-me/SKILL.md) — relentless interview, one question at a time, recommended answer with every question. This skill adds two superpowers:

1. **Challenge the plan against the project's existing knowledge** — vocabulary, prior decisions, code reality.
2. **Capture the resolutions in place** as the grill happens, so the next session doesn't start from zero.

Read `grill-me` first. Everything there still applies. This skill layers the docs work on top.

## Principle: discover, don't impose

This skill **does not assume** any particular directory layout, file naming convention, or doc format. Some projects keep their glossary in `CONTEXT.md`; some in `README.md`; some in a Notion page linked from `README.md`; some in a single `docs/` folder with mixed content; some scatter it across module READMEs; some have nothing yet.

Your first move is to find what's there. Your second move is to work with it. Imposing a layout on a project that already has one is how docs get abandoned.

## Phase 1 — Discover what's there

Before the first question, scan the repo and report what you find. Look in this order, stopping as soon as you have a coherent picture:

1. **Root signals.** `README.md`, `CONTRIBUTING.md`, `CONTEXT.md`, `GLOSSARY.md`, `ARCHITECTURE.md`, `NOTES.md`, `docs/`, `documentation/`. Read any that exist.
2. **Decision records.** Common conventions: `docs/adr/`, `docs/decisions/`, `decisions/`, `architecture/`, `rfcs/`. Also one-file conventions like `DECISIONS.md`. List what's there and the format used.
3. **External references.** Inside the READMEs and `package.json` / equivalent, look for Notion / Confluence / Obsidian / Linear / GitHub Wiki links that hold the canonical glossary.
4. **Per-module conventions.** Some projects keep `README.md` or `CONTEXT.md` per module/package. Check the top-level subdirectories.
5. **Implicit conventions.** Naming patterns in code that already encode a domain language (e.g., `Wallet`, `Coin`, `Customer` recur in module names — those terms are part of the de facto glossary even if not written down).

Report a one-line summary of what exists, e.g.:

> Found a root `README.md` with a "Concepts" section listing 8 domain terms. No ADRs in any standard folder, but `docs/decisions.md` exists as a single file. Per-module READMEs in `apps/` use the same vocabulary. Notion link in root README labelled "Engineering Wiki" — assuming live but unread.

## Phase 2 — Confirm where new things go

Before adding anything, **ask the user** where new artifacts should land. Use what you found in Phase 1 to make a concrete proposal, not an open question.

Examples of good proposals:

- "You have `docs/decisions.md` as a single-file ADR log. Should I append new decisions there, or start a folder convention like `docs/decisions/0001-x.md`?"
- "I don't see any glossary. Want me to add a `## Glossary` section to the existing root `README.md`, or create `CONTEXT.md` at root, or extend the Notion page?"
- "Decisions live in Notion per your README. Should I draft the ADR here and you paste it into Notion, or do you want me to create a local `docs/decisions/` shadow for in-repo decisions only?"

Wait for confirmation. **Never silently invent a directory layout.**

## Phase 3 — The grill itself

Now run the grilling protocol from [`grill-me`](../grill-me/SKILL.md), with three additions that exploit the docs you discovered.

### Challenge against the glossary

When the user uses a term that conflicts with — or shifts the meaning of — an existing term in the project's de facto vocabulary, call it out immediately. The glossary lives wherever Phase 1 found it: a README section, a `CONTEXT.md`, a Notion page, recurring class names in the code.

> "The README defines 'cancellation' as voiding the whole order. You're describing partial cancellation. Which is it — same term, new meaning? Or new term?"

Two valid outcomes:
1. **Glossary wins** — re-frame the plan in the existing vocabulary.
2. **Plan wins** — update the glossary, and write a decision record if the change is meaningful.

What is **not** valid: letting both definitions coexist.

### Cross-reference with code

When the user states how something works, check whether the code agrees. Contradictions are findings.

> "Your code in `redemption.ts` always cancels whole orders. You just said partial cancellation is possible — which is right?"

Reality wins. If the code is wrong, that's a bug or refactor candidate. If the description is wrong, update the plan.

### Update in place, where the user said to put it

When a term is resolved or a decision is made, update the artifact **right then**, in the location confirmed in Phase 2. Don't batch updates to the end of the session.

For terms: add to the glossary using the project's existing format. If the file is markdown, use the format already in the file (table, bulleted list, definition-style). Don't introduce a new format.

For decisions: write a decision record using the project's existing convention. Whether that's an ADR file, a Notion page, or an appended section in a single doc — match what's there.

## Phase 4 — Offer decision records sparingly

A decision record is worth writing only when **all three** are true:

1. **Hard to reverse.** The cost of changing your mind later is meaningful.
2. **Surprising without context.** A future reader will wonder "why did they do it this way?"
3. **The result of a real trade-off.** There were genuine alternatives and you picked one for specific reasons.

If any of the three is missing, **skip the record**. Writing too many decision records is worse than writing too few — signal-to-noise drops and people stop reading them.

When a record is warranted, follow the project's existing convention. If the project doesn't have one yet and the user asks you to start one, propose the lightest format that fits — usually four sections: **Context, Decision, Alternatives Considered, Consequences**.

## Anti-patterns

- **Inventing a `docs/adr/` directory** because the agent has seen one before. If the project doesn't have ADRs, ask before creating any.
- **Editing files behind the user's back.** Phase 2's confirmation step is non-negotiable.
- **Batching glossary updates to the end of the session.** Mid-session updates compound; end-of-session updates often get lost.
- **Forcing the same template every project.** Match what's there. The project's existing inconsistencies are usually load-bearing.
- **Treating an external link as a black box.** If the README points at a Notion glossary, ask the user to paste in the relevant section before grilling against it.

## Output

After the session you should have:

- A short written plan paragraph (same as `grill-me`).
- Inline updates to the project's existing knowledge artifacts, wherever they live.
- Zero or more new decision records in the project's existing convention.
- An explicit list of any unresolved questions, with owners.
