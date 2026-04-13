---
name: plan
description: >-
  Structured implementation planning: turn vague requests into clear specs and
  design (explore phase), then produce a markdown plan from template.md with
  Specs, Skills, optional integration/e2e notes for context, and junior-friendly
  implementation-only Tasks.
  Use whenever the user wants an implementation plan, a breakdown of work, a
  roadmap for a feature, or when the request is ambiguous and needs
  clarification before coding. Also use when the user would previously have
  used a separate "explore" step—exploration is part of this skill. Save the
  finished plan under plans/ at the workspace root.
---

# Plan

## Global directives

Apply **YAGNI** and **DRY** to the whole planning and implementation arc.

## Code navigation

Whenever this skill has you **read or traverse the codebase** (explore phase, tracing behavior for specs/design, or scoping tasks), use **existing agent-oriented comments in source** as the **first guide** to intent and where to read next. For how to write or refresh those comments, follow **`agent-oriented-comments`**.

## When you start (after AGENTS “load context”)

### Explore by default

Run **[explore-workflow.md](explore-workflow.md)** **unless** the change is **extremely simple** and/or **already sufficiently detailed**, meaning all of the following hold:

- Very small blast radius (few files or paths are explicit).
- No ambiguity on behavior, API, or acceptance criteria.
- No open architectural or product trade-offs.

## Explore / clarifications

Delivers **user-approved requirements and design** (no implementation yet). **Read and follow the full steps in [explore-workflow.md](explore-workflow.md).**

## User-visible formalization

**Show** the user explicit **requirements** and, when explore ran, the **design**. Ask for approval **before** you finalize the plan file.

## Skill evaluation

Decide which skills apply during implementation. List them in the **Skills** section of the plan with a short **why** per entry.

For any plan that **creates or modifies** application source, include **`agent-oriented-comments`** in **Skills** and expect implementers to apply it on **all touched source files** (including code that existed before the edit), per that skill.

## Building the plan document

**Before writing, read [template.md](template.md)** — it is the source of truth for section layout, task shape, and **Per-file instructions**. For a concrete task pattern, read **[example-task.md](example-task.md)**.

**Structure the plan according to [template.md](template.md).** Use the top-level headings defined there (e.g. **Specs**, **Skills**, **Integration and E2E evaluation**, **Tasks**). Do not replace them with synonyms.

**Specs** must reflect user-approved requirements and include the explore-skip line when explore was skipped. **Skills** lists skills for implementers.

## Integration and E2E evaluation

In the plan, include the **Integration and E2E evaluation** section as defined in [template.md](template.md).

## Tasks (junior implementer, no project context)

Each task must be:

- **Short and autonomous** — one implementation slice; **Scope / done when** makes acceptance clear without hidden repo knowledge.
- **What and why** — **Scope / done when** must state what to implement and why it matters (product or technical goal).
- **How, in detail** — **Per-file instructions** for **every** create / modify / delete: repo-relative paths, **What** and **How** (ordered steps, symbols, call sites, insertion points). Prefer concrete steps over “follow existing patterns” without pointers.

Each task uses the **execution order** defined in [template.md](template.md).

## Save the plan

Write the completed plan as Markdown under **`plans/`** at the **workspace root** (repository root), e.g. `plans/2026-04-13-feature-slug.md`. Create `plans/` if it does not exist. Use a clear filename (date + short slug). Present the plan to the user (chat summary and path to the file).

## Additional resources

- [explore-workflow.md](explore-workflow.md) — explore / clarifications phase (when the gate above applies).
- [template.md](template.md) — section layout, integration/e2e block, tasks + **Per-file instructions**.
- [example-task.md](example-task.md) — one filled task (progressive disclosure).
