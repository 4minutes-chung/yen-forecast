# Research Protocol

STATUS: DRAFT v2 (2026-10-09, task 004), not frozen. Do not estimate anything against this file until it says FROZEN. Freezing needs Steven's sign-off. After freezing, every change is logged in docs/DECISIONS.md with a reason.

## 1. Scope

- Target 1 question: how did USD/JPY's daily sensitivity to the JP 2Y yield change from YCC (2016) through the dismantling (2022-24) to after the exit (2024-26), and does the post-2025 decoupling from the 10Y differential also show at 2Y?
- In-sample and descriptive. Results describe co-movement, not causation, and are not forecasts.
- Target 0 (forecasts against a random walk) is pre-registered in section 14. It is frozen together with Target 1 and run only after the Target 1 note is done.

## 2. Data

| Series | Source | Use | Status |
|---|---|---|---|
| USD/JPY 17:00 JST (interbank mid) | BOJ Time-Series Data Search | Main FX series | Verified (task 005): DB FM08, code FXERD04, daily from 1998-01-05; script download via the BOJ API (getDataCode, CSV/JSON, no key) |
| USD/JPY NY noon | FRED DEXJPUS | Robustness | Verified (task 001) |
| JP 2Y, JP 10Y | MOF jgbcme CSVs, 15:00 JST close | Main | Verified (task 001) |
| US 2Y | FRED DGS2 | Main | Verified (task 001, 005): from 1976-06-01; H.15 CMT from FRBNY bid-side quotes at or near 3:30 PM ET; bond-equivalent (semiannual) par yield |
| US 10Y | FRED DGS10 | Slope term | Verified (task 005): from 1962-01-02; same basis as DGS2 |
| Intervention dates | MOF Foreign Exchange Intervention Operations (historical CSV) | Robustness | Verified (task 005): daily rows from 1991-05-13; daily detail published quarterly, about 5 weeks after quarter end |

All data enters through scripts in src/. Raw files are never committed.

## 3. Sample and regime windows

- Main sample: 2016-01-04 to the last date available at freeze. The end date is fixed in the freeze entry.
- Robustness sample: from 2010-01-04.
- Regime windows are labels for reporting, not imposed breaks: YCC (Sep 2016 to Nov 2022), transition (Dec 2022 to Mar 2024), post-exit (Apr 2024 onward), post-mid-2025 (Jul 2025 onward). Verified dates (task 005, BOJ statements and IMF CR 26/75 para 16): negative rate decided 29 Jan 2016, effective 16 Feb 2016; YCC introduced 21 Sep 2016; 10Y band widened 20 Dec 2022; greater flexibility 28 Jul 2023; 1% reference 31 Oct 2023; exit decided 19 Mar 2024, effective 21 Mar 2024; IMF dates the decoupling "since mid-2025" (yen appreciated through end-June 2025, depreciated since). The month labels differ from these decision dates by days to weeks; open for Steven (docs/verification/005_freeze_checklist.md, Table 3).

## 4. Variables, sign convention, timing

- Δs = 100 × change in ln(USD/JPY), in yen per dollar. Positive means a weaker yen.
- Δj2 = change in JP 2Y (bp). Δjsl = change in JP (10Y − 2Y) slope (bp). Δu2 and Δusl are the US equivalents.
- Sign convention: a higher JP 2Y with a stronger yen gives a **negative** coefficient on Δj2. A higher US 2Y with a weaker yen gives a positive coefficient on Δu2.
- Collinearity: use the 2Y and the 10Y−2Y slope, never the 2Y and 10Y together. Chosen now, before any estimation.
- Timing: FX at BOJ 17:00 JST; JP yields at the 15:00 JST close on the same day. US yields use the NY close before the FX observation (the NY close of day t−1 for Tokyo 17:00 on day t). Exact US alignment is fixed after the H.15 time-of-day check.

## 5. Specifications (fixed list)

