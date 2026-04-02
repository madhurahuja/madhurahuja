# Multi-Agent System Configuration

## Top-Level Development Agents

1. **Router Agent**
	- Analyzes request intent.
	- Routes tasks to implementation or review paths.
	- Ensures scope matches current phase and non-negotiables.

2. **Coder Agent**
	- Executes focused codebase changes.
	- Implements retrieval-first, static-first architecture.
	- Preserves $0/month operational model.

3. **Reviewer Agent**
	- Reviews diffs for correctness, security, and architectural alignment.
	- Verifies no violations of guardrails.
	- Emits explicit status: `APPROVED` or `CHANGES_REQUESTED`.

## Product Runtime Agents (Chat)

1. **Supervisor Agent (browser)**
	- Sanitizes and classifies user intent.
	- Routes to retrieval/navigation/contact/guardrail.

2. **Retrieval Agent (browser + worker)**
	- Executes local FTS5 search via sql.js.
	- Builds context and performs single Worker `/chat` call.

3. **Navigation Agent (browser)**
	- Maps keywords to known routes.

4. **Contact Agent (browser)**
	- Routes users to `/contact` and social channels.

5. **Guardrail Agent (browser)**
	- Handles out-of-scope requests with refusal + guided follow-up.

## Responsibilities & Boundaries

- Coder Agent must not commit or release without Reviewer Agent `APPROVED` status.
- Keep contexts bounded; include only files relevant to active task.
- Never route user requests to features that violate zero-cost constraint.
- Never bypass input sanitization before intent classification.
- Never bypass retrieval grounding when generating content answers.
