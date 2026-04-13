---
name: prompt-optimizer
description: >-
  Review and optimize prompts, skills, and agent instructions for clarity and
  precision. Use when reviewing SKILL.md files, AGENTS.md, subagent prompts,
  or any instructional text for AI agents.
---

# Prompt Optimizer

Review instructional text (skills, AGENTS.md, subagent prompts) and fix clarity issues.

**Scope**: text quality only. This skill does NOT gather requirements or create plans (see **`plan`** skill — explore and planning live there).

## What to Check

| Category | Look for |
|----------|----------|
| **Ambiguity** | Instructions interpretable in multiple ways. Would two different agents do different things? |
| **Contradictions** | Conflicting instructions within the same file or across files. |
| **Vagueness** | Instructions too abstract to act on. "Handle errors properly" → how? |
| **Redundancy** | Same instruction repeated in different words. Merge or remove. |
| **Missing context** | Instructions that assume knowledge the agent won't have. |
| **Ordering** | Most important instructions should come first. Bury details at the end. |
| **Conciseness** | Every sentence must justify its token cost. Cut filler. |

## Process

1. Read the full text.
2. Flag issues by category (table above).
3. For each issue: quote the problematic text, explain why it's a problem, propose a fix.
4. Present all issues to the user for approval before applying changes.

## Output Format

For each issue:
```
**[Category]** line/section reference
- Current: "..."
- Problem: ...
- Suggested: "..."
```

## DON'Ts
- Never rewrite the entire document without being asked. Flag issues, don't overhaul.
- Never change the intent or scope of instructions — only improve clarity.
- Never do requirements gathering or design work. That's the **`plan`** skill (explore phase).
- Never add new instructions or features. Only clarify existing ones.
