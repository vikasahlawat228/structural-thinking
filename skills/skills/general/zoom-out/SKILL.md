---
name: zoom-out
description: Pull the agent up a level and force broader context — explain a change, a plan, or a finding in terms of the whole system, not just the lines on screen. Works for code, docs, decisions, plans, org topology. Use when an agent (or you) is producing locally-correct but globally-wrong work, when joining an unfamiliar codebase, or when you need a higher-level perspective before committing.
category: both
---

# Zoom Out

Agents and humans are good at producing work that is correct in the file in front of them. Both are bad at noticing that the file shouldn't exist, contradicts a decision made elsewhere, or sits inside a system whose goals make the local change irrelevant.

This skill forces a wide-angle pass. It applies to code, but the same protocol works for product plans, organisational decisions, or any change embedded in a larger system.

## When to use it

- The change looks right in isolation but feels wrong in review.
- You're new to a codebase / domain / org and need orientation before changing anything.
- A slice is about to land and you want one last system-level sanity check.
- You're explaining the change to a stakeholder who doesn't have your context loaded.
- The diff spans more than three modules, or you've reached for a workaround twice, or you're about to write `// TODO: refactor later`.

## The five questions

Force a written answer to each before continuing. Skip questions only when the work genuinely doesn't touch their dimension.

### 1. Where does this live?

> "This change touches the `<module / area / context>`. Its responsibility is `<one sentence>`. The next level out is `<container / service / org unit>`."

Walk a four-level ladder when in doubt — code → component → container → system context. The level you stop at is the level the change actually lives in.

If you can't name the responsibility, find the project's existing glossary or context doc and read it. If none exists, run [`grill-with-docs`](../grill-with-docs/SKILL.md) before continuing.

### 2. Who calls this? Who does this call?

> "This is called from `<list of callers>`. It calls into `<list of callees>`. Input shape: `<…>`. Output shape: `<…>`."

Walk the graph two hops out for medium and large changes. For small changes, immediate callers are enough. **The greatest leverage in a system is at the interfaces** — the contracts between modules are usually where the real cost of a change lives.

### 3. What invariants is this change touching?

> "Before this change, invariant `<X>` held. After, the invariant becomes `<Y>`. Places that depended on `<X>`: `<list>`. They continue to work because `<why>`."

If you can't name an invariant, you may not be changing what you think you're changing. Invariants live at the interfaces (Q2) more often than in the code body.

### 4. Has anyone already decided this?

> "I checked `<wherever this project records decisions — docs/adr/, decisions/, a Notion page, a DECISIONS.md, README sections>`. Relevant prior decisions: `<list>`. This change is consistent / inconsistent because `<why>`."

This is **decision-record archaeology** — read before reasoning fresh. Discover whatever convention the project uses; don't assume `docs/adr/`. If inconsistent, surface it **now**, before code lands. Either the prior decision is wrong (write a superseding one) or this change is wrong (re-design).

### 5. What level am I actually intervening at?

The most often-missed question. Most local fixes try to change parameters when they should be questioning rules; or rules when they should be questioning goals. The ladder, from weakest to strongest leverage:

- **Parameter** — a number or threshold.
- **Information flow** — who sees what, when.
- **Rule** — a policy, a constraint, an invariant.
- **Goal** — what we're optimising for.
- **Paradigm** — the model we use to make sense of the system.

If you're about to tweak a parameter, ask: is the rule wrong? If you're about to add a rule, ask: is the goal wrong?

### 6. What's the smallest change that delivers the value?

> "I could make this change in `<N>` places. The smallest version that delivers the user-visible value is `<…>`. The rest is `<scope creep / future slice / dead weight>`."

The cheap pre-PR review the agent does on itself. If the smallest version doesn't deliver value, the change isn't actually framed correctly — go back to Q1.

## Sociotechnical sweep (for medium+ changes)

When a change is non-trivial, run a one-line check on the human system around it:

- Which team / person owns this module?
- Is this a stream-aligned change (their feature) or a platform change in disguise (their dependency)?
- If it crosses team boundaries, the social cost dominates the technical cost — plan accordingly.

The team topology is part of the architecture, not separate from it.

## Output

A short bulleted list or paragraph with the answers. Paste it into the session before any further code change.

For a non-trivial change, the zoom-out output also belongs in the PR description.

## Anti-patterns

- **Zooming out as a stalling tactic.** This is a context check, not an excuse to redesign. If the answers come out clean, ship.
- **Zooming out on a typo fix.** Wrong-sized process for a small change.
- **Zooming out without a glossary.** You're guessing at the system from filenames. Read whatever vocabulary exists first; if none exists and the work warrants it, run [`grill-with-docs`](../grill-with-docs/SKILL.md).
- **Grand-redesign reflex.** Gall's Law: a complex system that works has always evolved from a simple system that worked. Prefer evolving the working system over rewriting.
- **Treating "I checked the decisions" as a checkbox.** If you're not citing specific prior decisions by name, you didn't check.
