---
name: grill-me
description: Interview the user relentlessly about a plan, design, or idea until every branch of the decision tree is resolved. Use when the user wants to stress-test a plan, says "grill me", "poke holes in this", "challenge this", or when they hand over a vague brief that needs to be tightened before code is written.
---

# Grill Me

Interview the user relentlessly about every aspect of the plan until you reach shared understanding. Walk down each branch of the decision tree, resolving dependencies between decisions one by one. For each question, provide your recommended answer — but ask the question.

## Rules of engagement

- **One question at a time.** Wait for the answer before asking the next.
- **Recommend an answer.** Don't just ask — say what you'd do and why. The user agrees, disagrees, or refines. All three are productive.
- **Branch awareness.** If the answer to one question changes which questions matter next, acknowledge that and re-prioritise visibly.
- **No filler.** Don't repeat the user's answer back at them. Don't summarise after every reply. Just ask the next question.

## What to grill on

Move through these dimensions, in this order, skipping any that are already nailed down:

1. **Goal.** What does success look like? Who is this for?
2. **Interface.** What does the user-facing shape look like — function signature, screen, API call, CLI?
3. **Behaviour.** Happy path. Then name the unhappy paths and decide each.
4. **Boundaries.** What is explicitly *out* of scope?
5. **Constraints.** What can't change — public API, schema, dependency, performance?
6. **Acceptance.** How will we know it works? Express as test names where possible.
7. **Reversibility.** If this turns out wrong, how do we back out?

You can grill on anything else too — but these are the load-bearing dimensions for almost every plan.

## When to stop

Stop when:

- Every open branch has a decided answer or an explicit "we'll punt that to later".
- The user can summarise the plan back to you in one paragraph.
- You can write the acceptance tests without guessing.

Do **not** keep grilling once the plan is clear. Grilling has diminishing returns — past the point of clarity it becomes friction.

## When *not* to grill

- The user knows exactly what they want, has said so unambiguously, and the request is small. Don't grill on a typo fix.
- The question can be answered by reading the code. Read the code instead.
- The grill keeps producing questions about the same dimension — that's a sign the dimension is more important than the grill format can handle. Switch to a real design doc or [`grill-with-docs`](../grill-with-docs/SKILL.md).

## Output

The grill produces:

- A short written summary of the plan, one paragraph.
- An ordered list of open questions you punted.
- Optional: a list of "if this turns out wrong, here's how we back out".

This summary is the input to the next phase (PRD, slice plan, TDD — whichever is appropriate).
