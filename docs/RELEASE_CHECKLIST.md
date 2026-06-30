# Vanced Work Release Checklist

Use this before final delivery.

- [ ] User request is fully addressed.
- [ ] Quality gate completed: the result was checked against the actual user problem.
- [ ] If verification failed at first, the root cause was inferred, fixed, and the failed check was rerun.
- [ ] App footer timestamp updated in KST when app files changed.
- [ ] CSS cache query bumped when `Daily_Work_v24.css` changed.
- [ ] Font deployment uses OTF files only when font assets changed.
- [ ] JavaScript syntax validation passed when `index.html` changed.
- [ ] Local preview checked when UI or interaction changed.
- [ ] Code-change summary reported to the user before commit/push, and explicit user confirmation received.
- [ ] Deploy files copied to `_github_upload_VANCED-WORK` when needed.
- [ ] Full Codex operating bundle copied and staged for GitHub upload: `.codex/`, `agents/`, `docs/`, `MEMORY.md`, `PROJECT_MEMORY.md`, `AGENTS.md`.
- [ ] New requested artifacts are inside the existing source folder unless explicitly requested otherwise.
- [ ] Source/upload hashes or diffs checked when deploy files changed.
- [ ] Git status reviewed before commit/push.
- [ ] GitHub push completed or `Everything up-to-date` confirmed.
- [ ] Memory, agent, subagent, skill, and markdown documentation updated when durable rules or product decisions changed.
- [ ] Final answer includes file links, folder links, local preview, and GitHub repo.
