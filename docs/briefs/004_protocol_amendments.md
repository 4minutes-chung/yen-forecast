# TASK 004: Protocol amendments (option C') and Target 0 pre-registration

Scope: Steven agreed to option C' on 2026-10-09 (chat). Amend the draft protocol and record the decision. No code, no data, no estimation.

## Steps
1. docs/PROTOCOL.md (explicitly authorised by this brief):
   - Rewrite section 10 (link to the 0-2 year view): baseline = today's spot (random walk); scenarios are yield surprises relative to the curve-implied forward 2Y; betas matched to horizon; limitations stated (term premium in forwards; scaling a monthly beta assumes no mean reversion within the horizon); report "no usable view" when the band includes zero.
   - Add a Target 0 pre-registration section (DRAFT, joint freeze with Target 1): horizons 1, 3, 6, 12 months; long sample from about 1985; rolling window fixed now plus one robustness variant; exactly three predictor models fixed now with exact transformations and publication lags; random walk without drift as benchmark; MSFE ratio, Clark-West with HAC, direction test; headline significance threshold fixed now; honest power statement; disclosure of what literature was read before pre-registering.
   - Update section 1 (scope), section 11 (pre-freeze checklist) and section 12 (order of work) to match.
2. docs/DECISIONS.md (explicitly authorised by this brief): one entry recording C' in the words shown to Steven.
3. LOG.md entry, commit "task 004: protocol amendments and Target 0 pre-registration", push, handoff block.
4. Update the Notion project page (standing permission from Steven).

## Acceptance criteria
- PROTOCOL.md still says DRAFT, not FROZEN.
- Every Target 0 choice that could be made after seeing results is fixed in the text.
- No code, no data files, no estimation.
