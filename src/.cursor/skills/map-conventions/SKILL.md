---
name: map-conventions
description: >-
  Maintains root and nested AGENTS.md files: verified project commands (build,
  compile, run, dev server, test, lint/format, debug, CI, env vars, entrypoints)
  at the repo root, plus per-root convention docs for source trees, test trees,
  and other relevant areas (e.g. e2e, integration). Use when creating or editing
  AGENTS.md, or after changes to scripts, toolchain, CI, code/test conventions,
  or directory layout that should be reflected in agent-facing documentation.
---

# Map conventions (AGENTS.md)

Keep **AGENTS.md** accurate and well-scoped: root file for **how to operate** on the repo; nested files for **how to write** code and tests in each major area.

## When to apply

- Creating or editing any `AGENTS.md`.
- After changes that affect what an agent needs to know: `package.json` / task runners, build tools, run/debug entrypoints, test commands, lint/format, CI workflows, environment variables, or folder structure (e.g. new `e2e/`, `integration/`, packages in a monorepo).
- Whenever documented commands or conventions become wrong or incomplete.

## Official format and references

Content and structure of `AGENTS.md` files follow the open **AGENTS.md** format. Use these as the authoritative guides:

- [https://agents.md/](https://agents.md/)
- [https://agents.md/#examples](https://agents.md/#examples)
- [https://github.com/agentsmd/agents.md](https://github.com/agentsmd/agents.md)

**Notes:** The format is plain Markdown with no required fields. In nested trees, the nearest `AGENTS.md` to the work typically takes precedence; explicit user instructions override file content (see the site FAQ).

## Root `AGENTS.md`

At the **repository root** (or package root in a monorepo, when that root is the unit of work), include a clear section for **commands the agent can run**, for example:

- Install / sync dependencies
- Build / compile
- Run (app, dev server, CLI)
- Test (unit, integration — split if commands differ)
- Lint / format / typecheck
- Debug (attach, flags, or launcher) when the project supports it
- CI-equivalent checks (local invocations of what CI runs), when useful
- Environment variables and entrypoints needed to run or verify work

Prefer **real, verified** commands: run them or confirm them from project config (`package.json`, `Makefile`, `pyproject.toml`, CI YAML, etc.). If something does not apply, state **N/A** briefly rather than leaving placeholders forever.

Keep human-focused narrative in **README.md**; put agent-oriented operational detail here, per the AGENTS.md project goals.

## Nested `AGENTS.md` for relevant directories

Place an `AGENTS.md` in each **convention root** that agents should treat differently:

| Area | Typical path pattern | What to document |
|------|----------------------|------------------|
| Source code | e.g. `src/`, `lib/`, `apps/<name>/` | Code style, architecture patterns, error handling, naming, module boundaries, where types and helpers live |
| Tests | e.g. `tests/`, `__tests__/`, `spec/` | Framework, file naming, arrange-act-act / Given-When-Then, fixtures, mocks, coverage expectations |
| Other distinct roots | e.g. `e2e/`, `playwright/`, `perf/` | Commands, timeouts, test data rules, flakiness policy, how this area differs from unit tests |

Use **one `AGENTS.md` per root** when that area has its own commands, tools, or conventions. Do not hardcode paths in this skill: discover actual roots from the repo tree and configs.

## Workflow

1. **Discover roots** — From directory layout and build/test/CI config, list source root(s), test root(s), and any other agent-relevant roots (e2e, contracts, etc.).
2. **Root file** — Ensure a root `AGENTS.md` exists with an up-to-date **commands / operations** section. Update it when tooling or scripts change.
3. **Nested files** — For each relevant root that exists, ensure `<that-root>/AGENTS.md` exists. If missing, add a **minimal stub** with section headings, then fill from real project practice.
4. **Propagate updates** — After a change that alters documented facts, update every `AGENTS.md` that mentions those facts.
5. **Stay concise** — Prefer short lists and copy-pastable commands; avoid duplicating README prose.

## DON'Ts

- Do not treat an existing `AGENTS.md` as the only source of truth for paths or commands; validate against configs and the filesystem.
- Do not bury agent-critical commands only in README if they belong in `AGENTS.md`.
