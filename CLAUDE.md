# Yen forecasting research repo

Goal:
- Target 0 (umbrella): forecast and study USD/JPY. Forecasts are out-of-sample and always evaluated against a random-walk benchmark.
- Target 1 (current): how did USD/JPY's daily sensitivity to the JP 2Y yield change from YCC (2016) through the dismantling (2022-24) to after the exit (2024-26), and does the post-2025 decoupling from the 10Y differential also show at 2Y? Descriptive, not causal.
- Key use: insight into USD/JPY changes over the next 0-2 years.

Always-loaded context:
@docs/PROTOCOL.md
@docs/DECISIONS.md

Before starting any task, read the last 3 entries of docs/LOG.md.

## Rules
- Do only the task in the brief you are given. One task per session. If something outside scope is needed, stop and report it instead of doing it.
- Never edit docs/PROTOCOL.md or docs/DECISIONS.md unless the brief explicitly says to.
- No look-ahead. Every forecast uses only information available at the forecast date: no full-sample scaling or standardization, no forward-filling from future dates, and macro data lagged to its actual release date.
- Every forecast result (Target 0) is reported next to the random-walk benchmark on the identical sample and dates.
- Target 1 results are in-sample, descriptive estimates. Never describe them as causal or as forecasts.
- Never commit data files, credentials, API keys, or licensed data (Bloomberg, CEIC, Wind). Data enters only through scripts in src/.
- Notion: create a new page only when Steven asks. Never edit an existing Notion page without his direct instruction.
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
