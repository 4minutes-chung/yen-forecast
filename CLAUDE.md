# Yen forecasting research repo

Goal: out-of-sample forecasts of USD/JPY log changes, always evaluated against a random-walk benchmark.

Always-loaded context:
@docs/PROTOCOL.md
@docs/DECISIONS.md

Before starting any task, read the last 3 entries of docs/LOG.md.

## Rules
- Do only the task in the brief you are given. One task per session. If something outside scope is needed, stop and report it instead of doing it.
- Never edit docs/PROTOCOL.md or docs/DECISIONS.md unless the brief explicitly says to.
- No look-ahead. Every forecast uses only information available at the forecast date: no full-sample scaling or standardization, no forward-filling from future dates, and macro data lagged to its actual release date.
- Every model result is reported next to the random-walk benchmark on the identical sample and dates.
- Never commit data files, credentials, API keys, or licensed data (Bloomberg, CEIC, Wind). Data enters only through scripts in src/.
- Never access or edit Notion.
- Never delete files, rewrite git history, or force-push without Steven's explicit approval.
- If uncertain, stop and ask. Do not guess.

## After every task
1. Append to docs/LOG.md: date, task id, what changed, files touched, results (numbers with sample period), deviations from the brief, open issues.
2. Commit with message "task NNN: <short description>" and push.
3. Print this handoff block:

HANDOFF task NNN
Status: done / partial / blocked
Changed files:
Commands run and outcome:
Results (numbers, sample period):
Deviations from brief:
Open issues:
