# Vikas's Skills — Structural Thinking for Agentic Work

A personal collection of agent skills used for real engineering and structured problem-solving with AI agents — without sliding into vibe coding or producing a beautiful, expensive ball of mud.

The skills here are **small, composable, and adaptable**. They work with Claude Code, Cowork, the Anthropic Skill format generally, and translate cleanly to Gemini-style skill loaders. They are based on decades-old engineering and decision-making fundamentals — vertical slices, TDD, deep modules, ubiquitous language, MECE, Cynefin, Working Backwards, OODA. The ideas didn't break when AI showed up; they got more important.

Inspired by [`mattpocock/skills`](https://github.com/mattpocock/skills), expanded with a universal first-move protocol and generic structural-thinking moves. Keep iterating — make them yours.

## The two operating principles

1. **Diagnose before prescribe.** Every workflow opens with classification — what kind of problem is this, what does done look like, how big is the appetite? Cynefin + Working Backwards + Shape Up appetite, applied universally.
2. **Adapt, never impose.** Skills discover what your project already does (doc layout, decision records, vocabulary) and work with it. No skill hardcodes `docs/adr/` or `CONTEXT.md`. Where new artifacts must land, the skill asks before creating.

Full rationale lives in [`skills-design-principles.md`](../skills-design-principles.md) in the parent folder.

## How to use this repo

Every non-trivial request starts at [**`start`**](skills/general/start/SKILL.md). It runs a five-step protocol — **Sense → Classify domain → Imagine done → Constraints → Route** — and dispatches to the right next skill.

From there, software work falls into one of three bucket workflows. Non-software work (research, decisions, design calls, planning) routes from `start` to a general atomic skill directly.

| Bucket            | When                                                                                | Workflow                                                                                |
| ----------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------- |
| **Small change**  | One file, ≲ 2 hours, reversible. Bug fixes, copy tweaks, single-function changes.   | [`software-engineering/workflows/small-change`](skills/software-engineering/workflows/small-change/SKILL.md)     |
| **Medium feature**| One bounded context, 1–5 days. New user-visible capability.                          | [`software-engineering/workflows/medium-feature`](skills/software-engineering/workflows/medium-feature/SKILL.md) |
| **Large project** | Multi-week, multi-context, multi-agent. New subsystem, big refactor, big migration.  | [`software-engineering/workflows/large-project`](skills/software-engineering/workflows/large-project/SKILL.md)   |

## Two buckets

Skills live in one of two top-level folders:

- **`skills/general/`** — works on any problem (software, research, decisions, planning, writing). Universal moves: framing, interviewing, researching, orienting, transferring context.
- **`skills/software-engineering/`** — software-specific. PRDs, issues, TDD, debugging, architecture work, the three bucket workflows.

The universal `start` skill lives in `general/` and routes into either bucket depending on what the request is.

## The four failure modes these skills fight

1. **The agent didn't do what I wanted.** Fix: [`grill-me`](skills/general/grill-me/SKILL.md) or [`grill-with-docs`](skills/general/grill-with-docs/SKILL.md).
2. **The agent doesn't speak our domain or topic.** Fix: shared vocabulary captured wherever it already lives via [`grill-with-docs`](skills/general/grill-with-docs/SKILL.md); deep knowledge built via [`deep-dive`](skills/general/deep-dive/SKILL.md).
3. **The code doesn't work.** Fix: tight feedback loops via [`tdd`](skills/software-engineering/tdd/SKILL.md) and [`diagnose`](skills/software-engineering/diagnose/SKILL.md).
4. **We built a ball of mud.** Fix: periodic deepening via [`improve-architecture`](skills/software-engineering/improve-architecture/SKILL.md); design-aware planning via [`to-prd`](skills/software-engineering/to-prd/SKILL.md) and [`zoom-out`](skills/general/zoom-out/SKILL.md).

## Repo layout

```
.
├── README.md
├── CLAUDE.md                          # agent-side instructions
├── CONTEXT.md                         # OPTIONAL template — copy if you want this layout
├── docs/
│   └── adr/                           # OPTIONAL — example only; skills adapt to your layout
└── skills/
    ├── general/                       # works on any problem (~6 skills)
    │   ├── start/SKILL.md             # universal first-move protocol
    │   ├── grill-me/SKILL.md          # adversarial alignment interview
    │   ├── grill-with-docs/SKILL.md   # grill + adapt to your doc layout
    │   ├── deep-dive/SKILL.md         # research and curate any topic
    │   ├── zoom-out/SKILL.md          # broaden context; systems thinking
    │   └── handoff/SKILL.md           # compact a session for the next agent
    └── software-engineering/          # software-specific (~8 skills)
        ├── to-prd/SKILL.md
        ├── to-issues/SKILL.md
        ├── tdd/SKILL.md
        ├── diagnose/SKILL.md
        ├── improve-architecture/SKILL.md
        └── workflows/
            ├── small-change/SKILL.md
            ├── medium-feature/SKILL.md
            └── large-project/SKILL.md
```

`CONTEXT.md` and `docs/adr/` are **example templates** — not required. The skills work fine in a repo with a different doc layout, a Notion-only doc culture, or no docs at all yet.

## Skill format

Each skill is an Anthropic-format `SKILL.md`:

```yaml
---
name: <kebab-case-name>
description: <when to use this skill — phrasing matters; the loader picks based on this>
category: <both | software | generic>
---
```

…followed by markdown instructions. Consumed by Claude Code, Cowork, and any agent loader that follows the Anthropic skill spec.

The `category` tag is informational. Skills tagged `both` apply to any domain (software, research, decisions). Skills tagged `software` are software-engineering specific.

## Installing into a project

Drop the `skills/` directory into your project at one of:

- `~/.claude/skills/` — available to all your projects.
- `<repo>/.claude/skills/` — scoped to one project.
- Wherever your agent loader expects skills.

Then add a `CLAUDE.md` at your project root pointing the agent at `start` and at whatever knowledge artifacts your project already has. Use this repo's [`CLAUDE.md`](CLAUDE.md) as a starting point.

## Day-to-day

A typical session looks like this:

1. New request lands. **Run [`start`](skills/general/start/SKILL.md).**
2. It senses + classifies + names the end state + names the constraints + dispatches.
3. The matching workflow (or atomic skill) runs end-to-end, composing other atomic skills as needed.
4. If you're swapping agents or hitting context limits, run [`handoff`](skills/general/handoff/SKILL.md).
5. Weekly on large projects, run [`improve-architecture`](skills/software-engineering/improve-architecture/SKILL.md).

## License

MIT. Take, fork, customise. These skills should change as you change.

## Acknowledgements

Heavy structural inspiration from Matt Pocock's [`mattpocock/skills`](https://github.com/mattpocock/skills) — the "engineering fundamentals over vibe coding" framing and several specific atomic skills are his. The universal `start` protocol, the `deep-dive` skill, the adapt-don't-impose principle, and the bucket workflows are this repo's additions.

References worth reading (a fuller list lives in `skills-research-dump.md`):

- [The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V) — Hunt & Thomas
- [A Philosophy of Software Design](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X) — Ousterhout
- [Domain-Driven Design](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215) — Evans
- [Extreme Programming Explained](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658) — Beck
- [Thinking in Systems](https://www.chelseagreen.com/product/thinking-in-systems/) — Meadows
- [Working Backwards](https://www.workingbackwards.com/) — Bryar & Carr
- [Shape Up](https://basecamp.com/shapeup) — Singer
- ["A Leader's Framework for Decision Making"](https://hbr.org/2007/11/a-leaders-framework-for-decision-making) — Snowden & Boone (Cynefin)
