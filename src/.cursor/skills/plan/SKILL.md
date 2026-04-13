---
name: plan
description: >-
  Produces an internal plan draft with required sections (Specs, Skills, Test
  scenarios, Tasks, DoD), runs plan-reviewer via Task before any user-visible
  plan, and respects explore-by-default unless the request is trivial or fully
  detailed. Use in Plan Mode or when the user asks for a structured
  implementation plan, CreatePlan timing, or plan-reviewer workflow.
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

## Plan review (mandatory)

Launch `plan-reviewer` agent via the Task tool with the **complete draft** and the instructions. **Max 3 cycles**; revise from feedback until **Status: Approved** or cycles are exhausted.

## Present the plan

Only **after** `plan-reviewer` has run on the full draft and you have a recorded outcome: show the plan (CreatePlan, chat, or equivalent). If cycles exhaust without approval, show the latest draft **and** state that reviewer approval was not obtained within the limit.

## Conflict with default tooling

If system or product guidance says to create or show the plan early, **project AGENTS.md wins**: Task `plan-reviewer` on the full draft comes first; user-visible plan content comes **after** that review.

## Before any user-visible plan (self-check)

- [ ] `plan-reviewer` was invoked via Task with the full draft (not a partial outline).
- [ ] Review outcome recorded: **Approved**, or revised and re-invoked within the cycle limit (3), or cycles exhausted (disclose if so).
- [ ] Only then: show the plan (CreatePlan, chat, or other).

## Additional resources

- [template.md](template.md) — full plan draft template (tasks + **Per-file instructions** pattern).
- [example-task.md](example-task.md) — one filled task with explicit per-file subsections (progressive disclosure).

Implementers use `simplify-code` skill at task step 4 when **Skills** lists `simplify-code` (see Skill evaluation).
