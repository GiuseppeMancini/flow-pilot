# Example task (filled)

Illustrative only; paths and commands are placeholders. Shows **execution order** and **explicit per-file instructions** expected in each plan task.

---

## Task: Validate email on registration form

**Scope / done when:** submitting the registration form rejects invalid emails with a stable error id and accepts valid addresses per product rules R1–R3.

**Execution order (mandatory):**

1. **Write tests** — In `tests/unit/registration.test.ts`, add cases: valid email accepted; missing `@` rejected; empty string rejected. Tests should fail until implementation exists.
2. **Write code** — Implement minimal validation in the registration module so those tests pass.
3. **Run tests** — e.g. `npm test -- registration` — all green before refactor.
4. **Simplify / refactor** — Follow `.cursor/skills/simplify-code/SKILL.md`; e.g. extract shared email check or rename per its naming/redundancy tables; behavior must stay identical.
5. **Run tests again** — if any fail after step 4, adjust or partially revert the refactor, re-run until green (repeat as needed).

**Per-file instructions (mandatory):**

### `tests/unit/registration.test.ts` — **modify**

- **What:** automated coverage for R1–R3 on email validation at submit boundary.
- **How:** add or extend a `describe` for email validation; assert HTTP/status or returned error id `email-invalid` for bad inputs; assert success path for one valid email; use existing test helpers/fixtures if the repo provides them (name the helper file if known).

### `src/forms/registration.ts` — **modify**

- **What:** enforce R1–R3 before submit; surface `email-invalid` (or equivalent contract used elsewhere in the app) for invalid input.
- **How:** locate submit handler; call a small validator function before network call; do not change unrelated fields; keep exports stable unless spec requires otherwise.

### `src/forms/validators/email.ts` — **create** (only if no shared validator exists)

- **What:** single function e.g. `isValidEmail(s: string): boolean` or `validateEmail(s): Result` matching R1–R3.
- **How:** implement minimal logic; no extra rules beyond spec; add one-line doc comment on accepted format.

### (No file) — **delete**

- Omit this block if nothing is deleted. If a file is removed, use the template’s delete subsection: state replacement behavior and every call site to update.
