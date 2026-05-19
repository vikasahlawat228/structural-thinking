---
name: to-prd
description: Turn the current conversation into a product requirements document. No interview — just synthesise what's been discussed into a structured PRD ready to ship as a GitHub issue or markdown doc. Use after a grill-me or grill-with-docs session has established the plan.
---

# To PRD

You've already done the grill. The plan is in the session. This skill turns that material into a structured PRD with no further questions.

If the session does **not** already contain a clear plan, **stop**. Run [`grill-me`](../grill-me/SKILL.md) or [`grill-with-docs`](../grill-with-docs/SKILL.md) first. A PRD synthesised from a fuzzy session is a fuzzy PRD.

## What to produce

A single markdown document. For a **medium feature** keep it one page. For a **large project** it can be longer, but every section must still earn its space.

### Template — medium feature (one page)

```markdown
# <Feature Name>

## Why
One sentence — the user-facing problem this solves.

## Interface
The exact shape the caller sees. For UI: a sketch or component description.
For backend: a function signature, endpoint, or message schema.

## Behaviour
- Happy path: <one or two sentences>
- Named failure modes:
  - <Failure 1>: <what the user sees, what the system does>
  - <Failure 2>: <…>

## Out of scope
- <Bullet 1>
- <Bullet 2>

## Acceptance — test names
- <`test_name_one`>
- <`test_name_two`>
- <`test_name_three`>
```

### Template — large project (multi-page)

Adds these sections:

```markdown
## Milestones
1. **M1 — <name>**: <one-line outcome>
2. **M2 — <name>**: <one-line outcome>
3. …

## Modules touched
- `<module a>`: <what changes>
- `<module b>`: <what changes>

## Risks & open questions
- <Risk or open question 1, with proposed mitigation>
- <…>

## Decisions worth ADRs
- <Decision 1> → see ADR draft <link>
```

## Rules

- **Pull every concrete claim from the session.** Don't invent new requirements. If something is unclear, mark it explicitly `// TODO: confirm with Vikas` rather than guessing.
- **Use the project's `CONTEXT.md` vocabulary.** If the session used a fuzzy term, use the precise one. If a new term was introduced, add it to `CONTEXT.md`.
- **Acceptance criteria are test names.** Not paragraphs. `user_can_redeem_with_matching_currency` not "the user should be able to redeem coins".
- **Out-of-scope is a real section.** Half of misalignment is "we didn't say what we *weren't* building".
- **No estimates.** PRDs describe *what*, not *how long*.

## Output

Two options:

1. **Filed as a GitHub issue** if the project tracks work there. Title = feature name. Body = the PRD. Apply the `prd` label if you use one.
2. **Markdown in the repo** at `docs/prd/<slug>.md` if you don't use GitHub issues.

Either way, paste the rendered PRD back into the session so the user can sanity-check it before you slice it. Confirm before proceeding to [`to-issues`](../to-issues/SKILL.md).

## Anti-patterns

- **PRD-by-questionnaire.** This skill is synthesis, not interview. If you're asking questions, you skipped `grill-me`.
- **Acceptance criteria that aren't testable.** "Good UX" is not a test name. `signup_completes_under_3_seconds_on_3g` is.
- **PRDs that include "how".** Implementation goes in slices, not PRDs.
