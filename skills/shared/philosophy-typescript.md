---
extends: philosophy.md
language: TypeScript/JavaScript (frontend)
---

# TypeScript/JavaScript Philosophy

This extends `philosophy.md` with TypeScript/JavaScript frontend conventions. Read both.

## Types

Avoid `any` — it defeats the purpose. Prefer `unknown` when the type is genuinely
unknown and narrow it explicitly. Let TypeScript infer where it can; annotate public
interfaces, function signatures, and anything non-obvious.

Prefer `type` over `interface` for unions, intersections, and aliases.
Use `interface` when modeling object shapes that consumers may extend.

## Functional Patterns

Prefer pure functions. Use `const` everywhere — `let` only when reassignment is
unavoidable. Transform arrays with `map`/`filter`/`reduce` rather than looping
with mutation.

Avoid classes for stateless logic — a module of exported functions is simpler.

## Async

Use `async`/`await`. Never mix `.then()` chains with `await` in the same function.
Always handle rejections — unhandled promise rejections are silent bugs.

Don't `await` inside loops when calls are independent — use `Promise.all`.

## Modules

No barrel files (`index.ts` that re-exports everything) unless at a published package
boundary. Barrel files cause circular dependency risks and slow bundlers. Import
directly from the source file.

No `utils.ts`, `helpers.ts`, or `common.ts` — name the actual domain.
