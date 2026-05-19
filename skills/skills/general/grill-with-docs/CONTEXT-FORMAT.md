# CONTEXT.md Format

`CONTEXT.md` is the project's ubiquitous-language glossary. It is read by every agent at the start of every session and by every reviewer who needs to disambiguate a term.

## Goal

A reader unfamiliar with the project should be able to read `CONTEXT.md` in under five minutes and be able to follow a code review without stumbling on terminology.

## Sections

```markdown
# Project Name — Context

One paragraph describing what this project does, in the team's own words.

## Glossary

| Term | Definition | Related |
| ---- | ---------- | ------- |
| Customer | A business that has signed a brand-partnership agreement. Not the same as a User. | Brand, User |
| User | An end consumer using the mobile app. Authenticated by phone number. | Customer, Wallet |
| Wallet | Per-User store of coins, scoped to a currency. | Coin, Transaction |
| Coin | The reward currency. Issued in a Wallet's currency. Not transferable across currencies. | Wallet, Redemption |

## Domain rules

- A User has exactly one Wallet per currency. Wallets are created lazily on first earn.
- A Coin can only be redeemed at a Brand whose currency matches the Wallet currency.
- A Brand-Partnership lifecycle is: draft → pending → active → archived. Cannot be reversed.

## Anti-glossary

Terms we do **not** use, and what to say instead:

| Don't say | Say instead |
| --------- | ----------- |
| Account | User or Customer (be specific) |
| Points | Coins |
| Refund | Reversal (refund implies external payment) |
```

## Rules

- **Domain terms only.** No framework names, no library names, no infra primitives.
- **Definitions are short.** One or two lines per term.
- **Cross-reference.** Use the Related column to bind concepts together.
- **Versionable.** Update inline as decisions are made. Commit alongside the code change that introduced the term.
- **Single source of truth.** If `CONTEXT.md` says X and the code says Y, one of them is wrong. Fix it; don't let them drift.

## Multi-context projects

If you have more than one bounded context (e.g., `redemption` and `payouts`), use a root `CONTEXT-MAP.md` that points at per-module `CONTEXT.md` files:

```markdown
# Context Map

- **Redemption** — see `src/redemption/CONTEXT.md`. Owns Coin → Voucher conversion.
- **Payouts** — see `src/payouts/CONTEXT.md`. Owns Brand reimbursement cycles.
- **Shared** — see the glossary above. Terms that appear across contexts.

Terms that appear in both `redemption` and `payouts` are flagged: they will mean
slightly different things in each context. Resolve at the boundary.
```
