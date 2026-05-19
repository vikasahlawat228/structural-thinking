---
name: tdd
description: Test-driven development with a strict vertical-slice red-green-refactor loop. Use when building a feature or fixing a bug with test coverage. Trigger phrases include "TDD this", "red-green-refactor", "write tests first", "test-driven", or when running per-slice implementation under the medium-feature or large-project workflow.
---

# Test-Driven Development — Vertical Slices

## Philosophy

**Tests verify behaviour through public interfaces.** Code can change entirely; tests shouldn't.

A good test reads like a one-line spec: `user_can_redeem_coin_with_matching_currency`. It survives a refactor because it doesn't care about internal structure.

A bad test is coupled to implementation: it mocks internal collaborators, peeks at private state, or asserts on the shape of intermediate data. Warning sign: your test breaks when you refactor but behaviour hasn't changed. Those tests aren't testing behaviour — they're testing your scaffolding.

## The single most important rule

**Do not write all tests first, then all implementation.** That is horizontal slicing. It produces crap tests, because they describe imagined behaviour, not actual.

Write **one** test → make it pass → write the next test. This is vertical slicing.

```
WRONG (horizontal):
  RED:   test1, test2, test3, test4
  GREEN: impl1, impl2, impl3, impl4

RIGHT (vertical):
  RED→GREEN: test1 → impl1
  RED→GREEN: test2 → impl2
  RED→GREEN: test3 → impl3
```

The reason: each test should respond to what you learned in the previous cycle. You can only learn that by running the previous cycle.

## Workflow

### 1. Plan the interface

Before any code, agree with the user on:

- **Public interface shape** — what calls this from the outside?
- **List of behaviours** — not implementation steps; observable outcomes.
- **Priority order** — which behaviour matters most? Start there.

Look for opportunities for **deep modules** — a small interface hiding a lot of implementation. The opposite (shallow modules with chatty interfaces) is the slow road to the ball of mud.

You can't test everything. Pick the load-bearing behaviours. Edge cases that don't change the design get one combined test or none.

### 2. Tracer bullet — one test, one impl

Pick the first behaviour. Write **one** test that proves it.

```
RED:   write test → run it → confirm it fails for the right reason
GREEN: write minimum code → run test → confirm it passes
```

The tracer bullet proves the path works end-to-end. After this, you have a green baseline and a known interface shape.

### 3. Incremental loop

For each remaining behaviour:

```
RED:   write next test → fails
GREEN: minimal code → passes
```

Rules:

- **One test at a time.** Don't queue up tests.
- **Minimal code to pass.** Don't anticipate the next test.
- **Test through the public interface.** Never reach inside.
- **Confirm the failure mode.** A test that fails for the wrong reason is worse than no test.

### 4. Refactor — only on green

Once all the day's tests pass, look for refactors:

- Extract duplication.
- Deepen a module — push complexity behind a smaller interface.
- Apply structure where it now reveals itself.
- Re-read the tests — do any of them now look implementation-coupled? Sharpen them.

**Never refactor while red.** Get to green first, always.

## Per-cycle checklist

Before moving to the next test:

- [ ] Test describes a behaviour, not an implementation step.
- [ ] Test uses public interface only.
- [ ] Test would survive an internal refactor of the module.
- [ ] Code is the minimum needed to make this test pass.
- [ ] No speculative features added.

## Anti-patterns

- **Writing tests that just exercise types.** TypeScript already does that. Tests should assert behaviour.
- **Mocking internal collaborators of the module under test.** If you need to mock an internal function, the test is at the wrong seam.
- **Skipping the "watch it fail" step.** A test that passes the first time is a false positive waiting to happen — you don't know if it can fail.
- **Refactoring during red.** This is how green never returns.
- **Long test files with implementation interleaved.** One test should fit on one screen, top to bottom.

## When this skill isn't enough

- **The interface itself isn't clear.** Stop. Run [`grill-me`](../../general/grill-me/SKILL.md) on the interface design first.
- **You can't build a fast feedback loop.** Run [`diagnose`](../diagnose/SKILL.md) phase 1 to build one before resuming TDD.
- **Behaviour spans modules you don't know.** Run [`zoom-out`](../../general/zoom-out/SKILL.md) to orient before slicing.
