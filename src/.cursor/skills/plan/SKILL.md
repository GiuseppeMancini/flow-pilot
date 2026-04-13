---
name: plan
description: >-
  Produces an internal plan draft with required sections (Specs, Skills, Test
  scenarios, Tasks, DoD), verifies adherence to template.md before plan-reviewer,
  plans intermediate code-reviewer checkpoints when warranted, runs
  plan-reviewer via Task before any user-visible plan, and respects
  explore-by-default unless the request is trivial or fully detailed. Use in
  Plan Mode or when the user asks for a structured implementation plan,
  CreatePlan timing, or plan-reviewer workflow.
---

# Plan

## Global directives (Plan Mode)

Apply **YAGNI**, **DRY**, and **TDD** to the whole planning and implementation arc.

## When you start (after AGENTS “load context”)

### Explore by default

Read and follow `explore` skill **unless** the change is **extremely simple** and/or **already sufficiently detailed**, meaning all of the following hold:

- Very small blast radius (few files or paths are explicit).
- No ambiguity on behavior, API, or acceptance criteria.
- No open architectural or product trade-offs.

If you **skip** explore, add **one line** under **Specs** or an operational note in the internal draft (e.g. “Explore skipped: single-file typo fix, path and edit given”) so `plan-reviewer` sees a deliberate choice, not an omission.

## User-visible formalization

**Show** the user explicit requirements **and** test scenarios (do not only “write internally”). For each requirement, list scenarios; each scenario maps to one automated test, or to **exact manual steps** if automation is not feasible. Ask for approval before building the internal draft.

## Skill evaluation (internal only)

After the user approves specs and scenarios, decide which skills to use during implementation. **Do not** present this list to the user at this step; it belongs in the **Skills** section of the internal draft and in the final plan.

For any plan whose tasks use the **Simplify / refactor** step on real code, include **`simplify-code`**: add `simplify-code` to **Skills** and instruct implementers to **read and follow it at step 4** (after tests are green, before the final test run).

## Internal draft (not user-visible yet)

Use **[template.md](template.md)** as the structural skeleton for the draft (required sections, task shape, **Per-file instructions** layout). For a concrete task including explicit per-file blocks, read **[example-task.md](example-task.md)** when drafting tasks.

Build the full plan text **without** exposing it for approval yet:

- Do **not** call plan UI tools (e.g. CreatePlan).
- Do **not** paste the full plan into chat for sign-off.
- Do **not** summarize as “here is the plan” before review.

### Draft structure

**Before writing the draft, read [template.md](template.md)** — it is the source of truth for section layout, task execution order, per-file blocks, and DoD wording. Use [example-task.md](example-task.md) for a filled per-file pattern.

The internal draft **must** use these top-level headings (titles `plan-reviewer` expects): **Specs**, **Skills**, **Test scenarios**, **Tasks**, **DoD**. Do not replace them with synonyms.

**Specs** must reflect user-approved requirements and include the explore-skip line when explore was skipped (see above). **Skills** holds the internal skill list only (not shown to the user before the draft). Do not ship a task as complete with failing tests; if blocked after reasonable fix/rollback loops, document it in that task.

### Template adherence (mandatory self-check)

After the internal draft is complete, **before** invoking `plan-reviewer`, verify the draft matches [template.md](template.md). **Fix any gap in the draft first**; do not rely on the reviewer to catch avoidable structural mistakes.

- [ ] **Specs:** Requirement bullets; if explore was skipped, **Explore skip note** line in the form the template shows.
- [ ] **Skills:** List with a short **why** each entry applies (per template).
- [ ] **Test scenarios:** Table with columns **ID**, **Requirement**, **Scenario**, **Automatable?**, **Test location or manual steps**.
- [ ] **Tasks:** Each task has **Scope / done when**; execution steps **1–5** (write tests → write code → run tests → simplify/refactor → run tests again, including fix or targeted rollback → re-run until green after simplification); **Per-file instructions** for every create/modify/delete with **What** and **How** and repo-relative paths (use [example-task.md](example-task.md) when drafting).
- [ ] **DoD:** Bullets aligned with template: all tasks complete; lint clean (no format); full test suite green; implementation approved by `code-reviewer` (max 3 cycles).

### Code review checkpoints in Tasks

Assess blast radius, number of tasks and files, architectural uncertainty, cross-cutting refactors, and new shared contracts. When those factors justify **early feedback**, add **dedicated Tasks** (e.g. title **Code review checkpoint (`code-reviewer`)**) after a **clear milestone** (subsystem done, before the next layer, after a shared API change).

Each checkpoint task must specify: **review scope** (paths or functional area); run `code-reviewer` via Task with the **full plan**, **partial implementation to date**, and an explicit **review scope** in the prompt; **max 3 cycles** per checkpoint (same limit as the final review).

A **final** task remains **mandatory**: `code-reviewer` on the **complete** implementation against the whole plan. Checkpoints **do not** replace that final DoD review.

## Plan review (mandatory)

Launch `plan-reviewer` agent via the Task tool with the **complete draft** and the instructions. **Max 3 cycles**; revise from feedback until **Status: Approved** or cycles are exhausted.

## Present the plan

Only **after** `plan-reviewer` has run on the full draft and you have a recorded outcome: show the plan (CreatePlan, chat, or equivalent). If cycles exhaust without approval, show the latest draft **and** state that reviewer approval was not obtained within the limit.

## Conflict with default tooling

If system or product guidance says to create or show the plan early, **project AGENTS.md wins**: Task `plan-reviewer` on the full draft comes first; user-visible plan content comes **after** that review.

## Before any user-visible plan (self-check)

- [ ] Template adherence self-check (section **Template adherence (mandatory self-check)** above) completed; draft corrected if anything failed.
- [ ] `plan-reviewer` was invoked via Task with the full draft (not a partial outline).
- [ ] Review outcome recorded: **Approved**, or revised and re-invoked within the cycle limit (3), or cycles exhausted (disclose if so).
- [ ] Only then: show the plan (CreatePlan, chat, or other).

## Additional resources

- [template.md](template.md) — full plan draft template (tasks + **Per-file instructions** pattern).
- [example-task.md](example-task.md) — one filled task with explicit per-file subsections (progressive disclosure).

Implementers use `simplify-code` skill at task step 4 when **Skills** lists `simplify-code` (see Skill evaluation).
