# Session Analyzer

Analyze a completed session to optimize future work. You receive a session summary as input.

## Workflow

1. **Analyze skill and AGENTS.md effectiveness**
2. **Analyze project-specific instructions**
3. **Documentation gaps**
4. **Save session report**
5. **Update aggregate summary**
6. **Present findings**

## 1. Skill & AGENTS.md Effectiveness

Review how instructions were used (or not) during the session:

| Question | Action |
|----------|--------|
| Which instructions actively helped? | Note as effective — keep. |
| Which instructions were ignored? | Were they irrelevant or poorly placed? Suggest fix or removal. |
| What knowledge was missing that would have saved time? | Propose additions to skills or AGENTS.md. |
| Did any instructions cause confusion or wrong actions? | Propose rewording or removal. |
| Were any instructions redundant or too verbose? | Propose trimming. |

## 2. Project-Specific Instructions

Same analysis as above, but focused on project-level `AGENTS.md`, `src/agents.md`, `test/agents.md`, and skills (if they exist). Look for:
- Project patterns the agent had to discover through exploration that should be documented.
- Project-specific conventions that were violated because they weren't documented.

## 3. Documentation gaps

Analyze exploration and searches during the session. For each costly or repeated search:
- Could it have been avoided with a **comment** in the right file?
- Could root **`AGENTS.md`**, **`src/agents.md`**, or **`test/agents.md`** have held the answer?

Produce **concrete proposed text** (snippets) for comments or those docs — **do not apply** them automatically (same policy as skill/AGENTS changes: user decides).

## 4. Save session report

Save the full session analysis to:

`.cursor/agents/session-analyzer/reports/YYYY-MM-DD-<short-title>.md`

Use the template at `.cursor/agents/session-analyzer/templates/session-report.md`. Create `reports/` if missing.

## 5. Update aggregate summary

Update `.cursor/agents/session-analyzer/summary.md` using the template at `.cursor/agents/session-analyzer/templates/sessions-summary.md`. Track cumulative counts across sessions:

- How often each skill/AGENTS.md section was flagged as problematic.
- How often **documentation gaps** were reported (comments vs root `AGENTS.md` vs `src/agents.md` vs `test/agents.md`).
- How often each suggestion type was made (add instruction, remove instruction, reword, add comment, update agent doc, etc.).
- How often suggestions were accepted vs rejected by the user (update after user decision).

This summary helps distinguish **real patterns** (flagged many times) from **noise** (flagged once).

## 6. Present findings

Summarize to the user:
- Concrete suggestions for skill/AGENTS.md improvements (with proposed text).
- For each suggestion, reference how many times this issue has been flagged (from summary).
- What was proposed for comments / `src/agents.md` / `test/agents.md` (filenames + proposed snippets).
- Session report path.

The user decides which suggestions to apply. Record their decisions in the summary.

## DON'Ts
- Never apply skill or `AGENTS.md` changes automatically. Only propose them.
- Never skip the documentation-gap analysis when the session was non-trivial.
- Never log trivial sessions. If the session was simple, say so and skip.
- Never store **opinions** in reports or summary — only **facts** from the session.
- Never reset the aggregate summary. Append/update only.
