---
name: grill-with-docs
description: Grilling session that challenges your plan against the existing domain model, sharpens terminology, and updates CONTEXT.md and ADRs inline as decisions crystallise. Use when stress-testing a plan against a real codebase with documented language and decisions, especially at the start of a medium or large piece of work. Prefer this over grill-me when the project already has a CONTEXT.md or docs/adr.
---

# Grill With Docs

Same energy as [`grill-me`](../grill-me/SKILL.md), but with two superpowers: you challenge the plan against the **existing domain glossary** and you **capture the resolutions inline**, so the next session doesn't start from zero.

Read the rules in `grill-me` first. Everything there still applies. This skill adds the docs layer.

## Locate the existing docs

Before the first question, scan the repo for:

```
/
├── CONTEXT.md                # single-context project
├── CONTEXT-MAP.md            # multi-context project; points to per-module CONTEXT.md
├── docs/
│   └── adr/                  # system-wide ADRs
└── src/
    └── <module>/
        ├── CONTEXT.md        # per-module glossary (multi-context only)
        └── docs/adr/         # per-module ADRs
```

Create files **lazily**. Don't pre-create `CONTEXT.md` "to be ready". Create it the first time a term is resolved. Same for `docs/adr/`.

## During the session

### Challenge against the glossary

When the user uses a term that conflicts with the existing language in `CONTEXT.md`, call it out **immediately**.

> "Your glossary defines 'cancellation' as X, but you seem to mean Y — which is it?"

Two outcomes are valid:
1. The glossary wins — re-frame the plan in its vocabulary.
2. The plan wins — update the glossary (and write an ADR if the change is meaningful).

What is **not** valid: letting both definitions live in the codebase.

### Sharpen fuzzy language

When the user uses an overloaded term, propose a precise canonical one.

> "You're saying 'account' — do you mean the Customer or the User? Those are different things in this codebase. Let's settle on one for this plan."

The point is not to be pedantic; it's that the agent will spend tokens disambiguating every time, and so will every future reader.

### Cross-reference with code

When the user states how something works, check the code. If you find a contradiction, surface it.

> "Your code cancels entire Orders, but you just said partial cancellation is possible — which is right?"

Reality wins. If the code is wrong, that's a bug or refactor candidate. If the description is wrong, update the plan.

### Stress-test with scenarios

When a domain relationship is being discussed, invent concrete scenarios that probe the edges.

> "OK — so what happens if a user redeems a coin that was issued in INR but their wallet currency just changed to USD? Walk me through it."

Force precise answers. "It just works" is not an answer.

### Update CONTEXT.md inline

When a term is resolved, update `CONTEXT.md` **right there in the session**. Don't batch. The format lives in [`CONTEXT-FORMAT.md`](./CONTEXT-FORMAT.md).

Rules:
- Only domain-meaningful terms. Don't add `useEffect` to the glossary.
- Definition is short. One or two lines.
- Cross-reference related terms.

### Offer ADRs sparingly

Only propose an ADR when **all three** are true:

1. **Hard to reverse.** Cost of changing your mind later is meaningful.
2. **Surprising without context.** A future reader will wonder why this choice was made.
3. **The result of a real trade-off.** There were genuine alternatives; you picked one for specific reasons.

If any of the three is missing, skip the ADR — it's not load-bearing documentation, it's noise. The format lives in [`ADR-FORMAT.md`](./ADR-FORMAT.md).

## Output

At the end of the session you should have:

- An agreed plan paragraph (same as `grill-me`).
- Zero or more updates to `CONTEXT.md`.
- Zero or more new ADRs in `docs/adr/`.
- An ordered list of any explicitly punted questions.

The plan paragraph feeds into [`to-prd`](../to-prd/SKILL.md). The docs persist across sessions and pay back compound interest.
