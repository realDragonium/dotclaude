---
name: code-mode
description: Persistent coding mode that shapes all code generation to follow the project philosophy. Activate at the start of a session to enforce simplicity, composition, functional patterns, and precise naming in all code Claude writes. Use when starting a coding session, or when someone says "let's code", "start building", "implement this".
mode: true
---

Read and internalize: `${CLAUDE_PLUGIN_ROOT}/skills/shared/philosophy.md`

You are now in opinionated code writing mode. These rules apply to ALL code
you write for the rest of this session. They are not suggestions.

## NAMING — every name you write
Follow the naming principles in philosophy.md. When in doubt: name what it IS or DOES, not its category. Match existing codebase conventions — consistency beats your preference.

## STRUCTURE — every module/class/function
Follow the composition, functional, and simplicity principles in philosophy.md. Hard constraints: no inheritance except Error subclasses and unavoidable framework requirements; functions do one thing; side effects at boundaries only.

## ARCHITECTURE — every structural decision
Follow the architecture principles in philosophy.md. Default: fewest moving parts. Every layer, service, and abstraction has a cost — earn it with a concrete, current problem.

## WHAT YOU DON'T DO

- Don't add "helpful" abstractions the user didn't ask for
- Don't wrap things in classes when functions work
- Don't create `types.ts`, `constants.ts`, `utils.ts` junk drawers
- Don't add comments explaining obvious code — write clear code instead
- Don't over-engineer error handling for impossible states
- Don't add configuration/options for hypothetical future needs

## WHEN YOU'RE UNSURE

Ask: "What's the simplest thing that works here?"
Then write that. Add complexity only when the user explicitly needs it
and can name the concrete problem the complexity solves.
