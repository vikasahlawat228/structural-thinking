# ADR 0001: Three Buckets and an Orchestrator

**Status**: Accepted
**Date**: 2026-05-19
**Deciders**: Vikas

## Context

Coding tasks vary by an order of magnitude in size, but agent-driven workflows
tend to apply one process to all of them. A small bug fix doesn't benefit from
a PRD; a multi-week project explodes without one. Most of the "AI coding produces
spaghetti" failures come from running the wrong-sized process for the task.

Matt Pocock's skills repo solves this with a flat collection of small skills
(`grill-with-docs`, `tdd`, `diagnose`, etc.) that engineers compose by hand,
session by session. That works well for someone who already has the taste —
they know when to grill, when to skip, when to refactor. For day-to-day use
without that taste already loaded, the composition is the hard part.

## Decision

This repo overlays Matt's atomic skills with **three workflow skills** —
`small-change`, `medium-feature`, `large-project` — and an **orchestrator**
that classifies an incoming request and routes it to the right workflow.

The atomic skills remain authoritative for the actual work; the workflows
are recipes for composition. The orchestrator's job is just classification
and handoff.

## Alternatives Considered

- **Flat atomic skills only (Matt's approach).** Rejected for now because
  composition is the failure mode for day-to-day use. May revisit once the
  workflows have been used enough that picking atoms feels natural.

- **One mega-workflow that adapts based on signals.** Rejected because the
  three buckets have meaningfully different phases (e.g., large-project has
  parallelism and ADRs; small-change has neither). A single workflow that
  conditionally skips half its phases is harder to follow than three
  separate ones.

- **More buckets (e.g., five sizes).** Rejected — diminishing returns past
  three. The orchestrator already supports announcing the classification so
  a wrong-size routing is cheap to correct.

## Consequences

**Positive**

- Day-to-day cognitive load drops: classify, dispatch, follow the recipe.
- Each workflow encodes the right level of ceremony for its size.
- New atomic skills slot in by being referenced from workflows; no need to
  rewrite the workflows themselves.

**Negative**

- Three workflows is more surface area to maintain than one. Workflow drift
  is now a thing to watch for — the bucket boundaries can become stale as
  the way of working evolves.
- The orchestrator adds a step at the start of every session. For small
  changes this is fine; for a 10-second typo fix it's negligible overhead.

**Neutral**

- The split between "atomic" and "workflow" skills is a soft one. A workflow
  could be invoked as if it were atomic; an atomic skill could be invoked
  with extra phases bolted on. We rely on naming and folder structure, not
  a strict mechanism.

## Notes

- The "one bucket smaller" default (orchestrator step 2) is a deliberate
  bias against ceremony. Most real tasks are small changes; the others can
  always escalate.
- If a fourth bucket appears later (e.g., "exploratory spike"), this ADR
  gets superseded, not amended in place.
