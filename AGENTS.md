# Vanced Work Agent Guide

Use this file before modifying Vanced Work.

## Required Context

- Read `PROJECT_MEMORY.md`.
- Read `FIREBASE_SETUP.md` and `database.rules.json` when touching auth, users, Firebase, history, import/export, or migration.
- Read the relevant role document in `agents/`.
- For implementation tasks, inspect `index.html` and `Daily_Work_v24.css` before editing.
- For browser checks, use the in-app browser skill when the user explicitly mentions the browser plugin or when localhost verification is needed.
- Create future requested project artifacts inside the existing Vanced Work source folder unless the user explicitly asks otherwise.
- Read `docs/QUALITY_GATE.md` before final delivery.
- For fonts, use OTF files from `나눔스퀘어`; do not deploy TTF files unless explicitly requested.
- For every GitHub upload, include the full Codex operating bundle: `.codex/`, `agents/`, `docs/`, `MEMORY.md`, `PROJECT_MEMORY.md`, and `AGENTS.md`.
- After code changes are complete and validated, summarize what changed and wait for explicit user confirmation before any commit or push.

## Standard Work Loop

1. Inspect the requested area.
2. Patch the smallest safe surface.
3. Update footer timestamp in `index.html` to current KST when app files change.
4. Validate JavaScript syntax.
5. Verify the actual behavior or document why live verification was impossible.
6. Run the quality gate: if the result is not fully correct, infer the root cause, patch again, and repeat validation.
7. Copy deploy files to `_github_upload_VANCED-WORK` when app deploy files changed.
8. Copy the full Codex operating bundle to `_github_upload_VANCED-WORK` for every GitHub upload.
9. Before commit or push, summarize the changed files, behavior, and verification for the user and wait for explicit confirmation.
10. Commit or amend in the upload folder only after confirmation.
11. Push or verify `Everything up-to-date` only after confirmation.
12. Update memory, agents, subagents, skills, and markdown docs when project rules or durable decisions changed.
13. Final response includes changed files, folders, local URL, and GitHub URL.

## File Ownership

- `index.html`: behavior, markup, dialogs, Firebase, state, core CSS.
- `Daily_Work_v24.css`: visual overrides and responsive polish.
- `나눔스퀘어/*.otf`: NanumSquare font assets used by the app.
- `agents/*.md`: role-specific operating prompts.
- `.codex/skills/vanced-work-ops`: reusable Codex skill for this project.
- Codex operating bundle: `.codex/`, `agents/`, `docs/`, `MEMORY.md`, `PROJECT_MEMORY.md`, and `AGENTS.md`.
- GitHub Pages uses `index.html` and `Daily_Work_v24.css`.
- Firebase rules/config are maintained separately from GitHub Pages deployment.

## Safety Rules

- Never remove Firebase sync, auth, history, or migration logic unless explicitly requested.
- Never reintroduce unassigned tasks or routines.
- Never change GitHub remote without user confirmation.
- Avoid global redesign when the user asks for a local fix.
- Preserve `data-action` delegated event patterns.
- Prefer `apply_patch` for file edits.
- Do not edit `_github_upload_VANCED-WORK` as the source of truth for app changes. It lives inside the source workspace only as the GitHub upload clone; copy validated app files into it.
- Treat Firebase workspace data as production data.
- Back up JSON before import, migration, or data cleanup work.
- Do not mark work complete when the self-audit fails. Fix the failure or clearly report the blocker.

## Agent Collaboration

For multi-agent work:

- Feedback agent diagnoses the issue and prioritizes risk.
- Web development agent proposes or applies the scoped fix.
- Feedback agent reviews the revised result.
- Web development agent finalizes.
- Release manager verifies sync, commit, push, links, and the quality gate.

Keep agent rounds short, specific, and tied to the current user request.

## Useful Links

- GitHub: https://github.com/GYH-luke/VANCED-WORK
- Local app: http://127.0.0.1:8765/index.html
