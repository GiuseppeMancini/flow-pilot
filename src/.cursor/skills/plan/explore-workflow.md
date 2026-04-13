# Explore / clarifications

Part of the **`plan`** skill: run this **before** writing the plan file when the gate in [SKILL.md](SKILL.md) says to explore.

- **In:** Vague or underspecified request, or need to align with the codebase.
- **Out:** User-approved **requirements** and **design** — not production implementation.

**Code navigation** (comments as first guide, `agent-oriented-comments`) is defined once in [SKILL.md](SKILL.md) — it applies to the whole **`plan`** skill, including this flow.

## Flow

1. **Load agent context** — Root `AGENTS.md`; nested `AGENTS.md` on relevant paths; skim comments + code near the affected area.
2. **Explore project context** — Files, docs, recent commits tied to the request; thread comments → symbols → call sites.
3. **Clarify** — One question at a time; prefer **multiple choice**; cover goal, constraints, success criteria. If several independent subsystems appear, **decompose** before detail work.
4. **Formalize specs** — Explicit requirement bullets; get user confirmation.
5. **Options** — **2–3 approaches**, trade-offs, **your recommendation first**, then the rest.
6. **Design** — Architecture, components, data flow, errors, testing — scale depth to complexity; get approval **per section** when large.
7. **Hand off** — Confirmed specs + approved design feed the plan document ([template.md](template.md) sections).

## Principles

| Principle | In practice |
|-----------|-------------|
| **YAGNI** | Drop anything not required for this request. |
| **Isolation** | One job per component; clear interfaces; independently testable. |
| **Existing patterns** | Read the repo before inventing new conventions. |
| **Targeted fixes** | Include local problems that block this work; no unrelated refactors. |

## Guardrails

- No **production implementation** during this phase.
- Do **not** skip clarifying questions — validate assumptions even when the ask looks clear.
- Do **not** lock a design before presenting **alternatives** (step 5).
- Do **not** prefer “cleaner” patterns over **documented project style** without justification.
