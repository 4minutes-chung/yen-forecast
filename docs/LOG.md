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

## 2026-10-09 — Task 002: Prior-work check (1-hour cap)

- What changed: web search for prior work on whether USD/JPY's sensitivity to JGB 2Y changed across the 2022-2024 YCC/NIRP dismantling. Wrote the results file. No data, no code.
- Files touched: docs/briefs/002_prior_work.md (brief, verbatim), docs/lit/002_prior_work.md (new), docs/LOG.md.
- Results:
  - 8 verified hits (fetched this session), 5 leads not verified (403 or paywall). Full table in docs/lit/002_prior_work.md.
  - Verdict: (b) partly done. No verified paper formally tests a change in USD/JPY sensitivity to JGB 2Y across 2022-2024. Academic samples end Dec 2022 (RIETI DP 24-E-054) or Dec 2023 (BOJ WP Dec 2024; IMF WP/25/227).
  - IMF Country Report 26/75 (Apr 2026) documents the yen decoupling from the US-JP 10Y yield differential since mid-2025. Lazard (Jul 2026) and Ferrari-Kearns-Schrimpf (BIS WP 626) also point to a weaker, not stronger, rate-yen link after the lower bound.
  - Search time 10:54-11:05 HKT by system clock, inside the 1-hour cap.
- Deviations from brief:
  - Ran in the same session as task 001. This breaks the one-task-per-session rule; Steven pasted the brief into this session.
  - One curl download of the BIS WP 626 PDF went into the session scratchpad without asking first. It returned an HTML block page, not the PDF. WebFetch also auto-saved 4 PDFs (BOJ WP, IMF WP/25/227, RIETI DP 24-E-054, IMF CR 26/75) to the session tool-results folder. Text was extracted with pdftotext. Nothing was saved to the repo.
- Open issues:
  - Hypothesis direction: all evidence found points to a weaker link. Steven to decide whether to make the test two-sided and reword the question from "did it rise" to "did it change, and when". This affects the plan and the Notion page.
  - Notion project page not updated. It is now an existing page, so updating it needs Steven's direct instruction.
  - Unverified leads: Lee (SSRN), JP Morgan 2024 note, Minenna (Central Banking), FMIR 2025, Nishigaki (2025).
  - BIS WP 626 country list (BOJ included or not) not confirmed. Kano and Wada data and sample not read.
  - Research question still not recorded in the repo (CLAUDE.md goal is still forecasting). Step 3 brief.

## 2026-10-09 — Task 002 addendum: deep review and Notion update

- What changed: deep review of the task 002 findings, at Steven's request. Updated the existing Notion project page with the findings and the review; Steven gave a direct instruction to edit it.
- Files touched: docs/lit/002_deep_review.md (new), docs/LOG.md. Outside the repo: Notion page "日圓研究計劃" (status callout, research-question note, step 2 status, new sections "第 2 步結果" and "深度檢討", two open issues).
- Results:
  - Three competing hypotheses for the beta of ΔUSD/JPY on ΔJP 2Y: H1 rises after YCC dismantling (original plan); H2 falls after exit (lower-bound amplification, Ferrari et al.); H3 the 2Y link holds while the 10Y link decouples after 2025 (inference from IMF CR 26/75 and Masujima & Sato).
  - Design fixes found: BOJ publishes USD/JPY at 17:00 JST (interbank mid rate), released on the web since 4 Jan 2007, verified on boj.or.jp. Separate JP and US legs; report JGB 2Y volatility with each beta; robustness without intervention days and early August 2024.
  - Masujima & Sato regressions are in daily changes (checked in the PDF).
- Deviations from brief: none beyond those listed for task 002. No new downloads; one more WebFetch of boj.or.jp HTML.
- Open issues:
  - Steven to decide: reword the question to two-sided ("did it change, and when; does the 2025 decoupling show at 2Y") and pre-register H1, H2, H3.
  - BOJ 17:00 USD/JPY series code and start date in BOJ Time-Series Data Search not checked.
  - Five papers to read in full before the step 4 protocol (list in 002_deep_review.md section 7).

