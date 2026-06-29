# Vanced Work Memory

Active memory file:

- `PROJECT_MEMORY.md`

Also read:

- `AGENTS.md`
- `docs/OPERATIONS.md`
- `docs/QUALITY_GATE.md`
- relevant files in `agents/`

Use folder aliases from `PROJECT_MEMORY.md` to avoid path encoding issues in prompts.

Before final response, self-audit the result against the user request. If the audit fails, diagnose the root cause, fix it, and verify again.

Every GitHub upload must include the full Codex operating bundle: `.codex/`, `agents/`, `docs/`, `MEMORY.md`, `PROJECT_MEMORY.md`, and `AGENTS.md`.
