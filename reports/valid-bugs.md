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

- 1 lead(s) marked VALID at 2026-08-11 15:30:16 UTC
  - | **VALID** | 0 | — |

- 1 lead(s) marked VALID at 2026-08-11 16:45:52 UTC
  - | **VALID** | 0 | — |

- 1 lead(s) marked VALID at 2026-08-11 17:48:47 UTC
  - | VALID | 0 | — |

- 3 lead(s) marked VALID at 2026-08-11 18:45:02 UTC
  - **Verdict: VALID**
  - | Q7 | ⚠️ Triager would demand proof the token was valid |
  - | **VALID** | 1 | **ADK #2128** — `/run_sse` broadcasts OAuth `client_secret` (CVSS 9.1) |

- 1 lead(s) marked VALID at 2026-08-11 19:54:02 UTC
  - | VALID | 0 | — |

- 1 lead(s) marked VALID at 2026-08-11 21:35:24 UTC
  - | **VALID** | 0-1 | Lead 5 (HOLD pending dup-check) |

- 1 lead(s) marked VALID at 2026-08-12 03:34:04 UTC
  - | **VALID** | 0 | — |

- 4 lead(s) marked VALID at 2026-08-21 19:18:46 UTC
  - | Q3 Impact | ⚠️ If token is still valid → full account compromise. If expired → no impact |
  - | Q6 Rejected list | ⚠️ "Information disclosure" unless token is live/valid |
  - | Q7 Reasonable triager | ⚠️ Only if token is currently valid |
  - **Verdict: HOLD** — Severity depends entirely on whether the `ya29.*` token is still valid. Git history secrets are a real finding but bounty-eligible only if they lead to actual compromise. Google ma

- 1 lead(s) marked VALID at 2026-08-21 20:46:08 UTC
  - **Verdict:** HOLD — Depends on whether this is Google's own deployment vs. open-source self-hosted. If on `googleapis.com` or `cloud.google.com` → VALID.

- 4 lead(s) marked VALID at 2026-08-21 21:13:46 UTC
  - | Q7 Acceptable | Depends — if Google VRP accepts OSS bugs via the `google` github org, this is valid but already filed. |
  - | Q5 Novel | NO — this is a *fixed* commit. Reporting a bug that's already been patched is not a valid bounty submission. |
  - | Q2 Reachable | NO — these are GCP API discovery documents and authenticated API endpoints. Without valid GCP credentials and IAM permissions, you cannot call these APIs. The `$discovery/rest` endpoi
  - | 11 | GCP OAuth redirect_uri IPv6 | **HOLD** | Potentially valid but likely already reported by Benchikh (Apr 2025). Verify novelty. |

- 1 lead(s) marked VALID at 2026-08-21 23:45:00 UTC
  - | **VALID** | 0 | — |
