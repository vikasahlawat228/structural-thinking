---
name: improve-architecture
description: Find deepening opportunities and structural debt in a codebase. Runs as a periodic pass (weekly on large projects) or when agents start producing locally-correct but globally-wrong code. Discovers and respects the project's existing doc layout — does not impose one. Trigger phrases include "refactor pass", "clean up architecture", "deepen modules", "ball of mud", "tech debt audit".
category: software
---

# Improve Architecture

Codebases entropy. With AI agents, they entropy faster — agents write a lot of plausible code that gradually shallows every module. The defence is a **periodic, deliberate deepening pass**.

This skill is **not** for mid-slice refactoring. Slices ship; refactors get their own slices.

## When to run

- On a large project: weekly, or on a tangible signal that agents are getting confused.
- When a diff review surfaces the same architectural smell three times in a row.
- Before starting a new milestone — clean ground beats messy ground.
- After landing a difficult bug fix that exposed a structural problem.

**Do not** run this in the middle of a feature. Finish the feature, then run this on a clean tree.

## Step 1 — Re-read the load-bearing artifacts

Before touching code, read whatever the project uses as its long-lived knowledge — wherever it lives:

1. **The glossary / domain doc.** Whether that's `CONTEXT.md`, a README section, a Notion page, or `GLOSSARY.md`. What is the domain supposed to look like?
2. **The decision records.** In whichever convention the project uses (`docs/adr/`, `decisions/`, a single `DECISIONS.md`, a Notion database). What is being honoured?
3. **The most recent PRDs.** What was the direction of travel?

If the glossary is stale relative to what you know about the project, **that itself is the first problem.** Update it (per `grill-with-docs`) before continuing.

## Step 2 — Ousterhout red-flag scan

Walk through the codebase and tag every instance of each smell. These are the seven well-named smells from Ousterhout's *A Philosophy of Software Design* — the spine of the modern refactor toolkit.

### Smell 1 — Shallow modules

A module with a small interface **and** a small body. It earns nothing — the indirection costs more than it gives back. The best modules are *deep*: simple interface, substantial implementation.

Fix: collapse the module into its caller, or absorb related functionality so the body grows to match the cost of the interface.

### Smell 2 — Information leakage

The same piece of knowledge — a data format, a protocol detail, a regex — appears in more than one place. Changing it requires changing all of them, and someone always forgets one.

Fix: pull the knowledge behind a single owning module.

### Smell 3 — Temporal decomposition

Code organised by *when* things happen rather than *what* they're about. Often shows up as `step1.ts`, `step2.ts`, `validate-then-process-then-store.ts`.

Fix: re-organise by domain concept, not by step order.

### Smell 4 — Pass-through methods

A method that does nothing but forward its arguments to another method, often with a renamed parameter. Adds depth-of-call without depth-of-module.

Fix: inline it, or give it a real responsibility.

### Smell 5 — Repeated knowledge of the same fact

The same three files always change together. They are one module wearing a costume.

Fix: collapse them, or extract the shared concept into a fourth module they all depend on.

### Smell 6 — Chatty interfaces

Two modules constantly call back and forth. Each call is shallow; cost is in the round-trip.

Fix: merge them, or extract the chatty surface into a third module that owns the conversation.

### Smell 7 — Vague names and hard-to-describe interfaces

If you can't describe what a module does in one sentence, the module doesn't have one job. Vague names (`manager`, `helper`, `util`, `service`) are tells.

Fix: rename to the responsibility. If you can't, the module probably has more than one responsibility — split it.

## Step 3 — Domain drift check

Independent of the seven Ousterhout smells, run one more pass: does the code use the project's glossary vocabulary consistently?

> Variables named `account` when the glossary says `user`. Functions named `cancelOrder` when the glossary says `voidOrder`. Modules named `things-controller` when the domain calls them `redemption-handlers`.

Fix: rename the code, **or** update the glossary if the code's term is genuinely better (per `grill-with-docs`). Never let them drift. Aesthetic renames are noise — only rename to match the glossary or fix a real confusion.

## Step 4 — Tidy First decision per smell

For each smell you've tagged, make a Beck-style decision before doing anything:

- **Tidy First** — fix this before the next feature touches the area. Makes the next change easier.
- **Tidy After** — fix this right after the next feature lands. Often the right call when the feature reveals what's actually needed.
- **Don't Tidy** — leave it. Most smells aren't worth fixing. The point of a periodic pass is that there will be a next one.

