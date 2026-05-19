---
name: small-change
description: Tight reproduce → isolate → fix → verify loop for bugs, small fixes, and single-purpose changes. Use when the orchestrator classifies the task as a small change, or when the user says "quick fix", "bug", "tweak", "small change", "one-line", or describes a single-file/single-function issue.
---

# Small Change Workflow

A bug or a small change is **not** a small task. It is a task with a small **diff**. The process should be tight, but every phase still earns its place: skipping reproduction or skipping verification is the most common source of "fixed" bugs that aren't fixed.

This workflow runs in five phases. On a true small change, the whole thing fits in one focused session.

## Phase 0 — Confirm sizing (10 seconds)

The orchestrator probably sent you here. Sanity check:

- One module / one file most likely?
- Reversible without ceremony?
- Spec unambiguous?

If any of these is "no", stop and re-route to **`medium-feature`** or **`large-project`**. Don't be a hero.

## Phase 1 — Build a feedback loop

This is the core move. If you have a fast, deterministic, agent-runnable pass/fail signal for the bug, the fix is mechanical. If you don't, you're guessing.

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

## Phase 2 — Minimise

Strip the reproduction down. Remove anything that isn't load-bearing for triggering the bug. Each step should keep the bug reproducing.

Goals:

- Smallest possible input that triggers the failure.
- Shortest call path from the input to the failure.
- No incidental state — only what's required.

Time-box this to ~15 minutes. If minimisation isn't working, you have a tangled-coupling bug; escalate to `diagnose`.

## Phase 3 — Hypothesise

Generate **three ranked hypotheses** before changing any code. One-hypothesis debugging anchors on the first plausible idea.

Each must be **falsifiable**:

> "If \<X\> is the cause, then \<probe Y\> will make the bug \<disappear / get worse\>."

Cheap checkpoint: show the ranked list to the user. They often have context that re-ranks it instantly ("we just deployed a change to #2"). Proceed with your own ranking if they're AFK.

## Phase 4 — Fix + regression test

Order matters:

1. Write the regression test **first** at the right seam. Watch it fail.
2. Make the smallest change that turns it green. Resist scope creep.
3. Re-run the full Phase-1 feedback loop against the original (un-minimised) scenario.
4. Run the broader test suite for the affected module — make sure you didn't break a neighbour.

**No regression test exists where it should?** That itself is a finding. Either add a missing seam, or note explicitly that the architecture prevents locking this bug down. Don't hand-wave it.

## Phase 5 — Cleanup + ship

Required before declaring done:

- [ ] Original repro no longer reproduces.
- [ ] Regression test exists and passes.
- [ ] All `[DEBUG-...]` log tags grep'd out.
- [ ] Throwaway prototypes / harness scripts deleted (or moved to a clearly-marked debug location).
- [ ] Commit message states the winning hypothesis — so the next debugger learns.

**Then ask once:** *what would have prevented this bug?* If the answer is architectural (no good test seam, tangled callers, hidden coupling), file it for [`improve-architecture`](../../improve-architecture/SKILL.md). Do not refactor mid-fix.

## When to escalate

- The "small change" reveals a schema migration, public API change, or multi-module ripple → re-route to **`medium-feature`**.
- The bug needs domain-language clarification before it can even be defined → run [`grill-me`](../../../general/grill-me/SKILL.md) first.
- The fix exposes a structural problem → finish the fix narrowly, then hand off to [`improve-architecture`](../../improve-architecture/SKILL.md).

## Anti-patterns

- **Patching the symptom.** If the regression test is "the wrong-currency string isn't shown", the fix probably isn't to suppress the string — find why the wrong currency arrives in the first place.
- **Cleaning up code "while you're in there".** Stay narrow. Architectural improvements go in a separate diff.
- **Skipping the loop because the bug "looks obvious".** The obvious bug is rarely the real one. Run the loop.
- **Declaring done before re-running the original scenario.** A passing minimised test is not proof that the original repro is fixed.
