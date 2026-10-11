# Task 005 follow-up: council verdict and decision sheet

Date: 2026-10-10. These are **suggestions only**. Nothing in PROTOCOL.md or DECISIONS.md was changed. Steven ticks or overrides each row; task 006 then applies the ticks.

## 1. Decision sheet (Steven fills the last column)

| # | Item | Council default | Claude's view | Steven's call |
|---|---|---|---|---|
| 1 | Clark-West HAC (s.14.4) | Headline: the mean-adjusted West (1997)/Hodrick (1992) estimator that CW (2006) recommend. Newey-West with lag h−1 reported alongside. Recursive scheme labelled exploratory | Agree. The formula is in CW 2006, eq. 3.5 (working-paper version already read) | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept council default (West/Hodrick headline, NW lag h−1 alongside, recursive labelled exploratory) |
| 2 | T3 denominator (s.14.2) | Non-commercial open interest, as BNP use | Agree. BNP give no formula ("total open interest of non-commercial traders"), so Steven also picks one: long + short, or long + short + 2×spreading | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept non-commercial open interest; formula = long + short (noncomm_positions_long_all + noncomm_positions_short_all), spreading excluded. Reason: bounded in [−1, 1], uses only fields verified in task 005. BNP give no formula, so this is a choice, not a replication |
| 3 | T3 basis (s.14.2) | Keep T3. Base it on IMF CR 26/75's positioning account (disclosed as post-2016) and cite BNP for crash risk only | **Differ.** Keep BNP as the pre-2016 basis, worded honestly: "positions linked to crash risk; mean-return link untested; T3 tests it". A 2026 source as the basis would mean choosing a predictor because it worked in the test period. The IMF stays in the s.14.6 disclosure | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: OVERRIDE of council default: take Claude's view. Basis = BNP (2008) worded as "positions linked to crash risk; mean-return link untested; T3 tests it". IMF CR 26/75 stays in the s.14.6 disclosure only |
| 4 | First origin (s.14.3) | D0 + 119 + h | Agree. Arithmetic fix | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept: first origin = D0 + 119 + h |
| 5 | Main sample end (s.3) | 29 Jul 2026 | Agree. MOF monthly total for 29 Jun to 29 Jul 2026 = ¥0 (verified); the undated ¥15.4tn starts 30 Jul | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept: main sample ends 29 Jul 2026 (s.3 only; s.14.3 last-origin rule unchanged); fixed formally in the freeze entry |
| 6 | Regime windows (s.3) | Pre-YCC 4 Jan to 20 Sep 2016; YCC 21 Sep 2016 to 19 Dec 2022; transition 20 Dec 2022 to 18 Mar 2024; post-exit from 19 Mar 2024; post-mid-2025 from 1 Jul 2025 | Agree. Decision days are released around midday JST, before the 17:00 fix, so a window starts on the decision day | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept council dates (pre-YCC 4 Jan–20 Sep 2016; YCC 21 Sep 2016–19 Dec 2022; transition 20 Dec 2022–18 Mar 2024; post-exit from 19 Mar 2024; post-mid-2025 from 1 Jul 2025) |
| 7 | NW lag rule, Bai-Perron, qLL (s.5, s.6) | Fixed floor(4(T/100)^(2/9)); package defaults; log "full text not read" | Agree. Also write into s.4 the verified US timing: US close of t−1 (04:30/05:30 JST) precedes Tokyo 17:00 on t | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept: NW lag floor(4(T/100)^(2/9)); Bai-Perron and qLL package defaults, "full text not read" kept in s.11; add the US-close-of-t−1 timing sentence to s.4 |
| 8 | S3 and H3 (s.5, s.7) | Rename S3 "IMF-style 10Y check". H3 asks which S2 leg lost explanatory power. Fix the lit note. No new S3b spec: S3 is S2 with the restriction b_u2 = b_usl = −b_j2 = −b_jsl | Agree | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept: S3 renamed "IMF-style 10Y check"; H3 asks which S2 leg lost explanatory power; lit notes corrected; no S3b |
| 9 | CFTC release timing (s.14.2) | Use the latest report with as-of ≤ month-end minus L days: L = 35 before 1990, 14 for 1990-92, 10 after. In shutdown months, use only reports actually released. A unit test checks the assumed dates against the documented release rules | Agree. Shutdown suspensions verified for Dec 2018 to Jan 2019 and Oct to Nov 2025 (section 4). D0 stays Feb 1986 under L = 35 | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept: L = 35 days before 1990, 14 for 1990-92, 10 after; shutdown months use only reports actually released; unit test against documented rules |
| 10 | Noise floor for Δj2 (new, s.6) | Report the share of days with \|Δj2\| ≤ 0.5bp per window. Above 50%, that window's M1 is reported as "not identified". Robustness run drops days with \|Δj2\| < 1bp; H1 only counts if its break survives that run. State that 2Y OIS is not used | Agree. This is the main threat to H1 and H2: stale YCC-era quotes push the beta toward zero | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept: share of days with |Δj2| ≤ 0.5bp per window (>50% = "not identified"); separate robustness run dropping |Δj2| < 1bp (the two thresholds stay separate); H1 counts only if its break survives; 2Y OIS not used |
| 11 | M1 headline spec (new, s.6) | S2, not S1. S1 leaves out the US leg | Agree | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept: S2 is the M1 headline spec |
| 12 | Testing H4 (new, s.6 and s.7) | Chow test at the known date 1 Jul 2025, plus qLL. Reword H4 to match | Agree. With 15% trimming, Bai-Perron cannot date a break after about Dec 2024, so H4 would be true by construction | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept: Chow test at 1 Jul 2025 plus qLL for H4; H4 reworded |
| 13 | Calendar and 2-day grid (new, s.4) | Tokyo business days. US yield = last NY close before the Tokyo date. The 2-day grid starts on the first sample day; a shifted grid is robustness item 6 | Agree. This must be fixed before ingest | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept: Tokyo business days; US yield = last NY close before the Tokyo date; 2-day grid starts on the first sample day; shifted grid = robustness item |
| 14 | Multiple testing (new, s.7 and s.8) | One primary estimate and threshold per hypothesis. Only primary surprises count toward the stop rule | Agree. About 100 tests otherwise; some "surprise" would appear by chance | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept: one primary estimate and threshold per hypothesis (H1-H4), only primary surprises count toward the stop rule; no new metrics |
| 15 | Verifiable freeze (new, s.11) | Git tag plus SHA-256 of PROTOCOL.md, registered on OSF with an embargo (Steven does this himself). The ingest step writes a manifest: URL, retrieval time, SHA-256, row count | Agree on the tag, hash and manifest. OSF is optional: it needs Steven's own account | Delegated by Steven in chat 2026-10-11 ("YOU DECIDE"); decided by Claude: accept git tag + SHA-256 + ingest manifest; OSF optional, Steven's own account |

