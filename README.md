# Structural Thinking

Project folder for Vikas's agent-skills work. Contains the deliverable `vikasahlawat228/skills` repo and the research material used to build it.

## Contents

### `skills/`

The deliverable repo. Drop this into `vikasahlawat228/skills` on GitHub when ready.

- **`README.md`, `CLAUDE.md`, `CONTEXT.md`, `LICENSE`, `.gitignore`**
- **`docs/adr/0001-three-buckets-and-an-orchestrator.md`** — the founding decision record.
- **`skills/orchestrator/SKILL.md`** — meta router (small/medium/large classifier).
- **`skills/workflows/{small-change, medium-feature, large-project}/SKILL.md`** — the three bucket workflows.
- **`skills/atomic/...`** — nine atomic skills the workflows compose: `grill-me`, `grill-with-docs` (+ `CONTEXT-FORMAT.md`, `ADR-FORMAT.md`), `tdd`, `diagnose`, `to-prd`, `to-issues`, `zoom-out`, `handoff`, `improve-architecture`.

### `skills-research-dump.md`

~9,600-word knowledge dump from a research pass over the best minds on each skill's territory (Beck, Ousterhout, Fowler, Feathers, Evans, Vernon, Cagan, Singer, Pocock, Minto, Cynefin, etc.). Organised:

1. **Generic structural-thinking frameworks** — MECE, Pyramid Principle, Cynefin, OODA, Polya, first-principles, Munger, PDCA, A3, pre-mortem, Kahneman/Klein/Tetlock, Toulmin, Fermi, case-prep methods. Plus a proposed `thinking/` bucket of six atomic generic-problem-solving skills.
2. **Per-skill briefs** for the 12 existing skills — canonical sources, techniques worth borrowing, tensions/debates, non-software cousins, and concrete "what to add to SKILL.md" bullets.
3. **Cross-cutting** — patterns that appear across skills + a "highest-leverage additions to consider first" shortlist.

## Next pass

Read `skills-research-dump.md`. Either:

- Mark the bullets you want pulled into each SKILL.md and tell me to integrate. I'll edit surgically, no purpose-drift.
- Or just say "do the 8 highest-leverage ones from the bottom of the dump" and I'll start there.

If you want to also start the generic `thinking/` bucket (six new skills: `decompose`, `diagnose-context`, `structure-communication`, `estimate`, `pre-mortem`, `decide-under-uncertainty`), that's a separate pass — say the word.

## To push the skills repo to GitHub

```bash
cd ~/Documents/Claude/Projects/Structural\ Thinking/skills
git init
git add -A
git commit -m "Initial commit: orchestrator + 3 workflows + 9 atomic skills"
gh repo create vikasahlawat228/skills --public --description "Engineering discipline for agentic coding"
git remote add origin git@github.com:vikasahlawat228/skills.git
git push -u origin main
```
