# Valid Bugs Register

> NOTE: counts added before 2026-08-07 ~15:10 UTC were false positives of the
> counter regex (it matched "INVALID" verdicts and hypothetical "**VALID**"
> mentions). Manual verification found 0 reportable bugs in those cycles.
> The counter was fixed at 2026-08-07 ~15:05 UTC.

- 2026-08-07 15:05 UTC — MANUAL VERIFICATION: 0 valid bugs. Rejected:
  - ADK issue #5520 (ya29 token in git history) — already public, closed 2026-04-28 (Google OSS VRP, issuetracker 504158909)
  - ADK issue #2128 (/run_sse client_secret leak) — already public, closed 2025-07-23
  - google/xrblocks docusaurus Algolia apiKey 40150cc2... — public DocSearch search-only key (by design)
  - All other leads: API discovery, auth-gated 401s, scope metadata, patched commits
