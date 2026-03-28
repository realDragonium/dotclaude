---
extends: philosophy.md
language: Go
---

# Go Philosophy

This extends `philosophy.md` with Go-specific conventions. Read both.

## Package Structure

Organize by domain, not by layer. There is no service layer, controller layer,
or repository layer — these are Java/C# patterns that don't fit Go. Packages
represent domains: `user`, `order`, `payment`.

Within a domain package, persistence, business rules, and transport handling
can live together or be split by file — not by layer.

`*Service`, `*Controller`, `*Repository` suffixes are not idiomatic Go. Name
what the thing IS or DOES.

## Interfaces

Define interfaces at the point of use, not at the point of definition. Keep
them small — one or two methods. An interface with five methods is a class in
disguise.

Don't define interfaces for types you own. Define them when you need to swap
implementations: for testing, for multiple backends, for variation that is
concrete today.

## Errors

Errors are values. Return them explicitly. Wrap when you're adding context the
caller needs — not reflexively. Sentinel errors for expected failure states;
`fmt.Errorf` with `%w` for wrapping; custom types when callers need to inspect.

Never suppress errors silently. `_ = f()` on anything that returns an error is
a red flag.

## Concurrency

Goroutines and channels belong at I/O boundaries, not scattered through
business logic. Keep core logic synchronous and pure. Concurrency is a boundary
concern.

## Embedding

Struct embedding is composition, not inheritance — and it's fine for genuine
behavior reuse. Don't use it to build deep type hierarchies. If embedding is
adding methods to a type rather than composing behavior, question it.
