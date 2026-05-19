---
name: grill-me
description: Interview the user relentlessly about a plan, design, decision, or idea until every branch of the decision tree is resolved. Works for software, research, product, hiring, life decisions — anything ambiguous. Use when the user says "grill me", "poke holes in this", "challenge this", "stress-test this", or when they hand over a vague brief that needs to be tightened before any work begins.
category: both
---

# Grill Me

Interview the user relentlessly about every aspect of the plan until you reach shared understanding. Walk down each branch of the decision tree, resolving dependencies between decisions one by one. For each question, recommend an answer — but ask the question.

This works on any kind of problem. Software design, hiring decision, vendor selection, career move, product direction. The pattern is universal; only the dimensions in the "what to grill on" section change.

## Rules of engagement

- **One question at a time.** Wait for the answer before asking the next.
- **Recommend an answer.** Don't just ask — say what you'd do and why. The user agrees, disagrees, or refines. All three are productive.
- **Branch awareness.** If the answer to one question changes which questions matter next, acknowledge that and re-prioritise visibly.
- **Ask about past behaviour, not future intent.** "When did you last hit this problem?" beats "would you use this?" — people are unreliable predictors of their own future choices but accurate reporters of recent past.
- **No filler.** Don't repeat the user's answer back. Don't summarise after every reply. Just ask the next question.

## What to grill on

Move through these dimensions, in this order, skipping any that are already nailed down:

1. **Goal.** What does success look like? Who is this for? Why now?
2. **Shape.** What does the artifact look like from the outside — function signature, screen, API, document section, decision sentence?
3. **Behaviour / content.** Happy path. Then name the unhappy paths and decide each.
4. **Boundaries.** What is explicitly *out* of scope?
5. **Constraints.** What can't change — public API, schema, headcount, budget, dependency, performance?
6. **Acceptance.** How will we know it worked? Express as observable signals where possible (test names, metric thresholds, named outcomes).
7. **Reversibility.** If this turns out wrong, how do we back out? One-way or two-way door?
8. **Pre-mortem.** It is six months from now and this failed. What does the headline say?

The last one is the most underused. It surfaces risks the user is avoiding naming.

## When to stop

Stop when:

- Every open branch has a decided answer or an explicit "we'll punt that to later" with a known owner.
- The user can summarise the plan back to you in one paragraph.
- You can name the acceptance signals without guessing.

Do **not** keep grilling once the plan is clear. Grilling has diminishing returns — past the point of clarity it becomes friction.

## When *not* to grill

- The user knows exactly what they want, has said so unambiguously, and the request is small. Don't grill on a typo fix.
- The question can be answered by reading the code, the docs, or the data. Read it instead.
- The grill keeps producing questions about the same dimension — that's a sign the dimension is bigger than the grill format can handle. Switch to a real design doc, [`grill-with-docs`](../grill-with-docs/SKILL.md) if a codebase already has language to challenge against, or [`deep-dive`](../deep-dive/SKILL.md) if the dimension is "we don't understand this topic yet".

## Output

The grill produces:

- A short written summary of the plan or decision, one paragraph.
- An ordered list of open questions you explicitly punted, each with an owner if applicable.
- Optional: a list of "if this turns out wrong, here's how we back out".

This summary is the input to whatever comes next — PRD, slice plan, deep-dive, decision memo. Without it the grill was a conversation; with it the grill is an artifact.
