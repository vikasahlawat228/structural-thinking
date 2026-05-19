---
name: deep-dive
description: Conduct a deep, structured research pass on a topic and produce or extend a durable knowledge artifact. Works on any subject — a competitor, a technology, a domain, an unfamiliar codebase area, a regulatory regime, a research question. Adapts to whatever directory the user picks — extends an existing knowledge base if one exists, scaffolds a new one if not. Use when the user says "research X", "deep-dive on X", "build a knowledge base for X", "I want to understand X properly", or hands over a topic and asks for a thorough treatment.
category: both
---

# Deep Dive

Build durable knowledge on a topic. Output is an artifact, not a chat answer.

Distinct from [`grill-me`](../grill-me/SKILL.md) and [`grill-with-docs`](../grill-with-docs/SKILL.md): those are reactive alignment — interview the user, sharpen their plan. This skill is proactive curation — pick a topic, locate or scaffold a knowledge base, research deeply, write it, and keep it updated across sessions.

If a topic will be revisited even once, the doc pays for itself.

## Phase 1 — Scope the dive

Before any research, agree on three things with the user:

1. **The topic.** One precise sentence. "Postgres logical replication for our setup" beats "Postgres replication". The narrower the topic, the deeper the dive can go in fixed effort.
2. **The audience.** Who reads the resulting artifact and what should they be able to do after reading it? "Me, so I can decide whether to adopt." "The on-call rotation, so they can debug it at 3am." "A new hire, so they can ramp in two days." The audience determines depth, vocabulary, and what gets left out.
3. **The depth.** Three rough tiers:
   - **Brief** (~1–2 pages) — orientation. What it is, when to use it, where the bodies are buried.
   - **Standard** (~3–8 pages) — working knowledge. Enough to make a real decision or onboard someone.
   - **Deep** (8+ pages, multi-file) — reference. Survey, comparison, war stories, sources.

If the user can't pick a depth, default to **Standard** and grow.

## Phase 2 — Locate or scaffold the knowledge base

This skill **does not assume** any directory layout. Discover what's there before writing anywhere.

1. **Existing knowledge?** Scan the project for related material — a `docs/` folder, an `engineering-wiki/`, a `notes/`, a Notion link in the root README, an Obsidian vault, a single doc on the topic. If something exists and is on-topic, **extend it**; don't create a parallel artifact.

2. **Nothing exists, and the user has a preference.** Ask where new knowledge should live and offer concrete options:
   - "Drop it in `~/Documents/Claude/Projects/Structural Thinking/` next to the existing dump?"
   - "Add it as `docs/research/postgres-replication.md` in the relevant repo?"
   - "Make a new top-level folder for research artifacts? What naming convention?"

3. **Nothing exists, no preference.** Default to a new file in the most local directory that makes sense for the audience — repo-local if it's repo-specific, project-folder-local if it's cross-cutting. Always state the default before creating files: "I'll put this at `<path>` unless you'd rather another spot."

**Never silently invent a layout.** This rule is shared with `grill-with-docs` and applies anywhere knowledge artifacts touch disk.

## Phase 3 — Plan the artifact

Before researching, write the **table of contents** of the eventual doc. This is the most underrated move in long-form research — having an outline first forces the research to fill known gaps instead of wandering.

A good TOC for a standard-depth dive on a technical topic typically looks like:

1. **What is it, in one paragraph?** — the elevator pitch.
2. **Why it exists / what problem it solves** — the historical or design context.
3. **How it works** — the mechanism, at the level the audience needs.
4. **When to use / not use it** — explicit decision criteria.
5. **Trade-offs and live debates** — where credible practitioners disagree.
6. **Concrete examples** — at least one worked example. War stories if you can get them.
7. **Anti-patterns and failure modes** — what goes wrong, and why.
8. **References** — books, papers, repos, talks. Annotated.

For non-technical topics (competitor, market, regulatory area), adapt the sections — but **always keep "trade-offs / live debates"**, **"anti-patterns"**, and **"references"**. Their absence is the tell of shallow research.

Show the TOC to the user. Adjust before researching, not after.

## Phase 4 — Research breadth-first, then depth

Two passes.

**Breadth.** Cover the entire TOC at shallow depth first. Pull from primary sources where they exist (canonical books, papers, well-cited blog posts, repo READMEs). Aim to fill each section with something defensible before any section is "finished".

**Depth.** Now go back through and deepen the sections that matter most for the audience. Spend disproportionate time on:
- The mechanism (so the audience can reason about it).
- The trade-offs (so the audience can decide).
- The anti-patterns (so the audience doesn't repeat known failures).

Skip or shorten:
- History older than the last major change.
- Generic background the audience already has.
- Trivia.

**Cite as you write.** Inline links in markdown are sufficient — full citation discipline isn't required for a knowledge artifact, but every non-obvious claim should have something the next reader can check.

## Phase 5 — Surface live debates and uncertainty

Three things every deep dive must surface explicitly:

1. **Where credible practitioners disagree.** Name both positions, say which one the artifact prefers and why. Hiding disagreement makes the doc age badly.
2. **What you're uncertain about.** A list of open questions and what would resolve them. Better to admit unknown than to fill it with confident nonsense.
3. **What you didn't have time / access to investigate.** Future-you (or the next reader) can pick these up. Honest deferral compounds; pretend coverage doesn't.

## Phase 6 — Maintain across sessions

A deep dive isn't a one-shot — it's a living artifact. Each return session:

1. **Read the existing doc first.** Don't re-research what's already there.
2. **Append new findings, don't rewrite.** Use dated sub-sections when material is being added incrementally. ("Update 2026-06-12: …").
3. **Mark superseded claims explicitly.** If new information overturns something, strike or annotate the old claim — don't just delete it. The reasoning trail is part of the knowledge.
4. **Refactor when the doc has been amended four or five times.** At that point a clean pass through the structure pays back.
5. **Keep the "open questions" list current.** Resolved questions get the answer inline and a note in the changelog. New questions get added at the top of the list.

## Phase 7 — Hand off

Every deep-dive ends with:

- The artifact at its agreed location, written.
- A one-paragraph TL;DR at the top — pyramid-principle style: the reader should be able to stop after the TL;DR and have the gist.
- A short "what's missing" section listing the open questions and deferrals from Phase 5.
- If the audience is someone other than the user, a one-line "how to read this" pointer ("Skim sections 1, 4, 7 first — they cover the working knowledge").

## Anti-patterns

- **Researching first, structuring later.** The TOC must come first. Without it, research becomes a Wikipedia walk.
- **Filling every section to equal depth.** Audience-driven depth — deeper where decisions live, shallower elsewhere.
- **Hiding what you don't know.** Open questions and deferrals are features, not failures.
- **Inventing a directory.** Phase 2's discover-and-ask step is non-negotiable.
- **Rewriting from scratch on a second session.** Append, mark superseded, refactor only when needed.
- **Skipping the live-debates section.** Every interesting topic has one. If you can't find disagreement, the dive is too shallow.

## Output

- One markdown artifact at the agreed location, with a numbered TOC.
- A TL;DR at the top.
- A `## Open questions` section near the end.
- A `## References` section at the bottom with annotated links.
- A short message back to the user saying where the artifact landed and the headline finding(s).
