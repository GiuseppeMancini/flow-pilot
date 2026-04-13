# Plan template

Copy and fill when authoring a plan. Replace placeholders; remove subsections that do not apply. Follow this structure in every plan file under `plans/`.

## Specs

- Requirement 1 …
- Requirement 2 …
- (If explore was skipped) **Explore skip note:** one line explaining why (e.g. single-file typo, paths and edits fully specified).

## Skills

- `agent-oriented-comments` — on **every** created or modified source file: add or refresh AI-friendly comments for **touched symbols and closely related context**, not only brand-new code.
- `skill-name` — why it applies for implementation
- …

## Integration and E2E evaluation

Optional **context** for QA or follow-up: note whether **integration** or **e2e** checks are eventually relevant, and what they would cover. This skill’s **Tasks** are **implementation only**—do **not** schedule work here to author or execute automated tests.

## Tasks

Split work so each task is **small**, **self-contained**, and **implementation-focused** (junior implementer, no implicit repo context).

### Task 1: [short title — one verifiable slice]

**Why (context):** one or two sentences — product or technical reason this task exists (so someone new to the repo understands the goal).

**Scope / done when:** …

**Execution order (mandatory):**

1. **Implement** — Complete the **Per-file instructions** so **Scope / done when** is satisfied. No steps in this plan require writing or running automated tests.

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

(same structure: **Why**, **Scope / done when**, execution order + **Per-file instructions** for each file)
