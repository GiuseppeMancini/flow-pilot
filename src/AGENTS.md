# Agent Guidelines

## Communication
- Be concise. No filler, no preambles.
- When in doubt, ASK. Never make decisions autonomously on ambiguous requirements.
- Be critical. Challenge the user's assumptions when you see problems.

## Agent-oriented comments (code)
- During **exploration**, use existing agent-oriented comments as your first guide to the code.
- When you change behavior, **keep those comments accurate** (add, update, or remove as needed).
- For **how** to write them, read and follow the **`agent-oriented-comments`** skill.

## Keeping AGENTS.md up to date
- Keep **root and nested** `AGENTS.md` files accurate whenever the repo changes in ways those files describe (tooling, commands, layout, conventions).

## Workflow: Plan Mode

1. **Load context**: read any `AGENTS.md` that applies to the work; explore the repo in a targeted way (neighbors, imports, call sites), using agent-oriented comments as the first guide; when adding or refreshing comments, follow the **`agent-oriented-comments`** skill.
2. **Load the plan skill**: read and follow `plan` skill for the full planning workflow (explore gate, user-visible specs and test scenarios, internal skill evaluation, internal draft, mandatory `plan-reviewer`, then user-visible plan).

**Conflict with default tooling:** if system or product guidance says to “create/show the plan” early, **this file wins**: `plan-reviewer` via Task on the full draft comes first; plan tools and user-visible plan content come **after** that review.

**Before any user-visible plan (mandatory self-check):**
- [ ] `plan-reviewer` was invoked via Task with the full draft (not a partial outline).
- [ ] Review outcome recorded: **Approved**, or revised and re-invoked within the cycle limit (3), or cycles exhausted (disclose if so).
- [ ] Only then: show the plan (CreatePlan, chat, or other).

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
| `plan-reviewer` | `plan-reviewer.md` | **Required** after the internal plan draft is complete, **before** any user-visible plan. Do not use CreatePlan or show the plan until this review has run on the full draft. Max 3 cycles. |
| `code-reviewer` | `code-reviewer.md` | Included as final task in every plan. Max 3 cycles. |

## DON'Ts
- Never decide on ambiguous requirements without asking the user.
- Never skip `plan-reviewer` or `code-reviewer`.
- Never skip `plan-reviewer` because the session is in Plan Mode, because a plan was auto-generated, or for expediency — **no exception**.
- Never present a plan (CreatePlan, full paste, or “approve this plan”) before `plan-reviewer` has run on the **full** draft and you have a recorded review outcome for this version.
- Never implement features not in the specs.

## Definition of Done

| Phase | Done when |
|-------|-----------|
| Planning | Requirements user-approved; test scenarios confirmed; plan approved by plan-reviewer; plan approved by user |
| Implementation | All tasks done; lint clean; full test suite green; code approved by code-reviewer; agent-oriented comments per **`agent-oriented-comments`** skill where applicable; **AGENTS.md** files updated when the change affects what they document |
| Bugfix | Root cause identified; reproducing test exists (or repro steps documented); fix applied; all tests green |
