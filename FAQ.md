# Frequently Asked Questions

## Product

**Q: What is this project?**  
A: `madhurahuja.com` is a portfolio + blog with an AI-assisted knowledge interface grounded in local site content.

**Q: Is chat required to use the site?**  
A: No. Every route must remain fully functional without chat.

## Architecture

**Q: Why SQLite in the browser?**  
A: The dataset is small, mostly read-only, and fits well into a static `site.db` loaded by `sql.js` WASM.

**Q: Why FTS5 instead of vector search now?**  
A: FTS5 gives zero-cost, local retrieval with good performance. Vector search is deferred to a future phase only if needed.

**Q: Why Cloudflare Worker if retrieval is local?**  
A: The Worker protects secrets, enforces CORS/rate limits, and proxies the final Gemini generation request.

## Operations

**Q: Are there build-time external API calls?**  
A: No. CI runs `build:db` and `build` without external API dependencies.

**Q: Where are secrets stored?**  
A: Only in Cloudflare Worker secrets (`GEMINI_API_KEY`, `GITHUB_CLIENT_SECRET`).

**Q: What is the recurring hosting cost?**  
A: `$0/month` by design and policy.
