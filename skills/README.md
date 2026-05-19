# Vikas's Skills — Engineering Discipline for Agentic Coding

A personal collection of agent skills used to do real engineering with AI coding agents — without sliding into vibe coding or producing a beautiful, expensive ball of mud.

The skills here are **small, composable, and adaptable**. They work with Claude Code, Cowork, the Anthropic Skill format generally, and translate cleanly to Gemini-style skill loaders. They are based on decades-old engineering fundamentals (vertical slices, TDD, deep modules, ubiquitous language, ADRs) — the ideas didn't break when AI showed up; they got more important.

Heavily inspired by [`mattpocock/skills`](https://github.com/mattpocock/skills) and shaped to my (Vikas's) day-to-day workflow. Keep iterating — make them yours.

## How to use this repo

There are three sizes of work you'll do day to day:

| Bucket            | When                                                                                | Workflow                                                                |
| ----------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Small change**  | One file, ≲ 2 hours, reversible. Bug fixes, copy tweaks, single-function changes.  | [`workflows/small-change`](skills/workflows/small-change/SKILL.md)      |
| **Medium feature**| One bounded context, 1–5 days. New user-visible capability.                         | [`workflows/medium-feature`](skills/workflows/medium-feature/SKILL.md)  |
| **Large project** | Multi-week, multi-context, multi-agent. New subsystem, big refactor, big migration. | [`workflows/large-project`](skills/workflows/large-project/SKILL.md)    |

If you're not sure which one a request is, hand it to the **[orchestrator](skills/orchestrator/SKILL.md)** first — it classifies the task and routes it.

The three workflows compose the **atomic skills** in `skills/atomic/`. You can also invoke an atomic skill directly when you know which one you need.

## The four failure modes these skills fight

1. **The agent didn't do what I wanted.** Fix: [`grill-me`](skills/atomic/grill-me/SKILL.md) / [`grill-with-docs`](skills/atomic/grill-with-docs/SKILL.md).
2. **The agent is way too verbose / doesn't speak our domain.** Fix: shared `CONTEXT.md` via [`grill-with-docs`](skills/atomic/grill-with-docs/SKILL.md).
3. **The code doesn't work.** Fix: feedback loops via [`tdd`](skills/atomic/tdd/SKILL.md) and [`diagnose`](skills/atomic/diagnose/SKILL.md).
4. **We built a ball of mud.** Fix: weekly deepening via [`improve-architecture`](skills/atomic/improve-architecture/SKILL.md), and the design-aware planning in [`to-prd`](skills/atomic/to-prd/SKILL.md) and [`zoom-out`](skills/atomic/zoom-out/SKILL.md).

## Repo layout

```
.
├── README.md                  # you are here
├── CLAUDE.md                  # agent-side instructions pointing at these skills
├── CONTEXT.md                 # template — copy into your project repo
├── docs/
│   └── adr/                   # architecture decision records (one per file)
└── skills/
    ├── orchestrator/SKILL.md
    ├── workflows/
    │   ├── small-change/SKILL.md
    │   ├── medium-feature/SKILL.md
    │   └── large-project/SKILL.md
    └── atomic/
        ├── grill-me/SKILL.md
        ├── grill-with-docs/{SKILL.md, CONTEXT-FORMAT.md, ADR-FORMAT.md}
        ├── tdd/SKILL.md
        ├── diagnose/SKILL.md
        ├── to-prd/SKILL.md
        ├── to-issues/SKILL.md
        ├── zoom-out/SKILL.md
        ├── handoff/SKILL.md
        └── improve-architecture/SKILL.md
```

## Skill format

Each skill is an Anthropic-format `SKILL.md`:

```yaml
---
name: <kebab-case-name>
description: <when to use this skill — phrasing matters; the loader picks based on this>
---
```

…followed by markdown instructions. This format is consumed by Claude Code, Cowork, and any agent loader that follows the Anthropic skill spec. It also slots cleanly into Gemini-style skill systems.

## Installing into a project

Drop the `skills/` directory into your project at one of:

- `~/.claude/skills/` — available to all your Claude projects.
- `<repo>/.claude/skills/` — scoped to one project.
- Wherever your agent loader expects skills.

Then add a `CLAUDE.md` at your project root pointing the agent at the orchestrator and at your project's `CONTEXT.md`. Use the template in this repo's [`CLAUDE.md`](CLAUDE.md) as a starting point.

## Day-to-day

A typical session looks like this:

1. New request lands. **Run the [orchestrator](skills/orchestrator/SKILL.md).**
2. It classifies → small / medium / large.
3. The matching workflow runs end-to-end, composing atomic skills as needed.
4. If you're swapping agents or hitting context limits, run [`handoff`](skills/atomic/handoff/SKILL.md).
5. Weekly on large projects, run [`improve-architecture`](skills/atomic/improve-architecture/SKILL.md).

## License

MIT. Take, fork, customise. These skills should change as you change.

## Acknowledgements

The architecture here is shaped by, and in many places paraphrases, Matt Pocock's [`mattpocock/skills`](https://github.com/mattpocock/skills). The structure, naming, and the philosophy of "engineering fundamentals over vibe coding" is his. The customisations and the bucket-based orchestrator are mine. Standing on the shoulders of better engineers.

References worth reading:

- [The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V) — Hunt & Thomas
- [A Philosophy of Software Design](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X) — Ousterhout
- [Domain-Driven Design](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215) — Evans
- [Extreme Programming Explained](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658) — Beck
