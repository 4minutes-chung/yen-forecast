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

## 2026-10-09 — Task 005: Freeze checklist verification (PROTOCOL.md section 11)

- What changed: verified the section 11 checklist from primary sources (web reads only). Wrote the verification file. Filled verified values into PROTOCOL.md sections 2, 3, 9, 11 and 14 (STATUS still DRAFT v2; no design choice changed). No code, no data files, no estimation.
- Files touched: docs/briefs/005_freeze_checklist.md (new, verbatim), docs/verification/005_freeze_checklist.md (new), docs/PROTOCOL.md, docs/LOG.md. Outside the repo: Notion project page.
- Results:
  - BOJ: negative rate decided 29 Jan 2016 (effective 16 Feb 2016); YCC 21 Sep 2016; YCC changes 31 Jul 2018, 19 Mar 2021 (±0.25%), 20 Dec 2022 (±0.5%), 28 Jul 2023 (references, 1.0% offers), 31 Oct 2023 (1% reference); exit 19 Mar 2024 (effective 21 Mar); call rate 0.25% (31 Jul 2024, eff. 1 Aug), 0.5% (24 Jan 2025, eff. 27 Jan), 0.75% (19 Dec 2025, eff. 22 Dec), 1.0% (16 Jun 2026, eff. 17 Jun), 1.25% (18 Sep 2026, eff. 24 Sep).
  - BOJ 17:00 USD/JPY: FM08 / FXERD04, daily 1998-01-05 to 2026-10-07; BOJ API (launched 18 Feb 2026) returns CSV/JSON with no key; one-month test call (2026-09) returned 30 rows.
  - FRED: DGS1, DGS3, DGS5, DGS10 from 1962-01-02; DGS2 from 1976-06-01; all to 2026-10-07. DEXJPUS 1971-01-04 to 2026-10-02. CMT = FRBNY bid-side quotes at or near 3:30 PM ET, bond-equivalent (semiannual) par yields.
  - MOF interventions: CSV daily rows from 1991-05-13 (latest 2026-05-06); monthly totals at month-end, daily detail quarterly (Q2 2026 out 7 Aug 2026; Q3 2026 due 2-9 Nov 2026).
  - August 2024 window: 2-9 Aug 2024 (BIS Bulletin 90, Graph 1 shading).
  - CFTC: legacy futures-only, JPY code 097741, first as-of 1986-01-15, 1,937 reports to 2026-09-29. Derived D0 = Feb 1986.
  - References: citations confirmed via Crossref for BP 1998/2003, EM 2006, NW 1987/1994, CW 2006/2007, PT 1992, Fama 1984, EE 1995, BNP 2008. CW (2006) is valid with a rolling window (their own application uses R = 120); PT needs serially independent outcomes.
  - IMF CR 26/75 para 16: decomposition since Nov 2024 into rate differential (2Y and 10Y spreads combined), VIX, oil, residual; regression form not stated. Masujima & Sato Table 7: daily, Dec 2007-Dec 2022, N = 3,633; YCC_D = 1 from Sep 2016, interacted with the slope spread (1.078**) and CDS only.
- Deviations from brief:
  - PDFs were read with WebFetch, which keeps copies in the session tool-results folder (Steven approved this at the start). Text was extracted with pdftotext; two scanned papers (NW 1994, EE 1993 WP) were OCR'd with tesseract in the scratchpad. Nothing was saved to the repo or data/.
  - Read through tools without saving to the repo: the MOF intervention CSV, a one-month BOJ API sample (30 values), BOJ and CFTC metadata, and CFTC API aggregates (min/max date, count).
  - Bot challenges at JSTOR, OUP and ScienceDirect were not bypassed. Working-paper versions were read for CW 2006, NW 1994, EE 1995, BNP 2008 and PT 2006; Fama 1984 from a scan of the journal article on a course page.
  - Sections 4, 5 and 6 also contain "fixed after verification" text; they are outside the brief's edit scope and were left unchanged (paste-ready facts in the verification file, Table 3). New data-table rows (DGS1/3/5, CFTC) were not added.
  - The 2017, 2019 and 2020 BOJ statement lists were checked by title only; the 2020 "Enhancement of Monetary Easing" statements were not read in full.
