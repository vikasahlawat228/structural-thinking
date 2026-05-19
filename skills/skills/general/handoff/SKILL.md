---
name: handoff
description: Compact the current session into a handoff document so another agent (or future-you) can continue without re-reading the whole conversation. Works for any kind of work — coding, research, decisions, planning. Use when a session is getting long, when context is running out, when swapping agents, or at the end of a working day.
category: both
---

# Handoff

A long session accumulates a lot of noise — exploration paths that didn't pan out, intermediate drafts, side discussions, dead ends. The handoff document is the **load-bearing residue**: what the next agent needs to be productive in five minutes.

The next agent has zero context and finite working memory. Every line of the handoff competes for the small number of chunks they can hold. Compress ruthlessly.

## When to compact

- You're about to swap to a fresh agent.
- The session is approaching context limits.
- You're at the end of a working day on a multi-day task.
- A workflow phase is complete — the next phase deserves a clean start.

**Don't compact mid-phase.** Finish the slice (or the current step of any other workflow), then compact. Compacting mid-phase produces a handoff that's already stale.

## Pyramid-style top

The first thing the next agent reads must be a **single sentence** that names the load-bearing fact: the one thing the next agent must do, and why.

> "The next agent must implement the credit-engine after-effect handler — Slice 3 is RED on `referrer_gets_50_coins_on_signup`, blocked on a transaction-vs-after-effect choice that we leaned toward after-effect but haven't confirmed."

That sentence is the elevator. Everything below it is supporting detail. If the next agent reads only the first sentence and the body, they have the gist. If they read further, they get the proof.

This is Minto's pyramid applied to handoff. Top-down: answer first, then the supporting points, then the detail. Never a chronological narrative of the session.

## Required sections

In this order. The shape is borrowed from medical I-PASS shift handoffs (which empirically cut errors ~30% in NEJM 2014) crossed with NASA mishap-report structure.

### 1. Goal

One paragraph. What is this task? In the project's existing vocabulary (whichever glossary the project keeps, wherever it lives). Link the relevant PRD / issue / decision record / artifact.

### 2. Where we are

A short structured list. **Done**, **in progress**, **next**.

```markdown
**Done**
- Slice 1 — `signup_accepts_referral_code` (merged, PR #142)
- Slice 2 — `referral_code_validates_uniqueness` (merged, PR #143)

**In progress**
- Slice 3 — `referral_code_credits_referrer` — RED on `referrer_gets_50_coins_on_signup`,
  blocked on Question 1 below.

**Next**
- Slice 4 — `referrer_receives_notification`
```

### 3. Invariants (what must remain true)

The handful of system properties the next agent **must not break** without an explicit decision. This section is usually what's missing from a bad handoff.

```markdown
- Credit transactions are idempotent on `(referrer_user_id, referred_user_id, event_id)`.
- Self-referrals are rejected silently — no error shown to the user.
- Coin balances never go negative; over-spend attempts return `INSUFFICIENT_BALANCE`.
```

### 4. Decisions made this session

The choices the next agent must respect. Link to the project's decision records if any landed.

```markdown
- Credit happens on **signup**, not on first earn. (Vikas confirmed, this session.)
- Amount is `50` coins for referrer, `25` for referred. (PRD §2.)
- Per discussion: after-effect retries 3× then logs to dead-letter queue. (No decision record yet — see Question 1.)
```

### 5. Open questions

In priority order, with the position you'd take if the next agent has to decide without you.

```markdown
1. Credit inside signup transaction, or as an after-effect?
   Lean: after-effect (retry semantics). Vikas hasn't confirmed.
2. Do we backfill referrals for users who signed up in the last 30 days?
   Lean: no — out of scope for this PRD; spin out a follow-up.
```

### 6. Dead ends

What was tried and rejected, with one-line reasons. The most underused section.

