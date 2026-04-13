---
name: simplify-code
description: >-
  Guides behavior-preserving simplification and readability refactors using
  concrete patterns (structure, naming, redundancy). Use after tests pass and
  before declaring work done; during plan task step "Simplify / refactor"; or
  when the user asks to simplify, clarify, or reduce complexity without
  changing behavior.
---

# Simplify code

## Principles

1. **Preserve behavior exactly** — Output, side effects, and public contracts stay the same unless the user or an approved spec explicitly changes them. Prefer the smallest edit that improves clarity.
2. **Prefer clarity over cleverness** — Readable, boring code beats dense one-liners and clever abstractions.

## Workflow

1. **Green baseline** — Simplify only with a passing test suite (or the narrow tests for the touched code). If you lack tests, add minimal coverage first or decline risky refactors.
2. **Scan** — Use the tables below as **concrete signals** (not vague “code smells”).
3. **Change** — Apply one category of improvement at a time when possible; re-run tests after each meaningful chunk.
4. **If tests fail** — Fix or **rollback** the simplification; do not “fix forward” by changing behavior to match broken tests without explicit approval.

## Identify simplification opportunities

### Structural complexity

| Pattern | Signal | Simplification |
|--------|--------|----------------|
| Deep nesting (3+ levels) | Hard to follow control flow | Extract conditions into guard clauses or helper functions |
| Long functions (50+ lines) | Multiple responsibilities | Split into focused functions with descriptive names |
| Nested ternaries | Requires mental stack to parse | Replace with if/else chains, switch, or lookup objects |
| Boolean parameter flags | `doThing(true, false, true)` | Replace with options objects or separate functions |
| Repeated conditionals | Same `if` check in multiple places | Extract to a well-named predicate function |

### Naming and readability

| Pattern | Signal | Simplification |
|--------|--------|----------------|
| Generic names | `data`, `result`, `temp`, `val`, `item` | Rename to describe content: `userProfile`, `validationErrors` |
| Abbreviated names | `usr`, `cfg`, `btn`, `evt` | Use full words unless the abbreviation is universal or accepted in this project’s context (e.g. `id`, `url`, `api`) |
| Misleading names | `get*` that also mutates state | Rename to reflect actual behavior |

### Redundancy

| Pattern | Signal | Simplification |
|--------|--------|----------------|
| Duplicated logic | Same 5+ lines in multiple places | Extract to a shared function |
| Dead code | Unreachable branches, unused variables, commented-out blocks | Remove after confirming it is truly dead |
| Unnecessary abstractions | Wrapper that adds no value | Inline the wrapper; call the underlying API directly |
| Over-engineered patterns | Factory-for-a-factory; strategy with one strategy | Replace with the simple direct approach |
| Redundant type assertions | Casting to a type already inferred | Remove the assertion |

## DON'Ts

- Do not bundle simplification with feature changes or bug fixes in the same edit set when avoidable.
- Do not remove code because it “looks unused” without verifying (search callers, exports, build entry points).
- Do not change formatting-only rules when project standards forbid it; this skill is about structure and names, not team format wars.
