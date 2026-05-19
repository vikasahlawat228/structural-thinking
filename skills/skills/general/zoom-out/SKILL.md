---
name: zoom-out
description: Pull the agent up a level and force broader context — explain code in terms of the whole system, not just the lines on screen. Use when an agent is producing locally-correct but globally-wrong code, when joining an unfamiliar codebase, or when you need a higher-level perspective on a change before committing to it.
---

# Zoom Out

Agents are very good at writing code that is correct in the file they can see. They are bad at noticing that the file shouldn't exist, or should have been in a different module, or contradicts a decision made elsewhere.

This skill forces a wide-angle pass.

## When to use it

- The agent is making changes that look right in isolation but feel wrong in review.
- You're new to a codebase and need orientation before changing anything.
- A slice is about to land and you want one last system-level sanity check.
- You're explaining the change to a stakeholder who doesn't have the file context loaded.

## How to do it

Force the agent to answer these questions, in writing, before continuing:

### 1. Where does this live?

> "In this codebase, this change touches the `<module>` module. That module is responsible for `<one-sentence responsibility from CONTEXT.md>`."

If the agent can't name the responsibility — read `CONTEXT.md`. If `CONTEXT.md` doesn't define it — run [`grill-with-docs`](../grill-with-docs/SKILL.md).

### 2. Who calls this? Who does this call?

> "This function is called from `<list of callers>`. It calls into `<list of callees>`. The shape of the inputs is `<…>`; the shape of the outputs is `<…>`."

If the agent can only name immediate callers, that's fine for small changes. For medium and large, walk the graph two hops.

### 3. What invariants is this change touching?

> "Before this change, the invariant `<X>` held. After this change, the invariant becomes `<Y>`. The places that depended on `<X>` are `<list>`. They will continue to work because `<why>`."

If you can't name an invariant, you may not be changing what you think you're changing.

### 4. Has anyone already decided this?

> "I checked `docs/adr/`. The relevant ADRs are `<list>`. This change is consistent / inconsistent with them because `<why>`."

If inconsistent, surface it **now**, before code lands. Either the ADR is wrong (write a superseding one) or the change is wrong (re-design).

### 5. What's the smallest change that delivers the value?

> "I could make this change in `<N>` files. The smallest version that delivers the user-visible value is `<…>`. The rest is `<scope creep / future slice / dead weight>`."

This is the cheap pre-PR review the agent does on itself.

## Output

A short paragraph or bulleted list with the answers. Paste it into the session before any further code change.

## Anti-patterns

- **Zooming out as a stalling tactic.** This is a context check, not an excuse to redesign. If the answers are fine, ship and move on.
- **Zooming out on a typo fix.** Don't. The wrong-sized process for a small change.
- **Zooming out without `CONTEXT.md`.** You're guessing at the system from filenames. Build the doc first.
