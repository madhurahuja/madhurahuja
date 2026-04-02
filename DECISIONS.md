# Architecture Decision Records (ADR)

## ADR-001: Static-First Deployment
- **Context**: Need reliable hosting at `$0/month` with custom domain.
- **Decision**: Deploy SPA to GitHub Pages; avoid application server.
- **Consequences**: Low ops burden and no runtime server costs; SPA routing requires hash mode or 404 fallback strategy.

## ADR-002: Browser-Side Data Access
- **Context**: Site content is small and mostly read-only for visitors.
- **Decision**: Use build-time SQLite + runtime `sql.js` WASM in browser.
- **Consequences**: Fast local queries after initial load; write operations move to admin/build workflows.

## ADR-003: Retrieval with SQLite FTS5
- **Context**: Need zero-cost search and AI grounding without paid vector infrastructure.
- **Decision**: Use `content_chunks` + `content_fts` (FTS5) with trigger-based sync.
- **Consequences**: Excellent keyword retrieval; semantic edge cases deferred to optional future embeddings phase.

## ADR-004: Lightweight Agent Orchestration
- **Context**: Chat orchestration needed in client with minimal dependency overhead.
- **Decision**: Implement custom TypeScript orchestrator (supervisor/retrieval/navigation/contact/guardrail).
- **Consequences**: Small footprint and full control; requires disciplined local contracts and testing.

## ADR-005: Single-Call AI Proxy
- **Context**: Need key protection, CORS control, and rate limiting for Gemini usage.
- **Decision**: Cloudflare Worker handles `/chat` and `/github-auth`; browser performs local retrieval then one Worker call.
- **Consequences**: API key remains secret; predictable chat flow; worker complexity stays contained.

## ADR-006: Admin Ownership Model
- **Context**: Admin must be owner-only with minimal attack surface.
- **Decision**: GitHub OAuth identity gate + GitHub API commit flow for metadata updates.
- **Consequences**: Strong ownership boundary; edits are auditable through repository history and CI.
