# dotclaude

Opinionated Claude Code plugin that shapes how Claude thinks about your product, writes your code, and reviews the result.

## Install

```bash
# Add the repo as a marketplace source, then install
/plugin marketplace add realDragonium/dotclaude
/plugin install dotclaude@realDragonium-dotclaude

# Local development
claude --plugin-dir ./path-to-dotclaude
```

## The Workflow

These skills form a pipeline. Use them in order, or pick what you need:

```
product-reasoning  →  feature-discovery  →  code-mode + test-writing  →  review
"should we build?"    "what, how, where?"    "build it right"             "did we build it right?"
```

## Skills

### `/dotclaude:product-reasoning`
Interrogates feature proposals before any planning starts. Forces assumption audits, jobs-to-be-done analysis, and honest comparison against "do nothing."

```
/dotclaude:product-reasoning Add real-time collaboration to the editor
```

### `/dotclaude:feature-discovery`
Plans a feature through codebase discovery and iterative dialogue. Runs a `feature-scout` subagent to map the codebase first, then works through decisions one at a time (high-confidence suggestions vs. explicit trade-off questions), then compiles a decision spec. The spec feeds directly into test-writing and code-mode.

```
/dotclaude:feature-discovery User authentication with email + OAuth
```

### `/dotclaude:code-mode`
**Persistent mode** — activate once and it shapes all code Claude writes for the session. Enforces naming (no junk suffixes), composition (no inheritance), functional patterns (pure core, side effects at boundaries), and simplicity (no premature abstraction).

```
/dotclaude:code-mode
```

### `/dotclaude:test-writing`
Writes behavior-focused tests. Tests describe what the code does, not how. Pairs with feature-discovery spec output. Supports TypeScript/JavaScript, Python, and Go idioms.

```
/dotclaude:test-writing Write tests for src/auth/
```

### `/dotclaude:review`
Code and architecture review in one pass. Finds real bugs, security issues, and philosophy violations at the code level; challenges simplicity, structure, naming, and design decisions at the architecture level. Language-specific checklists for Go, Python, and TypeScript. Never flags formatting.

```
/dotclaude:review Review the last commit
/dotclaude:review Review the architecture of src/
```

## Philosophy

All skills share principles defined in `skills/shared/philosophy.md`:

- **Simplicity as default** — complexity must earn its place with a concrete, current problem
- **Composition over inheritance** — never in our own code; only for language/framework mandates
- **Functional patterns** — pure functions for core logic, side effects at boundaries only
- **Data is transparent** — public fields, valid by construction, no logic-free getters/setters
- **Precise naming** — no junk suffixes; name what the thing IS or DOES
- **Core at the center** — framework and persistence are outer layers; core logic has no framework imports
- **Formatting is automated** — never flagged in review; that's prettier/black/gofmt's job

Language-specific extensions live alongside the core philosophy:

| File | Covers |
|------|--------|
| `philosophy-go.md` | Flat package structure, small interfaces, explicit errors |
| `philosophy-python.md` | Layered architecture conventions, dataclasses, type hints |
| `philosophy-typescript.md` | Frontend patterns, types, async, no barrel files |

Edit any philosophy file to evolve. All skills pick up changes automatically.

## Structure

```
dotclaude/
├── skills/
│   ├── shared/
│   │   ├── philosophy.md              ← Core principles (edit this)
│   │   ├── philosophy-go.md           ← Go-specific extensions
│   │   ├── philosophy-python.md       ← Python-specific extensions
│   │   └── philosophy-typescript.md   ← TypeScript/JS extensions
│   ├── product-reasoning/
│   │   └── SKILL.md                   ← Challenge feature proposals
│   ├── feature-discovery/
│   │   └── SKILL.md                   ← Scout codebase, plan iteratively
│   ├── code-mode/
│   │   └── SKILL.md                   ← Persistent coding philosophy mode
│   ├── test-writing/
│   │   └── SKILL.md                   ← Behavior-focused test generation
│   └── review/
│       ├── SKILL.md                   ← Code + architecture review
│       └── references/
│           ├── typescript-checklist.md
│           ├── python-checklist.md
│           └── go-checklist.md
├── agents/
│   ├── feature-scout.md               ← Read-only codebase explorer
│   ├── product-critic.md
│   └── reviewer.md
└── README.md
```

## Extending

**Add a language checklist:** create `skills/review/references/<lang>-checklist.md`, add detection in `skills/review/SKILL.md`.

**Add a language philosophy:** create `skills/shared/philosophy-<lang>.md` following the existing pattern. Add it to the detection list in `philosophy.md`.

**Add a skill:** create `skills/<name>/SKILL.md` with frontmatter. It auto-discovers.

**Evolve the philosophy:** edit `skills/shared/philosophy.md`. Everything adapts.

## Known Caveats

- **`context: fork`**: may be ignored in some Claude Code versions. Invoke agents by name as a fallback.
- **Triggering**: use `/dotclaude:<skill>` explicitly for guaranteed invocation.
- **No nesting**: subagents can't spawn sub-subagents.
