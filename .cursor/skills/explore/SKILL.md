---
name: explore
description: >-
  Explore vague or ambiguous requests through structured dialogue to produce
  clear specs and a design. Use when the user's request is unclear, has multiple
  valid solutions, or needs requirements clarification before planning.
---

# Explore

Turn vague ideas into clear specs and designs through collaborative dialogue.

**Output**: formalized specs + approved design. No implementation, no plan.

## Workflow

1. **Load agent context**: read root `AGENTS.md`; if relevant, `src/agents.md` and/or `test/agents.md`. Skim agent-oriented comments and code near the affected area.
2. **Explore project context**: check files, docs, recent commits relevant to the request.
3. **Ask clarifying questions**: one at a time, prefer multiple choice. Understand purpose, constraints, success criteria.
4. **Formalize specs**: write down requirements explicitly. Ask user to confirm.
5. **Propose 2-3 approaches**: with trade-offs and your recommendation. Lead with the recommended option.
6. **Present design**: cover architecture, components, data flow, error handling, testing. Scale each section to its complexity. Ask user approval after each section.
7. **Finalize**: deliver confirmed specs + approved design back to the calling workflow (plan mode).

## Clarifying Questions

- Multiple choice when possible, open-ended when needed.
- Focus: what is the goal? what are the constraints? what does success look like?
- If the request spans multiple independent subsystems, flag it immediately — decompose before diving into details.

## Design Principles

- **YAGNI**: remove anything not strictly needed.
- **Isolation**: each component has one purpose, clear interfaces, testable independently.
- **Follow existing patterns**: explore the codebase before proposing new conventions.
- **Targeted improvements**: if existing code in the affected area has problems relevant to this work, include them in the design. Don't propose unrelated refactoring.

## DON'Ts
- Never produce a plan or implementation. Output is specs + design only.
- Never skip clarifying questions — even if the request seems clear, validate assumptions.
- Never propose a design without presenting alternatives first.
- Never ignore existing project patterns in favor of "better" approaches without justification.
