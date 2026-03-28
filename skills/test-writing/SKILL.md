---
name: test-writing
description: Write behavior-focused tests. Use when someone says "write tests", "add tests", "test this", "what should we test", or when implementing tests for a feature. Tests describe WHAT the code does, not HOW it does it. Pairs with behavior-map specs.
---

Read the shared philosophy: `${CLAUDE_PLUGIN_ROOT}/skills/shared/philosophy.md`

You are writing behavior-focused tests. Tests are a spec for what the code
does. They should read like documentation and survive refactoring.

## What to test
$ARGUMENTS

## If a behavior-map exists for this feature, read it first.
The behavior map's "Given/When/Then" list is your test list.

## CORE PRINCIPLE: Test behavior, not implementation

A good test answers: **"does this feature work correctly?"**
A bad test answers: "does this function call that function with these args?"

The test: if you refactor the internals completely but the behavior stays
the same, do all tests still pass? If not, the tests are coupled to
implementation.

## WHAT TO TEST (in priority order)

1. **Happy path behaviors** — the thing actually works for normal use
2. **Error behaviors** — it fails correctly (right error, right message, clean state)
3. **Edge cases** — empty input, boundary values, concurrent access
4. **Integration points** — behavior across module/service boundaries

## WHAT NOT TO TEST

- Private/internal functions directly — test them through public behavior
- Obvious getters/setters — no value
- Framework code — test YOUR logic, not the framework
- Formatting/display — unless it's core product behavior
- Implementation details — mock less, assert on outcomes more

## TEST NAMING

Tests read as behavior specs. The name describes the behavior, not the method:

```
// ✅ Behavior-focused
"returns empty array when user has no orders"
"rejects payment when card is expired"
"sends notification after successful signup"
"preserves existing data when update partially fails"

// ❌ Implementation-focused
"test getOrders"
"test handlePayment calls validateCard"
"test that sendEmail is called"
```

## TEST STRUCTURE

Every test follows **Arrange → Act → Assert**, clearly separated:

```
// Arrange — set up the scenario
// (describe the "Given" state)

// Act — do the thing
// (the "When" trigger, ONE action per test)

// Assert — verify the behavior
// (the "Then" outcome, focused on WHAT happened, not HOW)
```

One behavior per test. If a test needs "and" in its name, split it.

## MOCKING PHILOSOPHY

Mock at boundaries, not internally:

**Mock these** (external, slow, unpredictable):
- HTTP APIs / external services
- Databases (for unit tests — use real DB for integration)
- File system
- Time/dates
- Random/crypto

**Don't mock these** (your own code):
- Internal functions/modules — let them run for real
- Data transformations — test input → output directly
- Anything that's fast and deterministic

If you're mocking 5 things to test 1 behavior, the code is probably
too coupled. Flag it as a design issue, don't paper over it with mocks.

## LANGUAGE-SPECIFIC PATTERNS

Detect the language and apply:

**TypeScript/JavaScript:**
- Use `describe` for the feature/module, `it` for behaviors
- Prefer `toEqual` for values, `toThrow` for errors
- Use `beforeEach` for shared arrange, not shared state
- Async: always `await` assertions, use `rejects` for async errors

**Python:**
- Use `pytest` style (functions, not classes — no inheritance in tests either)
- `pytest.raises` for error behaviors
- Fixtures for shared arrange
- Parametrize for multiple inputs → same behavior

**Go:**
- Table-driven tests for multiple input/output cases
- `t.Run` for sub-behaviors
- `testify/assert` or stdlib — be consistent with the codebase
- Test helpers are fine, but `t.Helper()` them

## OUTPUT FORMAT

For each behavior being tested, output:

### [Feature/Module name]

**Behaviors to test:**
1. [behavior from spec or inferred from code]
2. ...

**Tests:**
```
[actual test code]
```

**Not tested (and why):**
- [thing] — [reason: trivial / framework code / tested elsewhere / etc.]
