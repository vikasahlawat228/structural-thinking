---
name: medium-feature
description: Plan → slice → ship loop for a user-visible feature that fits in one bounded context and 1–5 days of work. Use when start classifies the task as a medium feature, or when the user describes a new feature/capability that spans a handful of files in a single domain.
category: software
---

# Medium Feature Workflow

A medium feature is "small enough to fit in your head, big enough to mis-specify." The cheap mistakes are: starting to code before the interface is decided, and writing one giant test file before the implementation exists.

Six phases. Each has a clear exit condition; do not skip a phase, but do **not** dwell in any of them.

## Phase 1 — Set the appetite

Before grilling, name the **appetite** — how much is this *worth* in time and attention. Shape Up framing: fix the time, vary the scope.

For a medium feature the appetite is usually 1–5 days. If the user can't commit to an appetite, you don't have a medium feature — re-route to `large-project` or push back.

The appetite is the budget against which scope gets cut. When Phase 3 produces more slices than the appetite holds, slices get dropped, not the appetite extended.

## Phase 2 — Grill the request

Run [`grill-me`](../../../general/grill-me/SKILL.md). Goal: surface the decision tree before you touch code.

Force the user to choose on:

- **Interface shape** — what does the new function / component / endpoint look like from the caller's side?
- **Failure modes** — what happens on the unhappy paths?
- **Out-of-scope** — what is explicitly *not* part of this feature?
- **Acceptance signal** — what lets us declare it done?

Stop the grill when there are no open branches with material consequences. Don't grill for grilling's sake.

If the project already has a glossary / decisions / domain language, use [`grill-with-docs`](../../../general/grill-with-docs/SKILL.md) instead — challenge the plan against existing knowledge.

**Exit condition:** a written paragraph the user agrees with, covering interface + behaviour + out-of-scope.

## Phase 3 — Write the PRD-lite

Run [`to-prd`](../../to-prd/SKILL.md). For a medium feature, one page max.

Required sections (per `to-prd`): Why, press-release-style summary, Interface, Behaviour, Out of scope, Acceptance signals, FAQ. Cagan's four-risks check before declaring complete.

**Exit condition:** PRD-lite agreed in writing, filed wherever the project keeps PRDs.

## Phase 4 — Walking skeleton + slice plan

Run [`to-issues`](../../to-issues/SKILL.md) to break the feature into vertical slices.

**The first slice is always a walking skeleton** if this feature touches a new surface, endpoint, or subsystem — end-to-end, deployable, does almost nothing, but every layer is wired. Skip the skeleton only when extending a surface that already exists.

Then break the rest into vertical slices. Each slice:

- Independently shippable (could be merged on its own and add value).
- One TDD cycle to deliver.
- Named after a behaviour, not a layer (`signup_accepts_referral_code`, not `add_referral_column`).

A medium feature typically slices into 3–7 vertical slices including the skeleton. If you produce more than ~10, the task is actually a `large-project` — re-route.

**Exit condition:** ordered list of slices. Each slice has a one-line acceptance test in plain English. Walking-skeleton slice is explicit if present.

## Phase 5 — Implement, slice by slice

For each slice, run [`tdd`](../../tdd/SKILL.md):

```
RED → GREEN → REFACTOR → next slice
```

Hard rules:

- **One slice at a time.** Do not pre-write tests for slice 3 while implementing slice 1.
- **Tests on public interface only.** No mocking internal collaborators (classicist default — see `tdd`).
- **Refactor only on green.** Never refactor while a test is failing.
- **Commit per slice.** Each slice is one revertible unit. Tidyings get their own commit per `small-change` Phase 1.
- **End each slice in a deployable state**, even if dark-launched behind a flag. The handoff to the next slice is via merged-and-green code, not WIP.

If a slice's TDD cycle exposes a hidden cross-cutting concern (auth, data model, naming clash with existing domain), pause. Resolve the cross-cutter explicitly — either inline (small) or by spinning out a decision record (per `grill-with-docs`).

## Phase 6 — Verify, document, ship

Before declaring done:

- [ ] All acceptance signals from Phase 3 pass.
- [ ] All slice-level tests pass.
- [ ] Manual smoke on the integrated flow — not just unit tests.
- [ ] PR description references the PRD-lite and lists the slices.
- [ ] New domain terms added to the project's glossary (wherever it lives — per `grill-with-docs`).
- [ ] Hard-to-reverse decisions recorded in the project's decision-record convention.

Then run [`zoom-out`](../../../general/zoom-out/SKILL.md) once: how does this feature sit in the broader system? Anything to flag for [`improve-architecture`](../../improve-architecture/SKILL.md) later?

If you're swapping agents or the session is running long, run [`handoff`](../../../general/handoff/SKILL.md) before continuing — between phases, not within them.

## Anti-patterns

- **Horizontal slicing.** "I'll write all the tests, then all the code." Stop. The tests will be wrong because they test imagined behaviour. Go vertical, one slice at a time.
- **Skipping the grill.** "I already know what to build." The fact that you "know" is itself the bug — surface the assumption before you code it.
- **Pre-grokking the whole feature.** Don't try to design every slice before writing slice 1. Each cycle informs the next.
- **Letting the PRD-lite balloon into a PRD.** For a medium feature, page count is discipline. If it wants to be longer, escalate to `large-project`.
- **Skipping the walking skeleton.** New surfaces without an end-to-end skeleton fight the architecture instead of using it.
- **Scope creep against the appetite.** Drop slices, don't extend the appetite. That's the deal Shape Up makes with you.

## When to escalate

- The grill keeps producing new branches → escalate to `large-project`, use [`grill-with-docs`](../../../general/grill-with-docs/SKILL.md).
- More than ~10 slices → escalate to `large-project`.
- Cross-context impact discovered (touches multiple bounded contexts) → escalate to `large-project`.
- Appetite gets blown by ~50% → stop, reassess; either cut scope hard or re-route to `large-project`.
