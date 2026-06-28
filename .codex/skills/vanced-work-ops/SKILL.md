---
name: vanced-work-ops
description: Vanced Work task-management app operations skill. Use when editing, debugging, validating, documenting, or deploying the Daily/Vanced Work app, including Firebase account-based history, Daily/Weekly/Advertiser tab behavior, GitHub upload workflow, footer timestamp updates, and project agent coordination.
---

# Vanced Work Ops

## Required First Reads

Read these before edits:

- `PROJECT_MEMORY.md`
- `AGENTS.md`
- `FIREBASE_SETUP.md` and `database.rules.json` when touching Firebase, auth, history, import, export, or migration.
- Relevant role file in `agents/`.

Read these when needed:

- `docs/OPERATIONS.md` for validation and GitHub workflow.
- `docs/SUBAGENTS.md` for feedback/web/designer/release agent coordination.
- `docs/RELEASE_CHECKLIST.md` before final delivery.

## Project Map

- Source workspace alias: `SOURCE_WORKSPACE`.
- Upload workspace alias: `UPLOAD_WORKSPACE`, the child Git folder inside `SOURCE_WORKSPACE` named `_github_upload_VANCED-WORK`.
- GitHub: https://github.com/GYH-luke/VANCED-WORK
- Local preview: http://127.0.0.1:8765/index.html
- Main app: `index.html`.
- CSS override layer: `Daily_Work_v24.css`.
- GitHub Pages deploy files: `index.html`, `Daily_Work_v24.css`.
- Firebase project: `vanced-work`.
- RTDB production paths: `workspace`, `history`, `members`.

## Non-Negotiable Rules

- Update footer timestamp in `index.html` for every user-requested app change using current KST.
- If CSS changes, bump the `Daily_Work_v24.css?v=...` cache query in `index.html`.
- Prefer `Daily_Work_v24.css` for visual changes.
- Preserve Firebase sync, auth, history, and migration logic unless explicitly requested.
- Never reintroduce unassigned tasks or routines.
- Keep final responses short and include file links, folder links, local preview, and GitHub link.
- Treat localStorage migration keys and Firebase workspace data as production-risk surfaces.
- Do not store passwords, tokens, or secrets in memory or docs.
- Put future requested project artifacts inside `SOURCE_WORKSPACE` unless the user explicitly asks for another location.

## Standard Workflow

1. Inspect relevant code with `rg` and targeted reads.
2. Apply the smallest safe patch.
3. Validate inline JavaScript syntax in `index.html`.
4. Verify local behavior when the change touches UI or interactions.
5. Copy deploy files to the upload folder when app deploy files changed.
6. Compare source/upload hashes or diffs.
7. In the upload folder, fetch remote, commit or amend, and push when requested.
8. Report changed files, folders, commit or push state, and links.

## Validation Command

Run from `SOURCE_WORKSPACE`:

```powershell
@'
const fs = require('fs');
const html = fs.readFileSync('index.html', 'utf8');
const blocks = [...html.matchAll(/<script>([\s\S]*?)<\/script>/g)].map(m => m[1]);
let ok = true;
for (const [i, code] of blocks.entries()) {
  try { new Function(code); }
  catch (err) { ok = false; console.error(`Script block ${i + 1} syntax error: ${err.message}`); }
}
if (!ok) process.exit(1);
console.log(`JavaScript syntax OK (${blocks.length} inline blocks)`);
'@ | node -
```

## GitHub Sync

Use `UPLOAD_WORKSPACE` for Git operations:

```powershell
git fetch origin
git status --short --branch
git add index.html Daily_Work_v24.css
git commit -m "Update Vanced Work task management"
git push origin main
```

For documentation-only updates, add the changed documentation, agent, and skill files.

If `git push` says `Everything up-to-date`, GitHub already matches local.

## Common Risk Areas

- `renderDaily`, `renderDailyGroups`, `renderTaskRow`
- `renderWeekly`, weekly resize, weekly quick add
- advertiser filtering, order, and delete flows
- `normalizeTask`, `save`, `syncWorkspaceChanges`, Firebase listeners
- delegated `data-action` click/change handlers
- `LOCAL_MIGRATION_EXCLUDED_TASKS`
- localStorage keys beginning with `adwork.`
