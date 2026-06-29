# Vanced Work Quality Gate

Use this before saying a requested change is complete.

## Rule

Do not stop at "files changed." The result must be checked against the user's actual problem.

If the result is not correct, do not hand it back as done. Infer the most likely root cause, patch again, and rerun verification.

## Required Self-Audit

1. Restate the user's requested behavior in one sentence.
2. Identify the files and functions that control that behavior.
3. Confirm the patch touches the right layer: behavior in `index.html`, visual polish in `Daily_Work_v24.css`, documentation in markdown.
4. Run syntax or static checks for changed code.
5. Verify locally when the change affects UI, drag/drop, dialogs, filters, Firebase sync, or navigation.
6. Check for regression risks in nearby flows.
7. Confirm footer timestamp and CSS cache query when app files changed.
8. Confirm source and upload folders match before GitHub release.
9. Confirm the full Codex operating bundle is included in every GitHub upload, not only app deploy files.
10. Confirm memory, agents, subagents, skills, and markdown docs were updated when durable rules or product decisions changed.

## Failure Loop

When verification fails:

1. Capture the observed failure.
2. Infer the likely root cause from code and browser state.
3. Patch the smallest safe surface.
4. Rerun the same verification that failed.
5. Add one targeted regression check for the nearby behavior.

Only report completion after the failed check passes or after a real blocker is identified.

## Evidence To Report

Final responses should include the highest-signal evidence:

- validation performed
- local behavior checked
- source/upload sync state
- commit and push state when applicable
- remaining risk, if any
