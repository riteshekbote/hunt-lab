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

- 1 lead(s) marked VALID at 2026-08-11 03:00:37 UTC
  - | **VALID** | 0 | — |

- 1 lead(s) marked VALID at 2026-08-11 04:47:51 UTC
  - **Verdict: HOLD** — Novelty is nil (public issue #5520); validity hinges on whether token is still live. Revoke-check required; if live, demote to VALID via Google VRP.

- 1 lead(s) marked VALID at 2026-08-11 05:46:39 UTC
  - | **VALID** | 0 | — |

- 1 lead(s) marked VALID at 2026-08-11 06:47:10 UTC
  - | **VALID** | 0 | — |

- 1 lead(s) marked VALID at 2026-08-11 09:38:51 UTC
  - | VALID  | 0  | — |

- 1 lead(s) marked VALID at 2026-08-11 12:39:25 UTC
  - | VALID | 0 | — |
