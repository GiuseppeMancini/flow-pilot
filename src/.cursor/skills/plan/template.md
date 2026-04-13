# Plan draft template

Copy and fill for the **internal draft** (before `plan-reviewer`). Replace placeholders; remove sections that do not apply.

## Specs

- Requirement 1 …
- Requirement 2 …
- (If explore was skipped) **Explore skip note:** one line explaining why (e.g. single-file typo, paths and edits fully specified).

## Skills

- `skill-name` — why it applies for implementation
- `simplify-code` — when tasks include **Simplify / refactor** on code (implementers read `.cursor/skills/simplify-code/SKILL.md` at step 4)
- …

## Test scenarios

| ID | Requirement | Scenario | Automatable? | Test location or manual steps |
|----|-------------|----------|--------------|--------------------------------|
| T1 | … | Given … when … then … | yes | `path/to/test.file` |
| T2 | … | … | no | Step 1 … Step 2 … |

## Tasks

Split work so each task is **small**, **self-contained**, and **verifiable on its own** (junior implementer, no implicit repo context).

### Task 1: [short title — one verifiable slice]

**Scope / done when:** …

**Execution order (mandatory):**

1. **Write tests** — …
2. **Write code** — …
3. **Run tests** — command or scope; must be green before step 4.
4. **Simplify / refactor** — Read and apply `.cursor/skills/simplify-code/SKILL.md` (behavior-preserving clarity: structure, naming, redundancy). Re-run tests after each meaningful change.
5. **Run tests again** — if red: fix or targeted rollback → re-run until green (repeat as needed).

**Per-file instructions (mandatory — list every create / modify / delete):**

#### `path/to/file-a.ext` — **create**

- **What:** behavior, API, data the file must provide.
- **How:** ordered steps, symbols to add, insertion points, or short snippet.

#### `path/to/file-b.ext` — **modify**

- **What:** change in behavior, API, or data.
- **How:** ordered steps, functions/types to touch, call sites.

#### `path/to/file-c.ext` — **delete**

- **What:** responsibilities being removed.
- **How:** what replaces them (file path or pattern); **which call sites** to update and how.

---

### Task 2: …

(same structure: execution order + **Per-file instructions** for each file)

## DoD

- All tasks above complete.
- Lint clean (no format).
- Full test suite green.
- Implementation approved by `code-reviewer` (max 3 cycles).
