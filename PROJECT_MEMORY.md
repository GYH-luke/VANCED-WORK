# Vanced Work Project Memory

Last updated: 2026-07-02 11:17 KST

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
- Editable Weekly focus fields include a strikethrough button. Selecting focus text and pressing it toggles strikethrough, persists the formatting through Firebase, preserves legacy plain-text focus values, and permits only sanitized strikethrough and line-break markup.
- Routine tasks support `Daily`, `Weekly Mon` through `Weekly Fri` routine-day values. In the Daily tab, routine rows are anchored to the real current date and must remain visible for today's valid routine instances regardless of the selected date range filter.
- Deleting one generated routine task instance records that date in the parent routine's `skippedDates` map, so `materializeRoutinesForDate` must not recreate the same instance on render, reload, or Firebase sync. Undoing the deletion clears that skipped date and restores the task.
- Generated routine task IDs are deterministic per `routineId + date`, preventing concurrent tabs or devices from creating duplicate instances. Deleting a legacy duplicated routine instance removes every record for that same routine and date in one action, while ordinary tasks still delete individually.
- Firebase snapshots, local cache loads, and saves automatically collapse legacy routine task duplicates with the same `routineId + date`. The survivor preserves the most advanced status first, then the most recently updated record; ordinary tasks and routine instances on different dates are never merged.
- Every view must suppress a routine task whose parent routine marks that task date in `skippedDates`, even if an older open tab or PC writes a stale task record back to Firebase. The tombstone is authoritative; a stale task record must never make the deleted occurrence visible again.
- Daily table sorting supports stacked criteria. Clicking a sort header promotes it to the primary sort while preserving previous criteria as secondary sorts, such as start date ascending plus time ascending. Clicking an active sort header toggles ascending and descending in both directions.
- When Daily tasks have the same start date and start time, sorting must place the earlier deadline date first, and when the deadline date also matches, the earlier end/deadline time first.
- Changing a task start date keeps the existing end date when that end date is the same as or later than the new start date. Only an end date that would fall before the new start date is moved forward to match the new start date.
- Daily AI uses the real current date and the current Monday-Friday business-week start-date range. It includes assigned non-routine tasks active today, including completed tasks, only when their start date is in that current work list. Routine tasks appear only for a valid instance whose start date is the real current date, with semantic duplicates collapsed to one. Unassigned tasks and task times are omitted from the output.
- Daily AI output groups tasks by advertiser and inserts one blank line between advertiser groups for copy-ready readability.
- Daily task rows include a left move handle. Dragging uses a real placeholder row plus FLIP row motion, so nearby rows visibly move aside while the task is reordered within the same advertiser or moved into another advertiser section. The saved data uses a manual `order` value.
- Change history is admin-only.
- Global delete-all functionality is removed.
- Clicking selected `My tasks` toggles back to all assignees.
- Weekly and Advertiser tasks open the same edit dialog as Daily tasks.
- Daily delete undo popup appears beside the affected task.
- Unassigned tasks and routines must not appear in any tab.
- Weekly quick add defaults to the logged-in user unless the selected advertiser has existing tasks from other assignees.
- Weekly advertiser filter has an `All advertisers` reset plus a multi-select button.
- Weekly advertiser filters include a `My advertisers` mode between `All advertisers` and multi-select. It shows advertisers that contain at least one task assigned to the logged-in user within the currently filtered Monday-Friday week, while preserving all assignee rows inside those advertisers.
- Weekly tab includes a `완료 숨김` toggle in the filter bar, matching the Daily tab control and hiding completed weekly cards when enabled.
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
- For code changes, after editing and validation but before any commit or push, summarize exactly what changed and wait for user confirmation. Do not commit or push code changes until the user confirms that summary.

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
- Routine records may include `skippedDates`, a date-key map used to suppress regeneration of a deleted routine instance for a specific date.
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
8. Summarize changed files, behavior, and verification for the user, then wait for explicit confirmation before any commit or push.
9. Copy deploy files to `UPLOAD_WORKSPACE`.
10. Copy the full Codex operating bundle to `UPLOAD_WORKSPACE` for every GitHub upload, even when the user only changed app files.
11. Commit and push from `UPLOAD_WORKSPACE` only after the user confirms the pre-release summary.
12. Update memory, agent, subagent, skill, and markdown documentation when rules, workflows, or product decisions changed.
13. Final response includes changed files, folders, local URL, GitHub URL, and verification result.

## GitHub Workflow

Use `UPLOAD_WORKSPACE` for Git operations. Before running `git commit` or `git push` for code changes, report what changed and the verification result to the user, then wait for explicit confirmation.

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