- Open issues (for Steven, before freeze):
  - Clark-West HAC for overlapping forecasts: CW recommend the West (1997)/Hodrick (1992) estimator; the protocol uses Newey-West with lag h − 1.
  - T3: BNP divide by non-commercial open interest (protocol: open interest); BNP link positions to crash risk, not mean returns.
  - IMF's rate driver combines 2Y and 10Y spreads; S3 uses 10Y only. docs/lit/002 states IMF uses only 10Y (not edited).
  - Newey-West lag rule (section 5): NW 1994 automatic (n = [4(T/100)^(2/9)], m = [γ̂T^(1/3)]) or the fixed software rule.
  - Section 14.3: first origin D0 + 120 months is inconsistent for h > 1 (window starts before D0); D0 + 119 + h would be consistent.
  - ¥15,399.3bn intervention between 30 Jul and 26 Aug 2026 with daily dates unknown until 2-9 Nov 2026; affects the freeze end date for robustness 2.
  - Regime-window month labels vs BOJ decision dates (Dec 2022, Mar 2024); main sample starts before the negative rate took effect.
  - CFTC historical release dates must be rebuilt in code from documented lags; the start of the Friday-release convention is undocumented.
  - Bai-Perron (trimming, max breaks) and Elliott-Müller (critical values, serial correlation) full texts not read; checklist line left unticked.
- Follow-up commit (task 005 fix): added the regime-window row to the verification file's Table 3; reworded the PROTOCOL 14.2 T3 field list so the open-interest denominator stays open for Steven.

## 2026-10-10 — Task 005 addendum: process record, council on open issues, task 006 handoff

- What changed: at Steven's request, documented the task 005 process, ran an LLM council (5 advisors, 5 anonymous peer reviewers, chairman) on the 8 open issues, and wrote the handoff and plan for task 006. Smoke-tested the delegate CLIs (`grok -p`, `opencode run` with DeepSeek v4 flash). No PROTOCOL.md or DECISIONS.md change, no code, no data.
- Files touched: docs/verification/005_process.md (new), docs/verification/005_council_verdict.md (new), docs/handoff/006_handoff.md (new), docs/LOG.md. Outside the repo: Notion project page (new handoff section).
- Results:
  - Council: freeze in the next session; none of the 8 issues blocks it. Decision sheet with 15 rows: the 8 task 005 issues plus 7 found in peer review (noise floor for Δj2, S2 as the M1 headline, Chow test at 1 Jul 2025 because 15% trimming cannot date a break after about Dec 2024, calendar and 2-day grid, one primary estimate per hypothesis, verifiable freeze, CFTC shutdown handling).
  - Claude differs from the council on row 3: keep BNP as the pre-2016 basis for T3 and keep the IMF in the s.14.6 disclosure.
  - Verified on cftc.gov: COT reports were suspended during the Dec 2018-Jan 2019 shutdown (release 7864-19) and the Oct-Nov 2025 shutdown (release 9138-25). The 2013 suspension is a Grok lead only (not verified).
  - Verified (task 005 source): MOF intervention total for 29 Jun-29 Jul 2026 = ¥0, so a 29 Jul 2026 sample end is clean.
- Deviations from brief: work after task 005 was closed, in the same session; Steven asked for it. 11 subagents were used for the council (Steven invoked /llm-council). Grok, run headless, executes shell commands without asking; the repo was checked clean after each run.
- Open issues: Steven fills the decision sheet (rows 1-15) before task 006; task 006 brief in docs/handoff/006_handoff.md.

## 2026-10-11 — Task 006: Decisions applied, protocol v3 (not frozen)

- What changed: Steven wrote "GO ... YOU DECIDE AND YOU TO HELP" in chat, so Claude filled the 15-row decision sheet (last column labelled as delegated). PROTOCOL.md is now DRAFT v3, ready for sign-off. DECISIONS.md has one entry per row plus a refinement entry. IMF CR 26/75 method text corrected in the two lit notes. No code, no data, no estimation. NOT frozen: "freeze" has not been written in chat; no tag, no hash.
- Files touched: docs/briefs/006_decisions_and_freeze.md (new, verbatim), docs/verification/005_council_verdict.md (column filled), docs/PROTOCOL.md, docs/DECISIONS.md, docs/lit/002_prior_work.md, docs/lit/002_deep_review.md, docs/LOG.md.
- Calls (all accept the council default except where stated):
  - Row 2: T3 denominator = long + short (spreading excluded). Row 3: override, BNP is the basis worded as "crash-risk link, mean-return link untested; T3 tests it"; IMF stays in the s.14.6 disclosure.
  - Row 5: main sample ends 29 Jul 2026. Row 6: window dates as in the sheet. Row 9: L = 35 / 14 / 10 days.
  - Row 14 detail chosen by Claude (not in the sheet): H1 S2 M2 post-exit vs YCC; H2 S2 b_j2 YCC vs post-exit; H3 change in S2 b_jsl; H4 joint Chow at 1 Jul 2025. All 5%.
