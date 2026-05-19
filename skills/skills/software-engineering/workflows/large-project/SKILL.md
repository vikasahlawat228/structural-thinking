---
name: large-project
description: Multi-week project workflow with shared language, decision records, vertical-slice decomposition, parallel-agent execution, and weekly deepening. Adapts to the project's existing doc layout — does not impose one. Use when start classifies the task as a large project, or when the request spans multiple bounded contexts, introduces new subsystems, or will run for weeks.
category: software
---

# Large Project Workflow

A large project is where AI-assisted coding most often produces a beautiful, expensive ball of mud. The leverage is enormous — multiple agents writing code in parallel — but the entropy is also enormous. Without engineering fundamentals, the codebase compounds in complexity faster than any human team could ship features.

Seven phases. The first four are **front-loaded design work**; the last three are **execution with parallel agents** plus a sustaining loop. Do not skip ahead to execution. Most "I'll just start coding" large projects spend their second month undoing their first.

## Phase 1 — Establish shared language

Before any spec, before any code, the agent needs to speak the project's language. Run [`grill-with-docs`](../../../general/grill-with-docs/SKILL.md) at the highest level — what is this project, in your team's vocabulary?

`grill-with-docs` discovers what already exists — a `CONTEXT.md`, a README "Concepts" section, a Notion glossary, recurring class names — and works with whatever it finds. **This skill does not impose a directory layout.** If nothing exists yet, the user chooses where the glossary lives.

Outputs of this phase, *in whatever locations the project uses*:

- **A domain glossary** — terms the agents will use consistently. Per bounded context if the project has more than one.
- **Initial decision records** for the first round of hard-to-reverse choices — in whichever convention the project already follows (or a new one chosen explicitly).

Rules:

- Glossary entries are domain-meaningful, not implementation details.
- Don't write decision records for choices that aren't actually contested.
- If the same word means different things in different parts of the system, flag the boundary explicitly — that's a domain boundary worth naming.

**Exit condition:** the user can read the glossary and agree that "yes, this is how we talk about this product."

## Phase 2 — High-level architecture

Sketch the system boundaries. For each major component:

- **Responsibility** — one sentence: what does this own?
- **Public interface** — what does the outside world see?
- **Persistent state** — does it own data? How is it shaped?
- **Dependencies** — what does it call? What calls it?

Aim for **deep modules** — small interface, substantial implementation. Two components with a chatty interface is a smell — collapse them or push the chatter into a deeper third module.

Record architectural choices as decision records **only if** all three hold: hard to reverse, surprising without context, the result of a real trade-off.

**Exit condition:** a diagram + paragraph per component the user signs off on, plus decision records for any decisions that hit the bar above — filed in the project's existing convention.

## Phase 3 — PRD + slice plan

Run [`to-prd`](../../to-prd/SKILL.md) for the full project. A large PRD is multi-page; each section still earns its space.

Then run [`to-issues`](../../to-issues/SKILL.md) to break the PRD into **vertical slices**. For a large project:

- The first issue per new subsystem / surface is a **walking skeleton** (per `to-issues`).
- Slices are grouped into **milestones** (e.g., M1: read-only viewer, M2: admin actions, M3: bulk operations).
- Each slice is independently shippable.
- Each slice is labelled by which **module** it touches — this is what makes parallelism possible.

Identify the **independence graph**: which slices can run on parallel agents, which serialise. Slices that touch the same module always serialise — merge conflicts compound at the seams.

**Exit condition:** an issue per slice in whatever tracker the project uses, milestones tagged, "these can run in parallel" call-out per milestone.

## Phase 4 — Set up agent guardrails

Before you turn agents loose:

- **`CLAUDE.md` at repo root**, pointing at the project's domain glossary (wherever it lives), its decision records (wherever those live), and the relevant workflow skills. Add a "discover, don't impose" note so agents respect the existing layout.
- **TDD enforced per slice** — see [`tdd`](../../tdd/SKILL.md). Red-green-refactor is the only acceptable flow.
- **Git guardrails** — agents must not push, force-reset, or `git clean` autonomously.
- **Pre-commit hooks** — lint, typecheck, fast tests run automatically. The agent gets feedback before review.
- **A "no-go" list** — files, directories, or operations agents are forbidden from touching without human review (auth, migrations, payments, secrets).
- **DORA baseline** — note the current lead time, deployment frequency, change failure rate, and MTTR. These are the four metrics; agents-at-scale degrade them silently if you don't watch.

**Exit condition:** a fresh-cloned repo runs the project's check command (lint + typecheck + tests) green, and an agent can land a trivial PR end-to-end through CI.

