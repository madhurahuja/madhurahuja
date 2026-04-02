# Domain Glossary

- **Assistive Chat**: Chat UX that helps discovery but does not replace primary page navigation.
- **FTS5**: SQLite full-text search engine used for local retrieval over `content_chunks`.
- **content_chunks**: Canonical chunk table containing indexed text segments from blogs/projects/profile/etc.
- **content_fts**: FTS5 virtual table synchronized with `content_chunks` via triggers.
- **sql.js**: WebAssembly SQLite runtime used for in-browser read-only queries.
- **Retrieval-First**: Policy requiring local search/context assembly before LLM generation.
- **Grounded Response**: Answer generated only from retrieved site context.
- **Supervisor Agent**: Client-side classifier that sanitizes input and routes chat intent.
- **Guardrail Agent**: Client-side refusal/safety handler for out-of-scope or unsafe requests.
- **Owner Sovereignty**: Principle that `/admin` access is gated to one GitHub identity (project owner).
- **Derived Artifact**: Build output generated from source-of-truth content (e.g., `site.db`).
- **Source of Truth**: Authoritative editable content in Git (`/content` MDX + JSON data files).
