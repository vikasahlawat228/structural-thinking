# ADR Format

An Architecture Decision Record (ADR) captures a single hard-to-reverse decision and the reasoning behind it. The future reader's question — "why did they do it this way?" — is answered here.

## File naming

```
docs/adr/0001-use-event-sourcing-for-redemption.md
docs/adr/0002-postgres-for-write-model.md
docs/adr/0003-mongo-for-feed-projection.md
```

Sequential numbering. Never renumber. Use kebab-case for the slug.

## Template

```markdown
# ADR 0001: Use Event Sourcing for Redemption

**Status**: Accepted
**Date**: 2026-05-19
**Deciders**: Vikas, <other names>

## Context

What is the situation we're deciding about? Two or three paragraphs, in
the project's domain language. Explain why the decision is even needed —
what changed, what constraint forced our hand.

## Decision

The decision itself, in one or two sentences. Imperative voice.

> We will store every Redemption as an append-only event log in Postgres,
> with read models projected to Mongo for query.

## Alternatives Considered

Each alternative we seriously evaluated and why we did not pick it. One
paragraph each. Include the alternative we wish we could pick — "the dream
option" — and why it was infeasible.

- **CRUD on Postgres.** Simpler. Rejected because we need an audit trail
  for the compliance team and reconstructing one from row history is fragile.
- **Single Mongo collection.** Faster to write. Rejected because we lost
  transactional integrity on multi-coin redemptions in the spike.

## Consequences

What changes because of this decision. Both costs and benefits. Be honest
about the trade-offs.

- **Positive**: Audit trail is free. We can replay a User's history to
  reconstruct any wallet state.
- **Negative**: Adds a projection layer. Every new read query needs a
  projector. New engineers will need a 30-minute walkthrough.
- **Neutral**: The event schema is now versioned; we have to handle
  schema migration on the events themselves.

## Status transitions (optional)

If this ADR is later superseded, do **not** delete it. Add a status
section to the top:

> **Status**: Superseded by ADR 0017
```

## When to write an ADR

All three must be true:

1. **Hard to reverse.** Backing out is meaningfully painful.
2. **Surprising without context.** A future reader will want to know why.
3. **A real trade-off.** There were genuine alternatives.

If any is missing, you don't need an ADR. Most decisions don't earn one.

## When *not* to write an ADR

- "We picked React over Vue because it's the team standard." Not surprising, no real trade-off.
- "We added pagination to the list view." Reversible; not surprising.
- "We chose `Map` over `Record` for the cache type." Implementation detail.

If in doubt, **don't write one yet**. Writing too many ADRs is worse than writing too few — the signal-to-noise drops and people stop reading them.

## Superseding

When a decision is overturned, write a new ADR that **supersedes** the old one. Update the old one's status to `Superseded by ADR N`. Never delete.
