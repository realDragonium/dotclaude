---
name: arch-review
description: Challenge architecture decisions. Use when discussing system design, component boundaries, data models, API contracts, abstractions, or when someone proposes a technical approach. Also use when you see new services, layers, or abstractions being added. PROACTIVELY challenge architectural complexity.
context: fork
agent: arch-challenger
---

Read the shared philosophy first: `${CLAUDE_PLUGIN_ROOT}/skills/shared/philosophy.md`

You are reviewing an architecture decision for unnecessary complexity,
poor abstractions, naming problems, inheritance misuse, and missed
simpler alternatives.

## Context
- Recent changes: !`git diff --stat HEAD~5 2>/dev/null || echo "no recent changes"`
- Project structure: !`find . -maxdepth 3 -type f \( -name "*.ts" -o -name "*.py" -o -name "*.go" \) | head -50`

## Architecture under review
$ARGUMENTS

ultrathink

## 🧹 SIMPLICITY AUDIT (do this FIRST)
Evaluate every component, service, layer, and abstraction against the simplicity principles in philosophy.md.
- What concrete problem does each thing solve?
- What happens if you remove it entirely?
- Is it earning its complexity cost?

## 🏷️ NAMING REVIEW
Evaluate every name against the naming principles in philosophy.md.
- Which names describe a category rather than what the thing IS or DOES?
- Which would be more precise without their suffix?
- Where does naming diverge from existing codebase conventions?

## 🚫 INHERITANCE CHECK
Evaluate against the composition principles in philosophy.md.
- List every use of inheritance in the proposal
- For each: what interface or composition pattern would replace it?
- Is this one of the rare justified cases?

## 🔀 FUNCTIONAL ASSESSMENT
Evaluate against the functional principles in philosophy.md.
- Where is state being mutated that could be transformed instead?
- Are I/O boundaries clearly separated from business logic?
- Are there functions doing more than one thing?

## 🏗️ STRUCTURAL REVIEW
1. **Component boundaries** — Are responsibilities separated? Where will they blur?
2. **Dependency direction** — Does the graph point the right way? Cycles?
3. **Data flow** — Trace the critical path. Where does data transform? Where could it get lost?
4. **Coupling** — What changes if you swap out each component?

## 💥 FAILURE MODES
For each component:
- Slow (>5s)? Down? Returns garbage?
- Blast radius?
- Detection? Recovery?

## 🤔 "WHY NOT SIMPLER?"
For each major decision, propose a simpler alternative:
| Decision | Simpler Alternative | What You Lose | What You Gain |
|----------|-------------------|---------------|---------------|

## 📈 SCALING (only if relevant)
- Current bottleneck at 1x
- First break at 10x
- Cost trajectory: linear, quadratic, exponential?

## ❓ BLOCKING QUESTIONS
5-8 specific questions the architect must answer before proceeding.

## 📊 VERDICT
Rate each area: 🟢 Sound / 🟡 Over-engineered / 🔴 Blocking issue
Single-paragraph overall assessment.
