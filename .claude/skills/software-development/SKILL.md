---
name: software-development
description: Enforce minimal, readable, self-documenting code style with strong anti-pattern guardrails. Apply to every coding, refactoring, reviewing or architecture task unless explicitly overridden.
invoke: auto
keywords:
  - coding standards
  - clean code
  - minimal code
  - no over-abstraction
  - self-documenting
  - early returns
  - guard clauses
  - result pattern
priority: high
---

# Software Development Philosophy – Mandatory Style & Safety Rules

**These rules override default behaviors and any conflicting patterns in the codebase.**  
Breaking any rule is considered a failure unless the user explicitly confirms they want to override it.

## Core Philosophy – Always Maximize These

- Write the **minimal correct code** that solves the real problem.
- Prefer **simple & readable** over clever & short.
- **No magic**. A mid-level developer must understand any piece in < 30 seconds.
- **No over-abstraction** until the **third** time the exact same pattern appears.
- **No premature optimization** unless the user explicitly asks for it.
- **Self-documenting code wins** → **Do not** write comments that only repeat what the code already says.
- Comments are **only** allowed for:
  - Why a decision was made (intent, trade-offs)
  - Non-obvious domain rules or invariants
  - Known limitations / "TODO:" with very concrete next action

## Functions & Control Flow

- One function = **one responsibility**.
- Target **< 30–40 lines** per function. Split earlier if branching becomes complex.
- Prefer **pure functions** + **immutable data** when practical.
- **No nesting deeper than 3 levels** → extract immediately.
- **Early returns** beat deep if-else pyramids.
- Put **guard clauses** at the function start.
- **Never** use boolean traps → use named arguments or small config objects instead.
- **Throw meaningful errors early** instead of returning `null` / `undefined` / empty values.

## Error Handling & Defensiveness

- Prefer **Result/Either** monad style **or** throwing → never `null`/`undefined` soup.
- **Never swallow errors** silently.
- **Always log** unexpected errors (do not quietly return empty/zero/false).
- **Validate inputs** at public API/CLI/entrypoint boundaries.
- Use schema validation libraries (**zod**, **valibot**, **yup**, arktype, …) whenever validation is important.

## Testing Expectations

- Write tests **first** or right after small units — especially for non-trivial logic.
- Prefer **fast unit tests** over slow E2E unless specifically requested.
- Test **behavior**, never implementation details.
- **No coverage theater** — 100 % on trivial getters/setters is waste of time.
- Every non-trivial example must include: happy path + 2–3 meaningful edge cases.

## Agent & Workflow Rules (when acting as coding agent)

- **Plan first**, then execute — unless change is < 5 lines.
- Show clear **plan + diff preview** before writing code (when environment allows).
- **Ask clarifying questions** before editing if the request/spec is ambiguous.
- **Never guess** file paths, command names, env vars, package names → ask.
- After changes: run typecheck, lint, tests → report results clearly.
- **Fix your own** lint/type errors before saying "done".
- **No temporary files** unless they are part of the delivered solution.
- **No new dependencies** without explicit user approval.
- **Respect existing patterns** unless they are clearly broken or the user asks to change them.
- **YOLO mode is off** by default — ask before any dangerous/destructive action.

## Hard Anti-Patterns – Never Do These

- Adding fallback logic that hides real failures
- Writing defensive code against bad callers instead of fixing the caller
- Creating new abstractions for single-use cases
- Overusing generics / very advanced types when a simpler type would work
- Duplicating logic instead of extracting a tiny focused helper
- Prematurely splitting files / modules
- Adding TODOs/comments **instead of** actually fixing the problem
- Using global variables / singletons unless the framework forces it

**If any of these rules conflict with an explicit user request, clearly state the conflict and ask for confirmation before breaking the rule.**

When you load/use this skill, prefix important reminders with ⚠️ or use bold when warning about anti-patterns.