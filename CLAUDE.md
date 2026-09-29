# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project purpose

This is the starter repo for the "Set up Claude Code on a real project" course exercise. The Express API itself is not the point — it exists so the learner has a real codebase on which to configure Claude Code (a `CLAUDE.md`, `.claude/settings.json` permission rules, and a `NOTES.md`). **Do not change the app code** (`server.js`, `routes/`, `db/`, `tests/`) unless explicitly asked; the task is about Claude Code configuration, not features.

## Commands

```
npm install
npm run dev      # starts the API on http://localhost:3000 (node --watch server.js)
npm test         # runs tests with node:test + supertest
npm run lint     # eslint .
```

To run a single test file: `node --test tests/users.test.js`.

## Architecture

- `server.js` is the entry point: builds the Express `app`, mounts route modules, and only calls `app.listen()` when run directly (`require.main === module`). This lets `tests/*.test.js` `require("../server")` and hit routes via `supertest` without opening a real port.
- `routes/` — one file per resource (`users.js`, `health.js`), each exporting an `express.Router()` mounted in `server.js` under its path prefix (`/users`, `/health`).
- `db/store.js` — a tiny in-memory data module (plain functions + a module-level array). Stands in for a real database; data resets on every restart. Routes call into it instead of touching data directly.
- `.env.example` documents config (`PORT`); real values go in a git-ignored `.env`.

## Conventions

- Data access from routes goes through `db/store.js`, not inline arrays/logic in route handlers.
- ESLint config (`.eslintrc.json`) ignores unused `req`/`res`/`next` args and `_`-prefixed args — keep Express handler signatures as-is even when a param is unused.
