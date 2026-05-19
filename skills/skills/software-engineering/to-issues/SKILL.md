---
name: to-issues
description: Break a PRD or plan into independently-grabbable issues using vertical slices. Each issue is something one agent can pick up and ship end-to-end. Files in whatever tracker the project uses — GitHub, Linear, Jira, a markdown file, anywhere. Use after to-prd, or any time a plan needs to be decomposed into ordered, independent units of work.
category: software
---

# To Issues

Take a PRD (or any plan) and cut it into **vertical slices**. Each slice is one shippable unit of value an agent can grab and complete in one TDD cycle.

## Phase 1 — Discover the tracker

Don't assume GitHub Issues. Before writing anything, find out where issues live in this project:

1. Project's `README.md`, `CONTRIBUTING.md`, `CLAUDE.md` for guidance.
2. Look for existing issues — GitHub Issues, Linear, Jira, a `SLICES.md` doc, a Notion database, a Trello board. The recent ones tell you the convention (title style, labels, fields).
3. Check `package.json` / equivalent for tracker integrations.

State what you found in one line and confirm before filing anything new. If the project doesn't use a tracker, propose a single markdown file (`SLICES.md` or `docs/slices/<feature>.md`) and ask.

## Phase 2 — What a vertical slice is

A vertical slice goes top-to-bottom through whatever layers the system has — UI, service, store, integration — but is **narrow** in scope. It delivers one observable behaviour.

Examples:

- ✅ `signup_accepts_referral_code` — UI input + validation + storage + analytics event, all for the single new field.
- ✅ `admin_can_bulk_approve_coin_requests` — multi-select UI + bulk endpoint + transactional update + audit log.
- ❌ "Add referral_code column to users table" — horizontal slice, no user-visible value.
- ❌ "Build admin auth" — too broad; not one behaviour.

**Test:** if the slice can be merged on its own and adds value (or moves the project forward measurably), it's a real slice. If it can't, it isn't.

## Phase 3 — The walking-skeleton rule

For any new system, surface, or major subsystem in the plan, the **first issue** is a walking skeleton: end-to-end, deployable, does almost nothing — but every layer is touched. Cockburn's original definition.

A walking skeleton on a new payments service might be: "request → controller → service stub returning hardcoded 200 → response", with an end-to-end smoke test. No business logic. The first real slice goes on top of this.

Skip the walking skeleton only when extending a system that already has one — i.e., the layers are already wired up.

## Phase 4 — Slice the plan

For each behaviour in the PRD's acceptance section:

1. **Name it after the behaviour.** Imperative or assertion form, not "implement X".
2. **List what it touches.** Files, modules, schemas. This is the independence signal.
3. **Write a one-line acceptance test.** What proves it's done?
4. **Size it.** S (one cycle, < 4 hours), M (one day), L (more than a day — **re-slice**). Never ship L.

## Phase 5 — INVEST check

Before treating any slice as ready, run it through INVEST. Reject any slice that fails:

- **I**ndependent — minimal coupling to other slices in this batch.
- **N**egotiable — the *what* is fixed; the *how* is the implementer's call.
- **V**aluable — moves something a stakeholder cares about.
- **E**stimable — small enough that you can name a t-shirt size with confidence.
- **S**mall — fits in S or M. If L, re-slice.
- **T**estable — you can write the acceptance test before implementing.

"Independent" is a goal, not a gate — real systems have ordering constraints. But two slices that touch the same file are not independent, no matter what the labels say.

## Phase 6 — When a slice is too big, use a splitting pattern

If a slice doesn't fit in S or M, split it. Use one of the canonical Cohn / Lawrence splitting patterns — pick the one that fits:

- **By workflow step.** Split a multi-step flow into one slice per step.
- **By data variation.** Slice 1 handles the common case; Slice 2 handles the variant.
- **By interface.** Slice 1 ships the UI flow; Slice 2 ships the API call (or vice-versa with a stub).
- **By business rule.** Each rule gets its own slice.
- **By happy path vs edge cases.** Ship the happy path first; edges come later.
- **By CRUD operation.** Read first; write second; update third; delete fourth.
- **By tooling / instrumentation.** Ship without metrics/logging; add them as a follow-up slice.
- **By performance.** Ship correct-but-slow; optimise as a follow-up slice.
- **By acceptance criterion.** One slice per acceptance test.

If none of these fit, the slice is probably not vertical — re-frame the work.

## Phase 7 — Order and parallelism

Order slices for two purposes simultaneously:

- **Dependency order.** If slice B reads code slice A introduces, A goes first.
- **Maximum independence early.** Pack independent slices into early milestones so they can run on parallel agents.

For a medium feature, expect 3–7 slices, mostly sequential. For a large project, group by milestone and identify the **independent set** within each milestone — disjoint modules can run in parallel.

If more than ~10 slices come out of a single PRD, the work is large-project sized — confirm sizing before continuing.

## Issue template

Whatever the tracker, every issue must carry the same content:

```markdown
# <behaviour_name>

**Size**: S | M
**Milestone**: <name>
**Touches**: `<file or module>`, `<file or module>`

## Acceptance
`<test_name>` passes when…

## Parallel with
- `<other slice name>` (no module overlap)

## Blocked by
- `<other slice name>` (because <why>)
```

The exact field names match the tracker's convention if it has one (Linear uses Estimate; GitHub uses size labels; Jira uses Story Points). The content is the same.

## Output

Either way the tracker works, the **ordered list with parallelism hints must be visible in the session** before any slice is implemented. The user signs off before TDD starts.

## Anti-patterns

- **Horizontal slicing.** "Add the column", "add the endpoint", "add the UI" — three issues, none of them ship. Use one issue, vertical.
- **Implementation-named slices.** "Refactor X" is not a slice. "User can do Y" is.
- **L slices.** They will sprawl. Re-slice now, not after the slice fails.
- **Hidden dependencies.** If slice B needs the file slice A creates but you didn't write it down, the parallel agent on B starts blocked.
- **Skipping the walking skeleton on a new surface.** The first slice that's "real" without a skeleton underneath fights the architecture instead of using it.
- **Inventing a tracker convention.** Phase 1's discover step is non-negotiable.

## When this skill isn't enough

- The PRD itself is fuzzy → back to [`to-prd`](../to-prd/SKILL.md), or further back to [`grill-with-docs`](../../general/grill-with-docs/SKILL.md).
- All slices touch the same file → the design is too tangled to parallelise. Run [`improve-architecture`](../improve-architecture/SKILL.md) before slicing.
- More than ~10 slices for what was called a medium feature → confirm with `start` whether this is actually a large-project.