## 2026-10-09 — Task 003: Targets, repo rules, protocol draft

- What changed: recorded Steven's targets (Target 0 forecast and study the yen; Target 1 reworded descriptive question; key use 0-2 year insight). Updated the CLAUDE.md goal and rules, README, DECISIONS. Replaced the PROTOCOL.md placeholder with a Target 1 draft (DRAFT v1, not frozen). No code, no data, no estimation.
- Files touched: docs/briefs/003_targets_and_protocol_draft.md (new), CLAUDE.md, README.md, docs/DECISIONS.md, docs/PROTOCOL.md, docs/LOG.md.
- Results: no numbers. The protocol fixes specs S1-S3 and S2m; the decoupling measures M1 (beta), M2 (explained share), M3 (unexplained drift); hypotheses H1-H4 with predictions; Step 0 replication of the IMF mid-2025 10Y decoupling; a robustness list; a pre-freeze checklist.
- Deviations from brief:
  - Third task in this session; breaks the one-task-per-session rule. Steven said to continue in this session.
  - Added H4 (stable betas, decoupling in the unexplained part) and a monthly horizon check (S2m), beyond the three hypotheses in the deep review.
  - Work order changed: the plan put the rolling chart (old step 3) before pre-registration (old step 4). The protocol now puts freeze first, then data ingest, then rolling betas, because a rolling beta is already an estimate.
- Open issues:
  - Steven to sign off on and freeze PROTOCOL.md after the pre-freeze checklist (section 11).
  - Target 0 protocol not written. Steven to decide when it starts.
  - The Notion page still shows the old research question marked "待定". Update only on Steven's instruction.

## 2026-10-09 — Task 004: Protocol amendments (C') and Target 0 pre-registration

- What changed: rewrote PROTOCOL.md section 10 (0-2 year link) and added section 14 (Target 0 pre-registration). Updated sections 1, 11 and 12, and the status line (DRAFT v2). Recorded C' in DECISIONS.md. Wrote the plan to the Notion project page first, as Steven asked. No code, no data, no estimation.
- Files touched: docs/briefs/004_protocol_amendments.md (new), docs/PROTOCOL.md, docs/DECISIONS.md, docs/LOG.md. Outside the repo: Notion project page.
- Results:
  - Section 10: baseline = today's spot (random walk). Scenarios are yield surprises relative to the curve-implied forward 2Y at 1, 12 and 24 months. S2m monthly betas, with a 95% band; "no usable view" if the band includes zero. 6 months is dropped from the scenario table (its forward needs sub-1-year yields).
  - Section 14: three one-predictor models (T1 carry = US−JP 2Y level; T2 = one-month change in that differential; T3 = CFTC net non-commercial JPY positions / open interest, entering on its release date) against RW without drift. Rolling 120-month window (recursive as the only robustness variant); h = 1, 3, 6, 12. Clark-West with HAC lag h−1; Pesaran-Timmermann on non-overlapping origins; Bonferroni threshold 5%/12.
  - Power: about 30 non-overlapping 12-month OOS periods, so a true R² of 5% is detected about a third of the time at 5% and under 10% at the Bonferroni threshold (rough). The earlier "about four in ten" (41 observations, before taking out the estimation window) was too optimistic and is corrected on Notion.
  - Also verified this session: the MOF JP 2Y column is numeric 1988/8-1993/5 and 1997/12-2002/7 (the two chunks unread in task 001). JP 2Y is now numeric across every chunk read from 1979/8/22; a code check is still needed.
- Deviations from brief: fifth task in this session; breaks the one-task-per-session rule (Steven said to continue). The 6-month horizon was dropped from the scenario table (Target 0 keeps 6 months).
- Open issues:
  - Task 005: verify the section 11 checklist (BOJ dates, BOJ 17:00 series, DGS10, FRED 1Y/3Y/5Y, CFTC start date and release lag, intervention dates, test references).
  - D0 for Target 0 depends on the CFTC start date.
