---
name: agent-oriented-comments
description: >-
  How to write and maintain code comments for the next agent: purpose, contracts,
  cross-links, and cleanup. Applies on every edit to source files—new code,
  refactors, bugfixes, renames, and small fixes—not only greenfield additions:
  improve comments for touched symbols and immediate context. Use during
  exploration, whenever a file is touched, or when the user asks for
  agent-oriented comments, handoff notes in code, “comments for AI”, or to fix
  stale or noisy comments. Read this skill before bulk-commenting or
  refactoring documentation in source.
---

# Agent-oriented comments

These comments exist so **another agent** (or future you) can explore and edit the codebase **without re-deriving intent** from implementation alone. They are not a substitute for tests or specs; they reduce ramp-up cost.

## Principle

Every modification to a source file is a chance to **add, tighten, or remove** agent-oriented comments on **what you changed** and the **minimum surrounding context** needed to understand it (contracts, call sites you relied on, navigation). This applies to **existing** code you edit, not only new symbols or new lines. Stay **proportional**: do not treat an unrelated small edit as a mandate to comment an entire untouched file—focus on touched symbols, their immediate neighbors, and what the next reader must not get wrong.

## When to write or update

- **Exploration**: when you read a module to understand it, add a short note if something important is non-obvious and missing from comments.
- **Every file you change**: before finishing, scan **all** touched symbols—including functions, types, or blocks you modified in place, not just additions—update comments that became wrong, add what a newcomer would need, delete noise.
- **Behavioral change**: if contracts, errors, side effects, or call patterns change, the comment must change in the same edit.

## What to include

- **Purpose** of non-trivial types, public APIs, and non-obvious functions (one or two sentences; not a novel).
- **Contracts and edge cases**: preconditions, invariants, empty/null handling, ordering assumptions.
- **Side effects**: I/O, mutations, global state, caching, background work.
- **Errors**: what failures are expected, what is propagated vs swallowed, retry semantics if non-obvious.
- **Navigation**: pointers to important callers, callees, config keys, related files, or ADRs—**concrete** symbols or paths, not vague “see elsewhere”.

Prefer comments **above** the symbol they describe (or inline only when the line truly needs a local explanation).

## What to avoid

- Restating the name of the function or obvious control flow (“this loops over items”).
- Duplicating what types already express, unless the type is misleading or incomplete.
- Commented-out dead code—remove it or restore it; do not leave it “for history”.
- Stale blocks that contradict the implementation—**worse than no comment**.

## Style

- Match the **file’s language** and the project’s prevailing comment style (`//` vs `/* */`, docstrings, etc.).
- Stay **concise**; use bullets for multi-part contracts when it improves scanability.
- If the team uses a doc format (e.g. JSDoc, Rust `///`), use it for public APIs where it is already the norm.

## Quick examples (shape, not copy-paste)

**Good — saves exploration**

- “Called from `enqueueJob` only; must not block—worker thread picks up from queue.”
- “Returns `None` when feature flag `billing.new_flow` is off; callers fall back to legacy path in `legacy_checkout.py`.”

**Bad — noise**

- “Gets the user.” on `get_user(id)` with no extra contract.
- A five-paragraph essay repeating the plan from chat—put that in the plan or ADR, not inline.

## Check before you leave a file

- [ ] Comments next to changed logic still true?
- [ ] Pre-existing symbols you edited: commented where non-obvious (not only new code)?
- [ ] Any redundant or misleading comment removed?
- [ ] Would a stranger know **why** this exists and **what not to break**?
