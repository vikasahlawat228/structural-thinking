---
name: tdd
description: Test-driven development with a strict vertical-slice red-green-refactor loop. Use when building a feature or fixing a bug with test coverage. Trigger phrases include "TDD this", "red-green-refactor", "write tests first", "test-driven", or when running per-slice implementation under the medium-feature or large-project workflow.
category: software
---

# Test-Driven Development — Vertical Slices

## Philosophy

**Tests verify behaviour through public interfaces.** Code can change entirely; tests shouldn't.

A good test reads like a one-line spec: `user_can_redeem_coin_with_matching_currency`. The test name *is* the specification — it tells the next reader what the system does without making them read the body. Tests survive refactors because they don't care about internal structure.

A bad test is coupled to implementation: it mocks internal collaborators, peeks at private state, or asserts on the shape of intermediate data. Warning sign: the test breaks when you refactor but behaviour hasn't changed. Those tests aren't testing behaviour — they're testing scaffolding.

## The single most important rule

**Do not write all tests first, then all implementation.** That is horizontal slicing. It produces crap tests, because they describe imagined behaviour, not actual behaviour.

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

Each test must respond to what you learned in the previous cycle. You can only learn that by running the previous cycle.

## Workflow

### 1. Plan the interface

Before any code, agree with the user on:

- **Public interface shape** — what calls this from the outside?
- **List of behaviours** — not implementation steps; observable outcomes. The test names you would write.
- **Priority order** — which behaviour matters most? Start there.

Look for opportunities for **deep modules** — a small interface hiding substantial implementation. The opposite (shallow modules with chatty interfaces) is the slow road to the ball of mud.

You cannot test everything. Pick the load-bearing behaviours. Edge cases that don't change the design get one combined test or none.

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

Hard rules:

- **One test at a time.** Don't queue up tests in your head.
- **Minimal code to pass.** Don't anticipate the next test.
- **Test through the public interface.** Never reach inside.
- **Confirm the failure mode.** A test that fails for the wrong reason is worse than no test — make sure red is red for the right reason before going to green.

### 4. Refactor — only on green

Once the day's tests pass, look for refactors:

- Extract duplication.
- Deepen a module — push complexity behind a smaller interface.
- Apply structure where it now reveals itself.
- Re-read the tests — do any look implementation-coupled? Sharpen them.

**Never refactor while red.** Get to green first, always. Refactor on red is how green never returns.

## Choosing the right kind of test

### Example tests (the default)

Most TDD cycles use **example tests** — given specific inputs, assert specific outputs or observable effects. This is what Beck's TDD by Example teaches, and it's the right default for stateful behaviour, UI flows, and integration points.

### Property tests (when the unit is pure)

For pure functions and total functions over a clear domain, supplement examples with at least one **property test** — round-trip (`encode(decode(x)) == x`), idempotence (`f(f(x)) == f(x)`), invariants ("result is always in this range"). Properties catch classes of bugs example tests miss because the property-based runner generates inputs you wouldn't think to write.

Heuristic: **property when pure, example when stateful**. Don't force properties on stateful or side-effecting code — examples are clearer there.

### Snapshot / approval tests (for legacy)

When working in code without tests, characterise current behaviour with **snapshot or approval tests** before changing anything. The snapshot pins the current output; you then refactor knowing exactly what changed. Treat snapshots as a way *into* a TDD loop, not a permanent test strategy — pin behaviour, then write proper tests, then delete the snapshot.

## Classicist vs mockist — the live debate

There are two long-running schools of TDD, and you should know which one you're in.

- **Classicist (Detroit / Beck).** Test behaviour with real collaborators wherever possible. Mock only at the system's edges — file system, network, time, third-party APIs. Tests survive refactors because internal restructuring doesn't break them.
- **Mockist (London / GOOS).** Test interactions between collaborators using test doubles at every internal boundary. Each unit is tested in isolation. Tests are sensitive to internal structure by design.

**This skill defaults to classicist.** Mockist tests are valuable at architectural seams — places where the test *needs* to assert on a protocol of calls (e.g., "did we publish the correct event in the right order?"). For everything else, mocks couple tests to implementation and they break on refactor.

If you find yourself reaching for a mock to test an internal function, that's a sign the test is at the wrong seam — back up to the next layer out.

## Per-cycle checklist

Before moving to the next test:

- [ ] Test name describes a behaviour, not an implementation step.
- [ ] Test uses the public interface only.
- [ ] Test would survive an internal refactor of the module.
- [ ] Code is the minimum needed to make this test pass.
- [ ] No speculative features added.
- [ ] If pure: a property test was at least considered for this behaviour.

## Anti-patterns

- **Writing tests that just exercise types.** The type system already does that. Tests should assert behaviour.
- **Mocking internal collaborators of the module under test.** If you need to mock an internal function, the test is at the wrong seam.
- **Skipping the "watch it fail" step.** A test that passes the first time is a false positive waiting to happen.
- **Refactoring during red.** Green first.
- **One-assertion-per-test dogma.** Multiple assertions are fine if they're testing one behaviour. Pedantic splitting hurts readability.
- **Mock-the-world setups.** When the test's setup is longer than its assertion, the test is over-isolated.
- **Coverage as a goal.** Universally a metric trap; everyone still measures it. Write tests for behaviour, not for the coverage report.
- **Test-after.** Writing tests after the implementation skips the design pressure that red-first creates. The point of TDD is the design pressure, not the coverage.

## When this skill isn't enough

- **The interface itself isn't clear.** Stop. Run [`grill-me`](../../general/grill-me/SKILL.md) on the interface design first.
- **You can't build a fast feedback loop.** Run [`diagnose`](../diagnose/SKILL.md) phase 1 to build one before resuming TDD.
- **Behaviour spans modules you don't know.** Run [`zoom-out`](../../general/zoom-out/SKILL.md) to orient before slicing.
- **The codebase has no tests at all.** Don't start TDD on top of a black box. Build characterisation/snapshot tests first to pin current behaviour; *then* TDD the change.
