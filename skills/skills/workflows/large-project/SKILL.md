---
name: large-project
description: Multi-week project workflow with shared language, ADRs, vertical-slice decomposition, and parallel-agent execution. Use when the orchestrator classifies the task as a large project, or when the request spans multiple bounded contexts, introduces new subsystems, or will run for weeks.
---

# Large Project Workflow

A large project is where AI-assisted coding most often produces a beautiful, expensive ball of mud. The leverage is enormous — multiple agents writing code in parallel — but the entropy is also enormous. Without engineering fundamentals, the codebase compounds in complexity faster than any human team could ship features.

This workflow has six phases. The first three are **front-loaded design work**; the last three are **execution with parallel agents**. Do not skip ahead to execution. Most "I'll just start coding" large projects spend their second month undoing their first.

## Phase 1 — Establish shared language

Before any spec, before any code, the agent needs to speak your project's language. Run [`grill-with-docs`](../../atomic/grill-with-docs/SKILL.md) at the highest level: what is this project, in your team's vocabulary?

Outputs of this phase:

- **`CONTEXT.md`** at the relevant scope. One per bounded context (e.g., `src/redemption/CONTEXT.md`, `src/payouts/CONTEXT.md`). A `CONTEXT-MAP.md` at root if there are more than one.
- **Initial ADRs** in `docs/adr/` for the first round of hard-to-reverse decisions. Number them sequentially; do **not** retroactively renumber.

Rules:

- Don't couple `CONTEXT.md` to implementation. Only domain-meaningful terms.
- Don't write ADRs for decisions that aren't actually contested.
- If the same word means different things in different parts of the system, flag it explicitly — that's a domain boundary worth naming.

**Exit condition:** the user can read `CONTEXT.md` and agree that "yes, this is how we talk about this product."

## Phase 2 — High-level architecture

Sketch the system boundaries. For each major component:

- **Responsibility** — one sentence: what does this own?
- **Public interface** — what does the outside world see?
- **Persistent state** — does it own data? How is it shaped?
- **Dependencies** — what does it call? What calls it?

Aim for **deep modules** (small interface, deep implementation). When two components have a chatty interface, that's a smell — collapse them or push the chatter into a deeper third module.

Record architectural choices as ADRs **only if** all three hold: hard to reverse, surprising without context, the result of a real trade-off.

**Exit condition:** a diagram + paragraph per component the user signs off on, and an ADR for any decision that hits the bar above.

## Phase 3 — PRD + slice plan

Run [`to-prd`](../../atomic/to-prd/SKILL.md) for the full project. A large PRD is multi-page; that's fine — but each section must still be load-bearing.

Then run [`to-issues`](../../atomic/to-issues/SKILL.md) to break the PRD into **vertical slices**. For a large project:

- Slices are grouped into **milestones** (e.g., M1: read-only viewer, M2: admin actions, M3: bulk operations).
- Each slice still must be independently shippable.
- Each slice is labelled by which **module** it touches — this is what makes parallelism possible.

Identify the **independence graph**: which slices can be worked in parallel, which block which. Slices that touch independent modules can run on parallel agents; slices that touch the same module should serialise.

**Exit condition:** an issue per slice, milestones tagged, an explicit "these can run in parallel" call-out per milestone.

## Phase 4 — Set up the agent guardrails

Before you turn agents loose:

- **CLAUDE.md** at repo root pointing at `CONTEXT.md`, `docs/adr/`, and the relevant workflow skills.
- **TDD enforced** per slice — see [`tdd`](../../atomic/tdd/SKILL.md). Red-green-refactor is the only acceptable flow.
- **Git guardrails** — agents must not push, force-reset, or `git clean` autonomously.
- **Pre-commit hooks** — lint, typecheck, fast tests run automatically. The agent gets feedback before you do.
- **A "no-go" list** — files, directories, or operations agents are forbidden from touching without human review (auth, migrations, payments).

**Exit condition:** a fresh-cloned repo, running `npm install && npm run check`, produces green output. An agent can land a trivial PR end-to-end through your CI.

## Phase 5 — Execute in parallel, slice by slice

This is where the leverage lives. For each milestone:

1. Identify the **independent set** of slices.
2. Spin up one agent per slice — each running the [`tdd`](../../atomic/tdd/SKILL.md) loop on its own slice.
3. Each agent must:
   - Re-read `CONTEXT.md` and any ADRs in its module before starting.
   - Open one PR per slice.
   - Never touch out-of-scope modules without surfacing it as a question first.
4. Review each PR on landing. The review checklist:
   - Did it actually deliver the slice's acceptance test?
   - Does it use the language in `CONTEXT.md`?
   - Did it introduce a hidden cross-module coupling?
   - Did it deepen a module, or make one shallower?

If a slice's PR reveals a missing ADR (because the agent picked a path you didn't agree with), pause that lane, write the ADR, redirect the agent.

**For long horizons or agent swaps**, use [`handoff`](../../atomic/handoff/SKILL.md) to compact context between sessions. Don't carry tens of thousands of stale tokens.

## Phase 6 — Refactor weekly, don't refactor mid-slice

Software entropy accelerates with agents. The defence is **regular, deliberate deepening passes**. Once a week — or on a tangible signal like "agents are getting confused" — run [`improve-architecture`](../../atomic/improve-architecture/SKILL.md) on the codebase.

Specifically look for:

- Shallow modules — small bodies behind small interfaces. Collapse or deepen.
- Repeated coupling — the same three modules always change together. Probably one module.
- Drift from `CONTEXT.md` — code names don't match the glossary. Either rename code or update glossary; don't let them diverge.
- Dead ADRs — a decision that's been superseded. Mark it superseded explicitly, don't delete it.

**Never** refactor in the middle of a slice. The slice is the unit of progress; refactors get their own slice.

## When to compact, when to escalate

- Run [`handoff`](../../atomic/handoff/SKILL.md) **between phases**, not within them.
- Run [`zoom-out`](../../atomic/zoom-out/SKILL.md) when an agent is producing locally-correct but globally-wrong code.
- If a slice keeps failing TDD, the slice is mis-sized — re-slice, don't push harder.

## Anti-patterns

- **Starting Phase 5 before Phase 1.** "We'll figure out the language as we go." You will not. Agents will hallucinate twenty synonyms by the end of week one.
- **One giant ADR document.** ADRs are atomic; one decision per file.
- **Parallel agents on overlapping modules.** They will fight in the merge. Slice for independence first.
- **Refactoring + feature work in one PR.** The reviewer can't tell which change broke what.
- **Skipping the weekly deepening pass.** Entropy is real and it compounds. The cost of skipping is exponential in time.

## Quick reference — what runs when

| Phase | Atomic skills used | Output |
| ----- | ------------------ | ------ |
| 1     | `grill-with-docs`  | `CONTEXT.md`, initial ADRs |
| 2     | `zoom-out`         | Component sketch + ADRs |
| 3     | `to-prd`, `to-issues` | PRD, milestone-tagged issues |
| 4     | (setup only)       | CLAUDE.md, guardrails, hooks |
| 5     | `tdd`, `diagnose`, `handoff` | Slices merged |
| 6     | `improve-architecture`, `zoom-out` | Deepened modules, updated ADRs |
