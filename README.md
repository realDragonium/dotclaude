# dotclaude

Opinionated Claude Code plugin that shapes how Claude thinks about your product, writes your code, and reviews the result.

## Install

```bash
# From GitHub
claude plugin install github:your-username/dotclaude

# Local development
claude --plugin-dir ./path-to-dotclaude
```

## The Workflow

These skills form a pipeline. Use them in order, or pick what you need:

```
product-reasoning  →  behavior-map  →  code-mode + test-writing  →  code-grilling + arch-review
"should we build?"    "what should     "build it right"             "did we build it right?"
                       it do?"
```

## Skills

### `/dotclaude:product-reasoning`
Interrogates feature proposals. Forces assumption audits, jobs-to-be-done analysis, and honest comparison against "do nothing."

```
/dotclaude:product-reasoning Add real-time collaboration to the editor
```

### `/dotclaude:behavior-map`
Maps out all behavior for a feature before code is written. Produces a testable spec: states, transitions, edge cases, error behaviors, data shapes, and acceptance criteria. The output feeds directly into test-writing.

```
/dotclaude:behavior-map User authentication with email + OAuth
```

### `/dotclaude:code-mode`
**Persistent mode** — activate once and it shapes all code Claude writes for the session. Enforces naming (no junk suffixes), composition (no inheritance), functional patterns (pure by default), and simplicity (no premature abstraction).

```
/dotclaude:code-mode
```

### `/dotclaude:test-writing`
Writes behavior-focused tests. Tests describe what the code does, not how. Pairs with behavior-map output. Supports TS/JS, Python, and Go idioms.

```
/dotclaude:test-writing Write tests for src/auth/
```

### `/dotclaude:arch-review`
Challenges architecture decisions. Hunts unnecessary complexity, bad naming, inheritance, and always asks "why not simpler?"

```
/dotclaude:arch-review Review the current architecture of src/
```

### `/dotclaude:code-grilling`
Ruthless code review with language-specific checklists. Finds real bugs, enforces philosophy. Never flags formatting.

```
/dotclaude:code-grilling Review the last commit
```

## Philosophy

All skills enforce shared principles defined in `skills/shared/philosophy.md`:

- **Simplicity as default** — complexity must earn its place
- **Composition over inheritance** — every `extends` gets flagged
- **Functional patterns** — pure functions, immutable data, I/O at boundaries
- **Precise naming** — no useless suffixes (Service, Manager, Handler, Utils)
- **Formatting is automated** — never flagged, that's prettier/black/gofmt's job
- **Behavior-focused testing** — test what it does, not how it works

Edit `philosophy.md` to evolve. All skills pick up changes automatically.

## Structure

```
dotclaude/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── shared/
│   │   └── philosophy.md         ← Your principles (edit this)
│   ├── product-reasoning/
│   │   └── SKILL.md              ← Challenge feature proposals
│   ├── behavior-map/
│   │   └── SKILL.md              ← Map feature behavior before coding
│   ├── code-mode/
│   │   └── SKILL.md              ← Persistent coding philosophy mode
│   ├── test-writing/
│   │   └── SKILL.md              ← Behavior-focused test generation
│   ├── arch-review/
│   │   └── SKILL.md              ← Challenge architecture decisions
│   └── code-grilling/
│       ├── SKILL.md              ← Ruthless code review
│       └── references/
│           ├── typescript-checklist.md
│           ├── python-checklist.md
│           └── go-checklist.md
├── agents/
│   ├── product-critic.md
│   ├── arch-challenger.md
│   └── code-griller.md
└── README.md
```

## Extending

**Add a language checklist:** create `skills/code-grilling/references/<lang>-checklist.md`, add detection in the SKILL.md.

**Add a skill:** create `skills/<name>/SKILL.md` with frontmatter. It auto-discovers.

**Evolve the philosophy:** edit `skills/shared/philosophy.md`. Everything adapts.

## Known Caveats

- **`context: fork` bug** (#17283): may be ignored in some versions. Invoke agents by name as fallback.
- **Triggering**: use `/dotclaude:<skill>` explicitly for guaranteed invocation.
- **No nesting**: subagents can't spawn sub-subagents.

## License

MIT
