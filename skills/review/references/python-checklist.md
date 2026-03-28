# Python Review Checklist

## Philosophy Checks (Python specific)

### Naming
- `*_utils.py`, `*_helpers.py`, `common.py` → junk drawers, break them up
- Classes named `*Service`, `*Manager`, `*Handler` → what does it do? Name that
- `Base*` or `Abstract*` classes → almost always unnecessary (see inheritance)
- `*Mixin` → composition smell. Can this be a function or protocol?

### Inheritance
- `class X(Y):` → flag every one. Propose protocol + composition
- Multiple inheritance → almost always wrong in Python. Use protocols
- `ABC` / `@abstractmethod` → does this need to be a class at all?
  Protocols (structural subtyping) are usually better
- `super().__init__()` chains → complexity signal. Flatten.
- Mixins → "horizontal inheritance" is still inheritance

### Functional Patterns
- List comprehensions over `for` loops with `.append()` where clearer
- Generator expressions for large datasets over materializing lists
- `functools.partial`, `operator` module over lambdas-wrapping-functions
- Mutable class attributes → should be instance attributes or frozen dataclasses
- `@staticmethod` → this is just a function, put it at module level
- `@classmethod` for constructors only, not as a way to avoid passing `self`

## Classic Python Bugs
- Mutable default arguments (`def f(x=[])`) → almost always a bug
- Late binding closures in loops: `lambda: i` captures variable, not value
- Bare `except:` or `except Exception:` without re-raise → swallowing errors
- `is` vs `==` for value comparison (only `is` for None/sentinel)
- String concatenation in loops → `''.join()` or f-strings

## Type Safety
- Type hints present and accurate?
- `Optional[X]` → is None handled at every call site?
- `Any` → same rules as TS `any`, require justification
- Dict access without `.get()` → KeyError waiting to happen
- Union types → all variants handled?

## Concurrency & I/O
- File handles without context managers (`with open(...)`)
- DB connections not properly closed/pooled
- Threading without locks on shared state
- Mixing async with synchronous blocking calls

## Data
- Float `==` comparison → use `math.isclose()` or Decimal
- Encoding issues → `str` vs `bytes`, explicit encoding on I/O
- `pickle`/`eval`/`exec` on untrusted data → arbitrary code execution
