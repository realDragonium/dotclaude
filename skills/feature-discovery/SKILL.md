---
name: feature-discovery
description: Plan a feature through codebase discovery and iterative dialogue before writing any code. Runs a scout agent to map the codebase, works through decisions one at a time, then compiles a decision spec. Use AFTER product-reasoning (should we build it?) and BEFORE code-mode (building it).
---

Read the shared philosophy: `${CLAUDE_PLUGIN_ROOT}/skills/shared/philosophy.md`

You are running feature discovery for:

## Feature
$ARGUMENTS

ultrathink

---

## Phase 1 — Discovery

Use the Agent tool to run the feature-scout subagent.
Load its instructions from: `${CLAUDE_PLUGIN_ROOT}/agents/feature-scout.md`
Pass the feature description above as the agent's task.

Wait for the full report before continuing. Store it — you will reference it throughout.

---

## Phase 2 — Collaborative Dialogue

Present the discovery findings to the user. Keep the summary tight:
- What this feature touches (key components only)
- What patterns already exist that are relevant
- Gaps or inconsistencies that will affect decisions

Don't dump the full report. Highlight what matters for decision-making.
If the user asks to see the full report, show it.

Then work through decisions iteratively:

**High confidence** — obvious from codebase patterns, or simple with clear precedent:
Suggest and ask for confirmation. One sentence on why.
*"The codebase validates at the handler level using X pattern. I'd do the same here. Agree?"*

**Low confidence** — ambiguous, trade-offs, or no clear precedent:
Present 2–3 options with concrete trade-offs. Ask the user to decide.
*"Should this be synchronous or async? Sync is simpler but blocks the response. Async would need a queue we don't have yet."*

**Impact on existing behaviour:**
Flag explicitly before proceeding.
*"This would change how Y works. Is that intended?"*

Rules:
- Ask one to three questions at a time. Never dump a list.
- Wait for answers before the next decision point.
- Record every decision as you go: what was asked, what was decided, how confident you were.
- Apply the simplicity principles from philosophy.md to each proposed approach. Flag anything not earning its complexity.

---

## Phase 3 — Decision Spec

Once all decisions are resolved (or the user says they're done), compile:

---

### Decision Spec: [Feature Name]

**What it does**
Plain description from the user's intent and confirmed decisions.

**Components affected**
From the discovery report. Each entry: component + what changes.

**Decisions**
| Decision | What was decided | Source |
|----------|-----------------|--------|
| [topic] | [decision] | suggested+confirmed / explicitly stated |

**Simplicity check**
Is the proposed approach the fewest moving parts that solves the problem?
Flag anything not earning its complexity.

**Open items**
Unresolved questions, if any. If none, say so.

**Acceptance criteria**
Derived from confirmed decisions. Each criterion must be falsifiable.

---

Every line in this spec traces to something the user stated or confirmed. Nothing invented.
This spec is the input to test-writing and code-mode — not a plan, a decision record.
