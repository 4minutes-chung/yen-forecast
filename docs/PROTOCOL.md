# Research Protocol

STATUS: DRAFT v1 (2026-10-09), not frozen. Do not estimate anything against this file until it says FROZEN. Freezing needs Steven's sign-off. After freezing, every change is logged in docs/DECISIONS.md with a reason.

## 1. Scope

- Target 1 question: how did USD/JPY's daily sensitivity to the JP 2Y yield change from YCC (2016) through the dismantling (2022-24) to after the exit (2024-26), and does the post-2025 decoupling from the 10Y differential also show at 2Y?
- In-sample and descriptive. Results describe co-movement, not causation, and are not forecasts.
- Target 0 (forecasts against a random walk) has no protocol yet. It will be drafted separately.

## 2. Data

| Series | Source | Use | Status |
|---|---|---|---|
| USD/JPY 17:00 JST (interbank mid) | BOJ Time-Series Data Search | Main FX series | Exists (boj.or.jp); series code and start date to verify |
| USD/JPY NY noon | FRED DEXJPUS | Robustness | Verified (task 001) |
| JP 2Y, JP 10Y | MOF jgbcme CSVs, 15:00 JST close | Main | Verified (task 001) |
| US 2Y | FRED DGS2 | Main | Verified (task 001); time of day to verify |
| US 10Y | FRED DGS10 | Slope term | To verify |
| Intervention dates | Source to identify (MOF) | Robustness | To verify |

All data enters through scripts in src/. Raw files are never committed.

## 3. Sample and regime windows

- Main sample: 2016-01-04 to the last date available at freeze. The end date is fixed in the freeze entry.
- Robustness sample: from 2010-01-04.
- Regime windows are labels for reporting, not imposed breaks: YCC (Sep 2016 to Nov 2022), transition (Dec 2022 to Mar 2024), post-exit (Apr 2024 onward), post-mid-2025 (Jul 2025 onward). All dates to verify against BOJ statements and IMF CR 26/75 before freeze.

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
2. Drop intervention days and the early-August 2024 carry unwind (exact dates to verify).
3. Drop BOJ and FOMC meeting days; separately, meeting days only.
4. Sample from 2010.
5. Rolling windows of 125 and 500 trading days.

## 10. Link to the 0-2 year view

- Decompose the cumulative change in USD/JPY in each regime window into a rate-explained part and an unexplained part (from S2). This shows which driver matters now.
- Conditional scenario table: translate stated scenarios for JP 2Y and US 2Y over 6, 12 and 24 months into implied USD/JPY moves, using the latest-window betas with bands. Labelled conditional. It is not a forecast.
- Any unconditional directional claim belongs to Target 0 and must be tested against the random walk.

## 11. Before freezing

- [ ] YCC start, YCC adjustments, negative-rate start and exit dates from BOJ statements
- [ ] BOJ 17:00 JST USD/JPY series code and start date
- [ ] FRED DGS10; H.15 time-of-day basis for DGS2 and DGS10
- [ ] Source for intervention dates
- [ ] Exact dates for the August 2024 window
- [ ] Test references: Bai-Perron, Elliott-Müller, Newey-West lag rule
- [ ] Read IMF CR 26/75 para 16 (method) and Masujima & Sato Table 7
- [ ] Steven's sign-off

## 12. Order of work

003 (this draft) → Steven signs off and freezes → 004 data ingest (level plots allowed, no regressions) → 005 rolling betas and break tests under this protocol.

## 13. Done and stop rules

- Done: a 5-page note with one table and one figure by end of Feb 2027.
- Stop: two weekends of work with no surprise (section 8) means stop.
