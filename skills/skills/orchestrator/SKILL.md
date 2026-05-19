---
name: orchestrator
description: Meta-router for engineering tasks. Classifies an incoming request as a small change, medium feature, or large project, then hands off to the right workflow. Use at the start of any non-trivial coding session, or when you're unsure how heavy a process the task warrants. Trigger phrases include "where do I start", "how should I approach this", "is this a small fix or bigger", "plan this out", "kick this off".
---

# Orchestrator — Pick the Right Workflow

You are the dispatcher. Most damage in AI-assisted coding comes from running the **wrong-sized process** for the task: ceremony on a typo, or a hot-fix loop on a multi-week project. Your job is to size the request honestly, then point the agent at the right workflow.

Do not start coding from this skill. Classify, justify the classification briefly, and dispatch.

## Step 1 — Read the request

Pull these signals from the user's message and the immediate codebase context:

- **Scope of change** — single function, single file, multi-module, multi-service?
- **Reversibility** — easy to roll back, or hard once it lands?
- **Unknowns** — is the spec clear, or is the request itself fuzzy?
- **Time budget** — minutes, hours, days, weeks?
- **Surface area** — public API change, schema change, new dependency, or internal?

If three or more signals are unknown, stop and ask the user one tight clarifying question before classifying. Don't guess.

## Step 2 — Classify

Use these rubrics. Pick the **smallest** bucket the request fits — escalate only when the smaller bucket genuinely can't hold it.

### Small change

Fits when **all** of:

- Bug fix, copy tweak, dependency bump, single-purpose refactor, or one-function feature.
- One module / one file most likely; reversible without ceremony.
- Spec is unambiguous after at most one clarifying question.
- Fits in one focused session (≲ 2 hours of work).

Examples for Vikas: "the redeem button shows wrong currency", "rename `userId` to `memberId` in this hook", "add a tooltip to the streak badge", "the cron job logs `undefined`".

→ Dispatch to **`small-change`** workflow.

### Medium feature

Fits when **any** of:

- A new user-visible feature or capability that spans 2–5 files / 1 module.
- The behaviour is clear-ish but the **interface** still needs design.
- Touches one bounded domain (e.g., onboarding, redemption, payouts) but not multiple.
- Likely 1–5 days of work; deserves a PRD-lite and explicit slices, but not ADRs and CONTEXT churn.

Examples for Vikas: "add a referral code field to signup with validation + analytics", "make the partner dashboard filter by brand", "let admins bulk-approve coin requests".

→ Dispatch to **`medium-feature`** workflow.

### Large project

Fits when **any** of:

- Multi-week scope, multiple bounded contexts, or new subsystem.
- Cross-cutting decisions worth recording as ADRs.
- Will benefit from running **multiple agents in parallel** on independent slices.
- Touches data model, public API, or core domain language.
- A bad call here is expensive to reverse.

Examples for Vikas: "build a B2B brand-self-serve portal", "migrate the rewards ledger to event sourcing", "replace the legacy admin app".

→ Dispatch to **`large-project`** workflow.

## Step 3 — Announce the dispatch

Always tell the user what you picked and why, in one or two sentences. Use this template:

> Sizing this as a **\<bucket\>** because \<one-line reason citing the rubric\>. Running the `\<bucket\>` workflow. If you think this is bigger/smaller than that, say so now and I'll re-route.

This is non-negotiable. The user must have a chance to correct the routing **before** you commit to the process — bad routing costs the rest of the session.

## Step 4 — Hand off

Load the workflow skill and follow it. Do not interleave instructions across buckets — pick one workflow and run it. If midway through you discover the bucket was wrong (e.g., a "small change" reveals a hidden schema migration), **stop**, surface the surprise, and re-route with the user.

## Anti-patterns

- **Anchoring on the user's own framing.** "Quick fix" in the prompt does not make it a small change. Classify from signals, not adjectives.
- **Picking large by default.** Most requests are small changes. Running PRD+ADR ceremony on a typo wastes the session.
- **Silent escalation.** If you decide mid-task the bucket was wrong, announce it. Don't quietly turn a small change into a refactor.
- **Mixing workflows.** Don't run grill-with-docs *and* the small-change fast loop — pick one, finish it.

## Quick reference

| Signal                | Small         | Medium             | Large                              |
| --------------------- | ------------- | ------------------ | ---------------------------------- |
| Files touched         | 1–2           | 2–5                | 5+ across modules                  |
| Time budget           | < 2h          | 1–5 days           | weeks+                             |
| Spec clarity needed   | one question  | PRD-lite           | grill-with-docs + CONTEXT + ADRs   |
| Tests                 | regression    | TDD per slice      | TDD per slice + integration suites |
| Parallel agents       | no            | rarely             | yes — by vertical slice            |
| Documentation         | commit msg    | PR body            | ADRs + CONTEXT updates             |

When in doubt, **start one bucket smaller**. It is cheaper to escalate than to deflate.
