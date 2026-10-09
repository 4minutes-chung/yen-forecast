# Handoff for the next session (task 006)

Written 2026-10-10. Read this first in the next chat.

## 0. Where things stand

| Item | State |
|---|---|
| PROTOCOL.md | DRAFT v2 with task 005's verified values; not frozen |
| Open decisions | 15 rows in `docs/verification/005_council_verdict.md`, section 1. The 8 task 005 issues plus 7 found by the council |
| Data, code, estimates | None yet |
| Last commits | `0079ca9`, `4b18cb0` (task 005), plus the commit that adds this file |
| Delegate CLIs | Both tested headless on 2026-10-10: `grok -p` (grok-4.7, web search, can run shell) and `opencode run -m opencode-go/deepseek-v4-flash` (or `-pro`) |

## 1. Before the session (Steven, about 30 min)

Fill the "Steven's call" column for rows 1-15 in `docs/verification/005_council_verdict.md` with accept or your override, or paste your calls into the chat. Task 006 stops if any row is empty.

## 2. Brief to paste into the new session

```text
# TASK 006: Apply Steven's decisions on the task 005 issues (protocol v3); freeze only on Steven's word

Scope: turn Steven's decision sheet into protocol text. No code, no data download, no estimation. Save this brief verbatim as docs/briefs/006_decisions_and_freeze.md.

## Before starting
- Read the last 3 entries of docs/LOG.md, docs/handoff/006_handoff.md, docs/verification/005_council_verdict.md (decision sheet, section 1), Table 3 of docs/verification/005_freeze_checklist.md, and docs/PROTOCOL.md in full.
- Precondition: every row 1-15 of the decision sheet has Steven's call (in the file or pasted in chat). If any row is empty or unclear, stop and ask. Never fill a row with the council default on your own.

## Steps
1. docs/PROTOCOL.md (explicitly authorised by this brief; any section): apply each row exactly as Steven decided, and nothing else. Update section 12 (order of work) to: 006 decisions and freeze, 007 data ingest, 008 Step 0, 009 Target 1 estimation. Set STATUS to "DRAFT v3 (task 006), ready for sign-off".
2. docs/DECISIONS.md (explicitly authorised): one dated entry per changed row, each with a one-line reason.
3. docs/lit/002_prior_work.md row 6 and docs/lit/002_deep_review.md: correct the IMF CR 26/75 method text. The rate driver in the p.12 figure combines the 2Y and 10Y US-Japan spreads; the regression form is not stated. Delegate the edit to DeepSeek flash via opencode (command in docs/handoff/006_handoff.md, section 3), then check its diff yourself.
4. Run the Grok review of the PROTOCOL and DECISIONS diff (command in section 3 of the handoff). Fix real mismatches. In the LOG, list each Grok point and what you did with it. Run git status after every Grok or opencode call.
5. Freeze only if Steven writes "freeze" in chat after reading the diff. Then: set STATUS to "FROZEN (date)"; fix the main-sample end date (row 5); add a DECISIONS.md entry "Protocol v3 frozen"; create git tag protocol-v3-frozen; record the SHA-256 of PROTOCOL.md in DECISIONS.md and the LOG. If row 15 is ticked, Steven registers the hash on OSF himself.
6. LOG entry; commit "task 006: protocol v3, decisions applied" (and "task 006: protocol v3 frozen" if frozen); push, plus git push --tags if tagged; handoff block; update the Notion project page (standing permission).

## Acceptance criteria
- Every row 1-15 appears in PROTOCOL.md exactly as Steven called it; no other design change.
- The Grok review and how each point was handled are in the LOG.
- Without Steven's "freeze" in chat, STATUS stays DRAFT.
- No code, no data files, no estimation.
```

## 3. Delegate commands (run from any directory)

Grok review of the protocol diff, read-only:

```bash
grok -p "You review a research-protocol change. Run: git diff HEAD~1 -- docs/PROTOCOL.md docs/DECISIONS.md . Compare every change with the 'Steven's call' column in docs/verification/005_council_verdict.md section 1. Report: (1) rows not applied or applied differently, (2) changes no row covers, (3) any look-ahead risk. Do not edit any files." --cwd /Users/stevenchung/Desktop/yen-forecast
```

Grok lookup (leads only; read the primary page before writing a fact down):

```bash
grok -p "Web search, under 150 words, cftc.gov URLs only: (1) confirm the CFTC suspended Commitments of Traders reports during the Oct 2013 shutdown and the catch-up dates; (2) since when have COT reports been released on Fridays at 3:30 p.m. ET. Do not edit any files." --cwd /Users/stevenchung/Desktop/yen-forecast
```

DeepSeek flash for the lit-note edit (task 006 step 3; Claude writes the exact replacement text first):

```bash
opencode run -m opencode-go/deepseek-v4-flash --dir /Users/stevenchung/Desktop/yen-forecast "In docs/lit/002_prior_work.md, table row 6 only: replace the method cell with: <text from Claude>. Change nothing else in the file."
```

After every delegate run:

```bash
git -C /Users/stevenchung/Desktop/yen-forecast status --short
```

## 4. Plan after task 006

| Session | Task | Claude Opus | Grok | DeepSeek (opencode) | Done when |
|---|---|---|---|---|---|
| 006 | Decisions applied, protocol v3, freeze | Writes the protocol diff | Reviews the diff against the sheet | Flash: lit-note fix | Steven says "freeze"; tag pushed |
| 007 | Ingest BOJ FXERD04, MOF JGB, FRED (DGS1/2/3/5/10, DEXJPUS), MOF interventions. Manifest (URL, time, SHA-256, rows). Share of days with \|Δj2\| ≤ 0.5bp per window. Level plots only | Calendar and alignment code; synthetic tests; date ≤ origin checks | Runs the tests (`grok -p "Run python -m pytest -q in the repo and report failures; do not edit files"`) | Pro: routine download scripts in src/ | Tests pass; manifest committed; no regressions run |
| 008 | Step 0: S3 (IMF-style 10Y check) plus the Chow test at 1 Jul 2025 | Estimation code | Reviews by running tests | — | Step 0 pass/fail logged |
| 009 | Target 1 estimation: rolling betas, Bai-Perron, qLL, M1-M3, noise-floor rule | Estimation and interpretation | Reviews by running tests | Flash: LOG and table formatting | Primary estimates per hypothesis logged |
| Later | CFTC release calendar (shutdown months handled), then the Target 0 run | Timing-critical code | Verifies shutdown and release dates | — | After the Target 1 note |

- Python: use the conda `base` environment (global rule); no venv.
- Code review rule: all code is reviewed by Grok (or Gemini) by running tests, not by reading alone.
- One task per session. Do not start 007 in the 006 session.
