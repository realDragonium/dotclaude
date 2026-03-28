---
name: review
description: Full review — code and architecture in one pass. Use when reviewing code, PRs, diffs, architecture decisions, or when someone says "review this", "check my code", "what's wrong here", "review the architecture". Also use PROACTIVELY after significant code changes.
context: fork
agent: reviewer
allowed-tools: Read, Grep, Glob, Bash(git *)
---

Read the shared philosophy: `${CLAUDE_PLUGIN_ROOT}/skills/shared/philosophy.md`

## Under review
$ARGUMENTS

## Context
- Recent changes: !`git diff --name-only HEAD~1 2>/dev/null || echo "no git history"`
- Project structure: !`find . -maxdepth 3 -type f \( -name "*.ts" -o -name "*.py" -o -name "*.go" \) | head -50`

ultrathink

---

## GATING CRITERIA — only flag if ALL are true:
1. Affects correctness, reliability, performance, security, or maintainability
2. NOT a formatting issue (that's for automated tools — prettier, black, gofmt)
3. NOT something a linter would catch
4. You can explain the specific failure scenario OR the specific principle violated
5. The fix is concrete

## NEVER FLAG:
- Formatting (indentation, brackets, commas, import order)
- Style preferences without correctness impact
- "I would have done it differently" without a concrete reason

---

## CODE REVIEW

Detect language and load the relevant checklist:
- TypeScript/JavaScript → `${CLAUDE_PLUGIN_ROOT}/skills/review/references/typescript-checklist.md`
- Python → `${CLAUDE_PLUGIN_ROOT}/skills/review/references/python-checklist.md`
- Go → `${CLAUDE_PLUGIN_ROOT}/skills/review/references/go-checklist.md`
- Other → apply general principles only

Read the actual code, not just the diff. Trace data flow end-to-end.
Check error handling at every boundary. Trace user input to every usage point.

---

## ARCHITECTURE REVIEW

### 🧹 SIMPLICITY AUDIT
Evaluate every component, service, layer, and abstraction against the simplicity principles in philosophy.md.
- What concrete problem does each thing solve?
- What happens if you remove it entirely?
- Is it earning its complexity cost?

### 🏷️ NAMING
Evaluate every name against the naming principles in philosophy.md.
- Which names describe a category rather than what the thing IS or DOES?
- Which would be more precise without their suffix?
- Where does naming diverge from existing codebase conventions?

### 🚫 INHERITANCE
Evaluate against the composition principles in philosophy.md.
- List every use of inheritance
- For each: what interface or composition pattern would replace it?
- Is this one of the rare justified cases?

### 🏗️ STRUCTURE
1. **Component boundaries** — Are responsibilities separated? Where will they blur?
2. **Dependency direction** — Does the graph point the right way? Cycles?
3. **Data flow** — Trace the critical path. Where does data transform? Where could it get lost?
4. **Coupling** — What changes if you swap out each component?

### 💥 FAILURE MODES
For each component: Slow (>5s)? Down? Returns garbage? Blast radius? Detection? Recovery?

### 🤔 "WHY NOT SIMPLER?"
For each major decision:
| Decision | Simpler Alternative | What You Lose | What You Gain |
|----------|-------------------|---------------|---------------|

---

## OUTPUT FORMAT

### 🔴 P0 — Will cause a production incident
1. **[File:Line or Component]** [Category]: [One sentence]
   - Scenario: [What breaks and when]
   - Fix: [Code or approach]

### 🟠 P1 — Bug or security issue
1. **[File:Line or Component]** [Category]: [One sentence]
   - Scenario: [What breaks]
   - Fix: [Code or approach]

### 🟡 P2 — Design/philosophy violation with real impact
1. **[File:Line or Component]** [Category]: [One sentence]
   - Why: [Concrete impact — not "best practice"]
   - Fix: [Code or approach]

### ⚪ P3 — Worth noting
1. **[File:Line or Component]** [One sentence]

### 📊 VERDICT
- Reviewed: [files / components]
- P0: X | P1: X | P2: X | P3: X
- Overall: APPROVE / REQUEST CHANGES / BLOCK
- One-line rationale
