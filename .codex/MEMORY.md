# Vanced Work Memory

Active memory file:

- `PROJECT_MEMORY.md`

Also read:

- `AGENTS.md`
- `docs/OPERATIONS.md`
- `docs/QUALITY_GATE.md`
- relevant files in `agents/`

Use folder aliases from `PROJECT_MEMORY.md` to avoid path encoding issues in prompts.

Generated routine task deletions depend on the parent routine's `skippedDates` map. Preserve this field so a deleted routine date instance is not recreated after render, reload, or Firebase sync.

Before final response, self-audit the result against the user request. If the audit fails, diagnose the root cause, fix it, and verify again.

Every GitHub upload must include the full Codex operating bundle: `.codex/`, `agents/`, `docs/`, `MEMORY.md`, `PROJECT_MEMORY.md`, and `AGENTS.md`.
