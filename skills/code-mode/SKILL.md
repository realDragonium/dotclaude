---
name: code-mode
description: Persistent coding mode that shapes all code generation to follow the project philosophy. Activate at the start of a session to enforce simplicity, composition, functional patterns, and precise naming in all code Claude writes. Use when starting a coding session, or when someone says "let's code", "start building", "implement this".
mode: true
---

Read and internalize: `${CLAUDE_PLUGIN_ROOT}/skills/shared/philosophy.md`

You are now in opinionated code writing mode. These rules apply to ALL code
you write for the rest of this session. They are not suggestions.

## NAMING — every name you write

- Name what it IS or DOES. Not its category.
- No suffix junk: never generate `*Service`, `*Manager`, `*Handler`, `*Helper`,
  `*Utils`, `*Base`, `*Abstract`, `*Factory`, `*Impl`, `*Wrapper`, `*Common`.
  Name the actual thing.
- If you catch yourself writing `UserService`, stop. What does it do?
  `Users`? `authenticateUser`? `createAccount`? Name THAT.
- Match existing codebase conventions. Consistency > your preference.
- File names = what's in them. `utils.ts` is a junk drawer. Don't create one.

## STRUCTURE — every module/class/function

- **No inheritance.** Use interfaces, composition, or plain functions.
  The only exceptions: Error/Exception subclasses, unavoidable framework requirements.
- **Functions do one thing.** If the name needs "and", it's two functions.
- **Pure by default.** Side effects (I/O, DB, network, logging) belong at
  boundaries, not in business logic. Keep the core pure.
- **Prefer immutability.** `const` over `let`. New objects over mutation.
  Transform, don't mutate.
- **No premature abstraction.** Write the concrete thing first. Abstract
  only when you have 3+ concrete cases that genuinely share behavior.
  "We might need this" is not a reason.

## ARCHITECTURE — every structural decision

- Fewest moving parts that solve the problem.
- Don't introduce a layer/service/abstraction without a concrete reason.
- If a plain function works, don't wrap it in a class.
- If a direct call works, don't add a message queue.
- If a monolith module works, don't split into services.
- Every boundary you create has a cost. Earn it.

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
