# Vanced Work Project Memory

Last updated: 2026-06-30 09:50 KST

## Project Identity

- App name: Vanced Work v24.
- Purpose: shared daily, weekly, and advertiser task management for the team.
- App type: static HTML/CSS/JavaScript app, no build step.
- Firebase project: `vanced-work`.
- GitHub repo: https://github.com/GYH-luke/VANCED-WORK
- GitHub remote: `https://github.com/GYH-luke/VANCED-WORK.git`
- Branch: `main`.
- Local preview: http://127.0.0.1:8765/index.html

## Folder Aliases

Use these aliases to avoid path encoding issues in prompts:

- `SOURCE_WORKSPACE`: local working app folder under the Codex desktop workspace. It contains `index.html`, `Daily_Work_v24.css`, `.firebaserc`, `firebase.json`, `database.rules.json`, and `FIREBASE_SETUP.md`.
- `UPLOAD_WORKSPACE`: child Git folder inside `SOURCE_WORKSPACE`, named `_github_upload_VANCED-WORK`.
- Source of truth for edits: `SOURCE_WORKSPACE`.
- Source of truth for GitHub push: `UPLOAD_WORKSPACE`.
- User preference: place all future requested project artifacts inside `SOURCE_WORKSPACE` unless the user explicitly asks for another location.

When giving the user final results, include clickable links to the actual folders.

## Core Files

- `index.html`: app shell, JavaScript logic, dialogs, Firebase, state, and base inline CSS.
- `Daily_Work_v24.css`: external visual override layer. Prefer this for design-only changes.
- `나눔스퀘어/`: local NanumSquare font folder. Use OTF files only for app fonts; do not add TTF font files to deployment unless explicitly requested.
- `database.rules.json`: Firebase Realtime Database rules.
- `firebase.json` and `.firebaserc`: Firebase project config.
- `FIREBASE_SETUP.md`: Firebase and account setup notes.
- `AGENTS.md`: project operating guide for Codex and subagents.
- `MEMORY.md` and `.codex/MEMORY.md`: memory entry points.
- `docs/OPERATIONS.md`: release and validation workflow.
- `docs/SUBAGENTS.md`: multi-agent workflow.
- `docs/QUALITY_GATE.md`: mandatory self-audit and retry loop before final delivery.
- `docs/RELEASE_CHECKLIST.md`: short final QA checklist.
- `.codex/skills/vanced-work-ops`: project skill.
- Codex operating bundle for GitHub sync: `.codex/`, `agents/`, `docs/`, `MEMORY.md`, `PROJECT_MEMORY.md`, and `AGENTS.md`.

## Current Product Decisions

- Header brand is `Vanced Work`.
- The old `D` icon and subtitle are removed.
- Daily `Today` navigation should show today's date by D1.
- Weekly is fixed to business-week D5: Monday through Friday only. `Today` opens the current Monday-Friday range, and previous/next navigation moves by one calendar week.
- Advertiser `Today` navigation should start at today by D7.
- Weekly assignee columns stay compact, while focus columns use a wider layout with normal-weight 12px text and automatic vertical growth.
- Routine tasks support `Daily`, `Weekly Mon` through `Weekly Fri` routine-day values. In the Daily tab, routine task rows are shown only when the task start date is today's date; future or past routine copies in a multi-day range must stay hidden.
- Daily table sorting supports stacked criteria. Clicking a sort header promotes it to the primary sort while preserving previous criteria as secondary sorts, such as start date ascending plus time ascending. Clicking an active sort header toggles ascending and descending in both directions.
- Daily task rows include a left move handle. Dragging uses a real placeholder row plus FLIP row motion, so nearby rows visibly move aside while the task is reordered within the same advertiser or moved into another advertiser section. The saved data uses a manual `order` value.
- Change history is admin-only.
- Global delete-all functionality is removed.
- Clicking selected `My tasks` toggles back to all assignees.
- Weekly and Advertiser tasks open the same edit dialog as Daily tasks.
- Daily delete undo popup appears beside the affected task.
- Unassigned tasks and routines must not appear in any tab.
- Weekly quick add defaults to the logged-in user unless the selected advertiser has existing tasks from other assignees.
- Weekly advertiser filter has an `All advertisers` reset plus a multi-select button.
- Edit dialog includes a trash icon for deletion.
- Daily advertiser sections have a plus button that opens an `Add task` dialog with advertiser prefilled.
- Daily advertiser plus button should match the completion-check color and alignment.
- Daily task rows use subtle whole-row odd/even backgrounds for readability. Do not reintroduce separate schedule/work/control value background grouping in the Daily list.
- Daily table category headers should stay centered to the actual value columns, including the task-title column.
- Daily routine-day wording is `반복`, not `루틴요일`.
- The app font should load NanumSquare from `나눔스퀘어/NanumSquareOTF_acL/R/B/EB.otf`.

