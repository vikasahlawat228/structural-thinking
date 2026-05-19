---
name: diagnose
description: Disciplined debugging loop for hard bugs and performance regressions. The loop is the skill — reproduce, minimise, hypothesise, instrument, fix, regression-test. Use when a bug resists casual fixing, when reports say "intermittent", "slow", "sometimes wrong", or when the user says "diagnose this", "debug this", "why is this broken".
category: software
---

# Diagnose

**The loop is the skill.** If you have a fast, deterministic, agent-runnable pass/fail signal for the bug, the cause is mostly mechanical to find. If you don't, no amount of code-staring will save you. Everything else in this skill is in service of constructing and tightening that loop.

Six phases. Skip phases only when explicitly justified — and only in writing.

Before exploring code, read whatever knowledge the project already has — its glossary / context doc, its decision records — for a mental model of the relevant modules.

## Phase 1 — Build a feedback loop

This is the disproportionately important phase. Be aggressive. Be creative. Refuse to give up.

### Ways to construct one — try in roughly this order

1. **Failing test** at whichever seam reaches the bug — unit, integration, e2e.
2. **Curl / HTTP script** against a running dev server.
3. **CLI invocation** with a fixture input, diffing stdout against a known-good snapshot.
4. **Headless browser script** (Playwright / Puppeteer) for UI bugs.
5. **Replay a captured trace.** Save a real payload to disk; replay it through the code path in isolation.
6. **Throwaway harness.** Spin up a minimal subset of the system that exercises the bug with one call.
7. **Property / fuzz loop.** If the bug is "sometimes wrong output", run 1000 random inputs.
8. **Bisection harness.** If the bug appeared between two known states, automate "boot at state X, check, repeat" — then `git bisect run` it.
9. **Differential loop.** Run the same input through old-vs-new and diff outputs.
10. **HITL bash script.** Last resort. If a human must click, drive *them* with a structured script so the loop is still measurable.

### `git bisect run` recipe

When you can write a one-liner predicate, automate bisection:

```
git bisect start
git bisect bad HEAD
git bisect good <known-good-sha>
git bisect run sh -c './setup.sh && ./repro.sh; test $? -eq 0'
```

Predicate returns 0 if the bug is **absent**, non-zero if **present**. `bisect run` finds the introducing commit in `log₂(N)` steps. Resist hand-bisecting — humans introduce noise; machines don't.

### Iterate on the loop itself

Treat the loop as a product. Once you have *a* loop, ask:

- Can it be faster? Cache setup, skip unrelated init, narrow scope.
- Can the signal be sharper? Assert on the specific symptom, not "didn't crash".
- Can it be more deterministic? Pin time, seed RNG, isolate filesystem, freeze network.

A 30-second flaky loop is barely better than no loop. A 2-second deterministic loop is a debugging superpower. **Loop latency is the master variable** — below 1 second the work changes character entirely.

### Non-deterministic bugs

Goal: **higher reproduction rate**, not necessarily a clean repro. Loop 100×, parallelise, add stress, narrow timing windows, inject sleeps. A 50%-flake bug is debuggable; 1% is not — keep raising the rate until it is.

### Production-side loop — high-cardinality events

If the bug only reproduces in production, the loop is observability rather than tests. Emit one structured event per request with every dimension you might want to slice on (user, route, version, feature-flag, latency, error-class, request-shape). Query against the event stream after each hypothesis. Pre-defined dashboards don't help with unknown-unknowns; queryable wide events do.

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

## Phase 3 — Hypothesise (write a hypothesis log)

Generate **3–5 ranked hypotheses** before testing any of them. Single-hypothesis generation anchors on the first plausible idea.

Each hypothesis must be **falsifiable**:

> Format: "If \<X\> is the cause, then \<probe Y\> will make the bug \<disappear / get worse\>."

If you can't state the prediction, the hypothesis is a vibe — discard or sharpen.

Maintain a **running hypothesis log** through the session, in the format:

```
H1 [active]   <hypothesis>     prediction: <what we'll see>     status: <pending|disproven|confirmed>
H2 [active]   <hypothesis>     prediction: …                    status: …
H3 [parked]   <hypothesis>     parked because: <reason>
```

