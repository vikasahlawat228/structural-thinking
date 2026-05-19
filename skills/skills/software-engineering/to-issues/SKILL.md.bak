---
name: to-issues
description: Break a PRD into independently-grabbable issues using vertical slices ("tracer bullets"). Each issue is something one agent can pick up and ship end-to-end. Use after to-prd, or any time you need to decompose a plan into ordered, independent units of work.
---

# To Issues

Take the PRD (or any plan) and cut it into **vertical slices**. Each slice is one shippable unit of value an agent can grab and complete in one TDD cycle.

## What a vertical slice is

A vertical slice goes top-to-bottom through whatever layers your app has — UI, service, store, integration — but is **narrow** in scope. It delivers one observable behaviour.

Examples for Vikas's product:

- ✅ `signup_accepts_referral_code` — UI input + validation + storage + analytics event, all for the single new field.
- ✅ `admin_can_bulk_approve_coin_requests` — multi-select UI + bulk endpoint + transactional update + audit log.
- ❌ "Add referral_code column to users table" — horizontal slice, no user-visible value.
- ❌ "Build admin auth" — too broad; not one behaviour.

If a slice can be merged on its own and adds value (or moves the project forward measurably), it's a real slice. If it can't, it isn't.

## How to slice

For each behaviour in the PRD's acceptance section:

1. **Name it after the behaviour.** Imperative or assertion form, not "implement X".
2. **List what it touches.** Files, modules, schemas. This is your independence signal.
3. **Write a one-line acceptance test.** What proves it's done?
4. **Estimate complexity in t-shirt sizes.** S (one cycle), M (a day), L (more than a day — re-slice).

**Re-slice anything that's L.** A large slice will sprawl and the agent will mis-prioritise.

## Ordering rules

Order the slices for **two** purposes simultaneously:

1. **Dependency order.** If slice B reads code slice A introduces, A goes first.
2. **Maximum independence early.** Pack independent slices into early milestones so they can run in parallel.

For a medium feature, you'll have 3–7 slices, mostly sequential.

For a large project, group slices by milestone and within each milestone identify the **independent set** — slices that touch disjoint modules and can run on parallel agents.

## Output

Two options:

1. **GitHub issues.** One per slice. Title = behaviour name. Body = touched modules + acceptance test + size. Apply a `slice` label and a milestone.
2. **A `SLICES.md` doc** if you don't use a tracker.

Either way, the **ordered list with parallelism hints** must be visible in the session before any slice is implemented.

## What each issue must contain

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

## Anti-patterns

- **Horizontal slicing.** "Add the column", "add the endpoint", "add the UI" — three issues, none of them ship. Use one issue, vertical.
- **Implementation-named slices.** "Refactor X" is not a slice. "User can do Y" is.
- **L slices.** They will explode. Re-slice now.
- **Hidden dependencies.** If slice B needs the file slice A creates but you didn't write it down, the parallel agent on B will start blocked. Surface dependencies explicitly.

## When this skill isn't enough

- The PRD itself is fuzzy → go back to [`to-prd`](../to-prd/SKILL.md), or further back to [`grill-with-docs`](../grill-with-docs/SKILL.md).
- All slices touch the same file → the design is too tangled to parallelise. Consider an architecture pass before slicing.
