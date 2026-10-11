# TASK 006: Apply Steven's decisions on the task 005 issues (protocol v3); freeze only on Steven's word

Scope: turn Steven's decision sheet into protocol text. No code, no data download, no estimation. Save this brief verbatim as docs/briefs/006_decisions_and_freeze.md.

## Before starting
- Read the last 3 entries of docs/LOG.md, docs/handoff/006_handoff.md, docs/verification/005_council_verdict.md (decision sheet, section 1), Table 3 of docs/verification/005_freeze_checklist.md, and docs/PROTOCOL.md in full.
- Precondition: every row 1-15 of the decision sheet has Steven's call (in the file or pasted in chat). If any row is empty or unclear, stop and ask. Never fill a row with the council default on your own.

## Steps
1. docs/PROTOCOL.md (explicitly authorised by this brief; any section): apply each row exactly as Steven decided, and nothing else. Update section 12 (order of work) to: 006 decisions and freeze, 007 data ingest, 008 Step 0, 009 Target 1 estimation. Set STATUS to "DRAFT v3 (task 006), ready for sign-off".
2. docs/DECISIONS.md (explicitly authorised): one dated entry per changed row, each with a one-line reason.
3. docs/lit/002_prior_work.md row 6 and docs/lit/002_deep_review.md: correct the IMF CR 26/75 method text. The rate driver in the p.12 figure combines the 2Y and 10Y US-Japan spreads; the regression form is not stated. Delegate the edit to DeepSeek flash via opencode (command in docs/handoff/006_handoff.md, section 3), then check its diff yourself.
4. Run the Grok review of the PROTOCOL and DECISIONS diff (command in section 3 of the handoff). Fix real mismatches. In the LOG, list each Grok point and what you did with it. Run git status after every Grok or opencode call.
5. Freeze only if Steven writes "freeze" in chat after reading the diff. Then: set STATUS to "FROZEN (date)"; fix the main-sample end date (row 5); add a DECISIONS.md entry "Protocol v3 frozen"; create git tag protocol-v3-frozen; record the SHA-256 of PROTOCOL.md in DECISIONS.md and the LOG. If row 15 is ticked, Steven registers the hash on OSF himself.
6. LOG entry; commit "task 006: protocol v3, decisions applied" (and "task 006: protocol v3 frozen" if frozen); push, plus git push --tags if tagged; handoff block; update the Notion project page (standing permission).

## Acceptance criteria
- Every row 1-15 appears in PROTOCOL.md exactly as Steven called it; no other design change.
- The Grok review and how each point was handled are in the LOG.
- Without Steven's "freeze" in chat, STATUS stays DRAFT.
- No code, no data files, no estimation.
