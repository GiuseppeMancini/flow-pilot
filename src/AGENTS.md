# Agent Guidelines

## Communication
- Be concise. No filler, no preambles.
- When in doubt, ASK. Never make decisions autonomously on ambiguous requirements.
- Be critical. Challenge the user's assumptions when you see problems.

## Agent-oriented comments (code)
- During **exploration**, use existing agent-oriented comments as your first guide to the code.
- For **every source file you modify**, add, improve, or remove comments per the **`agent-oriented-comments`** skill—**including code that already existed** before your edit, not only new lines or new symbols. When behavior changes, update comments in the **same** edit.
- For **how** to write them, read and follow the **`agent-oriented-comments`** skill.

## Keeping AGENTS.md up to date
- Keep **root and nested** `AGENTS.md` files accurate whenever the repo changes in ways those files describe (tooling, commands, layout, conventions).

## Workflow: structured planning

1. **Load context**: read any `AGENTS.md` that applies to the work; explore the repo in a targeted way (neighbors, imports, call sites), using agent-oriented comments as the first guide; when adding or refreshing comments, follow the **`agent-oriented-comments`** skill.
2. **Load the plan skill**: read and follow the **`plan`** skill for the full workflow (explore gate when needed, user-visible requirements and design, skill list, plan document from `template.md`, save under `plans/` at the workspace root).

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
| `code-reviewer` | `code-reviewer.md` | At least one **final** task on the full implementation against the plan. Max 3 cycles per review. |

## DON'Ts
- Never decide on ambiguous requirements without asking the user.
- Never skip `code-reviewer` when the workflow requires a final review of the implementation.
- Never implement features not in the specs.

## Definition of Done

| Phase | Done when |
|-------|-----------|
| Planning | Requirements (and design when applicable) user-approved; plan file saved under `plans/` per **`plan`** skill; plan approved by user |
| Implementation | All tasks done; lint clean; full test suite green; code approved by code-reviewer; on **every touched source file**, agent-oriented comments per **`agent-oriented-comments`** skill (focus on modified symbols and strictly necessary context—proportional to the change); **AGENTS.md** files updated when the change affects what they document |
| Bugfix | Root cause identified; reproducing test exists (or repro steps documented); fix applied; all tests green |