## Phase 5 — Execute in parallel, slice by slice

Where the leverage lives. For each milestone:

1. Identify the **independent set** of slices.
2. Spin up one agent per slice — each running the [`tdd`](../../tdd/SKILL.md) loop on its own slice.
3. Each agent must:
   - Re-read the project's glossary and any decision records relevant to its module before starting.
   - Open one PR per slice; commit per slice (tidyings get their own commits per `small-change` Phase 1).
   - Never touch out-of-scope modules without surfacing it as a question first.
4. Review each PR on landing. Checklist:
   - Did it deliver the slice's acceptance signal?
   - Does it use the project's vocabulary consistently?
   - Did it introduce hidden cross-module coupling?
   - Did it deepen a module, or make one shallower?

If a slice's PR reveals a missing decision (the agent picked a path you didn't explicitly agree with), pause that lane, write the decision record in the project's convention, redirect the agent.

For long horizons or agent swaps, use [`handoff`](../../../general/handoff/SKILL.md) to compact context between sessions. Don't carry tens of thousands of stale tokens.

## Phase 6 — Toyota Kata cadence

Multi-week projects need an explicit improvement cadence, not just feature delivery. Borrow Toyota's Improvement Kata:

**Weekly target condition.** Each week, name what "better" looks like by the end of the week. Not a feature list — a **measurable state of the system**:

- "By Friday, the `redemption` lead time is < 2 days end-to-end."
- "By Friday, no slice required > 2 review rounds."
- "By Friday, the test suite runs in < 5 minutes."

**Weekly reflection.** At week's end: actual condition vs. target. Why the gap? What was the obstacle? What's the smallest experiment to address it next week? Capture in a running log (in whatever location the project uses for written status).

**DORA snapshot.** Re-measure the four metrics weekly. A degrading metric is an obstacle worth a target condition.

Without this cadence, a multi-week project becomes "ship slices forever" — which compounds nicely on the feature side and degrades silently everywhere else.

## Phase 7 — Refactor weekly, don't refactor mid-slice

Software entropy accelerates with agents. The defence is **regular, deliberate deepening passes**. Once a week — or on a tangible signal like "agents are getting confused" — run [`improve-architecture`](../../improve-architecture/SKILL.md).

Specifically look for:

- Shallow modules — small bodies behind small interfaces. Collapse or deepen.
- Repeated coupling — the same three modules always change together. Probably one module.
- Drift between code and glossary — code names don't match. Either rename code or update glossary; don't let them diverge.
- Superseded decisions that haven't been marked as such.

**Never** refactor in the middle of a slice. The slice is the unit of progress; refactors get their own slice.

## When to compact, when to escalate, when to slow down

- Run [`handoff`](../../../general/handoff/SKILL.md) **between phases**, not within them.
- Run [`zoom-out`](../../../general/zoom-out/SKILL.md) when an agent is producing locally-correct but globally-wrong code.
- If a slice keeps failing TDD, the slice is mis-sized — re-slice, don't push harder.
- If three or more DORA metrics are trending the wrong way, **slow Phase 5** and run Phase 7 before the next milestone.

## Anti-patterns

- **Starting Phase 5 before Phase 1.** "We'll figure out the language as we go." You will not. Agents will hallucinate twenty synonyms by the end of week one.
- **Forcing a directory layout.** This skill discovers; it does not impose. If the project uses Notion for decisions, use Notion. If it uses `DECISIONS.md` as a single file, append to it.
- **One giant decision document.** Decisions are atomic; one per record where the convention allows.
- **Parallel agents on overlapping modules.** They will fight in the merge. Slice for independence first.
- **Refactoring + feature work in one PR.** The reviewer can't tell which change broke what. Tidyings get their own commits.
- **Skipping the weekly deepening pass.** Entropy is real and it compounds. The cost of skipping is exponential in time.
- **No Toyota Kata cadence.** Without it, feature progress hides system regression.

## Quick reference — what runs when

| Phase | Skills used | Output |
| ----- | ------------ | ------ |
| 1     | `grill-with-docs`  | Domain glossary + initial decision records (in project's existing convention) |
| 2     | `zoom-out`         | Component sketch + decision records |
| 3     | `to-prd`, `to-issues` | PRD + milestone-tagged slices (walking skeleton first per new surface) |
| 4     | (setup only)       | CLAUDE.md, guardrails, hooks, DORA baseline |
| 5     | `tdd`, `diagnose`, `handoff` | Slices merged |
| 6     | (reflection)       | Weekly target condition + DORA snapshot |
| 7     | `improve-architecture`, `zoom-out` | Deepened modules, updated decisions |
