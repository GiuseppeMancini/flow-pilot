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

## Keeping AGENTS.md up to date
- Keep **root and nested** `AGENTS.md` files accurate whenever the repo changes in ways those files describe (tooling, commands, layout, conventions).

## Workflow: Plan Mode

1. **Load context**: read any `AGENTS.md` that applies to the work; explore the repo in a targeted way (neighbors, imports, call sites), using agent-oriented comments as the first guide.
2. **Evaluate scope**: does the request need the `explore` skill? (vague, ambiguous, multiple valid solutions) If yes, activate it first.
3. **Formalize specs**: write explicit requirements and define test scenarios for each requirement. Each scenario maps to a test. If a scenario cannot be automated, define exact manual steps for the user. Evaluate which skills to activate for the implementation. Ask user to approve before proceeding.
4. **Create the plan**. The plan must contain these sections:
   - **Specs**: the approved requirements.
   - **Skills**: skills to activate for the implementation.
   - **Test scenarios**: the approved test scenarios.
   - **Tasks**: list of self-contained tasks. Each task must follow this structure: implement → write tests → run tests. Apply YAGNI, DRY, TDD. Refactor touched areas when beneficial.
   - **DoD**: keep iterating until all of the following are satisfied: all tasks complete, lint clean (no format), full test suite green, implementation approved by `code-reviewer` (max 3 cycles).
5. **Plan review (mandatory)**: launch `plan-reviewer` via the Task tool. **Plan Mode is not an exception** — there is no shortcut. **Max 3 review cycles**; iterate the plan until approved or cycles exhausted per subagent rules.
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
| `plan-reviewer` | `.cursor/agents/plan-reviewer.md` | **Required** after drafting a plan (Plan Mode step 5). Do not present a plan to the user without this review. Max 3 cycles. |
| `code-reviewer` | `.cursor/agents/code-reviewer.md` | Included as final task in every plan. Max 3 cycles. |

## DON'Ts
- Never decide on ambiguous requirements without asking the user.
- Never skip `plan-reviewer` or `code-reviewer`.
- Never skip `plan-reviewer` because the session is in Plan Mode, because a plan was auto-generated, or for expediency — **no exception**.
- Never present a plan before the reviewer approves it.
- Never implement features not in the specs.

## Definition of Done

| Phase | Done when |
|-------|-----------|
| Planning | Requirements user-approved; test scenarios confirmed; plan approved by plan-reviewer; plan approved by user |
| Implementation | All tasks done; lint clean; full test suite green; code approved by code-reviewer; agent-oriented comments where applicable; **AGENTS.md** files updated when the change affects what they document |
| Bugfix | Root cause identified; reproducing test exists (or repro steps documented); fix applied; all tests green |
