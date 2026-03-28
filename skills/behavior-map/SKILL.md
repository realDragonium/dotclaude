---
name: behavior-map
description: Map out all behavior for a feature before writing code. Use when someone says "let's plan this feature", "map out the behavior", "what should this do", "spec this out", "before we build", or when a feature needs comprehensive behavior definition before implementation. Use AFTER product-reasoning (should we build?) and BEFORE code-mode (building it).
---

Read the shared philosophy: `${CLAUDE_PLUGIN_ROOT}/skills/shared/philosophy.md`

You are mapping out the complete behavior of a feature. The goal is a spec
that is precise enough to implement from AND to write tests from. No code yet —
just behavior.

## Feature
$ARGUMENTS

ultrathink

## 👤 ACTORS & TRIGGERS
Who/what initiates this behavior, and how?

| Actor | Trigger | Entry Point |
|-------|---------|-------------|
| | | |

## 🔄 STATE MAP
Every distinct state this feature can be in:

| State | How you get here | What the user sees/gets | Valid transitions |
|-------|-----------------|------------------------|-------------------|
| Empty/Initial | | | |
| Loading | | | |
| [Happy path] | | | |
| [Partial] | | | |
| Error | | | |

For each state: what data exists? What's visible? What actions are available?

## 📋 BEHAVIORS (the core of this spec)
Numbered, concrete, testable behaviors. Each one follows:
**"Given [state/context], when [action], then [outcome]"**

Group by area:

### Happy path
1. Given ..., when ..., then ...
2. Given ..., when ..., then ...

### Input handling
- What inputs are valid?
- What happens with empty input?
- What happens with malformed input?
- What happens with boundary values (max length, zero, negative)?
- What happens with concurrent identical input?

### Error behaviors
- For each external dependency (API, DB, file system): what happens when it fails?
- For each validation: what does the user see on failure?
- What's recoverable vs. terminal?
- Does the user get enough info to fix the problem?

### Edge cases
- Concurrent access — two users/processes at the same time
- Stale data — what if underlying data changed since the user loaded it?
- Partial completion — what if the process fails halfway?
- Undo/rollback — can the user reverse this? Should they be able to?
- Permissions — what if the user shouldn't have access?
- Scale — does behavior change with 0, 1, 100, 10000 items?

### Interactions with existing features
- What existing feature does this touch or overlap with?
- Does it change the behavior of anything already built?
- Are there shared data models or state?

## 🚫 EXPLICITLY OUT OF SCOPE
List things this feature does NOT do. Being explicit prevents scope creep
and makes test boundaries clear.

1. This feature does NOT ...
2. This feature does NOT ...

## 📊 DATA SHAPE
What data does this feature need, produce, or transform?

**Input:** what comes in, from where, in what shape
**Stored:** what persists, where, for how long
**Output:** what goes out, to where, in what shape
**Derived:** what's computed, from what, when

Keep it simple. If a plain object works, don't propose a schema.
If a single table works, don't propose three.

## ❓ OPEN QUESTIONS
Things that must be decided before implementation.
Format: `Q1: [Question] — blocks [which behaviors above]`

## ✅ ACCEPTANCE CRITERIA
The minimum set of behaviors from above that must work for this feature
to be considered done. Reference by behavior number.

**Must have:** behaviors #...
**Should have:** behaviors #...
**Nice to have:** behaviors #...
