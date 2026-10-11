# Task 002 addendum: deep review of prior work

Builds on docs/lit/002_prior_work.md (8 verified hits). Lines marked "inference" are my reading of the evidence, not something a source states.

## 1. What the evidence says

| Finding | Sources | Evidence quality | Agreement |
|---|---|---|---|
| Expansionary BOJ shocks weaken the yen | BOJ WP (Ikkatai et al. 2024), IMF WP/25/227 | Strong: two identified designs, but samples end Dec 2023 | High |
| The rate-differential channel is a minor part of the yen's response to BOJ policy | BOJ WP 2024 | Medium: one paper, policy days only | Low (single source) |
| The yen's sensitivity to rates changes across regimes | Kano & Wada 2017 (Abenomics), Masujima & Sato 2024 (2Y effect peaks 2012, 2017), Ferrari et al. 2017 (state-dependent) | Medium: three designs, three episodes | High that it varies; no agreement on direction |
| The yen-rate link weakened after 2024 / mid-2025 | IMF CR 26/75 (staff, descriptive), Lazard 2026 (no method) | Weak to medium: no formal test | Growing |
| The weakening is tied to term premium, fiscal risk and positioning | IMF CR 26/75; Masujima & Sato (sovereign-risk sensitivity rose under YCC) | Weak: staff narrative plus one older precedent | Emerging |

## 2. Paper by paper

| Paper | Strongest point | Main weakness | What it means for us |
|---|---|---|---|
| Ikkatai, Kawamoto & Sakura (BOJ WP, Dec 2024) | Uses the whole curve (OIS, 2Y, 10Y, 30Y), measures FX in Tokyo hours to avoid US-hours noise | Policy days only; ends Dec 2023; the only regime test is a 2012/2013 split (no change found) | Baseline expectation: a weak rate channel. Their FX timing gives us a fix for the timing mismatch (section 5) |
| Carrière-Swallow et al. (IMF WP/25/227, Nov 2025) | Intraday identification, 151 meetings, includes JGB 2Y | Ends Dec 2023; the yen is a side outcome | BOJ policy-day surprises were tiny (median 0.1bp). JGB 2Y barely moved under YCC, so a YCC-era beta will be estimated imprecisely |
| Masujima & Sato (RIETI DP 24-E-054, Apr 2024) | Closest design: daily log changes of FX on changes in the 2Y differential and spreads, with a YCC regime term | Regime date imposed (Sep 2016), not tested; ends Dec 2022; Bloomberg data | Shows the regression form works; we extend the sample and let the data choose the breaks. Also shows a sovereign-risk channel under YCC |
| Kano & Wada (JIMF 2017) | Documents a regime shift in the yen's maturity-specific rate sensitivity (after Abenomics) | Read abstract only; data and sample not seen | A shift in the beta on the differential can come from the foreign leg. We must estimate the JP 2Y and US 2Y legs separately |
| Ferrari, Kearns & Schrimpf (BIS WP 626, 2017) | Cross-country high-frequency evidence that FX sensitivity rises as rates approach the lower bound; short maturities matter most | Ends 2015; BOJ inclusion not confirmed | Gives a competing prediction (H2 below). Supports using the 2Y, not the 10Y |
| IMF Article IV (CR 26/75, Apr 2026) | Official statement that the yen has decoupled from the US-Japan yield differential since mid-2025 (10Y differential narrowed by over 100bp); unexplained part co-moves with futures positions | Decomposition into a rate driver (2Y and 10Y spreads combined), VIX, oil and a residual; regression form not stated (task 005) | The decoupling is now a known fact. The open question is whether it holds at 2Y (H3 below) |
| Lazard (Jul 2026, practitioner) | Clear claim: the differential explained about 90% before 2024, and less after | No method; "90% of variance" is probably a levels R², which is unreliable for trending series (inference) | Weak evidence. Do not copy levels regressions; use changes |
| BIS Bulletin 90 (Aug 2024) | Sizes the yen carry trade (about ¥40tn) and the August 2024 unwind | Not about rate sensitivity | A known outlier inside our window: report results with and without it |

## 3. Where the evidence points different ways: three testable hypotheses

