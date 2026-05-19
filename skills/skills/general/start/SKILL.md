---
name: start
description: Universal first-move protocol for any non-trivial request — software, research, decision, design, planning. Sense → Classify domain → Imagine done → Constraints → Route. Use at the start of any session where the task isn't a one-shot question or a trivial edit. Trigger phrases include "where do I start", "how should I approach this", "kick this off", "plan this out", "size this", "I want to do X", or any new request that hasn't been classified yet.
category: both
---

# Start — Five Moves Before Any Work Begins

Most damage from AI-assisted work is caused by running the wrong-sized process for the task — ceremony on a typo, or a hot-fix loop on a multi-week project. Worse: solving a different problem than the one the user actually has. This skill is the universal first move that prevents both.

Five steps. Do not skip. Do not start work from inside this skill — classify, name the end-state, name the constraints, then dispatch.

## Step 1 — Sense

Write **one sentence** that names what kind of request this is. Examples:

- "This is a bug in the redeem flow."
- "This is a design call about whether to event-source the rewards ledger."
- "This is a research question about Postgres replication trade-offs."
- "This is a planning ask for the next quarter."
- "This is a decision about whether to bring in a vendor or build."

If you cannot produce one sentence, the request is fuzzy — go to Step 4's last clause (still fuzzy → `grill-me`).

## Step 2 — Classify the domain

Pick one. The method you'll use depends entirely on this answer.

- **Clear** — cause and effect obvious, established playbook exists. Examples: production hotfix for a known issue, dependency bump, well-spec'd bug. → Run the known playbook; don't over-think.
- **Complicated** — cause and effect knowable with analysis. Examples: a feature with multiple acceptance criteria, a deliberate refactor, a research question with a defined target. → Apply expert analysis and a structured workflow.
- **Complex** — cause and effect only knowable in retrospect. Examples: new product surface, multi-context migration, "how do users behave with X", anything genuinely novel. → Run safe-to-fail experiments. Do not write a 6-month plan.
- **Chaotic** — no cause-and-effect map yet. Examples: live incident, production fire, "everything is broken". → Act to stabilise *first*. Re-classify once stable.

Misclassification is the most common failure mode. The biggest sin is treating a Complex problem as Complicated — applying analysis where experiments are needed.

## Step 3 — Imagine done

Write what success looks like in **one paragraph**, before solving anything.

- For software work: write the line that will appear in the PR description.
- For a research ask: write the table-of-contents the resulting doc will have.
- For a decision: write the sentence you'd send to the team after deciding.
- For a planning ask: write the headline of the resulting plan.

This is the cheapest insurance against solving the wrong problem. If you can't write this paragraph, you don't understand the request well enough to start — go to Step 4's last clause.

## Step 4 — Constraints

Three sub-questions. All cheap. Surface the answers explicitly to the user.

**Appetite.** How much is this *worth* in time and attention? A bug fix may be worth 30 minutes and zero ceremony. A migration may be worth six weeks and substantial ceremony. Ask the user explicitly if it isn't obvious. The wrong-sized process is the most common failure mode.

**Reversibility.** If this turns out wrong, how expensive is the rollback?
- *Two-way door* (easy to back out) → favour speed.
- *One-way door* (hard / impossible to back out) → favour deliberation, more grilling, more documentation.

**Blast radius.** Who else is affected if this is wrong? A one-hour change to billing code is *large* by blast radius even if *small* by effort. A large refactor on a sandbox tool is *small* by blast radius even if hours of work.

## Step 5 — Route

Given the diagnosis (Step 2), the end state (Step 3), and the constraints (Step 4), pick the next skill or workflow. Announce the routing decision to the user in one or two sentences before continuing:

> Sizing this as a `<bucket>` because `<one-line reason citing the rubric>`. Running `<skill>` next. If you think this is bigger/smaller, say so now.

### Routing table

| Situation | Route to |
| --------- | -------- |
| Software work, small effort, low blast radius, two-way door | `software-engineering/workflows/small-change` |
| Software work, medium effort, one bounded context, mostly two-way door | `software-engineering/workflows/medium-feature` |
| Software work, large effort or multi-context or one-way door | `software-engineering/workflows/large-project` |
| Research / learning question; needs a durable artifact | `general/deep-dive` |
| Plan / design / decision is still fuzzy after Steps 1-4 | `general/grill-me` |
| Plan touches a real codebase with existing language / decisions | `general/grill-with-docs` |
| Live production incident | Stabilise first, then `software-engineering/diagnose` |

When the bucket is genuinely ambiguous, **start one bucket smaller**. It is cheaper to escalate than to deflate.

## Anti-patterns

- **Anchoring on the user's own framing.** "Quick fix" in the prompt does not make the request a small change. Classify from signals, not adjectives.
- **Skipping Step 3.** "I'll figure out what done looks like as I go" is how agents ship the wrong feature beautifully.
- **Picking Complicated when the problem is Complex.** If nobody has solved this exact problem before, no amount of analysis substitutes for an experiment.
- **Single-axis sizing.** Effort alone is not enough — pair it with blast radius and reversibility.
- **Silent re-routing mid-task.** If you discover the bucket was wrong while running a workflow, *stop*, announce the surprise, re-classify with the user.

## Quick reference

| Step | Move | Output |
| ---- | ---- | ------ |
| 1 | Sense | One-sentence statement of the request |
| 2 | Classify domain | Clear / Complicated / Complex / Chaotic |
| 3 | Imagine done | One-paragraph end state |
| 4 | Constraints | Appetite + reversibility + blast radius |
| 5 | Route | Named next skill, announced to the user |
