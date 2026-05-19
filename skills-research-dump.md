# Skills Research Dump

A knowledge dump — sources, frameworks, techniques, tensions — to inform the next iteration of the `vikasahlawat228/skills` repo. **No SKILL.md was modified by this research.** That is the next pass, after Vikas reads this and points at what to integrate.

The dump is organised in three layers:

1. **Generic / structural thinking** — frameworks for tackling *any* problem like a case-prep or PM case-study. Plus a proposed `thinking/` bucket of atomic skills.
2. **Per-skill briefs** for the 12 existing skills (one section each, ordered by lifecycle).
3. **Cross-cutting patterns** — what shows up across multiple skills, and the highest-leverage additions to consider first.

The skill briefs all share the same shape: canonical sources, techniques worth borrowing, tensions worth knowing, non-software cousins, and "what to add to SKILL.md" — listed as bullet additions, not drafted content.

---

# Part 1 — Generic / structural thinking

## Framework reference

### 1. MECE (Mutually Exclusive, Collectively Exhaustive)
- **Originator:** Barbara Minto at McKinsey, 1960s. Codified in *The Pyramid Principle* (1978/1996).
- **Shape:** A decomposition of a problem space into buckets that don't overlap and together cover the whole. Used to structure analysis, recommendations, and slide outlines so nothing is double-counted or missed.
- **Worked example:** "Why is revenue down?" decomposes into Price × Volume; Volume into new + retained; existing into churn + expansion. Each branch disjoint; union covers all revenue movement.
- **Works when:** the problem has clean structural decomposition. **Fails when:** categories are inherently fuzzy or overlapping.
- **Read:** Minto, *The Pyramid Principle*; Rasiel, *The McKinsey Way*.
- **SE cousin:** Vertical slicing of features; exhaustive switch/case; equivalence partitioning.

### 2. Pyramid Principle (Minto)
- **Originator:** Barbara Minto, *The Pyramid Principle* (1978).
- **Shape:** Communicate top-down: lead with the answer (governing thought), then 2–5 supporting arguments grouped MECE, then evidence. Bottom-up induction for the writer; top-down delivery for the reader. SCQA opener: Situation, Complication, Question, Answer.
- **Worked example:** Exec memo: "We should kill product X" (answer), then three reasons, each backed by 3 data points. Reader can stop at any level and still have the gist.
- **Works when:** busy decision-makers, written or slide-based. **Fails when:** the goal is exploration or persuasion of a hostile audience.
- **Read:** Minto, *The Pyramid Principle*; Zelazny, *Say It With Charts*.
- **SE cousin:** ADR / RFC / PR-description structure; "TL;DR / context / detail".

### 3. Issue trees / hypothesis-driven analysis
- **Origin:** McKinsey practice tradition. Documented in Rasiel and Friga.
- **Shape:** Branch the central question into MECE sub-questions until each leaf is a falsifiable analysis you could do in a day. Form a hypothesis early; design the minimum analyses needed to confirm or kill it.
- **Worked example:** "Should we enter Brazil?" → Market attractive? + Can we win? + Worth it vs. alternatives? Under "Can we win": distribution, local competitors, regulatory. Hypothesis: "We can win in SP/Rio via partnership with retailer Y." Falsifiable by 3 expert interviews.
- **Works when:** ambiguous, broad strategic questions with finite time. **Fails when:** problem requires emergent discovery, or premature hypothesis locks in bias.
- **Read:** Rasiel, *The McKinsey Way*; **Conn & McLean, *Bulletproof Problem Solving* (2019)** — the most current and explicit treatment.
- **SE cousin:** Hypothesis-driven debugging; bisection; spike-then-build.

### 4. Cynefin (Snowden)
- **Originator:** Dave Snowden. Canonical paper: Snowden & Boone, "A Leader's Framework for Decision Making", *HBR* Nov 2007.
- **Shape:** Five domains. **Clear**: sense-categorize-respond, best practice. **Complicated**: sense-analyze-respond, good practice, experts. **Complex**: probe-sense-respond, emergent practice, safe-to-fail experiments. **Chaotic**: act-sense-respond, stabilize first. **Confusion**: figure out which domain you're in.
- **Worked example:** Pandemic. Initial outbreak = Chaotic (lockdown to stabilise). Once stable but uncertain = Complex (parallel interventions). Vaccine logistics = Complicated. Annual flu shots = Clear.
- **Works when:** diagnosing what kind of problem you face before picking a method. **Fails when:** people apply complicated-domain analysis to genuinely complex problems.
- **Read:** Snowden & Boone HBR 2007; Snowden et al., *Cynefin* (2020).
- **SE cousin:** Incident severity classification; "known issue vs novel".

### 5. OODA loop (Boyd)
- **Originator:** Col. John Boyd, USAF. *A Discourse on Winning and Losing* (briefing); canonical secondary source: Frans Osinga, *Science, Strategy and War* (2007).
- **Shape:** Observe, Orient, Decide, Act, looping. The heart is Orient — your mental model. Winning = looping faster and more accurately than the adversary.
- **Worked example:** Trading desk in flash crash: observe order book; orient against historical + macro; decide to cut exposure 50%; act; re-observe.
- **Works when:** adversarial, time-pressured, info-rich (trading, ops incidents). **Fails when:** problems require deep slow thought; "OODA" used as buzzword.
- **Read:** Osinga; Coram, *Boyd*; Richards, *Certain to Win*.
- **SE cousin:** Incident response loop; observability-driven ops.

### 6. Polya — *How to Solve It*
- **Originator:** George Polya, *How to Solve It* (1945).
- **Shape:** Four phases. (1) Understand the problem — restate, identify unknown, data, condition. (2) Devise a plan — find analogous problem, work backwards, specialise, generalise. (3) Carry out the plan — execute and check each step. (4) Look back — verify, derive differently, reuse the method.
- **Worked example:** Estimate height of a tree. Similar triangles: measure stick's shadow, compute ratio, multiply. Look back: sanity-check vs known building; method generalises.
- **Works when:** well-defined problems with definite answers. **Fails when:** problem is ill-defined or politically contested.
- **Read:** Polya, *How to Solve It*; *Mathematics and Plausible Reasoning*.
- **SE cousin:** Debugging cycle (reproduce / hypothesise / verify / postmortem).

### 7. First-principles thinking
- **Origin:** Aristotle's *Posterior Analytics*; modern champions Feynman, Munger, Musk.
- **Shape:** Strip a problem to physical or logical truths you're sure of, then rebuild upward. Refuses arguments from authority, convention, or analogy when the cost of being wrong is high.
- **Worked example:** Musk on batteries (~2008). By analogy: $600/kWh forever. From first principles: cobalt, nickel, aluminum, electrolyte salts on the LME ~$80/kWh. The remaining $520 is addressable.
- **Works when:** legacy assumptions are baked in and the actor has time + capital. **Fails when:** speed matters more than optimality, or "first principles" is used to dismiss tacit knowledge.
- **Read:** Feynman, *Lectures* vol 1; Munger, *Poor Charlie's Almanack*; Isaacson, *Elon Musk*.
- **SE cousin:** Rewriting vs. forking; latency-budget reasoning; Carmack's "what does the silicon actually need to do?"

### 8. Munger's latticework of mental models
- **Originator:** Charlie Munger. *Poor Charlie's Almanack* (2005), esp. the 1995 USC talk "The Psychology of Human Misjudgment".
- **Shape:** Worldly wisdom = ~100 big models from physics, biology, psychology, math, economics, used redundantly to check each other. Two recurring moves: **inversion** ("how do I guarantee failure?") and **checklist thinking**.
- **Worked example:** Investment in a retailer. Models: competitive moats, incentive-caused bias, scale advantages, base rates of retail failure, social proof. Cross-check; one model can lie, ten rarely do.
- **Works when:** consequential, infrequent decisions. **Fails when:** cargo-cult latticework with no internalisation.
- **Read:** *Poor Charlie's Almanack*; Cialdini, *Influence*; Hardin, *Filters Against Folly*.
- **SE cousin:** Inversion = chaos engineering; pre-commit checklists; code review heuristics.

### 9. PDCA / Deming cycle
- **Originator:** Shewhart (1930s, PDSA); popularised by W. Edwards Deming, *Out of the Crisis* (1982). Deming preferred PDSA — Study, not Check.
- **Shape:** Plan a small change with a prediction; Do (small-scale); Study results vs. prediction; Act — standardise if it worked, iterate if not.
- **Worked example:** Hospital reducing infection rate. Plan: hand-sanitiser at every bedside drops rate 20% in ward A. Do: 6-week pilot. Study: actual drop 15%. Act: roll out to B–D.
- **Works when:** repeating processes where measurement is cheap. **Fails when:** changes are large, one-shot, or interact in ways one ward can't isolate.
- **Read:** Deming, *Out of the Crisis*; Rother, *Toyota Kata* (2009).
- **SE cousin:** Feature-flag rollouts; canary releases; SRE error-budget loops.

