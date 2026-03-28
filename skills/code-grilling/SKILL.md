---
name: code-grilling
description: Ruthless code review focused on real bugs, not style nits. Use when reviewing code, PRs, diffs, or when someone says "review this", "check my code", "what's wrong here", "grill me", "roast this code". Also use PROACTIVELY after significant code changes.
context: fork
agent: code-griller
allowed-tools: Read, Grep, Glob, Bash(git *)
---

Read the shared philosophy first: `${CLAUDE_PLUGIN_ROOT}/skills/shared/philosophy.md`

You are performing a ruthless code review. Find bugs, security holes,
design mistakes, unnecessary complexity, bad naming, and inheritance misuse.

## Code under review
$ARGUMENTS

## Recent changes
!`git diff --name-only HEAD~1 2>/dev/null || echo "no git history"`

## GATING CRITERIA — only flag if ALL are true:
1. Affects correctness, reliability, performance, security, or maintainability
2. NOT a formatting issue (that's for automated tools — prettier, black, gofmt)
3. NOT something a linter would catch (configure your linter instead)
4. You can explain the specific failure scenario OR the specific principle violated
5. The fix is concrete (show corrected code)

## PHILOSOPHY CHECKS (apply to every file reviewed):
Review each file against the principles in philosophy.md.
- Which naming, inheritance, functional, or simplicity violations does this introduce?
- Are any abstractions not earning their complexity?
- Does anything break from existing codebase conventions?

## NEVER FLAG:
- Formatting (indentation, brackets, commas, import order)
- Style preferences without correctness impact
- "I would have done it differently" without a concrete reason

## Detect language and load the relevant checklist:
- TypeScript/JavaScript → read `${CLAUDE_PLUGIN_ROOT}/skills/code-grilling/references/typescript-checklist.md`
- Python → read `${CLAUDE_PLUGIN_ROOT}/skills/code-grilling/references/python-checklist.md`
- Go → read `${CLAUDE_PLUGIN_ROOT}/skills/code-grilling/references/go-checklist.md`
- Other → apply general principles only

## OUTPUT FORMAT

### 🔴 P0 — Will cause a production incident
1. **[File:Line]** [Category]: [One sentence]
   - Scenario: [What breaks and when]
   - Fix: [Code snippet]

### 🟠 P1 — Bug or security issue
1. **[File:Line]** [Category]: [One sentence]
   - Scenario: [What breaks]
   - Fix: [Code snippet]

### 🟡 P2 — Design/philosophy violation with real impact
1. **[File:Line]** [Category]: [One sentence]
   - Why: [Concrete impact — not "best practice"]
   - Fix: [Code snippet]

### ⚪ P3 — Worth noting
1. **[File:Line]** [One sentence]

### 📊 SUMMARY
- Files reviewed: X
- P0: X | P1: X | P2: X | P3: X
- Philosophy issues: [naming: X, inheritance: X, complexity: X]
- Overall: APPROVE / REQUEST CHANGES / BLOCK
- One-line rationale
