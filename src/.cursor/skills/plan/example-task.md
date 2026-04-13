# Example task (filled)

Illustrative only; paths are placeholders. Shows **implementation-only** execution order, explicit **Per-file instructions**, and **How** sections that include **code blocks** (partial / illustrative) so a junior implementer knows exactly where and what to edit.

---

## Task: Validate email on registration form

**Why (context):** Invalid emails cause support churn and bad data; the product rules R1–R3 must be enforced at submit time with a stable error contract.

**Scope / done when:** submitting the registration form rejects invalid emails with a stable error id and accepts valid addresses per product rules R1–R3.

**Execution order (mandatory):**

1. **Implement** — Complete the **Per-file instructions** below so **Scope / done when** is met.

**Per-file instructions (mandatory):**

### `src/forms/registration.ts` — **modify**

- **What:** enforce R1–R3 before submit; surface `email-invalid` (or equivalent contract used elsewhere in the app) for invalid input.
- **How:**
  1. Open `src/forms/registration.ts` and find the function that handles registration submit (often named `onSubmit`, `handleRegister`, or similar). If the file exports a single default component, the handler may be inline on the form element—search for `fetch`, `api.register`, or `POST`.
  2. **Insert the guard immediately before** any network call or state transition that persists the user. Do not move or duplicate the network call.

  Locate a block shaped like this (names may differ—adapt to the real file):

  ```ts
  async function submitRegistration(payload: RegistrationPayload) {
    // ... possibly other field checks ...

    await api.register(payload);
  }
  ```

  Change it so validation runs first and returns early with the stable error id. Example shape (fill in real imports and types from the file):

  ```ts
  import { validateEmailFormat } from "./validators/email";

  async function submitRegistration(payload: RegistrationPayload) {
    // ... possibly other field checks ...

    if (!validateEmailFormat(payload.email)) {
      throw new RegistrationError("email-invalid"); // or setError / return { error: "email-invalid" } per existing pattern
    }

    await api.register(payload);
  }
  ```

  3. Reuse the project’s existing error pattern: if the file uses `setFormError`, `toast`, or a `Result` type instead of `throw`, mirror that—do not introduce a new error channel unless Specs require it.
  4. Leave password, name, and other fields untouched unless Specs say otherwise.

### `src/forms/validators/email.ts` — **create** (only if no shared validator exists)

- **What:** single exported function matching R1–R3 (minimal rules: non-empty, contains `@`, basic local-part/domain sanity—tighten to match product doc).
- **How:**
  1. Create the file next to other validators or per `AGENTS.md` / existing imports in `registration.ts`.
  2. Start from this skeleton and complete the checks per R1–R3:

  ```ts
  /**
   * Returns true if `s` satisfies registration email rules R1–R3 (see Specs).
   */
  export function validateEmailFormat(s: string): boolean {
    if (!s || s.trim().length === 0) {
      return false;
    }
    // TODO: R2 — e.g. must contain '@', single @, non-empty local and domain parts
    // TODO: R3 — e.g. reject obvious garbage; align with product rules, not a full RFC parser unless spec says so
    return true;
  }
  ```

  3. Export only what `registration.ts` needs; avoid adding unused helpers.

### (No file) — **delete**

- Omit this block if nothing is deleted. If a file is removed, use the template’s delete subsection: state replacement behavior and every call site to update.
