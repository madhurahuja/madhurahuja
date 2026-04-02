# System Architecture — madhurahuja.com

## High-Level Runtime Model

1. Static React SPA (Vite + TypeScript) is served from GitHub Pages.
2. Browser loads `site.db` once and queries it locally via `sql.js` (WASM).
3. Chat flow runs primarily client-side (sanitize → classify intent → local FTS5 retrieval → context build).
4. Only one network call per content question is made to Cloudflare Worker `/chat`.
5. Worker forwards grounded prompt/context to Gemini `gemini-2.0-flash` and returns response.

## Components

- **Frontend/Client**
	- React 18 SPA with route contract:
		- `/`, `/me`, `/blogs`, `/blogs/:slug`, `/projects`, `/projects/:slug`, `/experience`, `/skills`, `/contact`, `/admin`
	- Chat UI: FAB + drawer (assistive overlay, not primary navigation)
	- Experience page: interactive timeline (horizontal desktop, vertical mobile)

- **Database / Content Layer**
	- Build-time SQLite generation from MDX + JSON source content
	- Runtime read-only access via `sql.js`
	- Search tables: `content_chunks` + `content_fts` (FTS5)
	- Trigger-based FTS synchronization on inserts/deletes

- **Edge API Layer (Cloudflare Worker)**
	- `POST /chat`: grounded answer generation proxy
	- `POST /github-auth`: GitHub OAuth code exchange
	- CORS allowlist + rate limiting + secrets isolation

## Data Flow

### Site Rendering Flow

1. Visitor opens SPA route.
2. App loads local `site.db` through `sql.js`.
3. UI queries structured tables (`profile`, `experiences`, `skills`, `blogs`, `projects`, `site_meta`) for rendering.

### Chat Flow (Retrieval-First)

1. User message enters chat drawer.
2. Input sanitizer validates and cleans query.
3. Supervisor classifies intent (`CONTENT_QUERY`, `NAVIGATION`, `CONTACT`, `OUT_OF_SCOPE`).
4. For content queries, local FTS5 search runs over `content_fts`.
5. Top results build context payload.
6. Browser sends one `POST /chat` request with `query + context`.
7. Worker calls Gemini and returns concise grounded answer with citations.

## Security & Guardrails

- Input sanitization and prompt-injection checks are mandatory before orchestration.
- Worker accepts only allowed origins for CORS.
- Worker applies `20 requests / 10 minutes / IP` rate limit.
- API keys remain only in Worker secrets; never exposed client-side.
- Browser-side DB is read-only for visitors.

## Trade-offs

- **Chosen**: FTS5 keyword retrieval (zero-cost, local, fast)
	- **Trade-off**: weaker semantic recall than vector search for terminology mismatch.
- **Chosen**: Static hosting with SPA
	- **Trade-off**: client-side routing needs hash strategy or 404 fallback workaround on GitHub Pages.
- **Chosen**: Lightweight custom agent graph
	- **Trade-off**: less framework automation, but lower complexity and dependency weight.

## Scaling Strategy

- Keep all core features within free tiers and static architecture.
- Optimize chunking/tags for FTS quality before introducing embeddings.
- Introduce optional hybrid/vector retrieval only when miss-rate justifies (future Phase 5).
- Maintain incremental phase delivery so each phase is independently deployable.
