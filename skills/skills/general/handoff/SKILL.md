---
name: handoff
description: Compact the current conversation into a handoff document so another agent (or a future you) can continue the work without re-reading the whole session. Use when a session is getting long, when context is running out, when swapping agents, or at the end of a working day.
---

# Handoff

A long session accumulates a lot of noise — exploration paths that didn't pan out, intermediate drafts, side discussions. The handoff document is the **load-bearing residue**: what the next agent needs to be productive in five minutes.

## When to compact

- You're about to swap to a fresh agent.
- The session is approaching context limits.
- You're at the end of a working day on a multi-day task.
- A workflow phase is complete — the next phase deserves a clean start.

Don't compact mid-phase. Finish the slice, then compact.

## What to include

Five sections, in this order. Be terse.

### 1. Goal

One paragraph. What is this task? In the project's domain language (use `CONTEXT.md`). Link the relevant PRD / issue.

### 2. Where we are

A short list. What is **done**, what is **in progress**, what is **next**.

```markdown
**Done**
- ✅ Slice 1 — `signup_accepts_referral_code` (merged in PR #142)
- ✅ Slice 2 — `referral_code_validates_uniqueness` (merged in PR #143)

**In progress**
- 🟡 Slice 3 — `referral_code_credits_referrer` (RED on `referrer_gets_50_coins_on_signup`,
  blocked on confirming credit timing — see Question 1 below)

**Next**
- ⏭ Slice 4 — `referrer_receives_notification`
```

### 3. Key decisions made this session

The handful of choices the next agent must respect. Link to ADRs if any were written.

```markdown
- Referral credit happens on **signup**, not on first earn. (Vikas confirmed in this session.)
- Credit amount is `50` coins for the referrer and `25` for the referred. (See PRD §2.)
- Self-referrals are rejected silently — same code applied; no error shown. (See ADR 0007.)
```

### 4. Open questions

Anything blocking, in priority order.

```markdown
1. Should the credit happen inside the signup transaction or as an after-effect?
   I lean toward after-effect for retry semantics, but Vikas hasn't confirmed.
2. Do we backfill referrals for users who signed up in the last 30 days?
```

### 5. Where to look in the code

Pointers — files, modules, ADRs, glossary terms — the next agent should read first.

```markdown
- `src/onboarding/signup.ts` — the signup flow
- `src/rewards/credit-engine.ts` — where the credit will land
- `CONTEXT.md` — see "Referral" and "Credit" entries
- ADR 0007 — self-referral semantics
```

## Format

A single markdown document, pasted into the session or saved as `HANDOFF.md` in the repo root.

The next agent's **first action** is to read this. Their **second action** is to read every file referenced in section 5.

## Rules

- **Terse.** This is a launchpad, not a memoir. If you're explaining what `signup.ts` does, you're explaining too much — link it.
- **Honest.** If you tried something that didn't work, say so in one line. Saves the next agent the same dead end.
- **Up to date.** If new context emerges after writing the handoff, update it. The handoff is the snapshot at the moment of the swap.

## Anti-patterns

- **Dumping the full transcript.** That's the noise; the handoff is the signal.
- **Including chitchat or pleasantries.** Cut.
- **Generating a handoff for a one-shot session.** No swap means no handoff needed.
