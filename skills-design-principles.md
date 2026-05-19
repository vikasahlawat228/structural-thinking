# Skills Design Principles

The rationale doc for `vikasahlawat228/skills`. Read this before editing any `SKILL.md`. Every claim in this doc has a citation; every directive in every SKILL.md that this doc influences must, in turn, have a citation back to either an authoritative source or to a clause of this doc.

---

## Part A — The question

Before we touch a skill, we have to answer a single question:

> **What is the single best "first move" for any non-trivial problem — generic or software-specific — and what skill architecture makes that first move reliable?**

Everything in this doc is in service of that question. The current repo has an answer for software-only work (`orchestrator` → small/medium/large). It does not have an answer for problems that aren't yet software at all (a research question, a decision, a design call, a non-code investigation). We need a unified answer.

---

## Part B — Survey of "first move" frameworks

A scan of the major problem-solving traditions, each summarised at one paragraph and reduced to its prescribed first move. Sources cited inline.

### Generic structural thinking

- **Polya, *How to Solve It* (1945).**[^polya] First move: **Understand the problem.** Restate it; identify the unknown, the data, and the condition. Polya treats premature planning as the most common failure mode.

- **MECE / Pyramid Principle — Minto (1978).**[^minto] First move: **Decompose into mutually exclusive, collectively exhaustive buckets.** Structure precedes argument; structure precedes solution.

- **Issue trees / hypothesis-driven analysis — McKinsey tradition; Conn & McLean (2019).**[^bps] First move: **Frame the question, branch into sub-questions, form a falsifiable hypothesis.** The cardinal sin is "boiling the ocean".

- **Cynefin — Snowden & Boone (HBR 2007).**[^cynefin] First move: **Diagnose which domain you are in** — Clear, Complicated, Complex, or Chaotic. Each domain demands a different method; applying the wrong method is the primary cause of failure.

- **OODA — Boyd (briefing, 1970s-90s; Osinga 2007).**[^ooda] First move: **Observe.** Then orient — and Boyd is emphatic that *orient* is where most failures live, not observe or act.

- **PDCA / PDSA — Shewhart / Deming.**[^deming] First move: **Plan a small change with an explicit prediction.** The discipline is the prediction, not the plan.

- **Toyota A3 / 5 Whys — Ohno (1988); Shook (2008).**[^a3] First move: **Define the gap between current state and goal state** on a single page. Causal analysis comes after the gap is named.

- **Pre-mortem — Klein (HBR 2007).**[^klein-premortem] First move: **Imagine you have already failed**, list the reasons. Inverts the planning fallacy.

- **First principles — Aristotle; Feynman; Munger.**[^feynman][^munger] First move: **Strip the problem to the irreducible truths you are certain of**, then rebuild.

- **Decisions under uncertainty — Kahneman (2011); Klein (1998); Tetlock (2015); Duke (2018).**[^kahneman][^klein-sources][^tetlock][^duke] First move: **Frame the call as a bet.** Identify the priors, the reversibility, and the reference class.

- **Toulmin model of argument (1958).**[^toulmin] First move: **Make the warrant explicit** — the bridge that says "this evidence supports this claim". Most arguments break at the warrant, not the data.

- **Working Backwards / PR-FAQ — Bryar & Carr (2021).**[^working-backwards] First move: **Write the press release for the launched-and-loved product** before you write code or specs.

- **Design thinking — IDEO; Kelley & Kelley (2013).**[^ideo] First move: **Empathise** — observe the user; the problem statement comes from observation, not from intuition.

- **TRIZ — Altshuller.**[^triz] First move: **Define the Ideal Final Result.** What would the perfect solution look like, even if physically impossible? Then trace back.

### Software-specific

- **Test-Driven Development — Beck (2002).**[^beck-tdd] First move: **Write a failing test.** A failing test is the first concrete statement of intent.

- **Working Effectively with Legacy Code — Feathers (2004).**[^feathers] First move: **Find a seam.** You cannot safely change behaviour you cannot characterise.

- **Domain-Driven Design — Evans (2003); Vernon (2013).**[^evans][^vernon] First move: **Build a ubiquitous language with the domain experts.** Code that does not share vocabulary with the business is technical debt by construction.

