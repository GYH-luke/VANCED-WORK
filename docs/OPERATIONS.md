# Vanced Work Operations

## Quick Start

1. Work in `SOURCE_WORKSPACE`.
2. Preview at http://127.0.0.1:8765/index.html.
3. Push through `UPLOAD_WORKSPACE`, the child folder named `_github_upload_VANCED-WORK`.
4. Put future project artifacts inside `SOURCE_WORKSPACE` unless the user explicitly asks otherwise.

## Before Editing

- Confirm whether the change is visual, behavior, Firebase, or GitHub-related.
- Prefer `Daily_Work_v24.css` for styling.
- Use `index.html` for behavior, markup, dialogs, footer timestamp, and state.
- Read `FIREBASE_SETUP.md` and `database.rules.json` before Firebase, history, account, import, export, or migration changes.

## After Editing App Files

Run JavaScript syntax validation from `SOURCE_WORKSPACE`:

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

If CSS changed, confirm the `Daily_Work_v24.css?v=...` cache query in `index.html` was bumped.

## Sync To Git Upload Folder

Copy deploy files from `SOURCE_WORKSPACE` to `UPLOAD_WORKSPACE`:

```powershell
Copy-Item -LiteralPath .\index.html -Destination .\_github_upload_VANCED-WORK\index.html -Force
Copy-Item -LiteralPath .\Daily_Work_v24.css -Destination .\_github_upload_VANCED-WORK\Daily_Work_v24.css -Force
```

Then run Git commands from `UPLOAD_WORKSPACE`:

```powershell
git fetch origin
git status --short --branch
git add index.html Daily_Work_v24.css
git commit -m "Update Vanced Work task management"
git push origin main
```

For documentation-only updates, add the changed documentation, agent, and skill files.

## Common Git Errors

- `Authentication failed`: Git credential is wrong. Clear Windows credential for `git:https://github.com`, then push again.
- `Password authentication is not supported`: use GitHub token or Git Credential Manager browser login.
- `fetch first`: remote has newer commits. Fetch, inspect, then rebase/align safely.
- `Everything up-to-date`: GitHub already has the same content.

## Final Response Requirements

Always include:

- changed files
- changed folders
- local preview link
- GitHub repo link
- verification or reason verification was skipped
