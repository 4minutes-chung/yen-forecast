# TASK 005: Verify the pre-freeze checklist (PROTOCOL.md section 11)

Scope: verify every open fact the protocol depends on, from primary sources, and write the results down. No code, no data download into the repo, no estimation, no design changes.
Save this brief verbatim as docs/briefs/005_freeze_checklist.md.

## Before starting
- Read the last 3 entries of docs/LOG.md, and docs/PROTOCOL.md (DRAFT v2) sections 2, 3, 9, 11 and 14.
- Read sources with WebFetch or the browser. Ask Steven before downloading any file. Nothing goes into data/.
- A fact counts as verified only if it was read from its primary source in this session. Record the URL.

## Items to verify
1. BOJ policy dates: negative-rate introduction, YCC introduction, YCC adjustments (expected Dec 2022, Jul 2023, Oct 2023), exit (expected Mar 2024), and every later rate change to date. Source: BOJ Statements on Monetary Policy. Record decision date, effective date, one-line content, URL.
2. BOJ 17:00 JST USD/JPY series in BOJ Time-Series Data Search: series code, first date, how a script can download it (CSV/API URL), terms of use.
3. FRED DGS1, DGS3, DGS5, DGS10 (recheck DGS2): date ranges; time of day the yields refer to (H.15 notes, Treasury yield-curve methodology); compounding basis.
4. Intervention dates: MOF foreign exchange intervention operations page. Record URL, coverage start, frequency, format, publication lag.
5. Early-August 2024 carry-unwind window: exact start and end dates to drop, with the sentence from BIS Bulletin 90 (or another primary source) that justifies them.
6. CFTC Commitments of Traders, JPY futures: first available date; report series (legacy futures-only); field names for non-commercial long, short and open interest; release day and lag.
7. Test references, full citation plus a one-line answer each: Bai-Perron; Elliott-Mueller qLL; Newey-West lag rule; Clark-West (valid with a rolling window?); Pesaran-Timmermann (needs independent observations?); HAC lag for overlapping h-month forecasts.
8. Pre-2016 references for the Target 0 predictors: Fama (1984); Eichenbaum and Evans (1995); Brunnermeier, Nagel and Pedersen (2008). Full citation and the one-line finding that justifies each predictor.
9. IMF Country Report 26/75 para 16: the staff method behind the decoupling. Masujima and Sato (2024, RIETI DP 24-E-054): Table 7 and how the YCC term is coded. Two or three lines each.

## Output
1. docs/verification/005_freeze_checklist.md: table first (item, status = verified / not found / conflicts with protocol, value, URL), then 2-4 bullets.
2. docs/PROTOCOL.md (explicitly authorised by this brief; sections 2, 3, 9, 11 and 14 only): replace "to verify" text with verified values and tick the checklist. Keep STATUS DRAFT. Do not change any design choice. If a verified fact forces a design change (for example CFTC data starts after 1995, or the BOJ 17:00 series cannot be downloaded by a script), stop and report it to Steven instead of editing.
3. docs/LOG.md entry, commit "task 005: freeze checklist verification", push, handoff block.
4. Update the Notion project page (standing permission from Steven): step 5 status, freeze checklist ticks, open issues.

## Acceptance criteria
- Items 1-9 each have a status and a URL, or "not found" with where you looked.
- PROTOCOL.md still says DRAFT; no design choice changed.
- No code, no data files, no estimation.
- Steven's sign-off (last checklist item) stays unticked: the freeze is his.