| ID | Equation | Frequency | Purpose |
|---|---|---|---|
| S1 | Δs = a + b_j2·Δj2 + b_jsl·Δjsl + e | Daily | JP legs only |
| S2 | Δs = a + b_j2·Δj2 + b_jsl·Δjsl + b_u2·Δu2 + b_usl·Δusl + e | Non-overlapping 2-day | Main: JP and US legs separated |
| S3 | Δs = a + b_10·Δ(US 10Y − JP 10Y) + e | Non-overlapping 2-day | Replicates the IMF CR 26/75 decoupling |
| S2m | S2 on non-overlapping month-end changes | Monthly | Horizon check for the 0-2 year use. Post-exit has about 30 observations, so these betas will be imprecise |

Inference: OLS with Newey-West HAC standard errors. The lag rule is fixed at freeze.

## 6. How time variation is measured

For S1, S2, S3 (and S2m where sample size allows):

1. Rolling betas over 250 trading days, with 95% HAC bands and the regime windows shaded.
2. Structural-break test with break-date confidence intervals (Bai-Perron: up to 3 breaks, 15% trimming).
3. Test for persistent time variation (Elliott-Müller qLL).
4. Three decoupling measures, reported for each regime window:
   - (M1) beta per bp;
   - (M2) explained share: R² and variance contribution b_j2² · var(Δj2) / var(Δs);
   - (M3) unexplained drift: intercept and cumulative residual.
5. Per window, report sd(Δj2) next to the beta. JGB 2Y barely moved under YCC, so YCC-window betas will be imprecise.

Not used for Target 1: the Giacomini-Rossi fluctuation test. It compares forecasts over time, so it belongs to Target 0.
All test references are to verify before freeze.

## 7. Hypotheses (written before any estimation)

Step 0: S3 must show the IMF mid-2025 weakening on at least one of M1-M3. If it does not, check data and timing before interpreting any 2Y result.

| H | Statement | Metric | Prediction |
|---|---|---|---|
| H1 | JP 2Y regains information as YCC is dismantled | M2 (and the absolute value of b_j2) | Near zero in the YCC window, higher after the transition; a break in b_j2 dated in or after the transition window |
| H2 | Lower-bound amplification (Ferrari, Kearns & Schrimpf) | M1 | Absolute value of b_j2 per bp largest in the YCC window, smaller after exit |
| H3 | The post-2025 decoupling sits in the long end, not the policy path | M1 for b_j2 and b_jsl, post-mid-2025 | b_j2 stays negative and significant; b_jsl weakens or turns positive |
| H4 | Betas are stable; the decoupling sits in the unexplained part | M1-M3 | No significant break in b_j2 or b_jsl after mid-2025, but R² falls or the cumulative residual drifts (yen weaker than rates imply). Consistent with IMF's positioning account |

H1 and H2 use different metrics and can both be true: the per-bp beta can fall while the explained share rises.

## 8. What counts as a surprise

- b_j2 positive and significant after mid-2025 (a higher JP 2Y with a weaker yen).
- A break in b_j2 dated before Dec 2022, outside the policy-change windows.
- S3 fails to show the IMF decoupling.
- Daily and monthly betas with opposite signs.

## 9. Robustness (fixed list; anything else is labelled exploratory)

1. DEXJPUS (NY noon) instead of BOJ 17:00.
2. Drop intervention days (MOF historical CSV) and the early-August 2024 carry unwind: 2 Aug 2024 to 9 Aug 2024 inclusive, the stress (2-5 Aug) and stabilisation (6-9 Aug) days shaded in BIS Bulletin 90, Graph 1.
3. Drop BOJ and FOMC meeting days; separately, meeting days only.
4. Sample from 2010.
5. Rolling windows of 125 and 500 trading days.

## 10. Link to the 0-2 year view (conditional, not a forecast)

