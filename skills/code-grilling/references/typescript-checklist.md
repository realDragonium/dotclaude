# TypeScript/JavaScript Review Checklist

## Philosophy Checks (TS/JS specific)

### Naming
- React components with `Base`/`Abstract` prefix → flag inheritance smell
- Files named `utils.ts`, `helpers.ts`, `common.ts` → junk drawers, break them up
- `I` prefix on interfaces (IUser) → unnecessary in TS, the type system handles this
- Classes named `*Service`, `*Manager` → what does it actually do? Name that

### Inheritance
- `class X extends Y` → propose composition pattern or just functions
- React class components → should be functional components (the framework moved on)
- Deep prototype chains → flatten with composition
- `abstract class` → almost always an interface + factory function instead

### Functional Patterns
- `let` where `const` works → prefer immutability
- `for` loops where `.map()/.filter()/.reduce()` are clearer
- Methods that mutate `this` → can this be a pure function?
- Side effects (I/O, logging, DB) mixed into business logic → separate them

## Type Safety
- Every `any` → require justification or proper typing
- Type assertions (`as Type`) → each one bypasses the compiler = potential runtime crash
- `!` non-null assertions → each one is a bet that will lose
- Optional chaining (`?.`) without fallback → silent undefined propagation
- Unconstrained generics (`<T>`) → types aren't doing their job

## Async Patterns
- Every `async` function: is the error handled? Unhandled rejections crash Node
- Fire-and-forget async (no `await`, no `.catch()`) → will fail silently
- `await` inside loops → usually should be `Promise.all()`
- Missing cleanup on cancellation/timeout

## Runtime Safety
- Object spread (`{...obj}`) → shallow copy, mutation bugs hide here
- In-place array methods (`.sort()`, `.reverse()`, `.splice()`) vs immutable
- Template literal injection in SQL/HTML/shell → goes through sanitization?
- `==` vs `===` → but more importantly, structurally wrong comparisons

## Module Patterns
- Circular imports → trace the graph
- Barrel files (`index.ts`) → unnecessary bundling cost?
- Side effects at module scope → code running on import