| | Hypothesis | Source | Prediction for the beta of ΔUSD/JPY on ΔJP 2Y |
|---|---|---|---|
| H1 | Ours: JGB 2Y regains information as YCC/NIRP are removed, so sensitivity rises | Original plan | Beta near zero and imprecise in 2016-2022, larger in absolute value after 2022-2024 |
| H2 | Lower-bound amplification: FX does more of the adjusting when rates are stuck, so sensitivity is highest under YCC and falls after exit | Ferrari et al. (if it carries over to Japan) | Beta per bp largest in 2016-2022, smaller after the exit |
| H3 | Composition (inference): the post-2025 decoupling comes from term premium and fiscal risk in long yields, not from the policy path | Inference from IMF CR 26/75 plus Masujima & Sato | 2Y beta stays negative (a higher JP 2Y means a stronger yen) after 2025, while the 10Y beta weakens or flips sign |

Comparing H3 against H1 and H2 is the most useful contribution available. It needs JP 10Y alongside JP 2Y, which the MOF file already has (task 001).

## 4. Gaps: what nobody we found has done

1. A formal break test of the yen's daily sensitivity to JP short rates over 2016-2026. All academic samples stop at Dec 2022 or Dec 2023.
2. Separate JP and US legs after 2022. Every post-2024 source uses one combined differential.
3. A 2Y versus 10Y comparison of the post-2025 decoupling (H3).
4. Handling the YCC-era low variance of JGB 2Y when comparing regimes.

## 5. Design risks the literature exposes, and fixes

| Risk | Source | Fix to consider in the step 3 and 4 briefs |
|---|---|---|
| Timing: JGB 3pm Tokyo close vs DEXJPUS NY noon | Task 001 | BOJ publishes USD/JPY at 17:00 JST (interbank mid rate) daily since 4 Jan 2007, with long series in BOJ Time-Series Data Search (verified on boj.or.jp; series code and start date not yet checked). Use the 17:00 JST rate for the JP leg; the US leg still needs 2-day changes or NY-time FX |
| Low JGB 2Y variance under YCC makes the beta noisy, and a rise in the beta may just reflect yields moving again | IMF WP/25/227; council point | Report each window's beta with its confidence band, its correlation, and JGB 2Y volatility side by side |
| A change in the combined-differential beta may come from the US side | Kano & Wada | Estimate separate JP 2Y and US 2Y coefficients |
| Positioning and intervention drive the residual | IMF CR 26/75; BIS Bulletin 90 | Robustness: drop intervention days and the early-August 2024 window; add CFTC yen positioning (weekly) as a check |
| Levels regressions inflate fit | Lazard's 90% figure (inference) | Changes only, as Masujima & Sato do |

## 6. Recommendation (Steven's decision)

- Reword the question to something like: "How did USD/JPY's daily sensitivity to the JP 2Y yield change from YCC (2016) through the dismantling (2022-24) to after the exit (2024-26), and does the post-2025 decoupling seen in the 10Y differential also show up at 2Y?"
- Write down H1, H2 and H3 with their predictions in the step 4 protocol before any estimation. Any of the three outcomes is then informative, and none is an empty null.
- Possible contribution sentence: "Official and market sources report that the yen decoupled from rate differentials after 2024; I test when the link changed and whether the change sits in the policy-path (2Y) or long-end (10Y) component."

## 7. Read in full before writing the step 4 protocol

1. IMF CR 26/75, para. 16 and any yen chart or annex: what method the "staff analysis" used.
2. Masujima & Sato (2024): model, Table 7, and how the YCC term is coded.
3. Ikkatai et al. (2024): the data section and the split-sample appendix (Chart A-2).
4. Ferrari, Kearns & Schrimpf: confirm the BOJ is in the sample, and their state-dependence specification.
5. Kano & Wada (2017): how they measure maturity-specific sensitivity.

## 8. Limits of this review

- The search took about 11 minutes and used web search engines only: no Google Scholar, EconLit or forward-citation tracing. A forward-citation search on Ferrari et al. and Kano & Wada is the best next search.
- Japanese-language work is not covered (for example Miyamoto 2023, cited in the BOJ WP, and Japanese-only BOJ Reviews).
- 5 leads remain unverified (see 002_prior_work.md).
