---
name: to-prd
description: Turn the current conversation into a product requirements document. No interview — just synthesise what's been discussed into a structured PRD ready to file in whatever issue tracker or doc location the project uses. Use after a grill-me or grill-with-docs session has established the plan.
category: software
---

# To PRD

The grill is done. The plan is in the session. This skill turns that material into a structured PRD with no further questions — and files it wherever the project actually keeps PRDs.

If the session does **not** already contain a clear plan, **stop**. Run [`grill-me`](../../general/grill-me/SKILL.md) or [`grill-with-docs`](../../general/grill-with-docs/SKILL.md) first. A PRD synthesised from a fuzzy session is a fuzzy PRD.

## Phase 1 — Discover the tracker convention

Before writing anything, find out where PRDs land in this project. Do not assume GitHub Issues, Linear, Notion, or a particular folder. Look in this order:

1. The project's `README.md`, `CONTRIBUTING.md`, or `CLAUDE.md` for explicit guidance.
2. Existing PRDs in the repo — `docs/prd/`, `docs/specs/`, `specs/`, `proposals/`, `rfcs/`, or a single doc that holds all of them.
3. Issue tracker conventions — labels, templates, recent issues tagged `prd` / `spec` / `proposal`.
4. External tools linked from any README — Linear, Notion, Confluence, Productboard.

State what you found in one line, then **ask** where the new PRD should land if it isn't obvious. Examples:

- "Existing PRDs live in `docs/proposals/0001-x.md`. I'll add this as `docs/proposals/00NN-<slug>.md` — sound right?"
- "I don't see a PRD convention. Options: a GitHub issue with the `prd` label; a new file at `docs/prd/<slug>.md`; somewhere else?"
- "Linear is linked from the README. Should I draft here for you to paste, or do you want a local shadow as well?"

Never silently invent a location.

## Phase 2 — Synthesise the PRD

A single markdown document. Length scales with the work: one page for a medium feature, multi-page for a large project. Every section earns its space.

### Required sections — every PRD

These four come straight from the research and refusing to skip them is the discipline:

- **Why.** One sentence — the user-facing problem this solves. If you can't write this without hedging, the plan isn't ready.
- **Press-release-style summary.** Borrowed from Amazon's PR/FAQ. Three to five sentences as if announcing the feature to its users: *"From today, users can do X. The way it works is Y. We built this because Z."* This forces the "would anyone care" test before the "how do we build it" trap.
- **Out of scope (no-gos).** Bulleted list of what this PRD is *not* doing. Half of all misalignment lives here. Shape Up calls these "no-gos" and treats them as load-bearing.
- **Acceptance — observable signals.** Express as test names or measurable outcomes, not paragraphs. `user_can_redeem_with_matching_currency`, not "the user should be able to redeem coins". For non-test signals: `signup_p95_latency_under_3s_on_3g`.

### Required body — medium feature (≤ one page)

```markdown
## Interface
The exact shape the caller sees. UI sketch / function signature / endpoint / message schema.

## Behaviour
- Happy path: <one or two sentences>
- Named failure modes:
  - <Failure 1>: <what the user sees, what the system does>
  - <Failure 2>: <…>

## FAQ
Three to five questions the next reader will have, answered. Drawn from
the grill session.
```

### Additional sections — large project

```markdown
## Milestones
1. **M1 — <name>**: <one-line outcome>
2. **M2 — <name>**: <one-line outcome>

## Modules touched
- `<module a>`: <what changes>

## Risks
For each risk, name the category (value / usability / feasibility / viability)
and the proposed mitigation. Borrowed from Cagan's four product risks.

## Open questions
With owners. Not nice-to-have — required.

## Decisions worth recording
List the decisions in this PRD that meet the bar for a decision record
(hard to reverse, surprising, real trade-off).
```

### The four-risks check (Cagan)

Before declaring the PRD complete, walk every section against the four product risks:

- **Value.** Will users care? Evidence?
- **Usability.** Can they actually use it without training?
- **Feasibility.** Can engineering build it within the appetite?
- **Viability.** Does it work for the business — legal, ops, support, GTM?

If any of the four is silently assumed, surface it as an open question.

## Phase 3 — Length discipline

For a medium feature, **two screens or less, with a TL;DR up top.** Linear's compression rule: if it doesn't fit, it's a doc not a spec. Force the synthesis.

For a large project, longer is fine — but every additional page must earn its space. Anything that doesn't change a decision is decoration.

## Phase 4 — File it where Phase 1 said

Render the PRD in the location confirmed in Phase 1. If the tracker requires a particular template, match it. If filing as a GitHub-style issue: title = feature name, body = the PRD, apply the project's `prd` or `spec` label.

Paste the rendered PRD back into the session so the user can sanity-check before slicing. Confirm before proceeding to [`to-issues`](../to-issues/SKILL.md).

## Rules

- **Pull every concrete claim from the session.** Don't invent new requirements. If something is unclear, mark it `// TODO: confirm` rather than guessing.
- **Use the project's existing vocabulary** — whatever artifact holds the glossary (README section, `CONTEXT.md`, Notion, recurring code names). If a new term was introduced in the session, propose adding it via `grill-with-docs`.
- **Acceptance criteria are observable signals.** Not paragraphs.
- **Out-of-scope is a required section,** not a nice-to-have.
- **No estimates in the PRD.** PRDs describe *what*; estimates and slicing belong in `to-issues`.

## Anti-patterns

- **PRD-by-questionnaire.** This skill is synthesis, not interview. If you're asking the user new questions, you skipped `grill-me`.
- **Acceptance criteria that aren't testable.** "Good UX" is not a test name.
- **PRDs that include "how".** Implementation goes in slices, not PRDs.
- **Inventing a folder layout.** Phase 1's discover-and-ask step is non-negotiable.
- **Silently skipping the four-risks check.** If a section is missing, name what's missing rather than fudging.
- **PRDs longer than the appetite warrants.** A two-week feature does not need a six-page PRD. Length tracks appetite.
