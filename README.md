# madhurahuja.com — Claude Context Pack

This repository stores AI-facing configuration and context files for the `madhurahuja.com` project.

## Primary Files

- `CLAUDE.md` — project identity, architecture contract, constraints, and engineering expectations
- `AGENTS.md` — top-level dev agents + runtime chat agent responsibilities
- `SYSTEM_PROMPT.md` — persistent behavior contract for coding agents
- `GUARDRAILS.md` — non-negotiable safety/security/product constraints
- `CONTEXT/ARCHITECTURE.md` — system architecture and data flow reference
- `DECISIONS.md` — architecture decision records (ADRs)

## Project Principles

1. `$0/month` recurring cost ceiling.
2. Static-first architecture (GitHub Pages + browser SQL + Cloudflare Worker).
3. Retrieval-grounded AI answers only.
4. Chat is assistive and never replaces core navigation.

## Notes

- This context pack is intended to guide implementation agents and reviewers.
- Keep all updates consistent with route, security, and deployment contracts in `CLAUDE.md`.