### 10. Toyota A3 / 5 Whys
- **Origin:** Toyota Production System. Ohno, *Toyota Production System* (1978/1988). A3 named for paper size.
- **Shape:** A3 forces an entire problem-solving cycle onto one tabloid sheet: background, current state, goal, root-cause analysis (5 Whys), countermeasures, implementation, follow-up. 5 Whys = walk symptom → root cause.
- **Worked example:** Machine stopped → fuse blew → bearing seized → insufficient lube → lube pump worn → no filter on intake. Fix = add filter, not replace fuse.
- **Works when:** mechanical/process problems with linear causality. **Fails when:** causes are systemic, multi-factor, or political. Fishbone (Ishikawa) for those.
- **Read:** Shook, *Managing to Learn* (2008) — the definitive A3 book; Ohno, *TPS*.
- **SE cousin:** Postmortem template — with the same warnings (Allspaw, "The Infinite Hows": https://www.kitchensoap.com/2012/02/10/the-infinite-hows-or-the-dangers-of-the-five-whys/).

### 11. Pre-mortem & after-action review
- **Originators:** Pre-mortem: Gary Klein, *HBR* September 2007. AAR: US Army Training Circular 25-20 (1993).
- **Shape:** **Pre-mortem**: before launch, imagine the project failed a year out; each person silently writes reasons; surface top risks. **AAR**: after action, ask what was supposed to happen, what did happen, why the gap, what to sustain/improve.
- **Worked example:** Product launch pre-mortem yields "we shipped without a clear ICP," "support couldn't handle volume," "competitor pre-empted us" — each becomes an explicit risk mitigation.
- **Works when:** complex projects, candid team. **Fails when:** culture punishes pessimism, or AAR becomes blame theatre.
- **Read:** Klein, *Sources of Power* (1998); Duke, *Thinking in Bets* (2018).
- **SE cousin:** Threat modeling, chaos game-days, blameless postmortems.

### 12. Decision-making under uncertainty (Kahneman / Klein / Tetlock)
- **Originators:** Kahneman, *Thinking, Fast and Slow* (2011); Klein, *Sources of Power* (1998); Tetlock, *Superforecasting* (2015) and *Expert Political Judgment* (2005).
- **Shape:** **Kahneman**: two systems; biases; reference-class forecasting beats inside-view. **Klein**: experts pattern-match (Recognition-Primed Decision); intuition reliable in high-validity environments. **Tetlock**: superforecasters are foxes not hedgehogs, update incrementally, think in probabilities, decompose Fermi-style.
- **Worked example:** Acquire competitor. Inside view: "synergies $50M." Reference class: base rate ~50% destroy value. Klein: ask M&A veterans what their gut says *and why*. Pre-commit decision criteria before seeing data.
- **Works when:** stakes high enough to slow down. **Fails when:** time too short (use Klein/RPD) or low-validity domain.
- **Read:** *Thinking, Fast and Slow*; *Superforecasting*; *Sources of Power*; Kahneman/Sibony/Sunstein, *Noise* (2021).
- **SE cousin:** Estimation; on-call triage; reversible vs. one-way-door decisions (Bezos).

### 13. Toulmin model of argument
- **Originator:** Stephen Toulmin, *The Uses of Argument* (1958).
- **Shape:** **Claim** (assertion), **Data/Grounds** (evidence), **Warrant** (the bridge — why data supports claim), **Backing** (authority for warrant), **Qualifier** ("probably"), **Rebuttal** (conditions under which claim fails).
- **Worked example:** Claim: "Migrate to Postgres." Data: "MySQL at 80% CPU at peak." Warrant: "Postgres has better query planner under our workload." Backing: "Benchmark X with our query shapes, 2024." Qualifier: "Assuming workload mix stays within 20%." Rebuttal: "Unless team lacks ops expertise."
- **Works when:** persuasion, RFCs, contested decisions. **Fails when:** audience wants narrative; Toulmin feels pedantic casually.
- **Read:** Toulmin, *The Uses of Argument*; Weston, *A Rulebook for Arguments*.
- **SE cousin:** ADR / RFC "Alternatives Considered"; tech design-doc structure.

### 14. Fermi estimation
- **Originator:** Enrico Fermi (Manhattan Project — estimating bomb yield from displaced paper).
- **Shape:** Decompose unknown into a multiplicative chain of factors each estimable within an order of magnitude. Errors partially cancel by CLT.
- **Worked example:** Piano tuners in Chicago. 3M people / 1M households / 1-in-20 own piano = 50k pianos / tuned once a year / tuner does 4/day × 250 days = 1000/yr → ~50 tuners.
- **Works when:** sparse data, need order-of-magnitude. **Fails when:** errors compound or one factor is fundamentally unknowable.
- **Read:** Weinstein & Adam, *Guesstimation* (2008); Mahajan, *Street-Fighting Mathematics* (2010).
- **SE cousin:** Back-of-envelope capacity planning; Jeff Dean's "numbers everyone should know"; cost spike.

### 15. PM / consulting case-interview method
- **Originators / canon:** Cosentino, *Case in Point* (1999, 11th ed); Cheng, *Case Interview Secrets* (2012); Lin, *Decode and Conquer* (2013) — CIRCLES framework for PM product design (Comprehend, Identify customer, Report needs, Cut, List solutions, Evaluate, Summarize); McDowell & Bavaro, *Cracking the PM Interview* (2014).
- **Shape:** Reusable scaffolds — profitability tree, market entry (market / competition / capability / economics), market sizing (Fermi). CIRCLES for product design. Interviewer values structure + numerical sanity + judgment over correct answer.
- **Worked example:** "Design Google Maps for cyclists." CIRCLES: scope = commuter; user = urban 25–40; needs = safe routes, elevation, theft-resistant parking; cut to top 2; list solutions; evaluate by reach × impact × effort; pick.
- **Works when:** time-boxed, ambiguous, demonstrate structure. **Fails when:** real product work — frameworks become a crutch skipping research.
- **Read:** Cosentino; Lin; McDowell & Bavaro.
- **SE cousin:** System-design interview scaffolds (requirements / API / data / scale / failure modes).

## Cross-cutting principles in the generic frameworks

Every framework above is, structurally, the same move applied to a different surface: **impose a finite, named structure on an unbounded problem so you can reason and communicate about it.** Minto names the answer-then-reasons skeleton; issue trees name the decomposition; Toulmin names the argument joints; Polya names the phases of solving; CIRCLES names the steps for a product question; PDCA names the iteration. The structure isn't true — it's a scaffold that makes thought inspectable.

A second meta-pattern is the **diagnose-before-prescribe split**. Cynefin diagnoses the domain before picking the method. Polya's "Understand" precedes "Plan." OODA's Orient precedes Decide. Pre-mortem precedes execution. The frameworks that most reliably fail (5 Whys, naive first-principles, OODA-as-buzzword) are the ones people apply *without* a diagnosis step.

A third pattern is the **explicit-uncertainty loop**: PDCA, OODA, hypothesis-driven, pre-mortem/AAR, Tetlock-style updating. All treat a decision as a bet with a feedback step. Any framework without a "look back / update" phase is a half-framework.

## Proposed generic bucket

**Recommendation: six atomic skills under a `thinking/` (or `structural/`) bucket, not one mega-skill.** Each framework activates in a different context (diagnosing, communicating, estimating, deciding); one mega-skill forces the agent to load and disambiguate everything at once.

1. **`thinking/decompose`** — MECE issue tree + hypothesis-driven analysis. Decompose fuzzy problem; surface hypotheses; identify the minimum analyses to test them. The single most reused move.
2. **`thinking/diagnose-context`** — Cynefin as meta-skill. Classify Clear / Complicated / Complex / Chaotic before choosing a method. Gates which other skills should fire.
3. **`thinking/structure-communication`** — Pyramid Principle / SCQA / Toulmin. The output-side counterpart to `decompose`. PRs, memos, RFCs, status updates.
4. **`thinking/estimate`** — Fermi. Decompose-and-multiply, sanity-check against base rates.
5. **`thinking/pre-mortem`** — Klein's pre-mortem + Munger's inversion + AAR template. The most underused move in software.
6. **`thinking/decide-under-uncertainty`** — Frame as a bet; set priors; identify reversibility; reference-class check; pre-commit criteria. Kahneman + Tetlock + Duke + Bezos one-way-door.

Deliberately *not* shipping first: Polya (covered by `decompose` + TDD-style), OODA (more useful as a cue than invocable skill), PDCA (covered implicitly by iterative skills), Munger latticework (sprawling — folded into `decide-under-uncertainty`), A3 / 5-Whys (existing postmortem covers), case-interview scaffolds (collapse into `decompose` and `structure-communication`).

## Software-engineering cousins

| Generic | SE practice | One-line justification |
|---|---|---|
| MECE | Equivalence partitioning / vertical slicing | Non-overlapping, exhaustive coverage of an input or feature space |
| Pyramid Principle | ADR / RFC / PR description | Answer-first writing so reviewers can stop at any depth |
| Issue trees | Hypothesis-driven debugging, spike trees | Decompose the bug surface, falsifiable hypothesis, cheapest test first |
| Cynefin | Incident severity + "known vs novel" triage | Different runbooks for known-good, expert-needed, exploratory, stabilise-first |
| OODA | On-call incident response | Observe metrics, orient against system model, decide mitigation, act, re-observe |
| Polya | Reproduce → hypothesise → fix → postmortem | The debugging cycle is Polya with different vocabulary |
| First-principles | Build-vs-buy, rewrite-vs-fork, latency budgeting | Strip inherited assumptions to find irreducible cost |
| Munger latticework | Code review heuristics + chaos engineering | Inversion + multi-lens checks against bias |
| PDCA | Canary releases / feature flags / error budgets | Small change, measured rollout, study, standardise or revert |
| A3 / 5 Whys | Postmortem template | Single-page root-cause narrative (with caveats) |
| Pre-mortem / AAR | Threat modeling, game-days, blameless postmortems | Imagine failure before launch; learn from it after |
| Decision under uncertainty | Reversible vs. one-way-door decisions, estimation | Bezos framing = Klein + Kahneman applied |
| Toulmin | ADR "Alternatives Considered" + rationale | Forces the warrant to be explicit |
| Fermi | Capacity planning, cost modeling, "numbers everyone knows" | Order-of-magnitude from sparse inputs |
| Case interview methods | System-design interview scaffolds | Reusable structures mirror profitability/market-entry trees |

## Generic best-minds short-list

Citing these in any new skill is high-leverage because they're widely recognised and you can borrow their authority cheaply:

- **Barbara Minto** — *The Pyramid Principle*. Modern structured business communication.
- **Charlie Munger** — *Poor Charlie's Almanack*. Latticework, inversion, bias catalog.
- **Daniel Kahneman** — *Thinking, Fast and Slow*; *Noise*.
- **Gary Klein** — *Sources of Power*. Defends expert intuition; invented pre-mortem.
- **Philip Tetlock** — *Superforecasting*. Calibrated probabilistic forecasting.
- **George Polya** — *How to Solve It*. The original.
- **John Boyd** — OODA. Osinga's *Science, Strategy and War* is the readable entry.
- **Dave Snowden** — Cynefin. 2007 HBR is the 30-minute version.
- **W. Edwards Deming** — *Out of the Crisis*. PDCA + variation.
- **Stephen Toulmin** — *The Uses of Argument*. Anatomised arguments.
- **Richard Feynman** — first-principles attitude. "The first principle is that you must not fool yourself."
- **Charles Conn & Robert McLean** — *Bulletproof Problem Solving* (2019). Current operational treatment of issue trees.
- **Annie Duke** — *Thinking in Bets*. Decisions as bets.
- **Lewis C. Lin** — *Decode and Conquer*. CIRCLES.

---

# Part 2 — Per-skill briefs

Order follows the lifecycle: classify → frame → spec → slice → execute → debug → review → handoff.

## orchestrator

### Canonical sources
- Ryan Singer, *Shape Up* — https://basecamp.com/shapeup — appetites (2-week vs. 6-week) as the canonical "size the work before scoping it" move.
- Will Larson, *An Elegant Puzzle* and https://lethain.com/migrations/ , https://lethain.com/sizing-engineering-teams/ — frameworks for sizing initiatives and matching them to process weight.
- Camille Fournier, *The Manager's Path* — the "how big is this thing" instinct; routing small/medium/large to different review surfaces.
- Martin Fowler, "Sacrificial Architecture" and "Yagni" — https://martinfowler.com/bliki/Yagni.html .
- Google's design-doc culture (Yacin Kaymak / Malte Ubl) — https://www.industrialempathy.com/posts/design-docs-at-google/ — explicit threshold for when a change warrants a design doc.
- RFC/design-doc culture writeups: Pragmatic Engineer https://newsletter.pragmaticengineer.com/p/rfcs-and-design-docs .
- Kent Beck, "3X: Explore/Expand/Extract" — stage of product dictates process weight. (Concept canonical; original FB note link uncertain.)
- Jeff Bezos — "one-way doors vs. two-way doors" — most-cited in his 2015 shareholder letter.

### Techniques worth borrowing
- Shape Up's **appetite** framing: ask "how much is this worth?" *before* "how long will it take?" — flip estimation into a budgeting decision.
- Larson's **migration sizing** table: count of call sites / services / teams touched as routing signal, not LOC.
- Google design-doc threshold: "if you'd want a second pair of eyes before merging, write the doc."
- Yagni check at routing.
- Two-way / one-way door classification — irreversibility is a sizing axis distinct from effort.
- 3X stage check: Explore → tracer-bullet; Extract → formal spec.

### Tensions / debates
- Shape Up's fixed appetite vs. agile estimation: Singer says estimate is the wrong primitive; many shops still need rough sizing for capacity planning.
- "Just ship it" (DHH, Linear) vs. "write the doc first" (Amazon, Google). Orchestrator must pick a default and let user override.
- **Effort-based sizing vs. risk-based sizing** — a 1-hour change to billing code is "large" by blast radius even if "small" by effort. Single-dial sizing is insufficient.

### Non-software cousins
- McKinsey "engagement sizing" — 1-day vs. 6-week vs. multi-month with different deliverables.
- Medicine: triage acuity scales (ESI levels 1–5).
- Journalism: brief vs. feature vs. investigation — same story, three formats by depth.

### Add to SKILL.md
- A **two-axis classifier (effort × blast-radius/reversibility)**, not a single size dial.
- An explicit "**ask the user the appetite** before classifying" step (Shape Up).
- A short rubric for when to escalate small → medium (touches >1 bounded context, irreversible, unclear acceptance criteria).

---

## grill-me

### Canonical sources
- Plato, *Meno* and *Theaetetus* — original Socratic elenchus. https://www.gutenberg.org/ebooks/1643
- Toyoda / Ohno — 5 Whys. https://en.wikipedia.org/wiki/Five_whys
- Minto, *The Pyramid Principle* — MECE / issue trees as the interview spine.
- Rasiel, *The McKinsey Way* — issue-tree decomposition.
- Kahneman — pre-mortem, base rates, planning fallacy.
- Klein — "Performing a Project Premortem", https://hbr.org/2007/09/performing-a-project-premortem .
- Tufte, "The Cognitive Style of PowerPoint" / Amazon 6-pager — silent reading + Q&A is structurally a grilling.
- Rob Fitzpatrick, *The Mom Test* — https://www.momtestbook.com/ — how to ask questions that don't get false validation.
- Will Larson, "Writing engineering strategy" — https://lethain.com/good-engineering-strategy-bad-engineering-strategy/ — reverse-engineer into questions.

### Techniques worth borrowing
- Socratic elenchus: get user to state a claim, produce a counter-example from their own words.
- 5 Whys: chain causal "why" until process or principle, not a person.
- MECE issue tree: at each branch, "what are the mutually exclusive children?" — forces coverage.
- Pre-mortem: "It's six months from now and this failed. Write the headline."
- **Mom Test rule:** ask about past behaviour, not future intent.
- SCQA as the interview's opening frame.
- "Name the thing you're avoiding asking" (Larson's strategy-doc gap test).

### Tensions / debates
- Relentlessness vs. user fatigue — need a stopping rule.
- 5 Whys is widely criticised (Allspaw, "The Infinite Hows" — https://www.kitchensoap.com/2012/02/10/the-infinite-hows-or-the-dangers-of-the-five-whys/) for finding a single root cause where many exist. Prefer "5 Hows" or causal graphs.
- Open vs. closed questions — Mom Test prefers open + past-tense; issue trees prefer closed/decomposable. Mix deliberately.

### Non-software cousins
- Trial cross-examination — leading questions to lock down commitment, then expose contradiction.
- Investigative journalism's 5 Ws + How.
- Therapy's motivational interviewing (Miller & Rollnick) — reflective listening to surface ambivalence.
- Oxford tutorial / viva — relentless questioning until the model breaks or holds.

### Add to SKILL.md
- Explicit **stopping rule**: stop when every leaf of the issue tree has a concrete decision or a logged unknown.
- A **pre-mortem prompt** as a required late-stage question.
- The **Mom Test rule**: prefer past-behaviour questions over future-intent.

---

## grill-with-docs

### Canonical sources
- Eric Evans, *Domain-Driven Design* (2003) — ubiquitous language, bounded contexts, context maps.
- Vaughn Vernon, *Implementing Domain-Driven Design* (2013) and *DDD Distilled* (2016).
- Alberto Brandolini, *Introducing EventStorming* — https://www.eventstorming.com/ — big-picture / process / design level; pivotal events.
- Michael Nygard, "Documenting Architecture Decisions" — https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions — the canonical ADR template.
- adr-tools / MADR — https://github.com/joelparkerhenderson/architecture-decision-record and https://adr.github.io/madr/ .
- ThoughtWorks Tech Radar — https://www.thoughtworks.com/radar/techniques/lightweight-architecture-decision-records .
- Simon Brown, C4 — https://c4model.com/ .
- Andrew Harmel-Law, "Scaling the Practice of Architecture, Conversationally" — https://martinfowler.com/articles/scaling-architecture-conversationally.html — ADRs as conversation artifacts.
- Cyrille Martraire, *Living Documentation* — https://leanpub.com/livingdocumentation — docs derived from and kept honest by code.

### Techniques worth borrowing
- Ubiquitous-language audit — every noun/verb in the interview must match `CONTEXT.md` or trigger a glossary update.
- **Bounded-context boundary check** — when a term means two things, that's a context boundary; log it.
- EventStorming **pivotal events** — ask user to name the 3–5 events the system pivots around; check against ADRs.
- Nygard ADR structure (Context / Decision / Status / Consequences) as inline update format.
- "**Superseded by**" links — never delete an ADR.
- Context-map vocabulary (Vernon): upstream/downstream, conformist, ACL.
- Living Documentation: pair each glossary term with an executable example where possible.

### Tensions / debates
- DDD's strategic patterns (context maps) vs. tactical (aggregates/entities) — Evans's tactical patterns often misapplied; Vernon and Brandolini push strategic-first.
- ADRs as historical record (Nygard) vs. ADRs as living spec (Harmel-Law).
- Ubiquitous language is hard in polyglot orgs — glossary becomes negotiation log, not dictionary.
- EventStorming purists (sticky notes, in-person) vs. async adapters.

### Non-software cousins
- Legal contracts — defined-terms section + precedent (superseded clauses).
- Lexicography (OED) — each sense dated and superseded, never deleted.
- Medicine — differential diagnosis docs revised with prior hypotheses retained.

### Add to SKILL.md
- A **preflight**: "before grilling, read `CONTEXT.md` and the last 5 ADRs"; output a list of terms in play and decisions touched.
- A **bounded-context detection prompt**: "does this term mean the same thing in module X and module Y?"
- An **ADR-update mode**: when a decision shifts, mark old ADR "Superseded by" and draft the new one in the same pass.

---

## to-prd

### Canonical sources
- Bryar & Carr, *Working Backwards* (2021) — https://www.workingbackwards.com/ — PR/FAQ and 6-pager mechanics.
- Marty Cagan, *Inspired* (2nd ed.) and *Empowered* — https://www.svpg.com/articles/ — outcome over output; four product risks (value/usability/feasibility/viability).
- Ryan Singer, *Shape Up*, chapter on pitches — https://basecamp.com/shapeup/1.5-chapter-06 — problem / appetite / solution / rabbit holes / no-gos.
- Lenny Rachitsky, "How the best product teams write specs" — https://www.lennysnewsletter.com/p/how-the-best-product-teams-write — templates across Linear, Figma, Airbnb.
- Joel Spolsky, "Painless Functional Specifications" — https://www.joelonsoftware.com/2000/10/02/painless-functional-specifications-part-1-why-bother/ .
- Linear Method — https://linear.app/method — terse, opinionated, ticket-as-spec.
- Jeff Patton, *User Story Mapping* — https://www.jpattonassociates.com/user-story-mapping/ — narrative backbone before requirements.
- Amazon PR-FAQ template (Ian McAllister summary on Quora; *Working Backwards* book is the canonical write-up).

### Techniques worth borrowing
- **Working Backwards press release** — lede + customer quote + how-it-works + FAQ. Forces "why would anyone care" *before* "how do we build it".
- Shape Up's five elements — and explicitly **rabbit holes** and **no-gos**, sections most PRDs lack.
- **Cagan's four risks** as a PRD checklist: value, usability, feasibility, viability — every PRD must show how each is addressed or deferred.
- Joel's "scenarios" — 3–5 named user vignettes that drive requirements.
- Patton's "narrative flow" — order requirements as the user's journey, not a feature list.
- Linear's compression rule: longer than two screens → it's a doc, not a spec. Force a TL;DR.
- Amazon's "silent reading" implication — PRD must be self-contained.

### Tensions / debates
- PR/FAQ (Amazon) optimises for executive alignment; pitch (Shape Up) optimises for builder commitment. Pick a default; note when to switch.
- Cagan's outcomes vs. ticket-shop reality — outcome-only PRDs die in backlog grooming.
- Spec-driven dev (BMAD/GSD/Spec-Kit) vs. Matt Pocock's critique that these over-own the process. PRD should be a **contract**, not a script.
- Long PRDs (Google design doc) vs. short pitches — depth correlates with reversibility, not importance.

### Non-software cousins
- Journalism's **inverted pyramid** — lede / nut graf / details.
- Film treatment / one-pager.
- Grant proposals — problem / approach / milestones / risks / budget.
- Legal brief — question presented / short answer / facts / argument.

### Add to SKILL.md
- A mandatory **"no-gos / out of scope"** section (Shape Up) and a **FAQ** section (Amazon).
- A **four-risks (value/usability/feasibility/viability) checklist** before complete.
- A **length cap** with an explicit escape hatch (`<= 2 pages unless flagged as a design doc`).

---

## to-issues

### Canonical sources
- Alistair Cockburn, "Walking Skeleton" — http://alistair.cockburn.us/Walking+skeleton .
- Bill Wake, "INVEST in Good Stories" — https://xp123.com/articles/invest-in-good-stories-and-smart-tasks/ .
- Mike Cohn, *User Stories Applied* — and "Patterns for Splitting User Stories" — https://www.mountaingoatsoftware.com/blog/nine-patterns-for-splitting-user-stories .
- Richard Lawrence, "Patterns for Splitting User Stories" — https://agileforall.com/patterns-for-splitting-user-stories/ — the canonical splitting cheat sheet.
- Jeff Patton, *User Story Mapping* — backbone + ribs.
- Hunt & Thomas, *The Pragmatic Programmer* — **tracer bullets**: full-stack thin path that proves the architecture.
- Henrik Kniberg, "Making sense of MVP" — https://blog.crisp.se/2016/01/25/henrikkniberg/making-sense-of-mvp .
- Gojko Adzic, *Impact Mapping* — https://www.impactmapping.org/ .
- Linear's writing on tickets — https://linear.app/docs/issues — vertical slice, single owner, single PR ideal.

### Techniques worth borrowing
- **Walking skeleton as issue #1** — end-to-end, deployable, does almost nothing, every layer touched.
- **Cohn/Lawrence splitting patterns** — by workflow step, data variation, interface, business rule, happy/edge, CRUD ops, tooling, acceptance criteria, performance.
- **INVEST check** on every issue: Independent, Negotiable, Valuable, Estimable, Small, Testable. Reject failing issues.
- Patton's **backbone + ribs**.
- **Tracer bullets** over completed-one-layer slices.
- Impact-map **deliverables as the issue source** — every issue traces to an actor and an impact.
- Kniberg's "each release is usable" test — re-slice if intermediate issues don't ship a usable thing.

### Tensions / debates
- Vertical slice (Cockburn, Cohn) vs. horizontal (build data model first) — default vertical; call out justified horizontal (shared infra).
- INVEST's "Independent" is often impossible — treat as a goal, not a gate.
- Story points vs. no-estimates (Woody Zuill, Allen Holub) — https://www.youtube.com/watch?v=QVBlnCTu9Ms — if slices small enough, counts replace estimates.
- "Independently shippable" is ambiguous: deployable (behind flag) vs. user-visible vs. value-delivering. Pick one and be explicit.
- Spec-driven auto-decomposition vs. human judgment — auto-splits often produce horizontal slices that violate INVEST.

### Non-software cousins
- Construction — critical-path scheduling, but each milestone leaves a habitable structure.
- Film production — shoot order vs. story order.
- Surgery — staged procedures with stable intermediate states.
- Journalism series — each installment stands alone but compounds.

### Add to SKILL.md
- A required **walking-skeleton issue as #1** for any new system or surface.
- **INVEST as an explicit rejection checklist** applied to every generated issue.
- A short **menu of splitting patterns** when an issue fails "Small".

---

## small-change

### Canonical sources
- Kent Beck, *Tidy First?* (O'Reilly, 2023) — https://www.oreilly.com/library/view/tidy-first/9781098151232/ — separates structural ("tidy") and behavioural changes.
- Kent Beck, "Empirical Software Design" Substack — https://tidyfirst.substack.com/ .
- Michael Feathers, *Working Effectively with Legacy Code* (2004) — seams, characterization tests.
- GeePaw Hill, "Many More, Much Smaller Steps" (MMMSS) — https://www.geepawhill.org/series/many-more-much-smaller-steps/ — step-size as the master variable.
- Camille Fournier — MMMSS applied to staff-engineering pacing (https://skamille.medium.com/ , specific post uncertain).
- Linus on `git bisect` — https://git-scm.com/docs/git-bisect — small commits are a debugging investment.

### Techniques worth borrowing
- Beck's **Tidy/Behavior separation** — never ship a commit that does both. Tidyings get their own PR.
- Feathers' **sprout method / sprout class** — add new behaviour alongside, don't edit in place.
- **Characterization test before change** — pin current behaviour even if you suspect it's wrong.
- **MMMSS heuristic** — when stuck, the step is too big; halve it.
- "**Reversible by default**" — prefer changes you can `git revert` without coordination.
- Trunk-based dev cadence (DORA) — merge daily, even behind a flag.

### Tensions / debates
- Beck vs. "stop the line refactoring" — *Tidy First* says interleave; older XP said refactor mercilessly mid-feature.
- Small commits vs. atomic PRs — Linus likes many small commits; GitHub culture rewards squash. Pick one consistently; bisect cost is real.
- Characterization tests can ossify bugs — Feathers acknowledges; the discipline is to mark them.

### Non-software cousins
- Surgery — minimally invasive technique.
- Toyota *jidoka* / andon — stop the line on the smallest anomaly.
- Climbing — "three points of contact."

### Add to SKILL.md
- Explicit **Tidy-vs-Behavior gate** at PR time.
- **MMMSS prompt**: "if this took >30 min, the step was too big — what was the missing intermediate?"
- Characterization-test checklist for legacy touchpoints.
- **Revert-cost heuristic** — if revert needs a meeting, the change isn't small.
- Pointer to Feathers' **seam catalog** (ch. 4).

---

## medium-feature

### Canonical sources
- Freeman & Pryce, *GOOS* (2009) — http://www.growing-object-oriented-software.com/ — canonical end-to-end TDD-driven feature build; outside-in; walking skeleton.
- Kent Beck, *TDD by Example* (2002) — per-slice red/green/refactor at feature scale.
- Will Larson, *An Elegant Puzzle* (2019) — https://lethain.com/elegant-puzzle/ — sizing + "do the work that's in front of you."
- Shape Up, Basecamp (Singer) — https://basecamp.com/shapeup — pitches, appetites, "fat marker sketches".
- Marc Brooker, "Writing is Magic" — https://brooker.co.za/blog/2022/11/30/cultures.html — writing-as-thinking before coding.
- John Carmack `.plan` files — https://github.com/ESWAT/john-carmack-plan-archive — public engineering log as cadence forcing function.

### Techniques worth borrowing
- GOOS **walking skeleton** — end-to-end no-op of the full slice on day one. Vertical-slice progenitor.
- Shape Up's **appetite, not estimate** — fix the time, vary the scope.
- **PRD-lite** = problem + non-goals + one user story per slice + rollback plan. Anything more = unclear thinking.
- Per-slice TDD with the acceptance test driving from the outside.
- Carmack/Stripe **writing discipline** — daily one-paragraph log compounds.

### Tensions / debates
- GOOS mockist style vs. Beck classicist style — at feature scale: "test the seams" vs. "test the behaviour".
- Shape Up's "no backlog" vs. agile's prioritised backlog — both work; mixing them poorly is the failure mode.
- PRD-heavy (Amazon 6-pager) vs. PRD-lite (Basecamp pitch) vs. PRD-as-issues (Linear) — medium features usually need the middle.

### Non-software cousins
- Film — pre-viz / animatic before shooting.
- Architecture — massing model before detail drawings.
- Cooking — *mise en place*.

### Add to SKILL.md
- Explicit **walking-skeleton gate** before slicing.
- A **vertical-slice template** bundling acceptance test + smallest UI + smallest backend + smallest data change.
- "**Appetite**" field instead of estimate.
- **Slice-handoff protocol** — each slice ends in a deployable state, even if dark-launched.
- Pointer to grill skill — grill happens *before* PRD-lite, not after.

---

## large-project

### Canonical sources
- Will Larson, *Staff Engineer* (2021) — https://staffeng.com/book — "writing engineering strategy" + "managing technical quality".
- Michael Nygard, ADR — https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions .
- Forsgren, Humble, Kim, *Accelerate* (2018) — https://itrevolution.com/product/accelerate/ — four key metrics; large projects are where change-failure-rate and lead-time degrade silently.
- Camille Fournier, *The Manager's Path* + https://skamille.medium.com/ — handoff and coordination at scale.
- Mike Rother, *Toyota Kata* (2009) — improvement kata + coaching kata.
- Stripe's writing culture (Brie Wolfson) — https://every.to/p/inside-stripe-s-writing-culture — long-running projects survive on written context.
- Amazon 6-pager tradition.

### Techniques worth borrowing
- `CONTEXT.md` / "primers" — living doc that any new agent reads first.
- ADR-per-decision — one file, immutable once accepted, superseded not edited.
- Weekly written status with **DORA four metrics** surfaced.
- Improvement-Kata target-condition cadence — define a 1–4 week target, run experiments, *hansei* (reflect) at the end.
- Larson's **explicit dependency map** at week 0, refreshed weekly.
- **Parallel-agent pattern** — separate worktrees/branches per agent, single integration owner, shared `CONTEXT.md` as canonical state.

### Tensions / debates
- ADRs vs. RFCs vs. design docs — ADRs are *decisions* (small, immutable); RFCs are *proposals* (mutable); design docs are *artifacts*.
- Parallel agents/engineers — faster wallclock vs. higher integration cost. Conway's Law: seams between agents become seams in the artifact.
- "Plan vs. discover" — Toyota Kata says you cannot plan an unknown; you set target conditions and experiment.

### Non-software cousins
- Naval expeditions — written orders, station bills, daily logs.
- Construction — RFI logs.
- Academic research — lab notebooks.

### Add to SKILL.md
- A `CONTEXT.md` schema — goal, non-goals, current target condition, open ADRs, dependency map, glossary.
- Inline **ADR template** (Nygard: context / decision / consequences).
- Weekly cadence — target condition review + DORA snapshot + reflection.
- Parallel-agent rules — one writer per file, integration commits through a named owner, every agent reads `CONTEXT.md` first.
- Hand-off protocol between phases (discovery → build → harden → ship).

---

## tdd

### Canonical sources
- Kent Beck, *Test-Driven Development by Example* (2002) — money + xUnit examples.
- Freeman & Pryce, *GOOS* (2009) — mockist / London-school; outside-in.
- Kent C. Dodds, "The Testing Trophy" — https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications .
- John Hughes, "QuickCheck" — https://dl.acm.org/doi/10.1145/351240.351266 — property-based testing's founding text.
- Scott Wlaschin, "Domain Modeling Made Functional" + https://fsharpforfunandprofit.com/pbt/ .
- Hillel Wayne, "What we talk about when we talk about types" — https://www.hillelwayne.com/post/constructive/ — and "Why Don't People Use Formal Methods?" — https://www.hillelwayne.com/post/why-dont-people-use-formal-methods/ .
- Michael Feathers, *WELC* — characterization tests as TDD's legacy cousin.
- DHH, "TDD is dead. Long live testing." (2014) — https://dhh.dk/2014/tdd-is-dead-long-live-testing.html and the "Is TDD Dead?" hangouts — https://martinfowler.com/articles/is-tdd-dead/ .

### Techniques worth borrowing
- Beck **red-green-refactor** as the smallest loop.
- GOOS **outside-in** — start at the acceptance test, push inward, mock collaborators.
- **Property-based supplement** — for any pure function, write at least one property (round-trip, idempotence, invariant) in addition to examples.
- **Test naming as specification** — `should_X_when_Y` or BDD `describe/it`.
- **Approval / snapshot tests** (Falco, https://approvaltests.com/) for legacy.
- Dodds **integration-heavy trophy** for UI / app code.

### Tensions / debates
- **Classicist vs. mockist** — Fowler "Mocks Aren't Stubs" — https://martinfowler.com/articles/mocksArentStubs.html . Classicists test behaviour with real collaborators; mockists test interactions with doubles.
- **TDD vs. property-based** — Hughes: examples are anecdotes, properties are theorems. Integration: example-first, property-second.
- **TDD vs. type-driven (Wayne, Wlaschin)** — strong types make many tests impossible-by-construction. Counter: most teams aren't on F# / Haskell / Idris.
- **TDD vs. write-tests-after** — Beck: design pressure of red-first is the point, not coverage.
- **Pyramid vs. Trophy vs. Honeycomb (Spotify)** — depends on whether unit boundaries are real.
- **Coverage as goal** — universally a metric trap, but everyone measures it.

### Non-software cousins
- Double-entry bookkeeping — every transaction recorded twice; mismatches surface.
- Scientific method — hypothesis → experiment → reproduce → peer review.
- Pre-flight checklists (Gawande).

### Add to SKILL.md
- **Opinionated stance**: classicist-default, mockist at architectural seams only.
- Per-slice **test list as planning artifact**.
- "Property when pure, example when stateful" heuristic.
- Explicit **refactor step** — most teams skip it; name what good refactor looks like.
- **Snapshot / approval test** for legacy.
- **Anti-patterns list**: test-after, coverage targets, one-assertion-per-test dogma, mock-the-world.
- Link to **"Is TDD Dead?"** so user can hold their own position.

---

## diagnose

### Canonical sources
- Andreas Zeller, *Why Programs Fail* (2nd ed., 2009) — https://www.whyprogramsfail.com/ — delta debugging, scientific method for debugging. Also *The Debugging Book* — https://www.debuggingbook.org/ .
- Bret Victor, "Inventing on Principle" — https://vimeo.com/36579366 — and "Learnable Programming" — http://worrydream.com/LearnableProgramming/ — feedback-loop latency as the master variable of creative work.
- Brendan Gregg, *Systems Performance* (2nd ed., 2020) — https://www.brendangregg.com/systems-performance-2nd-edition-book.html — USE method, TSA, flame graphs.
- John Allspaw, "Trade-Offs Under Pressure" / STELLA — https://snafucatchers.github.io/ .
- Richard Cook, "How Complex Systems Fail" — https://how.complexsystems.fail/ .
- Sidney Dekker, *The Field Guide to Understanding 'Human Error'* — old view vs. new view.
- David Woods — resilience engineering body of work — http://csel.osu.edu/woods.html .
- Hillel Wayne, "Debugging: Indispensable rules" — https://buttondown.com/hillelwayne .
- David Agans, *Debugging: The 9 Indispensable Rules* (2002) — http://www.debuggingrules.com/ .
- Charity Majors et al., *Observability Engineering* (O'Reilly, 2022) — https://www.honeycomb.io/wp-content/uploads/2022/06/Observability-Engineering-Honeycomb.pdf — high-cardinality wide events.
- Dan Luu — https://danluu.com/ — "Debugging stories", "Files are hard".
- Linus on `git bisect`.
- Carmack on `valgrind` and focused debugging sessions (Lex podcast appearance, exact timestamp uncertain).
- Ohno, *TPS* — 5 Whys.
- John Boyd / OODA applied to incidents (Allspaw — https://www.adaptivecapacitylabs.com/blog/ ).

### Techniques worth borrowing (feedback-loop construction is the focus)
- **Shorten the loop before debugging anything** (Victor). If the loop is >1s, fix the loop first.
- **Reproduce reliably before isolating** (Zeller's first rule).
- **Delta debugging / `git bisect run`** — automate the predicate; let the machine search.
- **Scientific method, explicit** — hypothesis → predicted observation → experiment → result → revise.
- **USE method** for systems perf — Utilisation, Saturation, Errors for every resource. Exhaustive, mechanical.
- **Flame graphs** as a feedback surface.
- **High-cardinality wide events** (Majors) — one event per request with every dimension.
- **Test in production** (Honeycomb sense — feature-flag + observe, not "skip staging").
- **OODA cadence in incidents** (Allspaw) — smallest reversible move beats best move.
- **Hypothesis log during incidents** — running doc of "what we thought, what we tried, what we saw" (STELLA-style).
- **5 Whys as starting point only** — Cook/Dekker would warn against single-root-cause.
- **Carmack's valgrind discipline** — clean baseline makes next bug findable.

### Tensions / debates
- **Root cause vs. contributing factors** — Cook/Dekker/Woods: "the root cause" is a comforting fiction; multiple contributors, "human error" is a label not an explanation.
- **Old-view vs. new-view incident analysis** (Dekker) — blameless postmortem isn't kindness, it's epistemic requirement.
- **Observability vs. monitoring** — monitoring = known-unknowns with dashboards; observability = unknown-unknowns with high-cardinality query.
- **Debugger vs. print** — Linus prefers `printk`; Carmack uses debugger. Real variable is loop latency.
- **"Just trying stuff" vs. principled** (Wayne) — random mutation can solve without explaining.
- **Speed vs. correctness in incident response** — fast wrong + revertible beats slow right.

### Non-software cousins
- Aviation incident investigation (NTSB, ASRS) — model for blameless postmortems.
- Medicine — differential diagnosis. Atul Gawande, *The Checklist Manifesto*.
- Electronics troubleshooting — bisecting a signal path.
- Toyota *andon* — pull the cord at the smallest anomaly.
- Detective work — Holmes; Agans's rules read like a detective manual.
- Science — Popper's falsification.

### Add to SKILL.md
- Open with "**the loop is the skill**". Latency of feedback loop is the master variable.
- **Loop-construction checklist** before debugging: reproduce in <10s? see state? revert?
- Explicit **hypothesis log template**.
- **USE method** as default for performance; **differential-diagnosis frame** for correctness.
- **`git bisect run` recipe** with predicate template.
- **High-cardinality event pattern** for production debugging.
- **OODA-cadence note** for incident mode — smallest reversible move beats best move.
- **Anti-patterns**: single-root-cause thinking, blame-the-operator, debugging without a repro, "fixed" without understanding why.
- Pointer to **small-change** skill — every fix is a small change.
- Pointer to **Cook's "How Complex Systems Fail"** as required pre-reading for incidents.

---

## zoom-out

### Canonical sources
- **Donella Meadows**, *Thinking in Systems* (2008). Twelve Leverage Points essay: https://donellameadows.org/archives/leverage-points-places-to-intervene-in-a-system/ . Spine of any zoom-out: paradigm > goals > rules > information flows > parameters.
- **Eberhardt Rechtin**, *The Art of Systems Architecting* (3rd ed.). Source of the "heuristics" tradition.
- **Russell Ackoff**, "From Mechanistic to Systemic Thinking" — https://www.youtube.com/watch?v=OqEeIG8aPPk . "A system is never the sum of its parts; it's the product of their interactions."
- **John Gall**, *Systemantics / The Systems Bible* — "Gall's Law: a complex system that works is invariably found to have evolved from a simple system that worked."
- **Simon Brown**, C4 — https://c4model.com/ — four zoom levels.
- **arc42** — https://arc42.org/ — §1 (Goals), §3 (Context), §9 (Decisions).
- **Michael Nygard**, ADR — zoom-out should always check: has this been decided already?
- **Will Larson**, *An Elegant Puzzle* + *Staff Engineer* — https://lethain.com/ — "Systems thinking" chapter; archetypes (Tech Lead / Architect / Solver / Right Hand) as zoom levels.
- **Charity Majors**, "The Sociotechnical Path to High-Performing Teams" — https://charity.wtf/ — the system is sociotechnical.
- **Team Topologies** — https://teamtopologies.com/ — cognitive load as a first-class architectural constraint.
- **Conway's Law** + **Inverse Conway Maneuver** — https://www.thoughtworks.com/radar/techniques/inverse-conway-maneuver .
- **Sweller — Cognitive Load Theory** (1988, *Cognitive Science*). Justifies the skill's existence.
- **DDD Strategic** — Evans ch. 14, Vernon. Context maps; bounded contexts.

### Techniques worth borrowing
- **Meadows' leverage points as a checklist** — am I tweaking a parameter when I should be questioning a rule? A rule when I should question the goal?
- **C4's four-level zoom** — always ask "what is the System Context here? Who/what is outside the box?"
- **arc42 §1, §3, §9 triplet** — Goals / Context / Decisions. If any is missing, that's where to zoom.
- **ADR archaeology** — grep `docs/adr`, `decisions/`, `*.adr.md` *before* reasoning fresh.
- **Context-map drawing** (DDD) — list bounded contexts and integration patterns (Shared Kernel, Customer/Supplier, Conformist, ACL, OHS, Published Language, Separate Ways).
- **Sociotechnical sweep** (Team Topologies) — who owns this? What team type? What cognitive load?
- **Invariant inventory** — before changing anything, list invariants (data integrity, ordering, idempotency, auth).
- **Rechtin heuristics as prompts** — "Simplify, simplify, simplify"; "Don't assume the original statement of the problem is necessarily best, or even right"; "Choose elements so they are as independent as possible".
- **Gall's Law** as a sanity check — if proposing a grand redesign, remember it almost certainly won't work from scratch.

### Tensions / debates
- How far to zoom is itself a judgment call — match to blast radius.
- **Strategic DDD is loved; tactical DDD is contested** — bounded contexts widely adopted; aggregates/repos less so (North, Wayne).
- **C4 vs. UML vs. "just draw a box"** — pragmatic middle: one-page context diagram per system.
- **Conway vs. Inverse Conway** — Inverse Conway is fashionable, but Ruth Malan (https://ruthmalan.com/ , exact post uncertain) cautions re-orging is politically loaded.

### Non-software cousins
- Meadows comes from environmental systems; examples are fisheries, thermostats, national debt.
- Ackoff — operations research; "f-Laws" read like architecture aphorisms.
- Rechtin — aerospace systems engineering at JPL (Deep Space Network).
- NASA mishap reports — Columbia, Challenger, Apollo 13. CAIB report: https://history.nasa.gov/columbia/CAIB.html .
- IDEO / Kelley brothers — design thinking's "reframe the problem".

### Add to SKILL.md
- A "**where does this code live?**" preflight: nearest README, CODEOWNERS, ADR folder, top-level architecture doc.
- Explicit **four-step zoom ladder** modelled on C4: code → component → container → system context, with one question per level.
- A **Meadows leverage-points reminder** as a prompt when the proposed fix feels too local.
- A "**who else has decided this?**" step — grep ADRs / design docs / RFCs / prior PRs before reasoning fresh.
- An **invariants checklist** drawn from Rechtin's "interfaces are leverage".
- A **sociotechnical pass** (Team Topologies) — which team, which interaction mode, stream-aligned vs. platform.
- A "**stop and zoom**" trigger list — diff spans >3 modules, can't name the invariant, reaching for a workaround twice, about to write `// TODO: refactor later`.
- A **Gall's Law guardrail** — prefer evolving the working system over grand redesign.

---

## improve-architecture

### Canonical sources
- **John Ousterhout**, *A Philosophy of Software Design* (2nd ed., 2021). Stanford CS190. Deep modules; complexity is incremental; information leakage; comments as design artifacts.
- **Martin Fowler**, *Refactoring* (2nd ed., 2018) — https://refactoringcatalog.com/ and https://martinfowler.com/books/refactoring.html . The catalogue.
- **Kent Beck**, *Tidy First?* (2023) + https://tidyfirst.substack.com/ . Economics of when to tidy before / after / never.
- **Michael Feathers**, *Working Effectively with Legacy Code* (2004). Seams; characterization tests.
- **Alistair Cockburn**, Hexagonal Architecture / Ports & Adapters — https://alistair.cockburn.us/hexagonal-architecture/ .
- **Robert C. Martin**, *Clean Architecture* (2017). Dependency rule, concentric circles.
- **Counterpoints to Clean Architecture:**
  - **Dan North**, "CUPID: for joyful coding" — https://dannorth.net/cupid-for-joyful-coding/ . SOLID/Clean is overprescribed.
  - **Hillel Wayne**, "I Have Complicated Feelings About SOLID" — https://www.hillelwayne.com/post/solid/ .
  - **Dan North**, "Patterns of Effective Delivery" (GOTO 2011) — https://dannorth.net/2011/10/24/patterns-of-effective-delivery/ . Exact title "It's the not knowing" uncertain.
- **Eric Evans** — DDD strategic ch. 14–16. Context maps; Anti-Corruption Layer.
- **Vaughn Vernon**, *Implementing DDD* (2013).
- **Simon Brown**, C4. Use when zooming in to improve.
- **arc42** §10 (Quality Requirements) and §11 (Risks and Technical Debt).
- **Forsgren / Humble / Kim**, *Accelerate* (2018). Loose coupling correlates with throughput + stability. https://dora.dev/ .
- **Team Topologies** — improvement that ignores cognitive load fails. Thinnest Viable Platform.
- **Inverse Conway Maneuver** — https://www.thoughtworks.com/radar/techniques/inverse-conway-maneuver .
- **Will Larson**, "Migrations" — https://lethain.com/migrations/ .
- **Sam Newman**, *Building Microservices* (2nd ed., 2021) + *Monolith to Microservices* (2019). Strangler fig (Fowler — https://martinfowler.com/bliki/StranglerFigApplication.html ).

### Techniques worth borrowing
- **Ousterhout's red flags as a scanning pass** — shallow module, information leakage, temporal decomposition, pass-through method, repeated knowledge, vague names, hard-to-describe interfaces.
- **The "deep module" test** — interface complexity / implementation complexity.
- **Beck's "tidy first?" decision tree** — tidy before if it makes the change obvious; tidy after if you learned something; never if you won't return.
- **Fowler's catalogue as vocabulary** — name the refactoring (Extract Function, Move Method, Replace Conditional with Polymorphism).
- **Feathers' seams** — identify (preprocessor / link / object) *before* changing behaviour. Characterization tests around the seam.
- **Ports & Adapters** as a default for new boundaries.
- **Anti-Corruption Layer** at every external system boundary you don't control.
- **Strangler fig** for any non-trivial replacement.
- **Migration discipline (Larson)** — for each migration, name the freeze date, deprecation date, removal date, owner.
- **Conway check** — if change crosses team boundaries, social cost dominates.
- **DORA-grounded targets** — aim improvements at loose coupling, deployability, testability, not conceptual cleanliness.

### Tensions / debates
- **Clean Architecture / SOLID is contested** — North, Wayne. Honest position: dependency inversion at *real* infrastructure boundaries is high-value; inside the domain often is not.
- **DRY vs. duplication-tolerance** — Sandi Metz: "duplication is far cheaper than the wrong abstraction" (RailsConf 2014, https://www.sandimetz.com/blog/2016/1/20/the-wrong-abstraction).
- **Microservices vs. modular monolith** — Newman softened over time; Shopify, Stack Overflow run modular monoliths.
- **Bottom-up Tidy First vs. top-down redesign** — Beck argues for compounding small changes; some situations need Larson-style migration.
- **Ousterhout vs. Uncle Bob on comments** — Ousterhout: comments are part of design; Martin: comments are failures of expressive code.
- **Hexagonal vs. Clean vs. Onion** — all variants of the same idea. Cockburn's original is smallest and most defensible.

### Non-software cousins
- **Christopher Alexander**, *Notes on the Synthesis of Form* + *A Pattern Language*. Original "pattern" idea.
- **Stewart Brand**, *How Buildings Learn* (1994). "Shearing layers" — site, structure, skin, services, space-plan, stuff. Coupling across rates is the real cost.
- **Lean / TPS** — *kaizen* is Tidy First's cultural cousin.

### Add to SKILL.md
- **Ousterhout-style red-flag scan** as the opening move.
- **Named-refactoring vocabulary** — when proposing changes, use Fowler's catalogue names.
- A **Tidy First / Tidy After / Don't Tidy** decision step before any structural change inside a feature PR.
- An **explicit seams-and-tests step** (Feathers) before changing legacy behaviour.
- A "**is this a migration?**" check — triggers Larson's freeze/deprecate/remove discipline and strangler-fig defaulting.
- An honest "**live debate**" callout — Clean Architecture is useful at real infra boundaries, not inside the domain. Cite North/Wayne as counterweight.
- A **DRY guardrail** quoting Metz — prefer duplication until abstraction is clear.
- A **Conway / Team Topologies check** — does this improvement cross team boundaries?
- A **DORA-grounded acceptance test** — does this improve deployability / loose coupling / testability, or only conceptual cleanliness?
- An **ADR output requirement** — every non-trivial improvement gets one (Nygard).

---

## handoff

### Canonical sources
- **Barbara Minto**, *The Pyramid Principle* (1987). Reference for top-down written communication.
- **Amazon — 6-pager and PR/FAQ.** Bezos 2017 letter: https://www.aboutamazon.com/news/company-news/2016-letter-to-shareholders ; Bryar & Carr, *Working Backwards* (2021).
- **NASA mishap reports:**
  - CAIB (Columbia) — https://history.nasa.gov/columbia/CAIB.html .
  - Rogers Commission (Challenger) — https://history.nasa.gov/rogersrep/genindex.htm — including Feynman's Appendix F.
- **John Allspaw** cites NASA reports regularly — https://www.adaptivecapacitylabs.com/blog/ ; "How Your Systems Keep Running Day After Day" — https://www.infoq.com/presentations/safety-resilient-systems/ .
- **Camille Fournier**, *The Manager's Path* (2017) + https://skamille.medium.com/ . Writing-for-the-next-person ethic.
- **Will Larson**, *Staff Engineer* + "Writing an Engineering Strategy" — https://lethain.com/eng-strategies/ — a direct handoff-doc template.
- **Michael Nygard** — ADR format.
- **IDEO** (Tom & David Kelley) — *Creative Confidence* (2013). "I like / I wish / what if" debrief.
- **Charity Majors**, "The Death of High-Context Engineering" — https://charity.wtf/ .
- **Sweller — Cognitive Load Theory** — the reader has zero context and ~4 working-memory chunks.
- **DeMarco & Lister**, *Peopleware* (3rd ed., 2013) — quantifies context loss.
- **Atul Gawande**, *The Checklist Manifesto* (2009) — encoding load-bearing steps.
- **Heath brothers**, *Made to Stick* (2007) — SUCCESs heuristic.

### Techniques worth borrowing
- **Pyramid Principle structure** — answer first; load-bearing points; detail. Never a chronological transcript.
- **Amazon 6-pager discipline** — narrative prose, not bullets. Sentences force precision.
- **PR/FAQ inversion** — write "done" first, then the FAQ a confused next engineer would ask.
- **NASA mishap-report structure** at miniature scale — timeline / contributing factors / proximate cause / distal cause / recommendations with owners.
- **ADR for every decision**, marked "Proposed" / "Accepted" / "Superseded".
- **State the invariants and known-broken assumptions** — what the next agent might *break* without knowing.
- **List the dead ends** — what was tried and rejected, with reasons.
- **Name open questions explicitly** — "Q1: is X idempotent? Unknown. Test before relying on it."
- **Heath SUCCESs check** on each load-bearing point.
- **IDEO "I like / I wish / what if"** as a separate reflection block.

### Tensions / debates
- **Narrative prose vs. structured bullets** — Amazon: narrative; most eng culture: bullets. Narrative forces causal connectives that bullets hide.
- **Long handoff vs. short handoff** — Larson and Fournier lean shorter; NASA goes longer. Right answer is *layered* — pyramid top ~100 words, full detail in appendix.
- **Process vs. artifact** — Majors: observability + good naming reduce need for handoff docs. True at system level; less true mid-task. Both/and.
- **Verbatim transcript vs. synthesis** — common failure is dumping agent's chain-of-thought. Minto: never.

### Non-software cousins
- **Medical shift handoffs — I-PASS** (Illness severity, Patient summary, Action list, Situation awareness, Synthesis by receiver). NEJM 2014: https://www.nejm.org/doi/full/10.1056/NEJMsa1405556 — cut errors ~30%.
- **Aviation** — sterile-cockpit rule + standardised crew briefings. Post-Tenerife Crew Resource Management.
- **Military — five-paragraph operations order (SMEAC)** — Situation / Mission / Execution / Admin/Logistics / Command/Signal.
- **Journalism** — inverted pyramid.
- **NASA flight controller handoffs** — explicit, scripted, witnessed.

### Add to SKILL.md
- A **Pyramid Principle top section** — single most load-bearing sentence ("the next agent must do X because Y"), then 3 supporting points, then detail.
- A fixed section list (**I-PASS + ADR + mishap-report hybrid**): Situation / Current State / Invariants / Open Questions / Dead Ends / Next Action / Appendix.
- An explicit "**invariants and known-broken assumptions**" section.
- A "**dead ends**" section — what was tried and rejected.
- A **narrative-prose preference** for the top of the doc; bullets only in appendix.
- A **length cap** — ~600 words body, unlimited appendix.
- A **SUCCESs check** on each load-bearing claim.
- A "**next action**" line — exact first command / file / query.
- **ADR pointers** — any architectural decision links to or stubs an ADR.
- An "**I like / I wish / what if**" reflection block, kept *separate* from the action handoff.
- A guardrail against dumping raw transcript — chain-of-thought goes in appendix, summarised in body.

---

# Part 3 — Cross-cutting

## Patterns that show up everywhere

Three observations from doing all four briefs:

**1. Cognitive load is the unifying constraint.** Sweller's 1988 paper turns up under zoom-out, handoff, and improve-architecture explicitly, and implicitly under tdd (small steps), small-change (MMMSS), and orchestrator (size the work to the budget). The whole repo can be framed as "manage cognitive load — yours, the agent's, and the next reader's".

**2. ADRs are the connective tissue.** Six of the twelve skills reference ADRs in some form. They are the load-bearing residue between sessions. Investing more in the ADR format (Nygard + supersedes-chains + cross-skill links) compounds across skills.

**3. Two debates appear repeatedly and the repo currently doesn't acknowledge them:**

- **Single-root-cause vs. resilience-engineering** — `diagnose` says "winning hypothesis" implying a single cause; Cook/Dekker/Woods reject that frame. The repo would benefit from a single explicit position.
- **Clean-architecture/SOLID** — `improve-architecture` references "deep modules" (Ousterhout) but doesn't acknowledge that SOLID/Clean is contested. Citing North and Wayne as counterweights makes the position more defensible.

## Highest-leverage additions to consider first

If integration time is limited, these are the bullet additions that would change the most behaviour per word:

1. **Walking-skeleton issue as #1** on every new system (`to-issues`). Forces end-to-end thinking before depth.
2. **Tidy/Behavior gate** at PR time (`small-change`). Beck's single most-cited recent practice.
3. **INVEST rejection checklist** on every generated issue (`to-issues`). Cheap filter, high signal.
4. **Pyramid-Principle top section + I-PASS sections** for handoff. The current `handoff` is chronological; restructuring it top-down + explicit invariants/dead-ends would change every session.
5. **Ousterhout red-flag scan** as the opening move for `improve-architecture`. Mechanical and high-yield.
6. **"The loop is the skill"** lede for `diagnose` plus a loop-construction checklist. Already implicit; making it explicit raises the floor.
7. **Add the four debates as named "live debates" callouts** in their respective skills (TDD-classicist-vs-mockist, root-cause-vs-resilience, SOLID-vs-CUPID, narrative-vs-bullets). Acknowledging these makes the repo feel authored, not generic.
8. **A `thinking/` bucket** with six atomic skills (decompose, diagnose-context, structure-communication, estimate, pre-mortem, decide-under-uncertainty). New surface area; opens generic problem-solving to the same workflow discipline.

## What's deliberately NOT recommended

A few common adjacencies that the research suggests would dilute the repo rather than strengthen it:

- **Don't add Polya, OODA, PDCA, A3 as standalone skills.** They overlap heavily with existing skills (debugging cycle, incident response, slice loop, postmortem) and adding them creates routing confusion.
- **Don't add a CIRCLES skill for PM-style design.** The Cohn/Lawrence splitting patterns + INVEST already cover the operational shape; CIRCLES is more interview prep than work.
- **Don't add a Clean-Architecture skill.** The live debate is real; `improve-architecture` should reference Hexagonal/Ports & Adapters as a defaults but not adopt SOLID wholesale.
- **Don't push DDD tactical patterns.** Strategic DDD (bounded contexts, context maps, anti-corruption layer, ubiquitous language) is gold. Tactical DDD (aggregates, entities, value objects, repositories) is contested and the repo doesn't need to take a side.

## Marked-uncertain references

To audit before citing in any SKILL.md:

- Dan North, "It's the not knowing" — exact title and URL uncertain. The talk is "Patterns of Effective Delivery" (GOTO 2011) — https://dannorth.net/2011/10/24/patterns-of-effective-delivery/ .
- Ruth Malan's specific Inverse-Conway critique post — known to write at https://ruthmalan.com/ but exact post uncertain.
- Brad Porter's Amazon 6-pager source — appears on Quora, exact citation needs verification.
- Camille Fournier MMMSS post — https://skamille.medium.com/ is the right author; exact post uncertain.
- Kent Beck "3X" canonical link — concept is well-established; the original Facebook note URL is uncertain.
- John Carmack `valgrind` discipline — discussed in his Lex Fridman appearance; exact timestamp uncertain.

---

*End of dump. Next pass: read this, point at the bullets you want me to integrate, and I'll edit the SKILL.md files surgically without changing each skill's original purpose.*