## User Preferences

- Always include changed file links and folder links in final responses.
- Always include local preview and GitHub repo links after completed app work.
- Always create requested files, docs, helper folders, and working artifacts inside the existing Vanced Work source folder.
- Always upload the full Codex operating bundle to GitHub with app releases, so other PCs have current memory, agents, skills, and markdown docs.
- For every user-requested app change, update footer timestamp in `index.html` using current KST.
- If CSS changes, bump the `Daily_Work_v24.css?v=...` cache query in `index.html`.
- Use the local `나눔스퀘어` folder for fonts and select OTF files only.
- Keep final explanations concise and practical.
- User is non-developer; explain operational choices plainly when asked.
- After every requested change, self-check whether the fix fully solves the reported problem. If not, infer the exact likely cause, patch again, and rerun verification before saying the work is complete.

## Firebase And Data Model

- Login email format: `{id}@vanced-work.app`.
- Admin user id: `luke`.
- Member access is controlled by `/members/{uid}/active === true`.
- Main Realtime Database paths:
  - `workspace`
  - `history`
  - `members`
- Workspace structure:
  - `workspace/tasks`
  - `workspace/routines`
  - `workspace/weeklyFocus`
  - `workspace/advertisers`
  - `workspace/meta/schemaVersion = 2`
- Active members can share workspace data.
- History should remain visible only to Luke/admin in UI and rules.
- Do not store passwords, tokens, or private credentials in memory.

## Local Storage And Migration

Known local keys:

- `adwork.todo.tasks.v5`
- `adwork.todo.routines.v5`
- `adwork.weeklyFocus.v1`
- `adwork.advertisers.v1`
- `adwork.cloudMigration.v1`

Rules:

- Treat migration code as production-risk work.
- Do not delete browser data casually.
- Do not reset `adwork.cloudMigration.v1` casually; it can cause duplicate migration or data loss.
- Import/load actions can replace state and sync to Firebase. Export JSON backup first.
- Protect `LOCAL_MIGRATION_EXCLUDED_TASKS`.

## Standard Work Loop

1. Read `PROJECT_MEMORY.md`, `AGENTS.md`, and any relevant role file in `agents/`.
2. Inspect relevant code with `rg` and targeted reads.
3. Apply the smallest safe patch.
4. Update footer timestamp when app files changed by user request.
5. Validate inline JavaScript syntax in `index.html`.
6. Verify UI behavior locally when the change touches layout or interaction.
7. Run the `docs/QUALITY_GATE.md` self-audit. If anything fails, diagnose the likely root cause, patch again, and repeat validation.
8. Copy deploy files to `UPLOAD_WORKSPACE`.
9. Copy the full Codex operating bundle to `UPLOAD_WORKSPACE` for every GitHub upload, even when the user only changed app files.
10. Commit and push from `UPLOAD_WORKSPACE` when requested.
11. Update memory, agent, subagent, skill, and markdown documentation when rules, workflows, or product decisions changed.
12. Final response includes changed files, folders, local URL, GitHub URL, and verification result.

## GitHub Workflow

Use `UPLOAD_WORKSPACE` for Git operations:

```powershell
git fetch origin
git status --short --branch
git add index.html Daily_Work_v24.css
git commit -m "Update Vanced Work task management"
git push origin main
```

For documentation-only releases, add the changed docs and skill files instead of app deploy files.

For any GitHub upload, also stage the full Codex operating bundle: `.codex/`, `agents/`, `docs/`, `MEMORY.md`, `PROJECT_MEMORY.md`, and `AGENTS.md`. This keeps company PCs and personal PCs on the same working context.

If `git push` says `Everything up-to-date`, GitHub already matches local.

If push is rejected with `fetch first`, fetch, inspect remote changes, and rebase only when safe.

## Common Risk Areas

- `renderDaily`, `renderDailyGroups`, `renderTaskRow`
- Daily advertiser section plus button and add/edit dialog flow
- `renderWeekly`, weekly quick add, weekly advertiser filter
- Advertiser view grouping, order, edit, and delete flows
- `normalizeTask`, `save`, `syncWorkspaceChanges`, Firebase listeners
- Delegated `data-action` click/change handlers
- Import/export and migration paths
- `LOCAL_MIGRATION_EXCLUDED_TASKS`
