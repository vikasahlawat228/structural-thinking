---
name: diagnose
description: Disciplined diagnosis loop for hard bugs and performance regressions. Reproduce → minimise → hypothesise → instrument → fix → regression-test. Use when a bug resists casual fixing, when reports say "intermittent", "slow", "sometimes wrong", or when the user says "diagnose this", "debug this", "why is this broken".
---

# Diagnose

A discipline for hard bugs. Skip phases only when explicitly justified — and only in writing.

When exploring the codebase, read the project's `CONTEXT.md` for a mental model of the relevant modules, and check `docs/adr/` for related decisions.

## Phase 1 — Build a feedback loop

**This is the skill.** Everything else is mechanical. If you have a fast, deterministic, agent-runnable pass/fail signal for the bug, you will find the cause. If you don't, no amount of code-staring will save you.

Spend disproportionate effort here. Be aggressive. Be creative. Refuse to give up.

### Ways to construct one — try them in roughly this order

1. **Failing test** at whichever seam reaches the bug — unit, integration, e2e.
2. **Curl / HTTP script** against a running dev server.
3. **CLI invocation** with a fixture input, diffing stdout against a known-good snapshot.
4. **Headless browser script** (Playwright / Puppeteer) for UI bugs.
5. **Replay a captured trace.** Save a real payload to disk; replay it through the code path.
6. **Throwaway harness.** Spin up a minimal subset of the system that exercises the bug with one call.
7. **Property / fuzz loop.** If the bug is "sometimes wrong", run 1000 random inputs.
8. **Bisection harness.** If the bug appeared between two known states, automate "boot at state X, check, repeat" — then `git bisect run`.
9. **Differential loop.** Run the same input through old-vs-new and diff outputs.
10. **HITL bash script.** Last resort. If a human must click, drive *them* with a structured script so the loop is still measurable.

### Iterate on the loop itself

Treat the loop as a product. Once you have *a* loop, ask:

- Can it be faster? Cache setup, skip unrelated init, narrow scope.
- Can the signal be sharper? Assert on the specific symptom, not "didn't crash".
- Can it be more deterministic? Pin time, seed RNG, isolate filesystem, freeze network.

A 30-second flaky loop is barely better than no loop. A 2-second deterministic loop is a debugging superpower.

### Non-deterministic bugs

Goal: **higher reproduction rate**, not necessarily a clean repro. Loop 100×, parallelise, add stress, narrow timing windows, inject sleeps. A 50%-flake bug is debuggable; 1% is not — keep raising the rate until it is.

### When you genuinely cannot build a loop

Stop and say so explicitly. List what you tried. Ask the user for:

- (a) access to whatever environment reproduces it,
- (b) a captured artifact (HAR file, log dump, core dump, screen recording with timestamps), or
- (c) permission to add temporary production instrumentation.

Do **not** proceed to hypothesise without a loop.

## Phase 2 — Reproduce

Run the loop. Watch the bug appear. Confirm:

- The loop produces the failure the user described — not a different failure that lives nearby.
- The failure is reproducible across runs (or, for flake bugs, at a high enough rate).
- You captured the exact symptom (message, output, timing) so later phases can verify the fix actually addresses it.

Do not proceed until you reproduce the bug.

## Phase 3 — Hypothesise

Generate **3–5 ranked hypotheses** before testing any of them. Single-hypothesis generation anchors on the first plausible idea.

Each hypothesis must be **falsifiable**:

> Format: "If \<X\> is the cause, then \<probe Y\> will make the bug \<disappear / get worse\>."

If you can't state the prediction, the hypothesis is a vibe — discard or sharpen.

**Show the ranked list to the user before testing.** They often have context that re-ranks instantly ("we deployed a change to #3 yesterday"). Cheap checkpoint, big time saver. Proceed with your own ranking if the user is AFK.

## Phase 4 — Instrument

Each probe maps to a specific prediction from Phase 3. **Change one variable at a time.**

Tool preference:

1. **Debugger / REPL inspection** if the env supports it. One breakpoint beats ten logs.
2. **Targeted logs** at the boundaries that distinguish hypotheses.
3. Never "log everything and grep".

**Tag every debug log** with a unique prefix, e.g. `[DEBUG-a4f2]`. Cleanup at the end is a single `grep`.

**Perf branch.** For performance regressions, logs are usually wrong. Instead: establish a baseline measurement (timing harness, `performance.now()`, profiler, query plan), then bisect. Measure first, fix second.

## Phase 5 — Fix + regression test

Write the regression test **before the fix** — but only if there is a **correct seam** for it.

A correct seam exercises the **real bug pattern** as it occurs at the call site. If the only available seam is too shallow, a regression test there gives false confidence.

**If no correct seam exists, that itself is the finding.** Note it. The architecture is preventing the bug from being locked down. Flag for [`improve-architecture`](../improve-architecture/SKILL.md).

If a correct seam exists:

1. Turn the minimised repro into a failing test at that seam.
2. Watch it fail.
3. Apply the fix.
4. Watch it pass.
5. Re-run the Phase 1 feedback loop against the original (un-minimised) scenario.

## Phase 6 — Cleanup + post-mortem

Required before declaring done:

- [ ] Original repro no longer reproduces.
- [ ] Regression test passes (or absence of seam is documented).
- [ ] All `[DEBUG-...]` instrumentation removed (`grep` the prefix).
- [ ] Throwaway harnesses deleted (or moved to a clearly-marked debug location).
- [ ] Winning hypothesis stated in commit / PR message — so the next debugger learns.

**Then ask:** what would have prevented this bug? If the answer is architectural (no good test seam, tangled callers, hidden coupling) hand off to [`improve-architecture`](../improve-architecture/SKILL.md) with the specifics. Make the recommendation **after** the fix is in.
