# TASK 001: Free daily data availability check (JP 2Y, US 2Y, USD/JPY)

Scope: read-only check that free daily data exists for the three series the
research question needs. No downloads into the repo, no code, no data files.
The whole plan depends on JP 2Y; if it is not available before 2016, stop and
report so the question can be changed first.

## Steps
1. Japan MOF English JGB interest rate page: confirm a daily 2Y column exists,
   its first date, last date, and whether a separate current-month file is needed.
2. Record the price and time basis from MOF's own Q&A (not from memory).
3. Record MOF terms of use.
4. FRED DGS2 and DEXJPUS: record frequency, date range, source release, and timing note.

## Acceptance criteria
- One row per series: source URL, first date, last date, price/time basis, reuse terms.
- Gate: JP 2Y daily data starts before January 2016 (pass / fail).
- LOG.md entry and handoff block.
