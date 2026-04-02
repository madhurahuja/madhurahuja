# Tooling & Integrations / Skills

## Primary Tooling Contracts

1. `read_file`
	- First choice for inspecting source-of-truth docs and configs.
2. `apply_patch`
	- Required for deterministic edits in this workspace.
3. `run_in_terminal`
	- Use for build/test/verification commands and admin scripts.
4. `get_errors`
	- Validate edits quickly after file changes.

## Architecture-Aware Usage

- Prefer static analysis of local files before proposing changes.
- Preserve retrieval-first and static-hosting constraints when suggesting tools or architecture.
- Use minimal tool calls and avoid unnecessary scans.
- Keep context bounded to active files to reduce token and reasoning noise.

## Domain Skills

- If skill instructions exist under `SKILLS/`, treat them as mandatory overlays.
- For review workflows, apply reviewer standards from `agents/code-reviewer.yml` and `AGENTS.md`.

## Safety Rules for Tool Use

- Never expose secrets in terminal output or committed files.
- Never run destructive commands unless explicitly requested.
- Validate command intent against project constraints before execution.
