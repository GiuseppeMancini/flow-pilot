# Agent Guidelines

## Communication
- Be concise. No filler, no preambles.
- When in doubt, ASK. Never make decisions autonomously on ambiguous requirements.
- Be critical. Challenge the user's assumptions when you see problems.

## Agent-oriented comments (code)
- In files you touch: add or refresh **concise** comments that speed exploration and edits for the next agent.
- Cover: purpose of types / methods / functions; non-obvious behavior (contracts, side effects, errors); links to other parts (callers, callees, important deps).
- Remove redundant or wrong comments; do not restate obvious names.
- Match the file’s language and comment style.

## Agent docs to keep current

| File | Keep updated with |
|------|-------------------|
| Root `AGENTS.md` | Verified **commands**: build, run, lint/format, test, debug, env vars, entrypoints, CI when relevant. Update when tooling or scripts change. Keep **Agent documentation layout → This repository** accurate when code/test trees appear or move. |
| `<convention-root>/AGENTS.md` | **Code roots**: project code conventions. **Test roots**: test conventions. **Create** when the directory exists and the file is missing (minimal stub + sections to fill). Paths come from **Agent documentation layout** above. |

### Agent docs prerequisites (blocking — all work)

Before **any work** on the repo, these checks must pass:

| Prerequisite | Pass when |
|--------------|-----------|
| Root `AGENTS.md` | **Verified project ops** (section above) has real content (not empty placeholders): build/run/test/lint or honest **N/A** for this repo type, entrypoints, env vars, CI when relevant. If that section is missing or placeholder-only, **stop**. |
| Nested `AGENTS.md` | For **each path** listed under **Agent documentation layout → This repository** (code and test roots), **if** that directory **exists**, file `<path>/AGENTS.md` **must exist** (minimal stub + sections per table above). If missing, **stop**. |

**Behavior:** If any check fails, **STOP**. This applies in **every mode and workflow** (including Plan Mode and Bugfix) — the first useful output is this request, not code changes, fixes, or a draft plan. Only after fixes or a clear user waiver may you continue.

## Workflow: Plan Mode

1. **Load context**: read root `AGENTS.md` (communication, comments policy, **Verified project ops**, **Agent documentation layout**, workflows). For each convention root listed for **this repository** whose directory exists, read `<path>/AGENTS.md`. Then explore the repo in a targeted way (neighbors, imports, call sites), using agent-oriented comments as the first guide.
2. **Agent docs prerequisites**: run the **Agent docs prerequisites (blocking — all work)** checks. If any fail → ask the user (or explicit waiver). **Do not** proceed to step 3 until resolved.
3. **Evaluate scope**: does the request need the `explore` skill? (vague, ambiguous, multiple valid solutions) If yes, activate it first.
4. **Formalize specs**: write explicit requirements and define test scenarios for each requirement. Each scenario maps to a test. If a scenario cannot be automated, define exact manual steps for the user. Evaluate which skills to activate for the implementation. Ask user to approve before proceeding.
5. **Create the plan**. The plan must contain these sections:
   - **Specs**: the approved requirements.
   - **Skills**: skills to activate for the implementation.
   - **Test scenarios**: the approved test scenarios.
   - **Tasks**: list of self-contained tasks. Each task must follow this structure: implement → write tests → run tests. Apply YAGNI, DRY, TDD. Refactor touched areas when beneficial.
   - **DoD**: keep iterating until all of the following are satisfied: all tasks complete, lint clean (no format), full test suite green, implementation approved by `code-reviewer` (max 3 cycles).
6. **Plan review (mandatory)**: launch `plan-reviewer` via the Task tool. **Plan Mode is not an exception** — there is no shortcut. **Max 3 review cycles**; iterate the plan until approved or cycles exhausted per subagent rules.
7. **Present plan** to user only after reviewer approval.

## Workflow: Bugfix

0. **Agent docs prerequisites**: same **Agent docs prerequisites (blocking — all work)** as above. If any fail → ask the user (or explicit waiver). **Do not** diagnose or change code until resolved.
1. **Diagnose**: find the root cause before attempting any fix.
   - Read error logs and stack traces.
   - Check recent changes (git log, git diff).
   - Trace the code path from the error backward.
   - Reproduce the bug reliably.
   - If the cause is unclear: add diagnostic logging, ask the user to perform specific actions and report results, narrow down with targeted tests.
   - When in doubt, consult the user before proceeding.
2. **Reproduce**: write a test that fails due to the bug. If a test isn't feasible, document exact reproduction steps.
3. **Fix**: apply the minimal fix addressing the root cause.
4. **Verify**: confirm the reproducing test passes and no existing tests break.

## Subagents

Launch subagents via Task tool. Read the agent file and include its instructions in the subagent prompt along with the relevant input (plan, implementation, session summary).

| Agent | File | When |
|-------|------|------|
| `plan-reviewer` | `.cursor/agents/plan-reviewer.md` | **Required** after drafting a plan (Plan Mode step 6). Do not present a plan to the user without this review. Max 3 cycles. |
| `code-reviewer` | `.cursor/agents/code-reviewer.md` | Included as final task in every plan. Max 3 cycles. |

## DON'Ts
- Never decide on ambiguous requirements without asking the user.
- Never skip **Agent docs prerequisites** for substantive work — not only Plan Mode; implementation and bugfix included unless the user explicitly waives.
- Never skip `plan-reviewer` or `code-reviewer`.
- Never skip `plan-reviewer` because the session is in Plan Mode, because a plan was auto-generated, or for expediency — **no exception**.
- Never present a plan before the reviewer approves it.
- Never implement features not in the specs.

## Definition of Done

| Phase | Done when |
|-------|-----------|
| Planning | **Agent docs prerequisites** satisfied or **explicitly waived** by the user; requirements user-approved; test scenarios confirmed; plan approved by plan-reviewer; plan approved by user |
| Implementation | **Agent docs prerequisites** satisfied or waived before work began; all tasks done; lint clean; full test suite green; code approved by code-reviewer; and where applicable: agent-oriented comments and the relevant root `AGENTS.md` / nested `<convention-root>/agents.md` files updated for the change |
| Bugfix | **Agent docs prerequisites** satisfied or waived before work began; root cause identified; reproducing test exists (or repro steps documented); fix applied; all tests green |