## 2. Session plan

| Session | Task | Who | Output |
|---|---|---|---|
| Today | Steven ticks or overrides rows 1-15 above | Steven, about 30 min | This file with the last column filled |
| 006 | Apply the decisions, protocol v3, freeze if Steven says so | Opus writes the diff; DeepSeek flash does the lit note; Grok reviews the diff | PROTOCOL.md v3, DECISIONS.md, tag `protocol-v3-frozen` |
| 007 | Ingest BOJ, MOF and FRED data with the manifest; zero-change shares; level plots only | DeepSeek pro drafts the routine download scripts; Opus does the calendar and alignment code; Grok runs the tests | src/ scripts, manifest, plots (no regressions) |
| 008 | Step 0 only: S3 plus the Chow test at 1 Jul 2025 | Opus; Grok review | Step 0 pass/fail |
| 009 | Target 1 estimation | Opus; Grok review | Rolling betas, break tests, M1-M3 |
| Later | CFTC release calendar, then the Target 0 run | Opus (timing-critical); Grok checks the shutdown dates | Run after the Target 1 note |

## 3. Tool split

| Tool | Does | Does not |
|---|---|---|
| Claude Opus | Protocol diffs; calendar, alignment and origin-index code; estimation; synthetic-data tests with known betas; checks that no input date is after the forecast origin | Make Steven's decisions |
| Grok (`grok -p`, grok-4.7) | Web lookups, such as release rules and shutdown dates. Reviews every code diff by running the tests | Approve econometrics on reading alone; write protocol text |
| DeepSeek via opencode (`opencode-go/deepseek-v4-flash` or `-pro`) | Flash: lit-note fixes, LOG entries, mechanical edits. Pro: routine download scripts (FRED) | Edit PROTOCOL.md or timing code. Steven diffs every edit |

## 4. Facts checked during the council

| Fact | Status | Source |
|---|---|---|
| MOF monthly intervention total, 29 Jun to 29 Jul 2026: ¥0 | verified (task 005) | https://www.mof.go.jp/english/policy/international_policy/reference/feio/monthly/20260731e.html |
| COT reports suspended during the Dec 2018 to Jan 2019 shutdown. Last report published 21 Dec 2018; the 28 Dec 2018 report came out 1 Feb 2019; after that, two reports a week until current | verified (read on cftc.gov) | https://www.cftc.gov/PressRoom/PressReleases/7864-19 |
| COT reports suspended during the 2025 shutdown. The first missed report was due 3 Oct 2025; catch-up started 19 Nov 2025; the backlog was planned to clear by the 23 Jan 2026 report | verified (read on cftc.gov) | https://www.cftc.gov/PressRoom/PressReleases/9138-25 |
| COT 2013 suspension (the 4 Oct 2013 report was missed; catch-up from 25 Oct 2013) and a 9 Dec 2025 revised catch-up schedule | Grok answer only, **not verified** | cftc.gov press releases 6745-13, 6738-13, 9147-25 (per Grok) |

## 5. How the council ran

| Stage | Who | Result |
|---|---|---|
| Advisors | 5 parallel agents: Contrarian, First Principles, Expansionist, Outsider, Executor | All five: freeze in one session; none of the 8 issues blocks it |
| Peer review | 5 anonymous reviewers (econometrician, FX practitioner, project manager, reproducibility, generalist) | All 5 rated the Contrarian strongest and the Expansionist's answer the biggest blind spot (scope creep; a 3-month hold-out has no power) |
| Chairman | 1 agent with all inputs | Produced rows 1-15. The new rows 10-15 came from peer review, not from the advisors |

- Where the council disagreed: dropping T3 (Contrarian) versus keeping it (majority). The chairman kept it.
- New risks only the peer review found: stale YCC-era 2Y quotes (row 10); the 15% trimming blind spot after Dec 2024 (row 12); about 100 tests in Target 1 (row 14); a freeze nobody else can check (row 15).