```markdown
- Tried triggering credit from a DB trigger — rejected because the trigger fires before
  the `users` row's `referred_by` column is set in the ORM, so we'd need a defer mechanism.
- Considered batching credits in a nightly job — rejected because referrer should see
  their coins within a minute (Slice 4 requires it).
```

This saves the next agent from re-walking the same paths. It also surfaces what you learned.

### 7. Next action

The exact first command, file, or query the next agent should run. Not "continue Slice 3" — specifically *what to do first*.

```markdown
Run: `npm run test src/rewards/credit-engine.test.ts -- --filter "referrer_gets_50_coins"`
Read: `src/rewards/credit-engine.ts` lines 40–80 (where the after-effect would land)
```

### 8. Where to look (appendix)

Pointers — files, modules, decisions, glossary terms, related PRs.

```markdown
- `src/onboarding/signup.ts` — the signup flow
- `src/rewards/credit-engine.ts` — where the credit will land
- Project glossary — entries for "Referral" and "Credit"
- Decision record on self-referral semantics (wherever the project keeps them)
```

## Length discipline

- Body (sections 1–7): **~600 words max**. If it's longer, compression is the next move; the next agent's working memory caps the load.
- Appendix (section 8, plus optional raw notes): unlimited, but clearly separated.

Force the compression. A 600-word body with a 5000-word appendix beats a 5000-word body every time.

## Format and tone

- **Prefer narrative prose at the top** (Pyramid + section 1). Sentences force the causal connectives — "because", "therefore", "however" — that bullets hide.
- **Bullets are fine in sections 2 onwards** where the content is genuinely list-shaped.
- Honest about what didn't work — section 6 is required, not optional.
- Up to date — if new context emerges after writing, update the doc. The handoff is the snapshot at the moment of the swap.

## Reflection (optional, kept separate)

If the session also surfaced **process improvements** — not task continuation — capture them in a short separate block, after the appendix:

```markdown
## Reflection — keep separate from action handoff
- **I like**: the slice sizing in Phase 4 felt right; every cycle finished in < 1 hour.
- **I wish**: we'd set the appetite explicitly in Phase 1 — would have cut the Phase 3 over-grilling.
- **What if**: we add a "credit timing" decision record to the project, since this question
  keeps coming up.
```

The action handoff (sections 1–8) is for the next agent's task. The reflection block is for process learning. Don't entangle them — they have different audiences.

## Where to put it

Discover what the project already does:

- Some projects keep handoffs in `HANDOFF.md` at the root.
- Some keep them as comments on the active issue / PR.
- Some keep them in a Notion or Linear "ongoing work" page.
- Some don't — paste into the session.

Default to a discoverable in-repo location if the project doesn't already have a convention. Ask if uncertain.

## Live debate — narrative vs bullets

Most engineering culture defaults to bullet lists; Amazon's six-pager culture insists on narrative prose. Bullets are faster to write and faster to skim; narrative forces precision (causal connectives, qualifiers, explicit warrants) that bullets hide.

**Position this skill takes:** narrative at the top (sections 1, 3, 5), bullets where the content is genuinely list-shaped (sections 2, 4, 6). The cost of getting this wrong is that ambiguity hides in the bullets.

## Anti-patterns

- **Dumping the full transcript.** That's the noise; the handoff is the signal.
- **Chronological narrative.** "First we tried X, then Y, then Z." Top-down, not first-to-last.
- **Skipping the invariants section.** The next agent will break something they don't know is load-bearing.
- **Skipping dead ends.** The next agent re-walks them.
- **Vague "next action".** "Continue with Slice 3" isn't an action; "run this command" is.
- **Mixing reflection into the action handoff.** Two different audiences. Keep separate.
- **Generating a handoff for a one-shot session.** No swap, no handoff needed.

## Output

A single markdown document. Posted into the session, **and** saved to the project's handoff location (per the discovery above) so it persists across sessions.

The next agent's first action is to read this document. Their second action is to read every file referenced in the appendix.
