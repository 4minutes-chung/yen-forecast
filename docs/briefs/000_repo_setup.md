# TASK 000: Repo setup (private GitHub)

Scope: set up the repo skeleton and push to a private GitHub repo. Nothing else.
No data pulls, no Python environment, no models.

## Steps
1. Check tools: run `git --version` and `gh auth status`.
   If gh is not authenticated, STOP and tell me. Do not attempt login or handle tokens.
2. Create this structure in the current directory:
   CLAUDE.md
   README.md
   .gitignore
   docs/PROTOCOL.md
   docs/DECISIONS.md
   docs/LOG.md
   docs/briefs/000_repo_setup.md   (save this brief here, verbatim)
   data/raw/.gitkeep
   data/processed/.gitkeep
   src/.gitkeep
   results/.gitkeep
3. Write CLAUDE.md with the exact content in the section "CLAUDE.md CONTENT" below.
4. docs/PROTOCOL.md: a title line plus "STATUS: DRAFT, not frozen. Do not implement against this file until it says FROZEN."
5. docs/DECISIONS.md: a title plus this first entry:
   - 2026-09-25: Repo is private on GitHub and is the single source of truth. Notion is Steven's dashboard, written by Steven only.
6. docs/LOG.md: a title, then the Task 000 entry (see logging rule in CLAUDE.md).
7. README.md: one paragraph. "Research project: out-of-sample forecasting of USD/JPY against a random-walk benchmark. Work in progress."
8. .gitignore: ignore data/raw/*, data/processed/* (keep the .gitkeep files), .env, .venv/, __pycache__/, *.pyc, .DS_Store, .ipynb_checkpoints/.
9. git init, make the first commit, then create the remote and push:
   `gh repo create yen-forecast --private --source=. --remote=origin --push`
   If the command fails, report the exact error and stop. Do not work around it.
10. Verify the repo is private: `gh repo view --json visibility`. It must show PRIVATE.

## Acceptance criteria
- All files above exist; CLAUDE.md content matches exactly.
- Remote exists, push succeeded, visibility = PRIVATE.
- No data files committed.
- LOG.md has the Task 000 entry; handoff block printed.

## CLAUDE.md CONTENT

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
