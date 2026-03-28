# Coding Philosophy

These principles govern all review, architecture, and product decisions.
They are not rigid rules — they are a compass. Apply judgment.

After reading this, detect the primary language of the codebase and read the relevant extension:
- Go → `${CLAUDE_PLUGIN_ROOT}/skills/shared/philosophy-go.md`
- Python → `${CLAUDE_PLUGIN_ROOT}/skills/shared/philosophy-python.md`
- TypeScript/JavaScript → `${CLAUDE_PLUGIN_ROOT}/skills/shared/philosophy-typescript.md`

## Core Principle: Simplicity as Default, Complexity as Earned

Simplicity is the default state. Complexity must justify its existence with
a concrete problem it solves. "We might need this later" is not justification.
"This is failing in production" is.

When reviewing, always ask: **can this be simpler without losing capability?**

## Composition Over Inheritance

In our own code: never. The only permitted uses are external constraints:
- Language-mandated patterns (e.g., extending Error/Exception classes)
- Framework requirements that cannot be avoided (and even then, question the framework choice)

Prefer: interfaces, composition, higher-order functions, dependency injection.
Flag every `extends`/inherits in a review. The question is always: is this an
external constraint, or a design choice? If the latter, it does not belong here.

## Functional Patterns

Prefer pure functions over methods with side effects. Prefer immutable data
transformations over mutating state. Prefer declarative over imperative when
it doesn't sacrifice readability.

This doesn't mean "everything must be FP" — it means the default should be
functional, and imperative/stateful code should exist at well-defined boundaries
(I/O, database, external APIs) rather than scattered throughout business logic.

Functions should do one thing. If the name needs "and" in it, it's two functions.

## Naming: Say What It Is, Nothing More

Names should be precise and honest. They should describe what something IS
or DOES, not what category it belongs to.

**Kill useless suffixes:**
- `OrderManager` → what does it manage? Name that thing
- `DataHelper` → helper for what? Name the actual operation
- `BaseController` → why does this exist? (see: inheritance)
- `AbstractFactory` → almost certainly over-engineered
- `*Utils` / `*Helpers` / `*Common` as class or file names → junk drawers; name what
  the thing actually does. As folder names they're fine if there's a clear theme.

**The test:** if you remove the suffix and the name still makes sense and is
more precise, the suffix was noise.

**Consistency matters more than any single naming choice.** If the codebase
uses one convention, follow it even if you'd prefer another. Inconsistency
is worse than a suboptimal-but-consistent convention.

## Formatting Is Not a Review Concern

Formatting (indentation, line breaks, bracket placement, import ordering,
trailing commas) is the job of automated tools: prettier, black, gofmt,
eslint --fix, etc.

**Never flag formatting in a review.** If the project doesn't have a formatter
configured, flag THAT as an issue (once), not individual formatting choices.

The only exception: if formatting obscures logic (e.g., a ternary nested
three levels deep that would be clearer as an if/else), that's a readability
concern, not a formatting concern. Flag the readability problem.

## Architecture: Fewest Moving Parts That Solve the Problem

- Don't split into microservices until a monolith is actually painful
- Don't add a message queue until synchronous is actually failing
- Don't add caching until you've measured the bottleneck
- Don't add an ORM if raw queries are clear and maintainable

Every architectural boundary has a cost: serialization, error handling,
deployment complexity, cognitive load. The benefit must exceed the cost.

## When Complexity Is Warranted

Complexity earns its place when:
- The simple approach has failed in practice (not in theory)
- The domain is genuinely complex (financial calculations, concurrent state)
- Safety/correctness requirements demand it (type systems, validation layers)
- Scale has been measured and the simple approach doesn't hold

In these cases, embrace the complexity — but contain it. Complex logic
should live behind simple interfaces.
