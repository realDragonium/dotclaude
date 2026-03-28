# Go Review Checklist

## Philosophy Checks (Go specific)

### Naming
- Go already discourages most bad naming patterns, but check:
- Package names: should be short, lowercase, no underscores. `package utils` → break it up
- Stuttering: `user.UserService` → `user.Store`, `user.Client`, etc.
- Interface names ending in `-er` are idiomatic (`Reader`, `Writer`) but
  `*Manager`, `*Handler` are still vague → name what it does
- Exported names that don't need to be exported → keep the API surface small

### Inheritance (Go doesn't have it, but watch for imitations)
- Struct embedding used as inheritance → does the embedded type's full method
  set make sense on the outer type? Embedding for a single method is a smell
- "God structs" that embed 5+ types → this is composition gone wrong
- Embedding to get "default implementations" → use explicit delegation

### Functional Patterns
- Go is not FP, but the principles still apply:
- Functions that take 5+ parameters → create an options struct or rethink the API
- Methods on structs that don't use `self`/receiver → make them plain functions
- Pointer receivers used everywhere "just in case" → value receivers for small immutable types
- Business logic mixed with I/O → separate them. Pure functions for logic,
  I/O at the boundaries

### Simplicity (Go-specific)
- Channels where a mutex would be simpler → channels are for communication, mutexes for protection
- `interface{}` / `any` where a concrete type or generic works
- Custom error types where `fmt.Errorf` with `%w` suffices
- Reflection where generics or code generation would be clearer

## Error Handling (the #1 source of Go bugs)
- EVERY returned error must be checked — `val, _ := f()` is almost always wrong
- `fmt.Errorf("context: %w", err)` not `%v` (preserves error chain for unwrapping)
- Don't log-and-return — either handle or propagate, not both
- Errors in defer: `defer f.Close()` should check the error
- Sentinel errors vs error types — is the checking approach consistent?

## Goroutine Safety
- Every `go func()` must have a clear termination path → goroutine leaks
- Channel operations: can this block forever? Select with timeout/context?
- Shared state without mutex or channel → run `-race` mentally
- Context propagation: is `ctx` threaded to every blocking operation?
- `WaitGroup.Add()` before `go func()`, not inside it

## Interface Design
- Accept interfaces, return structs
- Interfaces >3 methods → probably too big, split them
- `nil` interface vs nil-pointer-to-interface trap:
  `var x error = (*MyError)(nil)` is non-nil
- Interfaces defined by consumer, not producer (Go convention)

## Memory & Performance
- `append()` may or may not allocate → be explicit about capacity in hot paths
- `[]byte(s)` copies the string → avoid in hot paths
- `defer` in loops → runs at function exit, not iteration exit
- Map iteration order is random
- Struct field alignment → padding can significantly affect memory
