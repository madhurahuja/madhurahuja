# CLAUDE.md — madhurahuja.com

## Project Identity

- Domain: `madhurahuja.com`
- Owner: Madhur Ahuja (Maddy), Senior Principal Software Architect
- Purpose: Professional portfolio + blog + AI-assisted knowledge interface
- Tagline: "I teach what I build. I build what I don't yet understand."
- Hard constraint: Total recurring cost must remain `$0/month` with no exceptions.

## Core Technical Direction

- Frontend: Vite + React 18 + TypeScript SPA deployed as static assets to GitHub Pages.
- Data access: SQLite database prebuilt at build time and loaded in-browser through `sql.js` WASM.
- Search: Local browser-side SQLite FTS5 over `content_chunks` and `content_fts`.
- AI: Lightweight client-side agent orchestration + single network call to Cloudflare Worker `/chat`.
- LLM: Gemini `gemini-2.0-flash` free tier via Worker proxy.
- Hosting/runtime: No application server; static hosting + edge worker only.

## Product Principles

1. Chat is assistive, never primary.
2. Every page must be fully usable without chat.
3. All generated answers must be grounded in retrieved project content.
4. MDX + JSON are source-of-truth content; SQLite is derived artifact.
5. Build and release incrementally by phases; Phase 1 must be independently launchable.
6. Admin capabilities are owner-gated to a single GitHub identity.

## Route Contract

- `/` Landing page
- `/me` About
- `/blogs` Blog list
- `/blogs/:slug` Blog detail
- `/projects` Project list
- `/projects/:slug` Project detail
- `/experience` Interactive timeline (horizontal desktop, vertical mobile)
- `/skills` Skill matrix
- `/contact` Contact
- `/admin` Owner-only admin dashboard

## Data Model Contract

Required SQLite tables:

- `profile`
- `experiences`
- `skills`
- `blogs`
- `projects`
- `site_meta`
- `content_chunks`
- `content_fts` (FTS5 virtual table)
- `chat_logs` (optional)

FTS synchronization must be trigger-driven using insert/delete triggers on `content_chunks`.

## AI Chat Contract

- Intent routing: `CONTENT_QUERY`, `NAVIGATION`, `CONTACT`, `OUT_OF_SCOPE`.
- Retrieval first: query FTS5 locally, build context from top results.
- Generation second: send only `query + retrieved context` to Worker `/chat`.
- One network call per user message.
- Fallback behavior for empty retrieval must be graceful and route-suggestive.

## Security Contract

- Input sanitization required before orchestration.
- Prompt-injection pattern rejection required.
- Cloudflare Worker must enforce CORS allowlist to project domains only.
- Worker must apply rate limiting (`20 requests / 10 minutes / IP`).
- Gemini API key must never be exposed in browser code.
- SQL access in browser remains read-only for visitors.

## Build, Deploy, and Environments

- `npm run build:db`: parse content and generate SQLite + FTS5 index.
- `npm run build`: produce static production bundle.
- GitHub Actions deploy workflow builds DB then site and deploys to `gh-pages`.
- No build-time external API calls in CI.
- Worker secrets are managed only in Cloudflare (`GEMINI_API_KEY`, `GITHUB_CLIENT_SECRET`).

## Engineering Expectations

1. Keep functions focused and deterministic.
2. Use clear, descriptive naming.
3. Prefer straightforward architecture over clever abstractions.
4. Use strict typing where possible.
5. Keep diffs small and focused.
6. Add tests for new behavior when test infrastructure exists.
