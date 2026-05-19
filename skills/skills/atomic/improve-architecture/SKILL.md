---
name: improve-architecture
description: Find deepening opportunities and structural debt in a codebase. Informed by the domain language in CONTEXT.md and the decisions in docs/adr/. Use as a periodic pass (weekly on large projects) or when agents start producing locally-correct but globally-wrong code. Trigger phrases include "refactor pass", "clean up architecture", "deepen modules", "ball of mud".
---

# Improve Architecture

Codebases entropy. With AI agents, they entropy faster — agents can write a lot of plausible code that gradually shallows every module. The defence is a **periodic, deliberate deepening pass**.

This skill is **not** for mid-slice refactoring. Slices ship; refactors get their own slices.

## When to run

- On a large project: weekly, or on a tangible signal that agents are getting confused.
- When a diff review surfaces the same architectural smell three times in a row.
- Before starting a new milestone — clean ground beats messy ground.
- After landing a difficult bug fix that exposed a structural problem.

**Do not** run this in the middle of a feature. Finish the feature, then run this on a clean tree.

## Step 1 — Re-read the load-bearing docs

Before touching code:

1. Read `CONTEXT.md` — what is the domain supposed to look like?
2. Read `docs/adr/` — what decisions are we honouring?
3. Re-skim the most recent PRDs — what was the recent direction of travel?

If `CONTEXT.md` is stale relative to what you know about the project, that itself is the first problem. Update it before continuing.

## Step 2 — Hunt for the four smells

Walk through the codebase looking for:

### Smell 1 — Shallow modules

A module with a small interface **and** a small body. It earns nothing — the indirection costs more than it gives back.

> "John Ousterhout: the best modules are deep. They allow a lot of functionality to be accessed through a simple interface."

Fix: either collapse the module into its caller, or absorb related functionality into it so the body grows to match the cost of the interface.

### Smell 2 — Chatty interfaces

Two modules constantly call back and forth. Each call is shallow; the cost is in the round-trip.

Fix: either merge them, or extract the chatty surface into a third module that owns the conversation.

### Smell 3 — Domain drift

Code names don't match `CONTEXT.md`. Variables named `account` when the glossary says `user`. Functions named `cancelOrder` when the glossary says `voidOrder`.

Fix: rename the code, **or** update the glossary if the code's term is genuinely better. Don't let them drift.

### Smell 4 — Repeated coupling

The same three files always change together. They are one module wearing a costume.

Fix: collapse them, or extract the shared concept into a fourth module that they all depend on (and that depends on no other internals).

## Step 3 — Prioritise

For each smell you spot, score:

- **Reach** — how many parts of the codebase does it touch?
- **Confusion cost** — how often does an agent or reviewer trip on this?
- **Fix cost** — small / medium / large refactor?

Pick the **two or three** highest-reach, lowest-fix-cost items. Do **not** try to fix everything.

## Step 4 — Refactor in slices

Each refactor is its own vertical slice (just like a feature). Apply [`tdd`](../tdd/SKILL.md):

1. The existing tests should already cover the behaviour you're refactoring. If they don't, **add tests first** — capture behaviour before you change structure.
2. Refactor. Run tests after every step.
3. Commit per slice. Each refactor commit could be reverted independently.

If a refactor requires changing a public interface that other modules consume, that's a separate slice **and** an ADR.

## Step 5 — Record and re-baseline

Once the refactor lands:

- Update `CONTEXT.md` if any term moved or sharpened.
- If a previous ADR is now superseded, mark it (don't delete).
- Note in the PR description **why** you made the change (which smell, what reach).
- Add the refactor's commit / PR to a running `docs/refactor-log.md` if you keep one.

## Anti-patterns

- **Big-bang refactor.** Refactors that touch dozens of files in one commit cannot be reviewed. Slice them.
- **Refactor mid-feature.** Finish the feature; the refactor is its own slice.
- **Refactoring without tests.** If you can't prove behaviour was preserved, you didn't refactor; you rewrote.
- **Refactoring everything you spotted.** Pick two or three. Defer the rest. The point of periodic passes is that there will be a next one.
- **Renaming to match a vibe.** Renames must match `CONTEXT.md` or fix a real confusion. Aesthetic renames are noise.

## When this skill isn't enough

- The structural problem is a missing decision, not bad code → write an ADR via [`grill-with-docs`](../grill-with-docs/SKILL.md).
- The problem is a single-file mess → fix it in-flight, no skill needed.
- The smell touches multiple bounded contexts → escalate to a small architecture project; run [`large-project`](../../workflows/large-project/SKILL.md) on the refactor itself.