- Grok review (git diff b9b405a, headless; repo checked after the run, no stray edits). Each point and what was done:
  - Bai-Perron "package defaults" vs "up to 3 breaks, 15% trimming": kept; those two settings were pre-registered in task 004.
  - H1 gated on "not identified" (defined for M1): removed the extra gate; H1 now only needs its break to survive the 1bp run, as row 10 says.
  - H4 and H3 had compound primaries (qLL, b_j2 and b_jsl): H4 primary = joint Chow, qLL secondary; H3 primary = change in b_jsl.
  - s.8 "before Dec 2022" vs new YCC end: changed to 20 Dec 2022. s.8 vs "only primary counts": s.8 is now a closed list of four surprises, each one named estimate at 5%.
  - s.11 stale "Conflict" notes: marked as resolved with the row number.
  - Noise floor on daily vs 2-day S2: share computed on the daily series, 2-day share reported alongside.
  - Chow date taken from the IMF narrative: disclosed in H4. L-day CFTC rule is conservative: intended.
  - Not applied: rows listed as "changes no row covers" (STATUS, s.12 order from sheet section 2, DEXJPUS timing sentence from Table 3, H2 gate): kept, they follow the sheet's section 2 or Table 3.
  - The edits after the Grok review were not re-reviewed by Grok.
- Deviations from brief:
  - Precondition said never to fill rows with the council default on my own; Steven delegated the decisions in chat, so the column is labelled "Delegated by Steven in chat 2026-10-11". Steven should read rows 2, 3 and 14 before saying "freeze".
  - Step 3: DeepSeek flash (opencode) hung twice (over 5 min with no edit, and again on a trivial prompt). I killed it and made the lit-note edits myself; two extra lines in 002_prior_work.md (novelty paragraph) were also corrected for the IMF method. Steven should check the diff.
  - Steps 5 skipped by design (no freeze).
- Open issues:
  - Steven's "freeze" word; then tag protocol-v3-frozen, SHA-256, end-date confirmation. OSF registration optional (Steven's account).
  - s.2 data table still lacks rows for DGS1/3/5 and CFTC (no row covers them).
  - docs/lit/002_deep_review.md still lists "what method the staff analysis used" as an open question and its proposed question wording; left unchanged.
  - COT 2013 suspension unverified (Grok lead only).

## 2026-10-12 — Task 006 follow-up: second Grok review (still DRAFT v3)

- What changed: Steven asked to record the output on Notion and continue. Notion project page: new section "第 6 步結果：Task 006 套用決定" (15 calls, deviations, open issues) and status lines. Re-ran Grok on the post-fix diff (`git diff b9b405a`; repo clean after the run). Fixed what it found in PROTOCOL.md; no freeze, no tag, no hash.
- Files touched: docs/PROTOCOL.md, docs/DECISIONS.md, docs/LOG.md. Outside the repo: Notion project page.
- Grok second review, each point:
  - Five earlier fixes: confirmed.
  - H1 primary M2 is two statistics (R², variance contribution): primary is now the variance contribution; R² secondary.
  - Step 0 "at least one of M1-M3": now one test, M1 b_10 in S3, post-mid-2025 vs post-exit window before it; M2, M3 reported.
  - s.8 S2 vs S2m item named no coefficient; Bai-Perron item had an undefined break date: both now named (S2 b_j2 sign in the post-exit window; any point-estimate break date before 20 Dec 2022).
  - s.7 and s.8 both counted toward the stop rule: now only the closed s.8 list counts; H1-H4 primaries are hypothesis tests; s.13 reworded.
  - s.12 runs the Chow test in task 008 but s.7 limited Step 0 to S3: s.7 now says task 008 also runs H4's Chow test.
  - Look-ahead: training pair τ = t − h ends at S(t), which is after month-end t. Not a flaw: the origin time is S(t) (NY noon, first business day of t+1), so all inputs precede it. Written into s.14.3.
  - 1 Jul 2025 Chow date from the IMF narrative: already disclosed in H4.
- Deviations: none new. The edits after this second review were not re-reviewed by Grok.
- Open issues: unchanged from the previous entry (Steven's "freeze"; s.2 data-table rows; COT 2013; opencode hang).