- Driver decomposition: split the cumulative change in USD/JPY in each regime window into a rate-explained part and an unexplained part (from S2). This shows which driver matters now.
- Conditional scenario table:
  - Baseline: today's spot rate (random walk), consistent with Target 0.
  - A scenario is a yield surprise: the JP 2Y (or US 2Y) at the horizon ends X bp above or below the curve-implied forward 2Y for that horizon. Market-priced policy paths are already in yields and forward FX, so only the surprise part should move the yen.
  - Horizons and forwards: 1 month (baseline = current 2Y; the forward drift over one month is ignored); 12 months (2Y forward one year ahead, from the 1Y and 3Y yields); 24 months (2Y forward two years ahead, from the 2Y and 4Y yields; for the US, the 4Y is interpolated between 3Y and 5Y). Yields are semiannual-compounded. 6 months is dropped because the forward needs sub-1-year yields.
  - Betas: S2m (monthly) betas from the latest regime window for all three horizons. Daily S2 betas are shown next to them for comparison only.
  - Output: implied move in USD/JPY (%) with a 95% band. If the band includes zero, report "no usable rate-based view at this horizon" instead of a number.
- Stated limitations (not tested): forwards include a term premium, so "surprise vs forward" is approximate; scaling a monthly beta to a 12- or 24-month cumulative surprise assumes no mean reversion within the horizon.
- Any unconditional directional claim belongs to Target 0 (section 14) and must be tested against the random walk.

## 11. Before freezing

Tick rule (task 005): ticked = read from the primary source in task 005, even where the fact conflicts with the protocol (conflict noted); unticked = full text not read. Details and URLs: docs/verification/005_freeze_checklist.md.

- [x] YCC start, YCC adjustments, negative-rate start and exit dates from BOJ statements (section 3; later rate changes in the verification file)
- [x] BOJ 17:00 JST USD/JPY series code and start date (FM08 / FXERD04, from 1998-01-05)
- [x] FRED DGS10; H.15 time-of-day basis for DGS2 and DGS10 (FRBNY quotes at or near 3:30 PM ET; DGS10 from 1962-01-02)
- [x] Source for intervention dates (MOF historical CSV, from 1991). Note: ¥15.4tn intervened 30 Jul-26 Aug 2026; daily dates not published until 2-9 Nov 2026
- [x] Exact dates for the August 2024 window (2-9 Aug 2024, BIS Bulletin 90)
- [ ] Test references: Newey-West verified (lag rule options in the verification file); Bai-Perron and Elliott-Müller citation and abstract only (full text not read)
- [x] Read IMF CR 26/75 para 16 (method) and Masujima & Sato Table 7. Conflict: the IMF rate driver combines the 2Y and 10Y spreads; regression form not stated
- [x] FRED short-maturity US yields for forwards (DGS1, DGS3, DGS5, all from 1962-01-02)
- [x] CFTC Commitments of Traders for JPY futures: start date, which report series (legacy futures-only, non-commercial), and release lag (code 097741, from 1986-01-15; historical release dates must be rebuilt from documented lags). Conflict: BNP's denominator is non-commercial open interest
- [x] Clark-West test with a rolling window; HAC lag rule for overlapping h-month forecasts. Rolling: valid (CW 2006). Conflict: CW recommend the West (1997)/Hodrick (1992) estimator, not Newey-West with lag h − 1
- [x] Direction test with overlapping forecasts (Pesaran-Timmermann assumes independent observations: confirmed; non-overlapping origins are consistent)
- [x] Pre-2016 references for the Target 0 predictors: Fama (1984), Eichenbaum & Evans (1995), Brunnermeier, Nagel & Pedersen (2008). Conflict: BNP link positions to crash risk (skewness), not to mean returns
- [ ] Steven's sign-off (Target 1 and Target 0 together)

## 12. Order of work

003 draft → 004 amendments and Target 0 pre-registration → 005 verify the section 11 checklist → Steven signs off and freezes Target 1 and Target 0 together → 006 data ingest (level plots allowed, no regressions) → 007 Target 1 estimation (rolling betas, break tests, M1-M3) → Target 1 note (by end Feb 2027) → Target 0 run exactly as pre-registered.

## 13. Done and stop rules

- Done: a 5-page note with one table and one figure by end of Feb 2027.
- Stop: two weekends of work with no surprise (section 8) means stop.

## 14. Target 0 pre-registration (DRAFT; frozen together with Target 1; run after the Target 1 note)

