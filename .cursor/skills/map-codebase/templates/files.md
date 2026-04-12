# File index

**Scope**: first-party sources and configs only; exclude vendored/build output (see [paths.md](paths.md)).

## Example section: `src/`

### `src/main.ts`
**Responsibilities**: Application entry; starts HTTP server and loads config.
**Feature**: Core / API
**Connections**: Imports `app.ts`, `config/env.ts`; invoked by `npm run dev` (see conventions.md).

### `src/app.ts`
**Responsibilities**: Express (or framework) app factory; mounts routers and middleware.
**Feature**: Core / API
**Connections**: Used by `main.ts`; imports routes under `src/routes/`.

<!-- Add one block per file; group under ## relative/dir/ headings. Shard into files-*.md when this file exceeds ~250 lines. -->
