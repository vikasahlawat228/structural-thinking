---
name: medium-feature
description: Plan → slice → ship loop for a user-visible feature that fits in one bounded context and 1–5 days of work. Use when the orchestrator classifies the task as a medium feature, or when the user describes a new feature/capability that spans a handful of files in a single domain.
---

# Medium Feature Workflow

A medium feature is "small enough to fit in your head, big enough to mis-specify." The cheap mistakes are: starting to code before the interface is decided, and writing one giant test file before the implementation exists.

This workflow has five phases. Each phase has a clear exit condition; do not skip a phase, but do **not** dwell in any of them.

## Phase 1 — Grill the request

Run [`grill-me`](../../../general/grill-me/SKILL.md). Goal: surface the decision tree before you touch code.

Specifically, force the user to choose on:

- **Interface shape** — what does the new function / component / endpoint look like from the caller's side?
- **Failure modes** — what should happen on the unhappy paths?
- **Out-of-scope** — what is explicitly *not* part of this feature?
- **Acceptance signal** — what would let us declare it done?

Stop the grill when there are no open branches with material consequences. Do not grill for grilling's sake.

**Exit condition:** a written paragraph the user agrees with, covering interface + behaviour + out-of-scope.

## Phase 2 — Write the PRD-lite

Run [`to-prd`](../../to-prd/SKILL.md). For a medium feature, the PRD is short — one page, max.

Sections, in order:

1. **Why** — problem in one sentence.
2. **Interface** — the exact public-facing shape (function signature, screen sketch, endpoint).
3. **Behaviour** — happy path + named failure modes.
4. **Out of scope** — bullets.
5. **How we'll know it works** — acceptance criteria expressed as test names.

Submit as a GitHub issue (or your tracker equivalent) if the team works that way. Otherwise paste it into the conversation and have the user confirm it.

**Exit condition:** PRD-lite agreed in writing.

## Phase 3 — Slice into tracer bullets

Run [`to-issues`](../../to-issues/SKILL.md). Break the feature into **vertical slices** — each slice must be:

- Independently shippable (could be merged on its own and add value).
- One test-and-implement cycle to deliver.
- Named after a behaviour, not a layer (`signup_accepts_referral_code`, not `add_referral_column`).

A medium feature typically slices into 3–7 vertical slices. If you have more than 10, the task is actually a **`large-project`** — re-route.

**Exit condition:** an ordered list of slices. Each slice has a one-line acceptance test in plain English.

## Phase 4 — Implement, slice by slice

For each slice, run [`tdd`](../../tdd/SKILL.md):

```
RED → GREEN → REFACTOR → next slice
```

Hard rules:

- **One slice at a time.** Do not pre-write tests for slice 3 while implementing slice 1.
- **Tests on public interface only.** No mocking internal collaborators of the feature.
- **Refactor only on green.** Never refactor while a test is failing.
- **Commit per slice.** A slice that lands is one commit (or one PR) you could revert independently.

If a slice's TDD cycle exposes a hidden cross-cutting concern (auth, data model, naming clash with existing domain), pause. Resolve the cross-cutter explicitly — either inline (small) or by spinning out an ADR (worth recording).

## Phase 5 — Verify, document, ship

Before declaring done:

- [ ] All acceptance tests from Phase 2 pass.
- [ ] All slice-level tests pass.
- [ ] Manual smoke on the integrated flow — not just unit tests.
- [ ] PR description references the PRD-lite and lists the slices.
- [ ] If you introduced a new domain term, add it to `CONTEXT.md` (don't batch).
- [ ] If you made a hard-to-reverse decision, write an ADR.

Then run [`zoom-out`](../../../general/zoom-out/SKILL.md) once: how does this feature sit in the broader system? Anything to flag for `improve-architecture` later?

## Anti-patterns

- **Horizontal slicing.** "I'll write all the tests, then all the code." Stop. The tests will be wrong because they test imagined behaviour. Go vertical, one slice at a time.
- **Skipping the grill.** "I already know what to build." The fact that you "know" is itself the bug — surface the assumption before you code it.
- **Pre-grokking the whole feature.** Don't try to design every slice before writing slice 1. Each cycle informs the next.
- **Letting the PRD-lite balloon into a PRD.** For a medium feature, the page count is the discipline. If it wants to be longer, escalate to `large-project`.

## When to escalate

- The grill keeps producing new branches → escalate to **`large-project`**, run [`grill-with-docs`](../../../general/grill-with-docs/SKILL.md) instead.
- More than ~10 slices → escalate to **`large-project`**.
- Cross-context impact discovered (auth + payouts + admin) → escalate to **`large-project`**.

## When to compact

- If you're swapping agents or the session is running long, run [`handoff`](../../../general/handoff/SKILL.md) before continuing.
