---
extends: philosophy.md
language: Python
---

# Python Philosophy

This extends `philosophy.md` with Python-specific conventions. Read both.

## Layered Architecture

In Django, Flask, and FastAPI codebases, a service/repository layer is common
and appropriate. Follow the conventions of the framework and codebase:

- Django: models → services → views/serializers (MTV)
- FastAPI/Flask: routers → services → repositories
- `*Service`, `*Repository` suffixes are appropriate within an established layer

When a codebase has no established layer, don't introduce one speculatively.
Name what the thing does.

## Classes

Classes are appropriate for domain objects and anything with meaningful state.
The core philosophy still applies: prefer composition over inheritance, keep
inheritance for language-mandated patterns (Exception subclasses, ABC where
genuinely needed), and question deep hierarchies.

Use `@dataclass` or `NamedTuple` for value objects — immutable, data only.

## Type Hints

Use type hints on all public interfaces. They serve as documentation and enable
static analysis. Internal helpers can be less strict, but public APIs should be
typed.

## Functional Patterns

Prefer list comprehensions and generator expressions over loops with mutation.
Use `map`/`filter` when it reads clearly; comprehensions otherwise.

Avoid mutating function arguments — return new objects instead.