Show the ranked list to the user before testing. They often have context that re-ranks instantly ("we deployed a change to #3 yesterday"). Cheap checkpoint, big time saver. Proceed with your own ranking if the user is AFK.

### Two diagnostic frames

Pick the frame that fits the bug shape:

- **Differential diagnosis** (correctness bugs) — enumerate possible causes, rank by likelihood, test cheapest probe first. From medicine.
- **USE method** (performance bugs) — for every resource (CPU, memory, disk, network, queue depth), check **U**tilisation, **S**aturation, **E**rrors. Mechanical, exhaustive, surfaces the constrained resource fast. Brendan Gregg's method.

## Phase 4 — Instrument

Each probe maps to a specific prediction from Phase 3. **Change one variable at a time.**

Tool preference:

1. **Debugger / REPL inspection** if the env supports it. One breakpoint beats ten logs.
2. **Targeted logs** at the boundaries that distinguish hypotheses.
3. Never "log everything and grep".

**Tag every debug log** with a unique prefix, e.g. `[DEBUG-a4f2]`. Cleanup at the end is a single `grep`.

**Perf branch.** For performance regressions, logs are usually wrong. Establish a baseline measurement (timing harness, `performance.now()`, profiler, query plan, flame graph) before changing anything. Measure first, fix second.

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
- [ ] **Hypothesis log is part of the commit message or PR description** — what was thought, what was tried, what was confirmed. So the next debugger learns.

**Then ask:** what would have prevented this bug? If the answer is architectural (no good test seam, tangled callers, hidden coupling) hand off to [`improve-architecture`](../improve-architecture/SKILL.md). Make the recommendation **after** the fix is in.

## Live debate — root cause vs contributing factors

The classical debugging frame says: find the root cause, fix it. The resilience-engineering school (Richard Cook's "How Complex Systems Fail", Sidney Dekker, John Allspaw) argues the opposite: in complex systems "the root cause" is a comforting fiction; failures emerge from multiple contributing factors, and naming a single root cause makes the next failure more likely.

**Position this skill takes:** for narrow technical bugs in deterministic code, "root cause" is a useful frame and the hypothesis log converges on one cause. For production incidents involving operator action, coordination, or emergent system behaviour, **prefer "contributing factors"** — list them, rank by tractability, and address the most tractable. Blameless postmortems aren't a kindness; they're the only frame that gets accurate data.

If you find yourself writing "5 Whys" on an incident with humans in the loop, switch to causal-graph thinking — multiple parallel causes, not a single chain.

## OODA in incident mode

For live incidents (Cynefin: Chaotic) the discipline shifts from "find the cause" to "stabilise first, classify later". Run an explicit OODA cadence:

- **Observe** — what is the system actually doing right now?
- **Orient** — against your mental model. What changed? What's anomalous?
- **Decide** — smallest reversible action.
- **Act** — execute. Watch.
- Repeat.

A fast wrong move you can revert beats a slow right move. Update Orient aggressively as data arrives — Boyd's emphasis is that most failures live in Orient, not Observe.

Switch back to the disciplined six-phase loop once the system is stable.

## Anti-patterns

- **Single-root-cause fixation** on incidents with humans / coordination / emergent behaviour.
- **Blame-the-operator** framing. Operators are sensors of system pathology, not failure modes.
- **Debugging without a repro.** No amount of hypothesising substitutes for observing the bug happen.
- **"Fixed" without understanding why.** Random mutation can turn green; that doesn't mean you fixed the bug.
- **One-hypothesis tunnel vision.** Anchors on the first plausible idea. Always generate 3–5.
- **Log-everything-and-grep.** Targeted probes that distinguish hypotheses; nothing else.
- **Mid-fix architecture work.** Finish the fix narrowly; hand the architectural finding to `improve-architecture` after.

## When this skill isn't enough

- **You can't build a loop** — Phase 1 fails. Surface explicitly; ask the user for environment, artifacts, or instrumentation access.
- **The bug is at the team boundary, not the code** — escalate to coordination, not deeper debugging.
- **The hypothesis log fills a page and nothing converges** — the bug is in a higher layer. Run [`zoom-out`](../../general/zoom-out/SKILL.md) to find the right module to debug.
