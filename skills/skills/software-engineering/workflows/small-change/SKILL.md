---
name: small-change
description: Tight reproduce → isolate → fix → verify loop for bugs, small fixes, and single-purpose changes. Use when start classifies the task as a small change, or when the user says "quick fix", "bug", "tweak", "small change", "one-line", or describes a single-file/single-function issue.
category: software
---

# Small Change Workflow

A bug or a small change is **not** a small task. It is a task with a small **diff**. The process should be tight, but every phase still earns its place: skipping reproduction or skipping verification is the most common source of "fixed" bugs that aren't fixed.

This workflow runs in six phases. On a true small change, the whole thing fits in one focused session.

## Phase 0 — Confirm sizing (10 seconds)

`start` probably sent you here. Sanity check:

- One module / one file most likely?
- Reversible without ceremony? (Two-way door, not one-way.)
- Spec unambiguous?
- Blast radius small?

If any of these is "no", stop and re-route to `medium-feature` or `large-project`. Don't be a hero.

## Phase 1 — Tidy vs Behavior gate

Before any change, classify what you're about to commit:

- **Tidy change** — structural, no observable behaviour change. Rename, extract, reorder, format, dead-code removal.
- **Behaviour change** — bug fix, new logic, behaviour-altering refactor.

**Never combine the two in one commit.** Mixing them is the single most reviewed-cited TDD/refactor anti-pattern: reviewers can't tell what changed, and revert becomes a coordination problem.

If the change is tidy, ship it as a tidy commit. Don't go through the bug-fix flow below — skip to a small test/regression check and ship.

If the change is behavioural, continue.

## Phase 2 — Build a feedback loop

This is the core move. If you have a fast, deterministic, agent-runnable pass/fail signal for the change, the work is mechanical. If you don't, you're guessing.

Try, in order:

1. **Failing test** at the right seam (unit > integration > e2e). Fastest signal.
2. **Curl / HTTP script** against the dev server.
3. **CLI invocation** with a fixture input, diffed against known-good output.
4. **Headless browser script** (Playwright) for UI bugs.
5. **Replay a captured trace** — save a real payload, replay it through the code path.
6. **Throwaway harness** — minimal subset that exercises the bug with a single call.

When the bug is non-deterministic, the goal is **higher reproduction rate**, not a clean repro. Loop 100×, parallelise, narrow timing windows.

If you genuinely cannot build a loop: stop, list what you tried, and ask for access / artifacts / instrumentation permission. Do **not** proceed to fix without a loop.

Hand off to [`diagnose`](../../diagnose/SKILL.md) for the full feedback-loop discipline if the bug fights back.

## Phase 3 — Minimise

Strip the reproduction down. Remove anything that isn't load-bearing for triggering the bug. Each step should keep the bug reproducing.

Goals:

- Smallest possible input that triggers the failure.
- Shortest call path from the input to the failure.
- No incidental state — only what's required.

Time-box this to ~15 minutes. If minimisation isn't working, you have a tangled-coupling bug; escalate to [`diagnose`](../../diagnose/SKILL.md).

## Phase 4 — Hypothesise

Generate **three ranked hypotheses** before changing any code. One-hypothesis debugging anchors on the first plausible idea.

Each must be **falsifiable**:

> "If \<X\> is the cause, then \<probe Y\> will make the bug \<disappear / get worse\>."

Cheap checkpoint: show the ranked list to the user. They often have context that re-ranks it instantly ("we deployed a change to #2 yesterday"). Proceed with your own ranking if they're AFK.

If the step from suspicion to "I understand why" took more than 30 minutes, the step was too big — **halve it**. (MMMSS — many more, much smaller steps.) Re-state the next probe as something you can run in five minutes.

## Phase 5 — Fix + regression test

Order matters:

1. **Write the regression test first** at the right seam. Watch it fail.
2. Make the **smallest change** that turns it green. Resist scope creep.
3. Re-run the full Phase-2 feedback loop against the original (un-minimised) scenario.
4. Run the broader test suite for the affected module — make sure you didn't break a neighbour.

If you're in legacy code without a clear seam, use a **sprout** — add the new behaviour alongside the existing code (sprout method or sprout class), rather than editing in place. The original is preserved; the new behaviour is testable in isolation; the call site change is one line.

**No regression test exists where it should?** That itself is a finding. Either add a missing seam, or note explicitly that the architecture prevents locking this bug down. Don't hand-wave it.

## Phase 6 — Cleanup + ship

Required before declaring done:

- [ ] Original repro no longer reproduces.
- [ ] Regression test exists and passes.
- [ ] All `[DEBUG-...]` log tags grep'd out.
- [ ] Throwaway prototypes / harness scripts deleted (or moved to a clearly-marked debug location).
- [ ] Commit message states the winning hypothesis — so the next debugger learns.
- [ ] **Revert-cost check.** If reverting this commit would require a coordination meeting, the change wasn't actually small — flag it and re-classify.

**Then ask once:** *what would have prevented this bug?* If the answer is architectural (no good test seam, tangled callers, hidden coupling), file it for [`improve-architecture`](../../improve-architecture/SKILL.md). Do not refactor mid-fix.

## When to escalate

- The "small change" reveals a schema migration, public API change, or multi-module ripple → re-route to `medium-feature`.
- The bug needs domain-language clarification before it can even be defined → run [`grill-me`](../../../general/grill-me/SKILL.md) first.
- The fix exposes a structural problem → finish the fix narrowly, then hand off to [`improve-architecture`](../../improve-architecture/SKILL.md).
- The change touches a file that's edited by multiple people in flight → coordinate first.

## Anti-patterns

- **Patching the symptom.** If the regression test is "the wrong-currency string isn't shown", the fix is probably not to suppress the string — find why the wrong currency arrives.
- **Cleaning up code "while you're in there".** Stay narrow. Tidyings go in a separate commit (Phase 1's gate).
- **Skipping the loop because the bug "looks obvious".** The obvious bug is rarely the real one. Run the loop.
- **Declaring done before re-running the original scenario.** A passing minimised test is not proof the original repro is fixed.
- **Step too big.** If a probe takes more than 30 minutes, halve it. MMMSS.
- **Reverts that need coordination.** A revert that requires a meeting is the tell that the change wasn't small.
