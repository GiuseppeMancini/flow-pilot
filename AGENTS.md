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
| Root `AGENTS.md` | Verified **commands**: build, run, lint/format, test, debug, env vars, entrypoints, CI when relevant. Update when tooling or scripts change. |
| `src/agents.md` | Project **code** conventions (layout, naming, patterns, errors, logging, etc.). **Create** this file when `src/` exists and the file is missing (minimal stub + sections to fill). |
| `test/agents.md` | **Test** conventions (layout, fixtures, mocks, test data, what to test / skip). **Create** when `test/` exists and the file is missing (minimal stub + sections to fill). |

Do not create `src/` or `test/` only to add these files.

## Workflow: Plan Mode

1. **Load context**: read root `AGENTS.md` (commands + this workflow). If they exist and apply, read `src/agents.md` and/or `test/agents.md`. Then explore the repo in a targeted way (neighbors, imports, call sites), using agent-oriented comments as the first guide.
2. **Evaluate scope**: does the request need the `explore` skill? (vague, ambiguous, multiple valid solutions) If yes, activate it first.
3. **Formalize specs**: write explicit requirements and define test scenarios for each requirement. Each scenario maps to a test. If a scenario cannot be automated, define exact manual steps for the user. Evaluate which skills to activate for the implementation. Ask user to approve before proceeding.
4. **Create the plan**. The plan must contain these sections:
   - **Specs**: the approved requirements.
   - **Skills**: skills to activate for the implementation.
   - **Test scenarios**: the approved test scenarios.
   - **Tasks**: list of self-contained tasks. Each task must follow this structure: implement → write tests → run tests. Apply YAGNI, DRY, TDD. Refactor touched areas when beneficial.
   - **DoD**: keep iterating until all of the following are satisfied: all tasks complete, lint clean (no format), full test suite green, implementation approved by `code-reviewer` (max 3 cycles).
5. **Plan review**: launch a `plan-reviewer` subagent. Max 3 review cycles.
6. **Present plan** to user only after reviewer approval.

## Workflow: Bugfix

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
| `plan-reviewer` | `.cursor/agents/plan-reviewer.md` | After creating a plan (step 5 of Plan Mode). Max 3 cycles. |
| `code-reviewer` | `.cursor/agents/code-reviewer.md` | Included as final task in every plan. Max 3 cycles. |

## DON'Ts
- Never decide on ambiguous requirements without asking the user.
- Never skip plan-reviewer or code-reviewer.
- Never present a plan before the reviewer approves it.
- Never implement features not in the specs.

## Definition of Done

| Phase | Done when |
|-------|-----------|
| Planning | Requirements user-approved, test scenarios confirmed, plan approved by plan-reviewer, plan approved by user |
| Implementation | All tasks done, lint clean, full test suite green, code approved by code-reviewer, and where applicable: agent-oriented comments and the relevant `AGENTS.md` / `src/agents.md` / `test/agents.md` updated for the change |
| Bugfix | Root cause identified, reproducing test exists (or repro steps documented), fix applied, all tests green |
