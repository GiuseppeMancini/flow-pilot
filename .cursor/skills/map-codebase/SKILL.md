---
name: map-codebase
description: >-
  LEGACY / optional only. This repo uses root AGENTS.md, agent-oriented code comments,
  and src/agents.md / test/agents.md — not codebase/. Use only if the user explicitly
  asks to create or refresh a codemap under codebase/. Triggers (legacy): codemap,
  codebase map, map-codebase, file index, project map, codebase documentation.
---

# Map Codebase (legacy)

> **Not part of the default workflow here.** Navigation and conventions live in root `AGENTS.md`, **comments in code**, and `src/agents.md` / `test/agents.md` when those trees exist. Use the content below **only** when the user explicitly wants a `codebase/` codemap.

Maintain a **codemap** under `codebase/` at project root: factual, navigable knowledge for agents. **Wrong information is worse than missing information.** Verify before writing; prune stale entries; remove what you cannot confirm.

## Purpose

- Cut exploration cost: read `codebase/index.md` and targeted files instead of searching the whole tree each time.
- Keep **files.md** (or shards) as the canonical inventory of first-party files with roles and wiring.

## Structure

Guideline, not a rigid schema. Always keep `index.md` as the entry point.

```
codebase/
├── index.md           # Entry point — read first
├── architecture.md    # Stack, architecture, integrations, data flow
├── conventions.md     # Commands first, then naming, patterns, style
├── features.md        # Features, decisions, limitations
├── paths.md           # Directory map, entry points (not per-file detail)
├── files.md           # Full file index (see below); shard if too large
└── *.md               # Extra topics as needed (e.g. api-endpoints.md)
```

Templates live in this skill's `templates/` folder.

### files.md — full file index

**Scope**: every **first-party** source and config file the agent may need (e.g. `src/`, `tests/`, app configs at repo root). **Exclude** vendored trees (`node_modules`, `.venv`), build outputs (`dist`, `build`), and huge generated blobs; mention those only in `paths.md`.

For **each** included file, use this block (keep responsibilities to 1–2 lines):

```markdown
### `relative/path/to/File.ext`
**Responsibilities**: …
**Feature**: <!-- feature name or area; align with features.md -->
**Connections**: <!-- main imports, who imports this, configs, env, key collaborators -->
```

Group blocks under directory headings (e.g. `## src/services/`) so the index stays scannable.

**Sharding**: if `files.md` exceeds ~250 lines, split by top-level area, e.g. `files-src.md`, `files-tests.md`, `files-config.md`, link them from `index.md` and `files.md` (stubs or overview + links).

## Modes

### Full / init

Use when `codebase/` is missing, empty, or the user asks for a **full refresh** of the codemap.

1. Read `codebase/index.md` if it exists; otherwise create from templates.
2. Explore the repo; fill and correct **all** standard files + `files.md` coverage for the chosen scope.
3. Update `index.md` (TOC, quick reference, **Last updated** date).

### Update (scoped)

Use when the user (or workflow) states **what** to change — e.g. "update only `conventions.md` commands", "add `src/foo.ts` to files index", "refresh architecture stack section".

1. **Parse scope** from the request: which files under `codebase/` and which sections/topics.
2. **Touch only** those files/sections. Do not rewrite unrelated codemap files.
3. **Verify** only facts relevant to the scoped change against the repo.
4. If any codemap file was modified, update **`index.md`** timestamp (and TOC if files were added/removed/renamed).

If the scope is ambiguous, ask before editing.

## Reading

Always start with `codebase/index.md`. Then open only the files relevant to the task (e.g. `files.md` for "where is X implemented?", `conventions.md` for commands and lint).

## Writing rules

1. **Verify** against the codebase before adding or changing entries.
2. **Prune** removed/renamed modules; drop unverified claims.
3. **Factual** only; tight headings and bullets in topic files; **no code snippets** in the codemap (reference paths).
4. **No duplicates** — merge into existing sections.
5. **Split** oversized topic files: extract subtopics, link from `index.md`.
6. **index.md**: keep TOC in sync; bump **Last updated** on any codemap change.
7. **conventions.md**: keep **Commands** at the top (run, test, lint, typecheck, format, debug — as they exist in the project).

## Initialization

If `codebase/` is missing or empty, create it from `templates/`, then populate with verified facts (full mode).

## DON'Ts

- No secrets or credentials.
- No subjective opinions — only verified facts.
- Do not duplicate long prose from README — link to it.
- Do not keep entries you cannot verify; remove or mark removed after confirmation in update mode.
- Do not let sharded file-index docs drift: if one shard changes, update index/links as needed.