- **EventStorming — Brandolini.**[^eventstorming] First move: **Identify pivotal domain events.** Models follow events; events precede aggregates.

- **Shape Up — Singer / Basecamp (2019).**[^shapeup] First move: **Set the appetite** — how much is this *worth*, not how long will it *take*.

- **A Philosophy of Software Design — Ousterhout (2021).**[^posd] First move: **Decide what the module's interface should hide.** Depth is the design goal.

- **Refactoring — Fowler (1999/2018).**[^fowler-refactoring] First move: **Don't refactor without tests.** Safety is a precondition, not an outcome.

- **Tidy First? — Beck (2023).**[^tidy-first] First move: **Decide tidy-before, tidy-after, or don't tidy.** Mixing structural and behavioural changes in one commit is the canonical anti-pattern.

- **Accelerate / DORA — Forsgren, Humble, Kim (2018).**[^accelerate] First move: **Measure** — lead time, deployment frequency, change failure rate, MTTR. You cannot improve what you cannot see.

- **Site Reliability Engineering — Beyer et al., Google (2016).**[^sre] First move: **Define the SLO.** Reliability targets precede architecture.

- **Resilience engineering — Cook (1998); Dekker; Woods; Allspaw.**[^cook][^dekker] First move: **Assume the system is constantly close to failure.** "Root cause" is a comforting fiction; map contributors.

- **Spec-driven dev — BMAD / GSD / Spec-Kit; Pocock's critique.**[^pocock-skills] First move (per the spec frameworks): **Write the spec.** First move (per Pocock): **Grill the request until alignment is real, then slice.**

