# Log

## 2026-09-26 — Task 000: Repo setup (private GitHub)

- What changed: created repo skeleton (CLAUDE.md, README.md, .gitignore, docs/, data/, src/, results/), initialised git, created private GitHub remote `yen-forecast` and pushed.
- Files touched: CLAUDE.md, README.md, .gitignore, docs/PROTOCOL.md, docs/DECISIONS.md, docs/LOG.md, docs/briefs/000_repo_setup.md, data/raw/.gitkeep, data/processed/.gitkeep, src/.gitkeep, results/.gitkeep.
- Results: none (no data, no models). Sample period: n/a.
- Deviations from brief: the session's working directory was an unrelated existing repo (RMBS_Modelling), so with Steven's approval the repo was created in a new folder, /Users/stevenchung/Desktop/P12B_File/yen-forecast, instead of "the current directory". Single commit made (brief step 9 first commit and the "task 000" commit rule combined).
- Open issues: docs/PROTOCOL.md is DRAFT, not frozen.

## 2026-10-09 — Task 001: Free daily data availability check (JP 2Y, US 2Y, USD/JPY)

- What changed: read-only web check of free daily sources; no data downloaded, no code. Added brief and this entry.
- Files touched: docs/briefs/001_data_availability_check.md, docs/LOG.md.
- Results:
  - JP 2Y: MOF historical CSV https://www.mof.go.jp/english/policy/jgbs/reference/interest_rate/historical/jgbcme_all.csv (1974/9/24 to 2026/9/30) plus current-month CSV https://www.mof.go.jp/english/policy/jgbs/reference/interest_rate/jgbcme.csv (2026/10/1 to 2026/10/8). 2Y is "-" from 1978/5/22 to 1979/8/21; numeric from 1979/8/22. Basis: constant maturity, semiannual compound, from JSDA reference prices at Tokyo 3pm close; published 9:30am next business day (MOF Q&A). Negative values 2014-2023 are real values. 2016/1/4 2Y = -0.028. Terms: MOF site under Public Data License 1.0 (citation required).
  - US 2Y: FRED DGS2 (H.15), daily, 1976-06-01 to 2026-10-07. Time of day not stated on FRED page (unverified; check H.15 notes).
  - USD/JPY: FRED DEXJPUS (H.10), daily, 1971-01-04 to 2026-10-02, noon buying rates in New York.
  - Gate (JP 2Y starts before 2016): PASS.
- Deviations from brief: brief written after the check was done in the same session (Steven approved recording it as task 001 afterwards).
- Open issues:
  - 2Y coverage was checked by reading the MOF CSV in chunks, not row by row in code. Chunks covering about 1988/8-1993/5 and 1997/12-2002/7 were not read. Re-check for "-" or gaps in code when the ingest script is written.
  - Timing mismatch: JGB close (06:00 UTC) is about 10 hours before DEXJPUS NY noon on the same date. Plan uses 2-day changes.
  - CLAUDE.md goal (out-of-sample forecasts vs random walk) conflicts with the new descriptive research question (USD/JPY sensitivity to JP 2Y during YCC exit). Needs its own brief to change.
  - Task 000 entry records the repo path as P12B_File/yen-forecast; actual path is /Users/stevenchung/Desktop/yen-forecast.