Pick the **two or three highest-reach, lowest-fix-cost items**. Do not try to fix everything in one pass.

## Step 5 — Refactor in slices, using named moves

Each refactor is its own vertical slice (just like a feature). Apply [`tdd`](../tdd/SKILL.md):

1. If existing tests already cover the behaviour, good. If not, **add characterisation tests first** (Feathers' move) — pin current behaviour before you change structure. Identify the **seam** — a place you can alter behaviour without editing in place. Preprocessor seams, link seams, object seams.
2. Refactor using **named moves** from Fowler's catalogue — Extract Function, Inline Function, Move Function, Extract Class, Replace Conditional with Polymorphism, etc. Named moves are reviewable; "tidy it up" is not.
3. Run tests after every step.
4. Commit per slice. Each refactor commit is revertible independently.

If the refactor changes a public interface that other modules consume, that's a separate slice **and** a decision record in the project's convention.

## Step 6 — Migration check

If the refactor is non-trivial — touching multiple call sites, a public interface, or a data schema — it's a **migration**, not a refactor. Migrations need explicit dates:

- **Freeze date** — no new call sites of the old thing after this date.
- **Deprecation date** — old call sites warn but still work.
- **Removal date** — old thing is deleted.

Most architecture work fails in the long tail. The strangler fig pattern (route around the old, never rewrite in place) is the default tactic; assign an owner per call-site cluster.

## Step 7 — Conway check

Before landing the refactor: does the change cross team boundaries (per the project's CODEOWNERS or org map)?

If yes, the social cost dominates the technical cost. Don't just merge — coordinate with the affected teams first. Team Topologies framing: cognitive-load increase across boundaries is the failure mode.

## Step 8 — Record and re-baseline

Once the refactor lands:

- Update the glossary if any term moved or sharpened (in whichever artifact holds it).
- Mark superseded prior decisions explicitly — don't delete them; chain `Superseded by …`.
- PR description names the smell, the named refactor moves, and the reach.
- DORA acceptance check: did this improve deployability, loose coupling, or testability — or is it only conceptually cleaner? If only the latter, ask whether it was worth the slice.

## Live debate — SOLID / Clean Architecture vs CUPID

This skill leans on Ousterhout's deep-modules frame, not on SOLID or Clean Architecture wholesale.

- **SOLID / Clean Architecture (Martin)** prescribes concentric layers and dependency inversion at every internal boundary. Popular and contested. Useful at **real infrastructure boundaries** (DB, network, third-party).
- **CUPID (North)** — Composable, Unix-philosophy, Predictable, Idiomatic, Domain-based — argues that SOLID is overprescribed and ages badly. Hillel Wayne's "I Have Complicated Feelings About SOLID" is the surgical critique.

**Position this skill takes:** apply dependency inversion at infrastructure boundaries (Hexagonal / Ports & Adapters). Don't layer the domain into concentric onion rings without a specific reason — keep Ousterhout's deep modules instead.

## Live debate — DRY vs duplication-tolerance

Sandi Metz: "Duplication is far cheaper than the wrong abstraction." Premature DRY-via-shared-helper is the canonical refactor failure.

**Position this skill takes:** prefer duplication until the abstraction is clear from three or more concrete uses. When you do extract, name it for the domain concept, not for "the shared part".

## Anti-patterns

- **Big-bang refactor.** Refactors touching dozens of files in one commit cannot be reviewed. Slice them.
- **Refactor mid-feature.** Finish the feature; the refactor gets its own slice.
- **Refactoring without tests.** If you can't prove behaviour was preserved, you didn't refactor — you rewrote.
- **Refactoring everything spotted.** Pick two or three. Defer the rest.
- **Aesthetic renames.** Renames must match the glossary or fix a real confusion.
- **Premature DRY.** Wait for three concrete uses before abstracting.
- **Layered architecture for layered-architecture's sake.** Apply Hexagonal at infra boundaries only.
- **Imposing a doc layout.** Discover the project's convention. Don't create `docs/adr/` because you've seen one before.

## When this skill isn't enough

- The structural problem is a missing decision, not bad code → run [`grill-with-docs`](../../general/grill-with-docs/SKILL.md) and write the decision record first.
- The problem is a single-file mess → fix it in-flight, no skill needed.
- The smell touches multiple bounded contexts → escalate; run [`large-project`](../workflows/large-project/SKILL.md) on the refactor itself.
- The smell is at the team / org boundary, not the code → cite the Conway check and escalate to whoever owns the org topology.