Purpose: test whether three simple, theory-based predictors forecast USD/JPY better than a random walk at 1-12 months. Every choice below is fixed now, before any estimation on our data.

### 14.1 Data and timing

- Monthly. Predictors use values from the last business day of month t.
- Target: Δ_h s = 100 × [ln S(t+h) − ln S(t)], where S(t) is DEXJPUS (NY noon) on the first NY business day of month t+1 and S(t+h) is the same convention h months later. Starting the target after month-end avoids any intraday overlap with the predictors.
- Horizons: h = 1, 3, 6, 12 months. No 24-month horizon (about 20 non-overlapping observations even from 1985).
- Data start D0 = the later of Jan 1985 and the first month in which all three predictors exist. D0 applies to every model and to the random walk, so all are compared on identical dates. Derived (task 005): the first CFTC JPY report is as of 15 Jan 1986 and was published in Feb 1986 (pre-1990 rule: 11th or 12th day of the following month), so D0 = Feb 1986.

### 14.2 Models (exactly three, plus the benchmark)

| ID | Predictor x(t) | Source and lag | Basis (pre-2016; verified task 005) |
|---|---|---|---|
| RW | None: forecast of Δ_h s is 0 (random walk without drift) | — | Meese-Rogoff benchmark |
| T1 Carry | US 2Y − JP 2Y at month-end t (pp) | FRED DGS2, MOF jgbcme; market prices known at the close | Fama (1984), J. Monetary Econ. 14(3), 319-338: slopes of the spot change on the forward premium are negative; the premium has predictive content |
| T2 Rate momentum | One-month change in (US 2Y − JP 2Y) up to month-end t (pp) | Same | Eichenbaum & Evans (1995), QJE 110(4), 975-1009: peak exchange-rate response comes after the policy shock (delayed overshooting); their shocks are identified, T2 uses raw changes |
| T3 Positioning | CFTC net non-commercial JPY futures position (long − short) / open interest | CFTC COT, legacy futures-only, contract 097741, from 1986-01-15 (fields noncomm_positions_long_all, noncomm_positions_short_all, open_interest_all); latest report whose public release date is on or before the last business day of month t (reports as of Tuesday, now released Friday 3:30 p.m. ET; historical release dates rebuilt from CFTC's documented lags) | Brunnermeier, Nagel & Pedersen (2008), NBER Macro Annual 23, 313-348: positions predict crash risk (skewness), weakly; mean-return prediction not tested; denominator is non-commercial open interest (open for Steven) |

- Each model: Δ_h s = α + β·x(t) + e, estimated by OLS. One predictor per model. No combinations, no model averaging, no added predictors.

### 14.3 Estimation scheme

- Main: rolling window of 120 months. At origin t, use only pairs whose outcome is known at t (τ from t − h − 119 to t − h).
- First origin: D0 + 120 months. Last origin: the last month-end whose h-month outcome is available at freeze.
- Robustness (one variant only): recursive window from D0.

### 14.4 Evaluation

- MSFE ratio (model / RW) for each model and horizon.
- Clark-West test of each model against RW (nested, one-sided), Newey-West HAC standard errors with lag h − 1.
- Direction: share of origins where the forecast sign matches the realised sign; tested on non-overlapping origins (every h-th origin from the first) with Pesaran-Timmermann.
- Headline threshold: Bonferroni over 12 tests (3 models × 4 horizons), one-sided 5% / 12. All 12 raw p-values are reported.
- Sub-samples, reported but not headline: origins before Jan 2016 and from Jan 2016.

### 14.5 Expected power (rough)

With about 30 non-overlapping 12-month out-of-sample periods, a predictor with a true R² of 5% would be detected about a third of the time at 5%, and under 10% of the time at the Bonferroni threshold (normal approximation, in-sample t-statistic; a real out-of-sample test has less power). A null result is expected and is pre-declared as informative: "these three predictors do not beat a random walk at 1-12 months."

### 14.6 Disclosure

The predictors are fixed before any estimation in this repo, but after reading the 2024-26 literature in docs/lit/ (task 002). T3 overlaps with IMF CR 26/75, which links recent yen moves to futures positioning.