- **Anthropic agent skills (this repo's substrate).**[^anthropic-skills] First move: **The trigger description does the routing.** Skills only fire when their description matches the situation.

### What the AI-agent era adds

- **Pocock's "Four Failure Modes" (mattpocock/skills README, 2026).**[^pocock-failures] Misalignment, verbosity (no shared language), broken code (no feedback loop), ball of mud (no design care). Each failure mode is unaddressed by classical software practice — they show up specifically because the agent has more leverage than a human and accelerates entropy.

- **Bret Victor, "Inventing on Principle" (2012).**[^victor] First principle of creative work: **the latency of the feedback loop.** Below 1 second, behaviour changes qualitatively. Doubles as the core rationale for `diagnose`.

- **GeePaw Hill, "Many More, Much Smaller Steps" (MMMSS).**[^geepaw] First move: **Halve the step until you can see it land.** Step size is the master variable.

- **Charity Majors, "Observability is the way".**[^charity] First move in production debugging: **emit a high-cardinality wide event per request**, then query against it. Pre-defined dashboards lose to unknown-unknowns.

---

## Part C — What every framework has in common

Three meta-patterns appear in every framework worth borrowing from. Any skill we ship should respect all three.

### 1. Diagnose before prescribe

Cynefin diagnoses the domain before picking a method.[^cynefin] Polya's "understand" precedes "plan".[^polya] OODA's "orient" precedes "decide".[^ooda] Working Backwards diagnoses customer value before solution shape.[^working-backwards] Feathers locates the seam before changing behaviour.[^feathers] The frameworks that *most* reliably fail in practice (5 Whys as a single-thread root-cause hunt, OODA used as a buzzword, "first principles" used to dismiss tacit knowledge) are the ones people apply *without* the diagnosis step.[^cook][^allspaw-infinite-hows]

**Implication for skills:** every workflow skill must open with a diagnosis phase. No skill should land code or commit to a path before the situation is named.

### 2. Make structure explicit

Minto's pyramid names the answer-then-reasons skeleton.[^minto] Toulmin names the joints of an argument.[^toulmin] Issue trees name the decomposition.[^bps] CIRCLES names PM-design steps.[^lin] DDD names the bounded contexts.[^evans] PDCA names the iteration phases.[^deming] The structure isn't *true* — it is a scaffold that makes thought inspectable by collaborators and by your future self. Once the scaffold is visible, errors become locatable: missing branch, weak warrant, wrong domain, untested assumption.

**Implication for skills:** every skill must produce a named, scannable output — not just an answer. The output of `grill-me` is a plan paragraph + open-questions list. The output of `diagnose` is a hypothesis log. The output of `handoff` is a sectioned doc. Free-form prose is the failure mode.

### 3. Close the loop

PDCA, OODA, hypothesis-driven analysis, pre-mortem/AAR, Tetlock's calibration loop — every durable framework has an explicit "look back and update" phase.[^deming][^ooda][^bps][^klein-premostem-aar][^tetlock] Frameworks without a feedback step (5 Whys without revisiting, "we built the spec" without verifying) decay over time.

**Implication for skills:** every workflow skill ends with a verification or reflection phase. Cleanup, regression test, postmortem hypothesis logged in commit message, ADR superseded — these are not nice-to-haves.

---

## Part D — The starting phase, synthesised

Based on the survey and the three meta-patterns, here is the single opinionated **first move** for any problem the user brings to an agent in this repo. It absorbs Cynefin, Working Backwards, Shape Up, Bezos's one-way / two-way door, and the issue-tree tradition. It replaces and generalises the current `orchestrator` skill.

### The five steps

**Step 1 — Sense.** Name the situation in one sentence. "This is a bug in X." "This is a question about how to decide Y." "This is a design call for Z." "This is a research ask about W." Skipping this is how agents end up solving a different problem than the one they were given.[^polya][^minto]

**Step 2 — Classify the domain.** Apply Cynefin's diagnosis:[^cynefin]
- **Clear** (cause and effect obvious, best practice exists) → known-issue runbook.
- **Complicated** (cause and effect knowable with expertise) → analysis, expert input.
- **Complex** (cause and effect only known in retrospect) → safe-to-fail experiments, not big plans.
- **Chaotic** (no cause-and-effect yet) → act to stabilise, then re-classify.

Most "the project failed" stories are domain-misclassification: a complex problem treated as complicated, or a complicated problem treated as clear.

**Step 3 — Imagine done.** Before solving anything, write what success looks like in one paragraph. For software: write the line item the PR description will contain. For a research ask: write the table-of-contents the resulting doc will have. For a decision: write the sentence you'd send to the team after deciding. This is the Working Backwards inversion[^working-backwards] applied at every scale.

**Step 4 — Constraints.** Three sub-steps, all cheap:
- **Appetite** (Shape Up).[^shapeup] How much is this *worth*? A bug fix may be worth 30 minutes and zero ceremony; a migration may be worth six weeks and substantial ceremony. The wrong-sized process is the most common failure mode in AI-assisted coding.[^pocock-skills]
- **Reversibility** (Bezos one-way / two-way door).[^bezos] If wrong, how expensive is the rollback? One-way doors deserve more deliberation; two-way doors deserve speed.
- **Blast radius.** Who else is affected if this is wrong? A 1-hour change to billing code is *large* by blast radius even if *small* by effort.[^larson-migrations]

**Step 5 — Route.** Given the diagnosis (2), the end state (3), and the constraints (4), pick the next skill or workflow:
- Software work, small effort, low blast radius → `small-change` workflow.
- Software work, medium effort, single context → `medium-feature` workflow.
- Software work, large effort, multi-context → `large-project` workflow.
- Research/learning question → `deep-dive` (the new skill).
- Decision question → a future `decide-under-uncertainty` skill (deferred — see Part F).
- Communication output → a future `structure-communication` skill (deferred).
- Analytical decomposition needed → a future `decompose` skill (deferred).
- The problem is still fuzzy after steps 1-4 → `grill-me` or `grill-with-docs`.

Step 5 is the only step that's repo-specific. Steps 1-4 work for any problem any agent ever sees.

### What this replaces

This synthesis subsumes:
- The current `orchestrator` skill (which only does step 5 — and only for software).
- A standalone Cynefin "diagnose-context" skill (Part B of the research dump proposed one — it collapses into Step 2 of this protocol).
- An ad-hoc "frame the problem" skill that the repo doesn't currently have.

The first skill in the repo becomes a single universal entry point. Working name: **`start`** (recommendation argued in Part G).

---

## Part E — Skill architecture principles

Seven principles, each with a one-line rationale and a citation. Every new or revised SKILL.md must respect all seven; failures of principle should be flagged explicitly in PR.

### Principle 1 — Single responsibility

A skill does one thing. The current `tdd` skill does TDD. The current `diagnose` skill does diagnosis. Skills that do "TDD plus a bit of diagnosis if the test fails strangely" decay because the trigger description has to cover both and ends up triggering on neither cleanly. Rationale: the same principle that makes a Unix command durable — `cat` does one thing.[^unix-philosophy]

### Principle 2 — Adapt, never impose

A skill that touches files must discover what the project already does and work with it. A skill that hardcodes `docs/adr/`, `CONTEXT.md`, or `src/<module>/CONTEXT.md` excludes every project that doesn't use that layout — which is most of them. Rationale: this is exactly the point Vikas raised, and exactly the failure mode of BMAD/GSD/Spec-Kit (which Pocock critiques as "owning the process and burying developer judgment").[^pocock-skills]

Practically: discover the existing convention (`README.md`, `NOTES.md`, `glossary.md`, `decisions/`, `adr/`, `architecture/`, a Notion link, a Confluence link, an Obsidian vault) before writing. If nothing exists, ask the user where to put the artifact and offer a single sensible default — never assume.

### Principle 3 — Composable, not monolithic

Workflows compose atomic skills; atomic skills compose nothing. A workflow that inlines TDD instead of pointing at `tdd` is a maintenance liability. Rationale: deep modules with simple interfaces beat shallow modules with chatty interfaces, at the skill layer as well as the code layer.[^posd]

### Principle 4 — Trigger description = contract

The YAML `description` field is not metadata — it is the skill's loading condition. If the description doesn't match how a user would phrase the problem, the skill never fires. Rationale: Anthropic's loader uses descriptions for retrieval.[^anthropic-skills] The description is the user-facing API; the body is the implementation.

Every description must answer: *what does the user say or do that should trigger this?* Include trigger phrases verbatim.

### Principle 5 — Every directive is grounded (but citations live outside the SKILL.md)

Every non-obvious directive in a SKILL.md must be traceable to (a) an external authoritative source, or (b) a clause in this principles doc, or (c) an explicit local decision documented in `decisions/`. Lines that lack backing are either:
- Conventional wisdom worth keeping but should be marked as such (e.g., "—Pocock convention").
- Cargo-cult and should be removed.

**Important — citations live outside the SKILL.md.** Skills load into the agent's context every time they fire; carrying footnotes in every skill wastes tokens at runtime. The audit trail lives in two places only: `skills-research-dump.md` and this principles doc. While **writing** a SKILL.md, the author confirms every directive against these two sources — but the rendered SKILL.md stays clean.

Reviewers asking "why does the skill say X?" trace via the research dump or this doc, not via inline citations in the skill.

Rationale: this is what makes the repo defensible against "but why?" pushback and what makes it durable across Vikas's own future revisions, *without* paying a per-invocation token tax.

### Principle 6 — Surface live debates

Where a practice has real disagreement among credible practitioners, name the disagreement and the position the skill takes. Skills that pretend disagreements don't exist age badly. Rationale: Pyramid Principle treats "alternatives considered" as a section of any persuasive doc;[^minto] Toulmin treats explicit rebuttal as a structural component of a sound argument.[^toulmin]

Current repo has at least four active debates that should be surfaced:
- TDD classicist vs. mockist (`tdd`).[^fowler-mocks]
- Single-root-cause vs. multiple-contributors (`diagnose`).[^cook][^dekker]
- SOLID / Clean Architecture vs. CUPID / "just write the code" (`improve-architecture`).[^north-cupid][^wayne-solid]
- Narrative prose vs. bullet lists in writing artifacts (`handoff`, `to-prd`).[^amazon-narrative]

### Principle 7 — End with a loop

Every workflow skill ends with a verification or reflection step. Every atomic skill ends with a stop condition. Skills without an end-of-loop step encourage agents to keep going past the point of value. Rationale: the third meta-pattern in Part C — every durable framework has a "look back" phase.[^deming][^ooda][^tetlock]

---

## Part F — Writing standard (skills carry no citations)

Per the revised Principle 5: **SKILL.md files contain no footnotes, no inline citations, no `## References` section.** The audit trail lives only in `skills-research-dump.md` and this principles doc.

### What this means in practice

When writing or editing a SKILL.md:

1. For every non-trivial directive — phase, technique, anti-pattern, live debate, numeric claim — the author confirms a backing source in the research dump or principles doc *before* writing the line.
2. If a directive has no backing, it is either (a) cargo-cult and gets cut, or (b) a deliberate local convention which gets recorded in the dump first, then referenced in the skill.
3. The rendered SKILL.md contains the directive in plain prose. No source attribution inline.

Example — what a phase opening looks like in the new style:

```markdown
## Phase 1 — Build a feedback loop

If you have a fast, deterministic, agent-runnable pass/fail signal for the
bug, you will find the cause. If you don't, no amount of staring at code
will save you. Spend disproportionate effort here.
```

That directive is backed (Victor's "Inventing on Principle"; Zeller's *Why Programs Fail*) — but the backing is **not** rendered. A reviewer who asks "why?" reads the research dump's `diagnose` section.

### Tone consequences of this rule

- Skills read like opinionated playbooks, not academic papers.
- Anti-patterns become assertions ("Horizontal slicing produces tests insensitive to real behaviour"), not arguments.
- Live debates (Principle 6) are surfaced *as the position the skill takes*, with the other side named in one line — but without footnote sprawl.

### Audit mechanism

Whenever a SKILL.md is edited:

1. New directives go through the same backing check.
2. If a new directive doesn't trace to the research dump or this doc, add a short entry to the research dump *first*, then add it to the skill.
3. Any reviewer ("but why is this in the skill?") opens the research dump, finds the matching section, gets the citation.

This keeps skills fast at runtime and defensible offline — without the per-invocation token tax of inline citations.

---

## Part G — Updated skill map and naming

### What changes

Based on the synthesis above, the current 13 skills change as follows.

| Current skill | Disposition | Why |
| -------------- | ----------- | --- |
| `orchestrator` | **Replace with `start`** (or rename to `start`). Generalise to universal first-move protocol (Part D). | Current scope is software-only; we need a universal entry point. |
| `workflows/small-change` | Keep. Apply Principle 5 (cite). | Lifecycle still valid for SE work; needs sources. |
| `workflows/medium-feature` | Keep. Apply Principle 5. | Same. |
| `workflows/large-project` | Keep. Apply Principles 2, 5. | Currently hardcodes `CONTEXT.md` and `docs/adr/` — must adapt. |
| `atomic/grill-me` | Keep. Apply Principle 5. | Universal; high-leverage; mostly fine. |
| `atomic/grill-with-docs` | **Rewrite under Principle 2.** | The directory-structure constraint Vikas flagged. Discover docs; never assume layout. |
| `atomic/tdd` | Keep. Apply Principles 5, 6. | Surface classicist-vs-mockist live debate. |
| `atomic/diagnose` | Keep. Apply Principles 5, 6, 7. | Surface root-cause-vs-contributors debate; explicit feedback-loop lede. |
| `atomic/to-prd` | Keep. Apply Principles 2, 5. | Should not assume GitHub issues; should discover the project's tracker convention. |
| `atomic/to-issues` | Keep. Apply Principles 5. | INVEST + walking skeleton + Cohn/Lawrence splitting patterns. |
| `atomic/zoom-out` | Keep. Apply Principles 5. | Add Meadows / C4 / arc42 ladder. |
| `atomic/improve-architecture` | Keep. Apply Principles 5, 6. | Surface SOLID-vs-CUPID live debate. |
| `atomic/handoff` | Keep. Apply Principles 5. | Pyramid Principle + I-PASS structure. |

### What's new

Two new skills are scaffolded as part of this rework:

1. **`start`** (replacing/renaming `orchestrator`) — universal first-move protocol (Part D).
2. **`deep-dive`** — research-and-curate skill (next subsection).

A third addition — a small `thinking/` bucket of generic skills (`decompose`, `decide-under-uncertainty`, `structure-communication`, `estimate`, `pre-mortem`) — is **deferred to a later pass**. Rationale: the user wants depth before breadth. We earn the right to add five generic skills by first making the existing thirteen excellent.

### The new skill's name

Decision: **`deep-dive`**.

Considered alternatives:

- **`study`** — short and universal; but reads more like learning-for-its-own-sake than building a durable artifact.
- **`knowledge-base`** — names the artifact, not the action. Skills should be verbs.[^principle-trigger]
- **`research-and-curate`** — most explicit; too long; clumsy to invoke.
- **`dossier`** — accurate (the artifact is a dossier), but `compile a dossier on X` doesn't read as a natural agent invocation.
- **`deep-dive`** — the phrase Vikas used; verb-y; intuitive trigger ("deep-dive on Postgres replication"); doesn't collide with any existing skill in Pocock's repo; matches the dump-style output we already produced as a one-shot.

Skill description (draft, to be refined when we write it):

> Conduct a deep, structured research pass on a topic and produce a durable knowledge artifact in any directory the user chooses (or any existing one that already holds relevant docs). Adapts to the project's layout — does not impose one. Use when the user says "research X", "deep-dive on X", "build a knowledge base for X", or "I want to understand X properly". Distinct from `grill-with-docs` (which is interactive alignment) — this skill is proactive curation.

### Walk order

The order we'll edit, with one-line rationale per step:

1. **`start`** (was `orchestrator`) — establish the universal first move, since everything else routes through it.
2. **`grill-me`** — minor revisions; high leverage and we use it inside `start`.
3. **`grill-with-docs`** — rewrite under Principle 2 (the directory-structure fix).
4. **`deep-dive`** (NEW) — scaffold from scratch under all seven principles.
5. **`to-prd`** — adapt to tracker conventions, add Working Backwards / PR-FAQ / Shape Up / Cagan citations.
6. **`to-issues`** — add INVEST checklist, walking skeleton, Cohn/Lawrence splitting patterns, all cited.
7. **`small-change`** — add Tidy/Behavior gate (Beck), MMMSS heuristic (Hill), characterization tests (Feathers).
8. **`medium-feature`** — add walking skeleton gate (Cockburn/GOOS), appetite (Shape Up).
9. **`large-project`** — apply Principle 2 (don't assume directory layout); add Toyota Kata target conditions, DORA snapshot.
10. **`tdd`** — surface classicist-vs-mockist debate; add property-based supplement, snapshot-tests-for-legacy.
11. **`diagnose`** — "the loop is the skill" lede; surface root-cause-vs-contributors debate; USE method; OODA-for-incidents.
12. **`zoom-out`** — Meadows leverage points; C4 four-level ladder; ADR archaeology.
13. **`improve-architecture`** — Ousterhout red-flag scan; surface SOLID-vs-CUPID; Sandi Metz DRY guardrail.
14. **`handoff`** — Pyramid Principle top; I-PASS sections; SUCCESs check.

Per skill, the loop is:
1. Read the current SKILL.md.
2. Apply all seven principles + the citations from the research dump.
3. Decide: **generic / SE-specific / both**. Tag it in the SKILL.md frontmatter as `category:`.
4. Confirm with Vikas.
5. Commit. Move to the next.

---

## Part H — Generic vs SE classification (working tags)

Per Principle-9 question — at the end of each skill walk we'll formally decide. Initial intuitions, to be confirmed skill-by-skill:

| Skill | Initial category | Notes |
| ----- | ---------------- | ----- |
| `start` | **both** | Universal entry point by design. |
| `grill-me` | **both** | Socratic interviewing applies to any decision. |
| `grill-with-docs` | **SE-leaning, but both** | The pattern is universal; the doc shape is SE-flavoured. After the rewrite under Principle 2, should be "both". |
| `deep-dive` | **both** | Generic by design — fork-of from the research dump. |
| `to-prd` | **SE** | Product / engineering specific. A generic "structure-communication" skill could spin off later. |
| `to-issues` | **SE** | Issue trackers + slicing. Generic cousin = decomposition (deferred). |
| `small-change` | **SE** | Code-specific. |
| `medium-feature` | **SE** | Code-specific. |
| `large-project` | **SE** | Code-specific. |
| `tdd` | **SE** | Code-specific. |
| `diagnose` | **mostly SE** | Could be generalised to "debug any complex system" but SE is the natural fit. |
| `zoom-out` | **both** | Systems thinking applies universally. |
| `improve-architecture` | **SE** | Code-specific. Generic cousin = "audit a process". |
| `handoff` | **both** | Writing-for-the-next-reader. Universal. |

Outcome targets: ~5 "both" skills + ~9 "SE" skills, which justifies organising the repo as `skills/atomic/...` (current) plus a future `skills/generic/...` for skills tagged "both" that have generic invocation modes the SE versions don't surface.

---

## Part I — What we do now

This doc is the contract. With it, the walk in Part G is mechanical, not improvisational.

Vikas, please review:
- **Part D — the starting phase.** Does the five-step protocol match how you actually want to start any problem? Anything missing?
- **Part E — the seven principles.** Are any wrong or redundant? Any missing?
- **Part F — the writing standard.** Is footnote-based citation the right format for you? Too heavy, too light?
- **Part G — the walk order.** Is starting with `start` (universal entry) the right move, or do you want `grill-with-docs` (the fix you flagged) first?
- **The deep-dive name.** Confirm or veto.

Once you confirm, the next message starts the walk at item 1.

---

## References

[^polya]: George Polya, *How to Solve It*, Princeton University Press, 1945. The four-phase frame (Understand / Plan / Execute / Look back) is the foundational text on problem-solving methodology in mathematics and engineering.

[^minto]: Barbara Minto, *The Pyramid Principle: Logic in Writing and Thinking*, 1978/1996. Originated at McKinsey; the canonical reference for structured business communication.

[^bps]: Charles Conn and Robert McLean, *Bulletproof Problem Solving*, Wiley, 2019. The most current and explicit operational treatment of issue-tree / hypothesis-driven analysis.

[^cynefin]: David J. Snowden and Mary E. Boone, "A Leader's Framework for Decision Making", *Harvard Business Review*, November 2007.

[^ooda]: John Boyd, *A Discourse on Winning and Losing* (briefing). Secondary source: Frans Osinga, *Science, Strategy and War: The Strategic Theory of John Boyd*, Routledge, 2007.

[^deming]: W. Edwards Deming, *Out of the Crisis*, MIT Press, 1982. Deming preferred Plan-Do-Study-Act, arguing "Check" was too passive.

[^a3]: John Shook, *Managing to Learn: Using the A3 Management Process*, Lean Enterprise Institute, 2008. The definitive A3 book.

[^klein-premortem]: Gary Klein, "Performing a Project Premortem", *Harvard Business Review*, September 2007. https://hbr.org/2007/09/performing-a-project-premortem

[^klein-premostem-aar]: Same source as [^klein-premortem], plus US Army Training Circular 25-20 (1993) for AAR origins.

[^feynman]: Richard Feynman, "Cargo Cult Science" (1974 Caltech commencement). "The first principle is that you must not fool yourself — and you are the easiest person to fool."

[^munger]: Charlie Munger, *Poor Charlie's Almanack*, ed. Peter Kaufman, 2005. Especially the 1995 USC talk "The Psychology of Human Misjudgment".

[^kahneman]: Daniel Kahneman, *Thinking, Fast and Slow*, Farrar, Straus and Giroux, 2011.

[^klein-sources]: Gary Klein, *Sources of Power: How People Make Decisions*, MIT Press, 1998. Recognition-Primed Decision (RPD) — expert intuition in high-validity domains.

[^tetlock]: Philip E. Tetlock and Dan Gardner, *Superforecasting: The Art and Science of Prediction*, Crown, 2015.

[^duke]: Annie Duke, *Thinking in Bets: Making Smarter Decisions When You Don't Have All the Facts*, Portfolio, 2018.

[^toulmin]: Stephen Toulmin, *The Uses of Argument*, Cambridge University Press, 1958.

[^working-backwards]: Colin Bryar and Bill Carr, *Working Backwards: Insights, Stories, and Secrets from Inside Amazon*, St. Martin's Press, 2021.

[^ideo]: Tom Kelley and David Kelley, *Creative Confidence: Unleashing the Creative Potential Within Us All*, Crown Business, 2013.

[^triz]: Genrich Altshuller, *The Innovation Algorithm: TRIZ, Systematic Innovation and Technical Creativity*, Technical Innovation Center, 1999 (Eng. transl.).

[^beck-tdd]: Kent Beck, *Test-Driven Development by Example*, Addison-Wesley, 2002.

[^feathers]: Michael Feathers, *Working Effectively with Legacy Code*, Prentice Hall, 2004. Seams and characterization tests.

[^evans]: Eric Evans, *Domain-Driven Design: Tackling Complexity in the Heart of Software*, Addison-Wesley, 2003.

[^vernon]: Vaughn Vernon, *Implementing Domain-Driven Design*, Addison-Wesley, 2013.

[^eventstorming]: Alberto Brandolini, *Introducing EventStorming*, Leanpub, ongoing. https://www.eventstorming.com/

[^shapeup]: Ryan Singer, *Shape Up: Stop Running in Circles and Ship Work that Matters*, Basecamp, 2019. https://basecamp.com/shapeup

[^posd]: John Ousterhout, *A Philosophy of Software Design*, 2nd ed., Yaknyam Press, 2021.

[^fowler-refactoring]: Martin Fowler, *Refactoring: Improving the Design of Existing Code*, 2nd ed., Addison-Wesley, 2018.

[^tidy-first]: Kent Beck, *Tidy First? A Personal Exercise in Empirical Software Design*, O'Reilly, 2023.

[^accelerate]: Nicole Forsgren, Jez Humble, Gene Kim, *Accelerate: The Science of Lean Software and DevOps*, IT Revolution Press, 2018. DORA metrics. https://dora.dev/

[^sre]: Betsy Beyer et al. (eds.), *Site Reliability Engineering: How Google Runs Production Systems*, O'Reilly, 2016.

[^cook]: Richard I. Cook, "How Complex Systems Fail", 1998. https://how.complexsystems.fail/

[^dekker]: Sidney Dekker, *The Field Guide to Understanding 'Human Error'*, 3rd ed., CRC Press, 2014. Old view vs. new view of operator error.

[^pocock-skills]: Matt Pocock, mattpocock/skills repo README (2026). https://github.com/mattpocock/skills

[^pocock-failures]: Same as [^pocock-skills]; specifically the "Why These Skills Exist" four-failure-modes section.

[^anthropic-skills]: Anthropic Skill spec. The trigger description is the loading condition; the body is the implementation.

[^victor]: Bret Victor, "Inventing on Principle" (2012). https://vimeo.com/36579366

[^geepaw]: GeePaw Hill, "Many More, Much Smaller Steps" (MMMSS) series. https://www.geepawhill.org/series/many-more-much-smaller-steps/

[^charity]: Charity Majors, Liz Fong-Jones, George Miranda, *Observability Engineering*, O'Reilly, 2022.

[^allspaw-infinite-hows]: John Allspaw, "The Infinite Hows (or, the Dangers Of The Five Whys)", 2012. https://www.kitchensoap.com/2012/02/10/the-infinite-hows-or-the-dangers-of-the-five-whys/

[^lin]: Lewis C. Lin, *Decode and Conquer*, 2013. The CIRCLES framework for PM product-design questions.

[^bezos]: Jeff Bezos, 2015 letter to shareholders. https://www.aboutamazon.com/news/company-news/2016-letter-to-shareholders (covering the prior year). Two-way doors vs. one-way doors.

[^larson-migrations]: Will Larson, "Migrations: the sole technique for scaling engineering velocity". https://lethain.com/migrations/

[^larson-strategy]: Will Larson, "Writing an engineering strategy". https://lethain.com/eng-strategies/

[^unix-philosophy]: Doug McIlroy, originator of Unix pipelines: "Make each program do one thing well." Doug McIlroy, *Bell System Tech. J.*, 1978.

[^fowler-mocks]: Martin Fowler, "Mocks Aren't Stubs". https://martinfowler.com/articles/mocksArentStubs.html

[^north-cupid]: Dan North, "CUPID: for joyful coding". https://dannorth.net/cupid-for-joyful-coding/

[^wayne-solid]: Hillel Wayne, "I Have Complicated Feelings About SOLID". https://www.hillelwayne.com/post/solid/

[^amazon-narrative]: Jeff Bezos, 2017 letter to shareholders (2018 published). Narrative six-pagers in lieu of slide decks.

[^nejm-ipass]: Amy J. Starmer et al., "Changes in Medical Errors after Implementation of a Handoff Program", *New England Journal of Medicine*, 2014. https://www.nejm.org/doi/full/10.1056/NEJMsa1405556

[^principle-trigger]: skills-design-principles.md, Part E, Principle 4.
