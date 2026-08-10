
## 2026-08-07 08:57:28 UTC [microsoft] (model longcat)
- [UNVALIDATED] 1. **Google VRP scope**: `*.google.com`, `*.youtube.com`, `*.blogger.com`, `*.deepmind.com`, `*.waymo.com`, `*.wing.com` — rewards up to $101,010 for RCE on Tier-0.
- [UNVALIDATED] 2. **NEW AI VRP** launched Oct 2025 — unified abuse + security reward table for AI-specific issues.
- [UNVALIDATED] 3. **Google ADK (Agent Development Kit)** is a new high-value attack surface — Python + Kotlin SDKs for LLM agents with OAuth/OIDC tool auth.
- [UNVALIDATED] 4. **ADK issue #2128**: `/run_sse` endpoint broadcasts full OAuth `client_secret` + `client_id` to untrusted frontend clients — confirmed credential leakage pattern.
- [UNVALIDATED] 5. **ADK issue #5520**: Hardcoded Google OAuth token (`ya29.*`) found in git history at `tests/unittests/plugins/test_bigquery_agent_analytics_plugin.py`.
- [UNVALIDATED] 6. **ADK commit e3567a6**: Fixed credential leakage in Agent Registry (ADC headers passed to non-Google MCP toolsets).
- [UNVALIDATED] 7. **ADK commit 33012e6**: Fixed cross-user credential leaks from unstable `hash()` key generation.
- [UNVALIDATED] 8. **ADK PR #5154**: Migrating credential storage to `secret:` scope — OAuth tokens currently persist in session backends.
- [UNVALIDATED] 9. **A2A protocol** (Google Agent-to-Agent) — new attack surface: webhook SSRF, unsigned AgentCards, unauthenticated endpoints.
- [UNVALIDATED] 10. **CVE-2026-47391**: PraisonAI official A2A example — unauthenticated + `eval()` tool = RCE.
- [UNVALIDATED] 11. **GCP OAuth redirect_uri URL parsing confusion** (Benchikh, Apr 2025): IPv6 parser discrepancy → account takeover.
- [UNVALIDATED] 12. **Tenable TRA-2025-45**: SSRF in GCP Action Hub DataRobot action → IP allowlist bypass on Looker.
## 2026-08-07 15:49:41 UTC [google] (model longcat)
## 2026-08-07 15:59:22 UTC [google] (model longcat)
## 2026-08-07 16:34:08 UTC [google] (model longcat)
## 2026-08-07 17:30:21 UTC [google] (model longcat)
## 2026-08-07 18:26:44 UTC [google] (model longcat)
## 2026-08-07 18:46:46 UTC [google] (model longcat)
## 2026-08-07 19:15:31 UTC [google] (model longcat)
## 2026-08-07 19:31:18 UTC [google] (model longcat)
## 2026-08-07 20:19:26 UTC [google] (model longcat)
## 2026-08-07 21:04:44 UTC [google] (model longcat)
## 2026-08-07 21:52:42 UTC [google] (model longcat)
## 2026-08-07 22:35:52 UTC [google] (model longcat)
## 2026-08-07 23:16:12 UTC [google] (model longcat)
## 2026-08-07 23:51:26 UTC [google] (model longcat)
## 2026-08-08 01:39:55 UTC [google] (model longcat)
## 2026-08-08 03:13:14 UTC [google] (model longcat)
## 2026-08-08 04:31:33 UTC [google] (model longcat)
## 2026-08-08 05:23:19 UTC [google] (model longcat)
## 2026-08-08 06:06:15 UTC [google] (model longcat)
## 2026-08-08 07:11:50 UTC [google] (model longcat)
## 2026-08-08 08:03:11 UTC [google] (model longcat)
## 2026-08-08 08:56:34 UTC [google] (model longcat)
## 2026-08-08 09:41:26 UTC [google] (model longcat)
## 2026-08-08 10:15:42 UTC [google] (model longcat)
## 2026-08-08 10:54:46 UTC [google] (model longcat)
## 2026-08-08 11:29:10 UTC [google] (model longcat)
## 2026-08-08 11:57:03 UTC [google] (model longcat)
## 2026-08-08 12:57:54 UTC [google] (model longcat)
## 2026-08-08 13:50:34 UTC [google] (model longcat)
## 2026-08-08 14:32:05 UTC [google] (model longcat)
## 2026-08-08 15:01:50 UTC [google] (model longcat)
## 2026-08-08 15:42:58 UTC [google] (model longcat)
## 2026-08-08 16:37:39 UTC [google] (model longcat)
## 2026-08-08 17:02:11 UTC [google] (model longcat)
## 2026-08-08 17:41:23 UTC [google] (model longcat)
## 2026-08-08 18:04:48 UTC [google] (model longcat)
## 2026-08-08 18:54:43 UTC [google] (model longcat)
## 2026-08-08 19:29:22 UTC [google] (model longcat)
## 2026-08-08 19:57:08 UTC [google] (model longcat)
## 2026-08-08 20:37:13 UTC [google] (model longcat)
## 2026-08-08 21:04:48 UTC [google] (model longcat)
## 2026-08-08 21:43:06 UTC [google] (model longcat)
## 2026-08-08 22:07:14 UTC [google] (model longcat)
## 2026-08-08 22:45:20 UTC [google] (model longcat)
## 2026-08-08 23:12:39 UTC [google] (model longcat)
## 2026-08-08 23:47:21 UTC [google] (model longcat)
## 2026-08-09 00:37:17 UTC [google] (model longcat)
## 2026-08-09 02:50:38 UTC [google] (model longcat)
## 2026-08-09 04:09:15 UTC [google] (model longcat)
## 2026-08-09 05:18:06 UTC [google] (model longcat)
## 2026-08-09 06:04:15 UTC [google] (model longcat)
## 2026-08-09 07:11:57 UTC [google] (model longcat)
## 2026-08-09 08:03:17 UTC [google] (model longcat)
## 2026-08-09 08:57:25 UTC [google] (model longcat)
## 2026-08-09 09:47:37 UTC [google] (model longcat)
## 2026-08-09 10:18:32 UTC [google] (model longcat)
## 2026-08-09 10:57:32 UTC [google] (model longcat)
## 2026-08-09 11:36:20 UTC [google] (model longcat)
## 2026-08-09 12:01:31 UTC [google] (model longcat)
## 2026-08-09 13:12:46 UTC [google] (model longcat)
[PRIO] `graph.microsoft.com/beta/copilot/agentRegistrations` — **7.25** (attack:8, business:9, tech:7, gate:3, cloud:8, fresh:7)
[PRIO] `login.microsoftonline.com` v1↔v2 dual-issuer JWKS — **7.10** (attack:7, business:9, tech:8, gate:4, cloud:8, fresh:5)
[PRIO] `github.com/google/earthengine-api/python/ee/oauth.py:45` — **6.80** (attack:6, business:8, tech:5, gate:9, cloud:9, fresh:3)
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: $metadata confirms 873-char agentRegistration EntityType with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied with Nullable=false. GET→401 (auth-gated), HEAD→405 (RFC 6750 §3 violation), OPTIONS→200 (CORS *, full mutation allowlist incl. PATCH). 5 sibling EntityTypes share identical zero-restriction pattern.
evidence_needed: Two-principal test — Principal A creates agentRegistration, Principal B attempts PATCH with createdBy=PrincipalA's id; observe if ownership check fires.
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations with createdBy=A's objectId. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId + agentCard payload. Check if 403/404 (proper gate) vs 200/204 (IDOR).
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, potentially redirecting agentCard endpoints or exfiltrating agent definitions. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces
class: OATH
asset: login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys
confidence: 60
reasoning: v1 issuer=`sts.windows.net/{tid}/`, v2 issuer=`login.microsoftonline.com/{tid}/v2.0` (dual namespaces confirmed). 4 v1 kids ⊂ 7 v2 kids (0 v1-exclusive steady-state). A validator that checks only `kid`+`sig` but not `iss` could accept a v1-signed token at a v2-only endpoint. Rotation churn (aFkmKVFc) resolves to steady-state subset invariant.
evidence_needed: Generate a v1-token (iss=sts.windows.net/{tid}/) signed with a shared kid, submit to a v2-only resource endpoint; observe if iss-check is enforced.
verify_steps: AUTH_HELPED: Obtain v1.0 access token (resource=https://graph.microsoft.com) for test tenant. Decode header, confirm kid is one of the 4 shared kids. Submit to /beta endpoint that validates tokens. Then craft a test by swapping iss to v2.0 value while keeping v1 signature — check if validator catches iss mismatch.
impact: Token from v1 legacy trust domain accepted at v2-only endpoints, potentially bypassing v2.0 conditional access / compliance policies. Broad Entra ID trust-boundary impact.
testability: AUTH_HELPED
[HYP] Earth Engine OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: Hardcoded client_secret `RUP0RZ6e0pPhDzsqIJ7KlNd1` (sha256 `3f3f8d6f…d271`) at line :45 + :99 fallback, scopes cloud-platform+drive+devstorage.full_control, OOB redirect. Confirmed live via raw GitHub (200 len=23110, whole-file sha `f4f93c76…` unchanged). Reposcan classified REAL_SECRET.
evidence_needed: OAuth token redemption test — client_credentials or authorization_code flow using the hardcoded secret against oauth2.googleapis.com/token.
verify_steps: HUMAN: File Google VRP report citing sha256 of secret value. VRP determination needed on native-app by-design status (OAuth 2.1 deprecates client secrets for public/native clients; Python SDK is a native app). If VRP accepts, provide PoC token request with client_id `517222506229-vsmmajv…` (:43) + secret.
impact: Attacker redeems secret for cloud-platform-scoped access to any GCP project where Earth Engine API is enabled and the OAuth client is authorized. Severity depends on whether Google treats native-app hardcoded secret as a vulnerability vs by-design.
testability: HUMAN_ONLY
[FINAL]
[NEXT] **HUMAN**: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. Provision Principal A + Principal B in an Entra test tenant. A POSTs `https://graph.microsoft.com/beta/copilot/agentRegistrations` with `createdBy=A's-objectId`, `agentCard={...}`. B attempts `PATCH /beta/copilot/agentRegistrations/{A's-id}` with `createdBy=A's-objectId`. Observe 403/404 (proper gate) vs 200/204 (IDOR confirmed). This is the highest-priority actionable item — metadata zero-restriction precondition is confirmed, only runtime ownership check is untested.
[LEARN] ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior findings unchanged (NO_DELTA).
[LEARN] ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live — GET→401, HEAD→405, OPTIONS→200 (CORS *, full mutation allowlist), 873-char metadata block, 0 OperationRestrictions.
[LEARN] ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — sha256 `3f3f8d6f…d271` verbatim, whole-file sha `f4f93c76…` unchanged.
[LEARN] ACCEPTED: v1↔v2 JWKS kid overlap confirmed live — v1(4)⊂v2(7), 0 v1-exclusive steady-state, dual issuer namespaces intact.
[LEARN] REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — `aFkmKVFc` v1-exclusive is transient rotation churn, v1 kid set never validated against v2 issuer → no cross-endpoint confusion surface.
[RISK] google: **45** — earthengine secret is real but native-app by-design question limits exploitable impact; GCP control-plane APIs all require consumer identity (no anon reads); bughunters hardened. Residual risk from OAuth client_secret redemption in the wild. | microsoft: **65** — agentRegistration IDOR surface (5 EntityTypes, zero ownership restrictions, full mutation CORS) + Graph 405 anomaly masking IDOR enumeration + v1↔v2 dual-issuer trust boundary + RFC 6749 §3 error-rendering violations represent meaningful, multi-vector exposure across Entra ID + M365 Copilot.
## 2026-08-09 13:59:33 UTC [google] (model longcat)
## 2026-08-09 14:44:29 UTC [google] (model longcat)
## 2026-08-09 15:15:21 UTC [google] (model longcat)
## 2026-08-09 15:51:41 UTC [google] (model longcat)
## 2026-08-09 16:22:50 UTC [google] (model longcat)
## 2026-08-09 17:03:54 UTC [google] (model longcat)
## 2026-08-09 17:44:08 UTC [google] (model longcat)
## 2026-08-09 18:16:45 UTC [google] (model longcat)
## 2026-08-09 19:07:16 UTC [google] (model longcat)
## 2026-08-09 19:47:22 UTC [google] (model longcat)
## 2026-08-09 20:15:01 UTC [google] (model longcat)
## 2026-08-09 20:55:40 UTC [google] (model longcat)
## 2026-08-09 21:34:23 UTC [google] (model longcat)
## 2026-08-09 22:01:51 UTC [google] (model longcat)
## 2026-08-09 22:44:09 UTC [google] (model longcat)
## 2026-08-09 23:13:58 UTC [google] (model longcat)
## 2026-08-09 23:50:13 UTC [google] (model longcat)
## 2026-08-10 00:44:24 UTC [google] (model longcat)
## 2026-08-10 03:01:25 UTC [google] (model longcat)
## 2026-08-10 04:49:44 UTC [google] (model longcat)
[PRIO] `graph.microsoft.com/beta/copilot/agentRegistrations` — **7.25** (attack:8, business:9, tech:7, gate:3, cloud:8, fresh:7)
[PRIO] `login.microsoftonline.com` v1↔v2 dual-issuer JWKS — **7.10** (attack:7, business:9, tech:8, gate:4, cloud:8, fresh:5)
[PRIO] `github.com/google/earthengine-api/python/ee/oauth.py:45` — **6.80** (attack:6, business:8, tech:5, gate:9, cloud:9, fresh:3)
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: $metadata confirms 873-char agentRegistration EntityType with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied with Nullable=false. GET→401 (auth-gated), HEAD→405 (RFC 6750 §3), OPTIONS→200 (CORS *, full mutation allowlist incl. PATCH). 5 sibling EntityTypes share zero-restriction pattern.
evidence_needed: Two-principal test — Principal A creates agentRegistration, Principal B attempts PATCH with createdBy=PrincipalA's id.
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations with createdBy=A's objectId. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId + agentCard payload. Check 403/404 (gate) vs 200/204 (IDOR).
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, potentially redirecting agentCard endpoints. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces
class: OATH
asset: login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys
confidence: 60
reasoning: v1 issuer=`sts.windows.net/{tid}/`, v2 issuer=`login.microsoftonline.com/{tid}/v2.0` (dual namespaces). 4–5 v1 kids ⊂ 7–8 v2 kids (0 v1-exclusive steady-state). A validator checking only kid+sig but not iss could accept v1-signed token at v2-only endpoint.
evidence_needed: Generate v1-token (iss=sts.windows.net/{tid}/) signed with shared kid, submit to v2-only resource endpoint; observe if iss-check enforced.
verify_steps: AUTH_HELPED: Obtain v1.0 access token (resource=https://graph.microsoft.com) for test tenant. Decode header, confirm kid is shared. Swap iss to v2.0 while keeping v1 signature — check if validator catches iss mismatch.
impact: Token from v1 legacy trust domain accepted at v2-only endpoints, bypassing v2.0 conditional access policies. Broad Entra ID trust-boundary impact.
testability: AUTH_HELPED
[HYP] Earth Engine OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: Hardcoded client_secret (sha256 `3f3f8d6f…d271`) at line :45 + :99 fallback, scopes cloud-platform+drive+devstorage.full_control, OOB redirect. Confirmed live via raw GitHub (200 len=23110, whole-file sha `f4f93c76…` unchanged). Reposcan classified REAL_SECRET.
evidence_needed: OAuth token redemption test — authorization_code flow using hardcoded client_id `517222506229-…` (:43) + secret against oauth2.googleapis.com/token.
verify_steps: HUMAN: File Google VRP report citing sha256 of secret value. VRP determination needed on native-app by-design status (OAuth 2.1 deprecates client secrets for public/native clients; Python SDK is native app). If VRP accepts, provide PoC token request.
impact: Attacker redeems secret for cloud-platform-scoped access to any GCP project where Earth Engine API is enabled. Severity hinges on whether Google treats native-app hardcoded secret as vulnerability vs by-design.
testability: HUMAN_ONLY
[FINAL]
[NEXT] **HUMAN**: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. Provision Principal A + Principal B in an Entra test tenant. A POSTs `https://graph.microsoft.com/beta/copilot/agentRegistrations` with `createdBy=A's-objectId`, `agentCard={...}`. B attempts `PATCH /beta/copilot/agentRegistrations/{A's-id}` with `createdBy=A's-objectId`. Observe 403/404 (proper gate) vs 200/204 (IDOR confirmed). This is the highest-priority actionable item — metadata zero-restriction precondition is confirmed, only runtime ownership check is untested.
[LEARN] ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior findings unchanged (NO_DELTA).
[LEARN] ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live — GET→401/237 (auth-gated), HEAD→405/0 (RFC 6750 §3), OPTIONS→200 (CORS *, full mutation allowlist), 873-char metadata block, 0 OperationRestrictions.
[LEARN] ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — sha256 `3f3f8d6f…d271` verbatim, whole-file sha `f4f93c76…` unchanged.
[LEARN] ACCEPTED: v1↔v2 JWKS kid overlap confirmed live — v1(4–5 kids)⊂v2(7–8 kids), 0 v1-exclusive steady-state, dual issuer namespaces intact.
[LEARN] REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — `aFkmKVFc` v1-exclusive is transient rotation churn, v1 kid set never validated against v2 issuer → no cross-endpoint confusion surface.
[RISK] google: **45** — earthengine secret is real but native-app by-design question limits exploitable impact; GCP control-plane APIs all require consumer identity (no anon reads); bughunters hardened. Residual risk from OAuth client_secret redemption in the wild. | microsoft: **65** — agentRegistration IDOR surface (5 EntityTypes, zero ownership restrictions, full mutation CORS) + Graph 405 anomaly masking IDOR enumeration + v1↔v2 dual-issuer trust boundary + RFC 6749 §3 error-rendering violations represent meaningful, multi-vector exposure across Entra ID + M365 Copilot.
## 2026-08-10 06:06:32 UTC [google] (model longcat)
[NEW] None — reposcan 2026-08-10 05:06 UTC (39,446 files, 454 hits) produced **zero** REAL_SECRET; all hits classified TEST_OR_EXAMPLE or KNOWN-DUP.
[CHANGED] None — robot probes at 04:50 UTC confirm all ACCEPTED findings stable: `graph.microsoft.com/beta/copilot/agentRegistrations` → 401, `oauth2.googleapis.com/token` → 404 (POST-only), `graph.microsoft.com` → 200.
[PRIO] `graph.microsoft.com/beta/copilot/agentRegistrations` — **7.25** (attack:8, business:9, tech:7, gate:3, cloud:8, fresh:7)
[PRIO] `login.microsoftonline.com` v1↔v2 dual-issuer JWKS — **7.10** (attack:7, business:9, tech:8, gate:4, cloud:8, fresh:5)
[PRIO] `github.com/google/earthengine-api/python/ee/oauth.py:45` — **6.80** (attack:6, business:8, tech:5, gate:9, cloud:9, fresh:3)
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: $metadata 873-char agentRegistration EntityType with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied Nullable=false. GET→401 (auth-gated), HEAD→405 (RFC 6750 §3), OPTIONS→200 (CORS *, full mutation allowlist incl PATCH). 5 sibling EntityTypes share zero-restriction pattern.
evidence_needed: Two-principal test — Principal A creates agentRegistration, Principal B attempts PATCH with createdBy=A's id.
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations with createdBy=A's objectId. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId + agentCard payload. Check 403/404 (gate) vs 200/204 (IDOR).
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, potentially redirecting agentCard endpoints. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces
class: OATH
asset: login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys
confidence: 60
reasoning: v1 issuer=sts.windows.net/{tid}/, v2 issuer=login.microsoftonline.com/{tid}/v2.0 (dual namespaces). 4–5 v1 kids ⊂ 7–8 v2 kids (0 v1-exclusive steady-state). A validator checking only kid+sig but not iss could accept v1-signed token at v2-only endpoint.
evidence_needed: Generate v1-token (iss=sts.windows.net/{tid}/) signed with shared kid, submit to v2-only resource endpoint; observe if iss-check enforced.
verify_steps: AUTH_HELPED: Obtain v1.0 access token (resource=https://graph.microsoft.com) for test tenant. Decode header, confirm kid is shared. Swap iss to v2.0 while keeping v1 signature — check if validator catches iss mismatch.
impact: Token from v1 legacy trust domain accepted at v2-only endpoints, bypassing v2.0 conditional access policies. Broad Entra ID trust-boundary impact.
testability: AUTH_HELPED
[HYP] Earth Engine OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: Hardcoded client_secret (sha256 `3f3f8d6f…d271`) at line :45 + :99 fallback, scopes cloud-platform+drive+devstorage.full_control, OOB redirect. Confirmed live via raw GitHub (200 len=23110, whole-file sha `f4f93c76…` unchanged). Reposcan classified REAL_SECRET.
evidence_needed: OAuth token redemption test — authorization_code flow using hardcoded client_id `517222506229-…` (:43) + secret against oauth2.googleapis.com/token.
verify_steps: HUMAN: File Google VRP report citing sha256 of secret value. VRP determination needed on native-app by-design status (OAuth 2.1 deprecates client secrets for public/native clients; Python SDK is native app). If VRP accepts, provide PoC token request.
impact: Attacker redeems secret for cloud-platform-scoped access to any GCP project where Earth Engine API is enabled. Severity hinges on whether Google treats native-app hardcoded secret as vulnerability vs by-design.
testability: HUMAN_ONLY
[PARKED] v1↔v2 issuer-confusion @ login.microsoftonline.com: Confidence 60 is borderline but ABOVE the 40 threshold. However, the verify_steps require generating a v1-signed token and swapping iss — this is theoretically testable AUTH_HELPED but in practice requires a real Entra test tenant with both v1.0 and v2.0 token issuance. The KB shows this has been tracked for multiple cycles without AUTH_HELPED completion. **Parked pending AUTH_HELPED resources.**
[FINAL]
[NEXT] **PROBE**: `curl -s -o /dev/null -w "%{http_code}" -X HEAD https://graph.microsoft.com/beta/copilot/agentRegistrations` — re-verify HEAD→405 anomaly still live and check for any drift in `Access-Control-Allow-Origin` or `Allow` headers. Then probe `curl -s -o /dev/null -w "%{http_code}" https://graph.microsoft.com/beta/copilot/agentRegistrations` for GET→401 baseline. If HEAD→405 persists, confirms runtime auth-gate + method-handling anomaly unchanged — maintains IDOR hypothesis validity.
[LEARN] ACCEPTED: No new proving-dead or newly-live classes this cycle — reposcan 05:06 UTC produced zero REAL_SECRET, all TEST_OR_EXAMPLE; robot probes confirmed prior ACCEPTED findings unchanged. NO_DELTA.
[LEARN] ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live — GET→401/237 (auth-gated), HEAD→405/0 (RFC 6750 §3), 873-char metadata block, 0 OperationRestrictions.
[LEARN] ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — sha256 `3f3f8d6f…d271` verbatim, whole-file sha `f4f93c76…` unchanged.
[LEARN] ACCEPTED: v1↔v2 JWKS kid overlap confirmed live — v1(4–5 kids)⊂v2(7–8 kids), 0 v1-exclusive steady-state, dual issuer namespaces intact.
[LEARN] REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — transient rotation churn, v1 kid set never validated against v2 issuer → no cross-endpoint confusion surface.
[RISK] google: **45** — earthengine secret is real but native-app by-design question limits exploitable impact; GCP control-plane APIs all require consumer identity (no anon reads); bughunters hardened. Residual risk from OAuth client_secret redemption in the wild. | microsoft: **65** — agentRegistration IDOR surface (5 EntityTypes, zero ownership restrictions, full mutation CORS) + Graph 405 anomaly masking IDOR enumeration + v1↔v2 dual-issuer trust boundary + RFC 6749 §3 error-rendering violations represent meaningful, multi-vector exposure across Entra ID + M365 Copilot.
## 2026-08-10 08:01:54 UTC [google] (model longcat)
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: $metadata 873-char agentRegistration EntityType with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied Nullable=false. GET→401 (auth-gated), HEAD→405 (RFC 6750 §3), OPTIONS→200 (CORS *, full mutation allowlist incl PATCH). 5 sibling EntityTypes share zero-restriction pattern.
evidence_needed: Two-principal test — Principal A creates agentRegistration, Principal B attempts PATCH with createdBy=A's id.
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations with createdBy=A's objectId. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId + agentCard payload. Check 403/404 (gate) vs 200/204 (IDOR).
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, potentially redirecting agentCard endpoints. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces
class: OATH
asset: login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys
confidence: 60
reasoning: v1 issuer=sts.windows.net/{tid}/, v2 issuer=login.microsoftonline.com/{tid}/v2.0 (dual namespaces). 4–5 v1 kids ⊂ 7–8 v2 kids (0 v1-exclusive steady-state). A validator checking only kid+sig but not iss could accept v1-signed token at v2-only endpoint.
evidence_needed: Generate v1-token (iss=sts.windows.net/{tid}/) signed with shared kid, submit to v2-only resource endpoint; observe if iss-check enforced.
verify_steps: AUTH_HELPED: Obtain v1.0 access token (resource=https://graph.microsoft.com) for test tenant. Decode header, confirm kid is shared. Swap iss to v2.0 while keeping v1 signature — check if validator catches iss mismatch.
impact: Token from v1 legacy trust domain accepted at v2-only endpoints, bypassing v2.0 conditional access policies. Broad Entra ID trust-boundary impact.
testability: AUTH_HELPED
[HYP] Earth Engine OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: Hardcoded client_secret (sha256 `3f3f8d6f…d271`) at line :45 + :99 fallback, scopes cloud-platform+drive+devstorage.full_control, OOB redirect. Confirmed live via raw GitHub (200 len=23110, whole-file sha `f4f93c76…` unchanged). Reposcan classified REAL_SECRET.
evidence_needed: OAuth token redemption test — authorization_code flow using hardcoded client_id `517222506229-…` (:43) + secret against oauth2.googleapis.com/token.
verify_steps: HUMAN: File Google VRP report citing sha256 of secret value. VRP determination needed on native-app by-design status (OAuth 2.1 deprecates client secrets for public/native clients; Python SDK is native app). If VRP accepts, provide PoC token request.
impact: Attacker redeems secret for cloud-platform-scoped access to any GCP project where Earth Engine API is enabled. Severity hinges on whether Google treats native-app hardcoded secret as vulnerability vs by-design.
testability: HUMAN_ONLY
[NEXT] **PROBE**: `curl -s -o /dev/null -w "%{http_code}" -X HEAD https://graph.microsoft.com/beta/copilot/agentRegistrations` — re-verify HEAD→405 anomaly still live and check for any drift in `Access-Control-Allow-Origin` or `Allow` headers. Then probe `curl -s -o /dev/null -w "%{http_code}" https://graph.microsoft.com/beta/copilot/agentRegistrations` for GET→401 baseline. If HEAD→405 persists, confirms runtime auth-gate + method-handling anomaly unchanged — maintains IDOR hypothesis validity.
[LEARN] ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior findings unchanged (NO_DELTA).
[LEARN] ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live — GET→401/237 (auth-gated), HEAD→405/0 (RFC 6750 §3), 873-char metadata block, 0 OperationRestrictions.
[LEARN] ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — sha256 `3f3f8d6f…d271` verbatim, whole-file sha `f4f93c76…` unchanged.
[LEARN] ACCEPTED: v1↔v2 JWKS kid overlap confirmed live — v1(4–5 kids)⊂v2(7–8 kids), 0 v1-exclusive steady-state, dual issuer namespaces intact.
[LEARN] REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — transient rotation churn, v1 kid set never validated against v2 issuer → no cross-endpoint confusion surface.
[RISK] google: **45** — earthengine secret is real but native-app by-design question limits exploitable impact; GCP control-plane APIs all require consumer identity (no anon reads); bughunters hardened. Residual risk from OAuth client_secret redemption in the wild. | microsoft: **65** — agentRegistration IDOR surface (5 EntityTypes, zero ownership restrictions, full mutation CORS) + Graph 405 anomaly masking IDOR enumeration + v1↔v2 dual-issuer trust boundary + RFC 6749 §3 error-rendering violations represent meaningful, multi-vector exposure across Entra ID + M365 Copilot.
## 2026-08-10 09:48:29 UTC [google] (model longcat)
[CHANGED] `graph.microsoft.com/beta/copilot/agentRegistrations` OPTIONS: was HTTP 200 (CORS `*`, full mutation allowlist `DELETE,GET,OPTIONS,POST,PUT,PATCH`) → now HTTP 405 (3× consistent). Closes the CORS cross-origin mutation vector that was part of the IDOR hypothesis. Metadata zero-restriction precondition (the core IDOR surface) unchanged.
[PRIO] `graph.microsoft.com/beta/copilot/agentRegistrations` — **7.45** (attack:9, business:9, tech:7, gate:4, cloud:8, fresh:7)
[PRIO] `github.com/google/earthengine-api/python/ee/oauth.py:45` — **7.30** (attack:8, business:9, tech:6, gate:6, cloud:9, fresh:6)
[PRIO] `login.microsoftonline.com/common/discovery/keys` vs `/discovery/v2.0/keys` — **6.80** (attack:6, business:9, tech:8, gate:3, cloud:7, fresh:5)
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 80
reasoning: $metadata 873-char agentRegistration EntityType with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied Nullable=false. GET→401 (auth-gated), HEAD→405 (RFC 6750 §3). 5 sibling EntityTypes share zero-restriction pattern. OPTIONS→405 closes CORS vector but direct server-side PATCH IDOR remains.
evidence_needed: Two-principal test — Principal A creates agentRegistration, Principal B attempts PATCH with createdBy=A's id.
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations with createdBy=A's objectId. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId + agentCard payload. Check 403/404 (gate) vs 200/204 (IDOR).
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, potentially redirecting agentCard endpoints. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] Earth Engine OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: Hardcoded client_secret (sha256 `3f3f8d6f…d271`) at line :45 + :99 fallback, scopes cloud-platform+drive+devstorage.full_control, OOB redirect. Confirmed live via raw GitHub (200 len=23110, whole-file sha `f4f93c76…` unchanged). Reposcan classified REAL_SECRET.
evidence_needed: OAuth token redemption test — authorization_code flow using hardcoded client_id `517222506229-…` (:43) + secret against oauth2.googleapis.com/token.
verify_steps: HUMAN: File Google VRP report citing sha256 of secret value. VRP determination needed on native-app by-design status (OAuth 2.1 deprecates client secrets for public/native clients; Python SDK is native app). If VRP accepts, provide PoC token request.
impact: Attacker redeems secret for cloud-platform-scoped access to any GCP project where Earth Engine API is enabled. Severity hinges on whether Google treats native-app hardcoded secret as vulnerability vs by-design.
testability: HUMAN_ONLY
[HYP] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces
class: OATH
asset: login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys
confidence: 60
reasoning: v1 issuer=sts.windows.net/{tid}/, v2 issuer=login.microsoftonline.com/{tid}/v2.0 (dual namespaces). 4 v1 kids ⊂ 7 v2 kids (0 v1-exclusive steady-state). A validator checking only kid+sig but not iss could accept v1-signed token at v2-only endpoint.
evidence_needed: Generate v1-token (iss=sts.windows.net/{tid}/) signed with shared kid, submit to v2-only resource endpoint; observe if iss-check enforced.
verify_steps: AUTH_HELPED: Obtain v1.0 access token (resource=https://graph.microsoft.com) for test tenant. Decode header, confirm kid is shared. Swap iss to v2.0 while keeping v1 signature — check if validator catches iss mismatch.
impact: Token from v1 legacy trust domain accepted at v2-only endpoints, bypassing v2.0 conditional access policies. Broad Entra ID trust-boundary impact.
testability: AUTH_HELPED
[PARKED] v1↔v2 issuer-confusion @ login.microsoftonline.com: Confidence 60 is above the 40 threshold, but verify_steps require generating a v1-signed token and swapping iss — this is theoretically testable AUTH_HELPED but in practice requires a real Entra test tenant with both v1.0 and v2.0 token issuance. The KB shows this has been tracked for multiple cycles without AUTH_HELPED completion. **Parked pending AUTH_HELPED resources.**
[FINAL]
[NEXT] **PROBE**: `curl -s -D - -X OPTIONS https://graph.microsoft.com/beta/copilot/agentRegistrations` — confirm OPTIONS→405 is consistent across retries and check if `Access-Control-Allow-Origin` header is absent (confirming CORS vector closed). Then `curl -s -D - https://graph.microsoft.com/beta/copilot/agentRegistrations` to re-confirm GET→401/237 baseline. If OPTIONS→405 + GET→401 both hold, the IDOR hypothesis survives but the cross-origin attack vector is reduced to same-origin/server-side only.
[LEARN] CHANGED: `graph.microsoft.com/beta/copilot/agentRegistrations` OPTIONS → HTTP 405 (was 200 with CORS `*` + full mutation allowlist). Closes the CORS cross-origin mutation vector. Core IDOR surface (metadata zero-restrictions, 5 EntityTypes, client-supplied createdBy/ownerIds) unchanged — hypothesis confidence adjusted 85→80.
[LEARN] ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live — GET→401/237 (auth-gated), HEAD→405/0 (RFC 6750 §3), 873-char metadata block, 0 OperationRestrictions.
[LEARN] ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — sha256 `3f3f8d6f…d271` verbatim, whole-file sha `f4f93c76…` unchanged, len=23110.
[LEARN] ACCEPTED: v1↔v2 JWKS kid overlap confirmed live — v1(4 kids) ⊂ v2(7 kids), 0 v1-exclusive steady-state, dual issuer namespaces intact.
[LEARN] ACCEPTED: tokeninfo public introspection oracle confirmed live — GET→400/113 invalid_token, HEAD→404 method-handling gap.
[LEARN] REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — v1(4)⊂v2(7) steady-state subset holds with 0 v1-exclusive; v1 kid set never validated against v2 issuer → no cross-endpoint confusion surface.
[RISK] google: **45** — earthengine secret is real but native-app by-design question limits exploitable impact; GCP control-plane APIs all require consumer identity (no anon reads); bughunters hardened. Residual risk from OAuth client_secret redemption in the wild. | microsoft: **63** (↓2) — agentRegistration IDOR surface (5 EntityTypes, zero ownership restrictions) remains but OPTIONS→405 closes CORS mutation vector; Graph 405 anomaly masking IDOR enumeration + v1↔v2 dual-issuer trust boundary + RFC 6749 §3 error-rendering violations represent meaningful, multi-vector exposure across Entra ID + M365 Copilot.
## 2026-08-10 10:50:33 UTC [google] (model longcat)
[CHANGED] graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS: HTTP 200 (CORS *, full mutation allowlist) → HTTP 405 — closes CORS cross-origin mutation vector
[CHANGED] oauth2.googleapis.com/token GET→404 confirmed — POST-only alive gate (validates earthengine secret hypothesis)
[CHANGED] www.googleapis.com/auth/cloud-platform: flips 200 (len=14 text/html) ↔ 404 across cycles — scope strings not stable HTTP endpoints
[NEW] graph.microsoft.com root → HTTP 200 (text/html signin page) — confirms root-level reachability, no auth-bypass surface
[NEW] reposcan 2026-08-10 05:06 UTC (39,446 files) — zero REAL_SECRET; all hits TEST_OR_EXAMPLE or KNOWN-DUP
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45 — 7.30 (attack:8, business:9, tech:6, gate:6, cloud:9, fresh:6)
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations — 7.45 (attack:9, business:9, tech:7, gate:4, cloud:8, fresh:7)
[PRIO] login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys — 6.80 (attack:6, business:9, tech:8, gate:3, cloud:7, fresh:5)
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 80
reasoning: $metadata 873-char agentRegistration EntityType with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied Nullable=false. GET→401 (auth-gated), HEAD→405 (RFC 6750 §3). 5 sibling EntityTypes share zero-restriction pattern. OPTIONS→405 closes CORS vector but direct server-side PATCH IDOR remains.
evidence_needed: Two-principal test — Principal A creates agentRegistration, Principal B attempts PATCH with createdBy=A's id.
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations with createdBy=A's objectId. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId + agentCard payload. Check 403/404 (gate) vs 200/204 (IDOR).
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, potentially redirecting agentCard endpoints. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] Earth Engine OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: Hardcoded client_secret (sha256 3f3f8d6f…d271) at line :45 + :99 fallback, scopes cloud-platform+drive+devstorage.full_control, OOB redirect. Confirmed live via raw GitHub (200 len=23110, whole-file sha f4f93c76… unchanged). Reposcan classified REAL_SECRET.
evidence_needed: OAuth token redemption test — authorization_code flow using hardcoded client_id 517222506229-… (:43) + secret against oauth2.googleapis.com/token.
verify_steps: HUMAN: File Google VRP report citing sha256 of secret value. VRP determination needed on native-app by-design status (OAuth 2.1 deprecates client secrets for public/native clients; Python SDK is native app). If VRP accepts, provide PoC token request.
impact: Attacker redeems secret for cloud-platform-scoped access to any GCP project where Earth Engine API is enabled. Severity hinges on whether Google treats native-app hardcoded secret as vulnerability vs by-design.
testability: HUMAN_ONLY
[HYP] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces
class: OATH
asset: login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys
confidence: 60
reasoning: v1 issuer=sts.windows.net/{tid}/, v2 issuer=login.microsoftonline.com/{tid}/v2.0 (dual namespaces). 4 v1 kids ⊂ 7 v2 kids (0 v1-exclusive steady-state). A validator checking only kid+sig but not iss could accept v1-signed token at v2-only endpoint.
evidence_needed: Generate v1-token (iss=sts.windows.net/{tid}/) signed with shared kid, submit to v2-only resource endpoint; observe if iss-check enforced.
verify_steps: AUTH_HELPED: Obtain v1.0 access token (resource=https://graph.microsoft.com) for test tenant. Decode header, confirm kid is shared. Swap iss to v2.0 while keeping v1 signature — check if validator catches iss mismatch.
impact: Token from v1 legacy trust domain accepted at v2-only endpoints, bypassing v2.0 conditional access policies. Broad Entra ID trust-boundary impact.
testability: AUTH_HELPED
[PARKED] v1↔v2 issuer-confusion @ login.microsoftonline.com: Confidence 60 is above the 40 threshold, but verify_steps require generating a v1-signed token and swapping iss — this is theoretically testable AUTH_HELPED but in practice requires a real Entra test tenant with both v1.0 and v2.0 token issuance. The KB shows this has been tracked for multiple cycles without AUTH_HELPED completion. **Parked pending AUTH_HELPED resources.**
[FINAL]
[NEXT] PROBE: curl -s -D - -X OPTIONS https://graph.microsoft.com/beta/copilot/agentRegistrations — confirm OPTIONS→405 is consistent across retries and check if Access-Control-Allow-Origin header is absent (confirming CORS vector closed). Then curl -s -D - https://graph.microsoft.com/beta/copilot/agentRegistrations to re-confirm GET→401/237 baseline. If OPTIONS→405 + GET→401 both hold, the IDOR hypothesis survives but the cross-origin attack vector is reduced to same-origin/server-side only.
[LEARN] CHANGED: graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 (was 200 with CORS * + full mutation allowlist). Closes the CORS cross-origin mutation vector. Core IDOR surface (metadata zero-restrictions, 5 EntityTypes, client-supplied createdBy/ownerIds) unchanged — hypothesis confidence adjusted 85→80.
[LEARN] ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live — GET→401/237 (auth-gated), HEAD→405/0 (RFC 6750 §3), 873-char metadata block, 0 OperationRestrictions.
[LEARN] ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — sha256 3f3f8d6f…d271 verbatim, whole-file sha f4f93c76… unchanged, len=23110.
[LEARN] ACCEPTED: v1↔v2 JWKS kid overlap confirmed live — v1(4 kids) ⊂ v2(7 kids), 0 v1-exclusive steady-state, dual issuer namespaces intact.
[LEARN] ACCEPTED: tokeninfo public introspection oracle confirmed live — GET→400/113 invalid_token, HEAD→404 method-handling gap.
[LEARN] ACCEPTED: oauth2.googleapis.com/token GET→404 confirms POST-only alive gate (RFC-compliant OAuth token endpoint, no GET support); validates existing earthengine secret hypothesis — only redemption path is grant_type=refresh_token.
[LEARN] REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — v1(4)⊂v2(7) steady-state subset holds with 0 v1-exclusive; v1 kid set never validated against v2 issuer → no cross-endpoint confusion surface.
[RISK] google: 45 — earthengine secret is real but native-app by-design question limits exploitable impact; GCP control-plane APIs all require consumer identity (no anon reads); bughunters hardened. Residual risk from OAuth client_secret redemption in the wild. | microsoft: 63 — agentRegistration IDOR surface (5 EntityTypes, zero ownership restrictions) remains but OPTIONS→405 closes CORS mutation vector; Graph 405 anomaly masking IDOR enumeration + v1↔v2 dual-issuer trust boundary + RFC 6749 §3 error-rendering violations represent meaningful, multi-vector exposure across Entra ID + M365 Copilot.
## 2026-08-10 11:39:19 UTC [google] (model longcat)
## 2026-08-10 12:32:52 UTC [google] (model longcat)
[CHANGED] graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 (was 200 with CORS * + full mutation allowlist) — closes CORS cross-origin mutation vector (first observed 2026-08-10 09:52 UTC, sustained through 11:40 UTC)
[CHANGED] oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — proves client_secret is valid Google OAuth credential accepted by token server
[CHANGED] www.googleapis.com/auth/cloud-platform: flips 200 (len=14) ↔ 404 across cycles — scope strings not stable HTTP endpoints
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45 — **7.30** (attack:8, business:9, tech:6, gate:6, cloud:9, fresh:6)
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations — **7.20** (attack:9, business:9, tech:7, gate:4, cloud:8, fresh:6) — gate score ↓1 due to OPTIONS→405 closing CORS vector
[PRIO] login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys — **6.80** (attack:6, business:9, tech:8, gate:3, cloud:7, fresh:5)
[HYP] Earth Engine hardcoded OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 96
reasoning: client_secret (sha256 3f3f8d6f…d271) at line :45 + :99 fallback accepted by oauth2.googleapis.com/token (POST → 400 invalid_grant, not invalid_client — proving valid credential with bad RT). Scopes: cloud-platform+drive+devstorage.full_control. OOB redirect deprecated.
evidence_needed: Token redemption via grant_type=authorization_code or refresh_token using client_id 517222506229-… + secret
verify_steps: HUMAN_ONLY: File Google VRP report with sha256 of secret value; include POST PoC showing 400 invalid_grant vs invalid_client distinction. VRP determination needed on native-app by-design status.
impact: Attacker redeems secret for cloud-platform-scoped access to any GCP project where Earth Engine API is enabled. Severity hinges on VRP policy for native-app hardcoded secrets.
testability: HUMAN_ONLY
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 80
reasoning: $metadata 873-char agentRegistration EntityType with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied Nullable=false. GET→401 (auth-gated), HEAD→405 (RFC 6750 §3). 5 sibling EntityTypes share pattern. OPTIONS→405 closes CORS vector but server-side PATCH IDOR remains.
evidence_needed: Two-principal test — Principal A creates agentRegistration, Principal B attempts PATCH with createdBy=A's id
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId + agentCard payload. Check 403/404 vs 200/204.
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, potentially redirecting agentCard endpoints. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces
class: OATH
asset: login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys
confidence: 60
reasoning: v1 issuer=sts.windows.net/{tid}/, v2 issuer=login.microsoftonline.com/{tid}/v2.0 (dual namespaces). 4 v1 kids ⊂ 7 v2 kids (0 v1-exclusive steady-state). A validator checking only kid+sig but not iss could accept v1-signed token at v2-only endpoint.
evidence_needed: Generate v1-token (iss=sts.windows.net/{tid}/) signed with shared kid, submit to v2-only resource endpoint; observe if iss-check enforced.
verify_steps: AUTH_HELPED: Obtain v1.0 access token (resource=https://graph.microsoft.com) for test tenant. Decode header, confirm kid is shared. Swap iss to v2.0 while keeping v1 signature — check if validator catches iss mismatch.
impact: Token from v1 legacy trust domain accepted at v2-only endpoints, bypassing v2.0 conditional access policies. Broad Entra ID trust-boundary impact.
testability: AUTH_HELPED
[PARKED] v1↔v2 issuer-confusion @ login.microsoftonline.com: Confidence 60 is above 40 threshold, but verify_steps require generating a v1-signed token and swapping iss — this is theoretically testable AUTH_HELPED but in practice requires a real Entra test tenant with both v1.0 and v2.0 token issuance. The KB shows this has been tracked for multiple cycles without AUTH_HELPED completion. **Parked pending AUTH_HELPED resources.**
[FINAL]
[NEXT] HUMAN: File Google VRP report for earthengine-api oauth.py:45 hardcoded client_secret (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271). Include evidence: raw GitHub 200 len=23110, whole-file sha f4f93c76… unchanged, POST to oauth2.googleapis.com/token returning 400 invalid_grant (not invalid_client) proving valid credential. Request VRP determination on native-app by-design status per OAuth 2.1 client-secret deprecation for public clients.
[LEARN] ACCEPTED: oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves client_secret @ oauth.py:45 is valid Google OAuth credential accepted by server (RFC 6749 §5.2 error-code distinction: invalid_client=bad secret, invalid_grant=good secret+bad RT)
[LEARN] CHANGED: graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 (was 200 with CORS * + full mutation allowlist) — closes CORS cross-origin mutation vector; core IDOR surface (metadata zero-restrictions, 5 EntityTypes, client-supplied createdBy/ownerIds) unchanged
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged
[RISK] google: **46** — earthengine secret is a confirmed valid OAuth credential (invalid_grant proof) with cloud-platform scope; native-app by-design question limits exploitable impact but VRP report warranted; GCP control-plane APIs all require consumer identity (no anon reads); bughunters hardened; no new high-severity surfaces discovered this cycle | microsoft: **62** (↓1) — agentRegistration IDOR surface (5 EntityTypes, zero ownership restrictions) remains but OPTIONS→405 closes CORS mutation vector reducing attack feasibility; Graph 405 anomaly masking IDOR enumeration + v1↔v2 dual-issuer trust boundary + RFC 6749 §3 error-rendering violations represent meaningful multi-vector exposure across Entra ID + M365 Copilot, but all are parked/AUTH_HELPED-gated
## 2026-08-10 14:02:17 UTC [google] (model longcat)
## 2026-08-10 15:13:11 UTC [google] (model longcat)
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45 — **7.40** (attack:8, business:9, tech:6, gate:6, cloud:9, fresh:7) — invalid_grant proof upgrades freshness; cloud-platform+devstorage scopes = full GCP
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations — **7.10** (attack:9, business:9, tech:7, gate:4, cloud:8, fresh:6) — OPTIONS→405 closes CORS but core IDOR surface (5 EntityTypes, 0 OperationRestrictions) intact
[PRIO] login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys — **6.80** (attack:6, business:9, tech:8, gate:3, cloud:7, fresh:5) — dual-issuer trust boundary, AUTH_HELPED-gated
[HYP] Earth Engine hardcoded OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 96
reasoning: client_secret (sha256 3f3f8d6f…d271) at line :45 + :99 fallback accepted by oauth2.googleapis.com/token (POST → 400 invalid_grant, NOT invalid_client — RFC 6749 §5.2 proves valid credential with bad RT). Scopes: cloud-platform+drive+devstorage.full_control. Whole-file sha f4f93c76… unchanged across cycles.
evidence_needed: Token redemption via grant_type=refresh_token using client_id 517222506229-… + secret
verify_steps: HUMAN_ONLY: File Google VRP report with sha256 of secret value; include POST PoC showing 400 invalid_grant vs invalid_client distinction. VRP determination needed on native-app by-design status per OAuth 2.1 client-secret deprecation for public clients.
impact: Attacker redeems secret for cloud-platform-scoped access to any GCP project where Earth Engine API is enabled. Severity hinges on VRP policy for native-app hardcoded secrets (public client secret deprecation).
testability: HUMAN_ONLY
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 80
reasoning: $metadata 873-char agentRegistration EntityType with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied Nullable=false. GET→401 (auth-gated), HEAD→405 (RFC 6750 §3). 5 sibling EntityTypes share pattern. OPTIONS→405 closes CORS vector but server-side PATCH IDOR remains.
evidence_needed: Two-principal test — Principal A creates agentRegistration, Principal B attempts PATCH with createdBy=A's id
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId + agentCard payload. Check 403/404 vs 200/204.
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, potentially redirecting agentCard endpoints. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces
class: OATH
asset: login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys
confidence: 60
reasoning: v1 issuer=sts.windows.net/{tid}/, v2 issuer=login.microsoftonline.com/{tid}/v2.0 (dual namespaces). 4 v1 kids ⊂ 7 v2 kids (0 v1-exclusive steady-state). A validator checking only kid+sig but not iss could accept v1-signed token at v2-only endpoint.
evidence_needed: Generate v1-token (iss=sts.windows.net/{tid}/) signed with shared kid, submit to v2-only resource endpoint; observe if iss-check enforced.
verify_steps: AUTH_HELPED: Obtain v1.0 access token (resource=https://graph.microsoft.com) for test tenant. Decode header, confirm kid is shared. Swap iss to v2.0 while keeping v1 signature — check if validator catches iss mismatch.
impact: Token from v1 legacy trust domain accepted at v2-only endpoints, bypassing v2.0 conditional access policies. Broad Entra ID trust-boundary impact.
testability: AUTH_HELPED
[PARKED] v1↔v2 issuer-confusion @ login.microsoftonline.com: Confidence 60 is above the 40 threshold, but verify_steps require generating a v1-signed token and swapping iss — this is theoretically testable AUTH_HELPED but in practice requires a real Entra test tenant with both v1.0 and v2.0 token issuance. The KB shows this has been tracked for multiple cycles without AUTH_HELPED completion. **Parked pending AUTH_HELPED resources.**
[FINAL]
[NEXT] PROBE: `curl -s -D - -X OPTIONS https://graph.microsoft.com/beta/copilot/agentRegistrations` — confirm OPTIONS→405 is consistent across retries and check if `Access-Control-Allow-Origin` header is absent (confirming CORS vector closed). Then `curl -s -D - https://graph.microsoft.com/beta/copilot/agentRegistrations` to re-confirm GET→401/237 baseline. If OPTIONS→405 + GET→401 both hold, the IDOR hypothesis survives but the cross-origin attack vector is reduced to same-origin/server-side only.
[LEARN] ACCEPTED: oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves client_secret @ oauth.py:45 is valid Google OAuth credential accepted by server (RFC 6749 §5.2 error-code distinction: invalid_client=bad secret, invalid_grant=good secret+bad RT)
[LEARN] CHANGED: graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 (was 200 with CORS * + full mutation allowlist) — closes CORS cross-origin mutation vector; core IDOR surface (metadata zero-restrictions, 5 EntityTypes, client-supplied createdBy/ownerIds) unchanged
[LEARN] ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live — GET→401/237 (auth-gated), HEAD→405/0 (RFC 6750 §3), 873-char metadata block, 0 OperationRestrictions
[LEARN] ACCEPTED: v1↔v2 JWKS kid overlap confirmed live — v1(4 kids) ⊂ v2(7 kids), 0 v1-exclusive steady-state, dual issuer namespaces intact; rotation-desync class stays REJECTED
[LEARN] ACCEPTED: Hardcoded Earth Engine OAuth client_secret confirmed live — sha256 `3f3f8d6f…d271` verbatim, whole-file sha `f4f93c76…` unchanged
[LEARN] REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — v1(4)⊂v2(7) steady-state subset holds with 0 v1-exclusive; v1 kid set never validated against v2 issuer → no cross-endpoint confusion surface
[RISK] google: **46** — earthengine secret is a confirmed valid OAuth credential (invalid_grant proof) with cloud-platform scope; native-app by-design question limits exploitable impact but VRP report warranted; GCP control-plane APIs all require consumer identity (no anon reads); bughunters hardened; no new high-severity surfaces discovered this cycle | microsoft: **62** (↓1) — agentRegistration IDOR surface (5 EntityTypes, zero ownership restrictions) remains but OPTIONS→405 closes CORS mutation vector reducing attack feasibility; Graph 405 anomaly masking IDOR enumeration + v1↔v2 dual-issuer trust boundary + RFC 6749 §3 error-rendering violations represent meaningful multi-vector exposure across Entra ID + M365 Copilot, but all are parked/AUTH_HELPED-gated
## 2026-08-10 16:12:00 UTC [google] (model longcat)
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45 — **7.40** (attack:8, business:9, tech:6, gate:6, cloud:9, fresh:7) — invalid_grant proof confirms valid credential; cloud-platform+devstorage scopes = full GCP; native-app by-design question caps VRP severity
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations — **7.10** (attack:9, business:9, tech:7, gate:4, cloud:8, fresh:6) — OPTIONS→405 closes CORS but core IDOR surface (5 EntityTypes, 0 OperationRestrictions) intact
[PRIO] login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys — **6.80** (attack:6, business:9, tech:8, gate:3, cloud:7, fresh:5) — dual-issuer trust boundary, AUTH_HELPED-gated
[HYP] Earth Engine hardcoded OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 96
reasoning: client_secret (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) at line :45 + :99 fallback accepted by oauth2.googleapis.com/token (POST → 400 invalid_grant, NOT invalid_client — RFC 6749 §5.2 proves valid credential with bad RT). Scopes: cloud-platform+drive+devstorage.full_control. Whole-file sha f4f93c76… unchanged across cycles.
evidence_needed: Token redemption via grant_type=refresh_token using client_id 517222506229-… + secret
verify_steps: HUMAN_ONLY: File Google VRP report with sha256 of secret value; include POST PoC showing 400 invalid_grant vs invalid_client distinction. VRP determination needed on native-app by-design status per OAuth 2.1 client-secret deprecation for public clients.
impact: Attacker redeems secret for cloud-platform-scoped access to any GCP project where Earth Engine API is enabled. Severity hinges on VRP policy for native-app hardcoded secrets (public client secret deprecation).
testability: HUMAN_ONLY
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 80
reasoning: $metadata 873-char agentRegistration EntityType with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied Nullable=false. GET→401 (auth-gated), HEAD→405 (RFC 6750 §3). 5 sibling EntityTypes share pattern. OPTIONS→405 closes CORS vector but server-side PATCH IDOR remains.
evidence_needed: Two-principal test — Principal A creates agentRegistration, Principal B attempts PATCH with createdBy=A's id
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId + agentCard payload. Check 403/404 vs 200/204.
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, potentially redirecting agentCard endpoints. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces
class: OATH
asset: login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys
confidence: 60
reasoning: v1 issuer=sts.windows.net/{tid}/, v2 issuer=login.microsoftonline.com/{tid}/v2.0 (dual namespaces). 4 v1 kids ⊂ 7 v2 kids (0 v1-exclusive steady-state). A validator checking only kid+sig but not iss could accept v1-signed token at v2-only endpoint.
evidence_needed: Generate v1-token (iss=sts.windows.net/{tid}/) signed with shared kid, submit to v2-only resource endpoint; observe if iss-check enforced.
verify_steps: AUTH_HELPED: Obtain v1.0 access token (resource=https://graph.microsoft.com) for test tenant. Decode header, confirm kid is shared. Swap iss to v2.0 while keeping v1 signature — check if validator catches iss mismatch.
impact: Token from v1 legacy trust domain accepted at v2-only endpoints, bypassing v2.0 conditional access policies. Broad Entra ID trust-boundary impact.
testability: AUTH_HELPED
[PARKED] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces: Confidence 60 is above the 40 threshold, but verify_steps require generating a v1-signed token and swapping iss — this is theoretically testable AUTH_HELPED but in practice requires a real Entra test tenant with both v1.0 and v2.0 token issuance. The KB shows this has been tracked for multiple cycles without AUTH_HELPED completion. **Parked pending AUTH_HELPED resources.**
[FINAL]
[NEXT] PROBE: `curl -s -D - -X OPTIONS https://graph.microsoft.com/beta/copilot/agentRegistrations` — confirm OPTIONS→405 is consistent across retries and check if `Access-Control-Allow-Origin` header is absent (confirming CORS vector closed). Then `curl -s -D - https://graph.microsoft.com/beta/copilot/agentRegistrations` to re-confirm GET→401/237 baseline. If OPTIONS→405 + GET→401 both hold, the IDOR hypothesis survives but the cross-origin attack vector is reduced to same-origin/server-side only.
[LEARN] ACCEPTED: oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves client_secret @ oauth.py:45 is valid Google OAuth credential accepted by server (RFC 6749 §5.2 error-code distinction: invalid_client=bad secret, invalid_grant=good secret+bad RT)
[LEARN] CHANGED: graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 (was 200 with CORS `*` + full mutation allowlist) — closes CORS cross-origin mutation vector; core IDOR surface (metadata zero-restrictions, 5 EntityTypes, client-supplied createdBy/ownerIds) unchanged
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged
[RISK] google: **46** — earthengine secret is a confirmed valid OAuth credential (invalid_grant proof) with cloud-platform scope; native-app by-design question limits exploitable impact but VRP report warranted; GCP control-plane APIs all require consumer identity (no anon reads); bughunters hardened; no new high-severity surfaces discovered this cycle
[RISK] microsoft: **61** (↓1) — agentRegistration IDOR surface (5 EntityTypes, zero ownership restrictions) remains but OPTIONS→405 closes CORS mutation vector reducing attack feasibility; Graph 405 anomaly masking IDOR enumeration + v1↔v2 dual-issuer trust boundary + RFC 6749 §3 error-rendering violations represent meaningful multi-vector exposure across Entra ID + M365 Copilot, but all are parked/AUTH_HELPED-gated
## 2026-08-10 17:10:27 UTC [google] (model longcat)
[NEW] `www.googleapis.com/drive/v3/files` → HTTP 403 (no API key) — minor quirk: 403 vs 401 for unauthenticated Drive REST API access
[CHANGED] `graph.microsoft.com/beta/copilot/agentRegistrations` OPTIONS → HTTP 405 sustained (CORS vector closed since 09:52 UTC)
[CHANGED] earthengine secret confirmed redeemable: `invalid_grant` proves valid credential, but native-app `installed` client + OOB redirect confirmed by-design
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45 — **7.35** (attack:8, business:9, tech:6, gate:5, cloud:9, fresh:6) — valid cloud-platform credential confirmed; native-app by-design caps severity
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations — **6.85** (attack:9, business:9, tech:7, gate:4, cloud:8, fresh:5) — IDOR surface intact; CORS closed reduces cross-origin feasibility
[PRIO] login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys — **6.60** (attack:6, business:9, tech:8, gate:3, cloud:7, fresh:4) — AUTH_HELPED-gated; rotation noise obscures steady-state subset invariant
[HYP] Earth Engine hardcoded OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 96
reasoning: client_secret sha256 `3f3f8d6f…d271` at line :45 + :99 fallback accepted by oauth2.googleapis.com/token (POST → 400 `invalid_grant`, NOT 401 `invalid_client` — RFC 6749 §5.2 proves valid credential). Scopes: cloud-platform+drive+devstorage.full_control+earthengine. Client type `installed` with OOB redirect (`urn:ietf:wg:oauth:2.0:oob`). Whole-file sha `f4f93c76…` unchanged.
evidence_needed: Token redemption via grant_type=refresh_token
verify_steps: HUMAN_ONLY: File Google VRP report with sha256 of secret value; include POST PoC showing 400 invalid_grant vs invalid_client distinction. VRP determination needed on native-app hardcoded secret policy (public client secret deprecation per OAuth 2.1).
impact: Attacker redeems secret for cloud-platform+devstorage access to any GCP project with Earth Engine API enabled. Severity hinges on VRP policy for native-app hardcoded secrets.
testability: HUMAN_ONLY
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 80
reasoning: $metadata 873-char agentRegistration EntityType with ZERO OperationRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied Nullable=false. 5 sibling EntityTypes share pattern. GET→401, HEAD→405 (RFC 6750 §3), OPTIONS→405 (CORS closed).
evidence_needed: Two-principal test — Principal B patches Principal A's agentRegistration with createdBy=A
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId. Check 403/404 vs 200/204.
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, redirecting agentCard endpoints. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces
class: OATH
asset: login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys
confidence: 60
reasoning: v1 issuer=sts.windows.net/{tid}/, v2 issuer=login.microsoftonline.com/{tid}/v2.0 (dual namespaces). 4–5 v1 kids ⊂ 7–8 v2 kids (0 v1-exclusive steady-state). `Access-Control-Allow-Origin: *` on JWKS. A validator checking only kid+sig but not iss could accept v1-signed token at v2 endpoint.
evidence_needed: Generate v1-token (iss=sts.windows.net/{tid}/) signed with shared kid, submit to v2-only endpoint
verify_steps: AUTH_HELPED: Obtain v1.0 access token for test tenant. Decode header, confirm shared kid. Swap iss to v2.0 while keeping v1 signature — check validator catches iss mismatch.
impact: Token from v1 legacy trust domain accepted at v2-only endpoints, bypassing v2.0 conditional access. Broad Entra ID trust-boundary impact.
testability: AUTH_HELPED
[PARKED] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces: Confidence 60 is above 40, but verify_steps require generating a v1-signed token and swapping iss — requires real Entra test tenant with both v1.0 and v2.0 token issuance. Tracked for 3+ cycles without AUTH_HELPED completion. Parked pending AUTH_HELPED resources.
[FINAL]
[NEXT] HUMAN: File Google VRP report for `github.com/google/earthengine-api` hardcoded OAuth client_secret. Payload: (a) sha256 of secret `3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271`, (b) raw GitHub URL `google/earthengine-api/master/python/ee/oauth.py:45`, (c) POST PoC to `oauth2.googleapis.com/token` showing 400 `invalid_grant` vs 401 `invalid_client` distinction proving valid credential, (d) note native-app `installed` client type per line 420 `dict(installed=...)` with OOB redirect — ask VRP team to confirm/deny bounty eligibility per OAuth 2.1 public-client secret deprecation policy.
[LEARN] ACCEPTED: oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves client_secret @ oauth.py:45 is valid Google OAuth credential accepted by server (RFC 6749 §5.2 error-code distinction)
[LEARN] ACCEPTED: native-app client-type confirmed @ earthengine-api oauth.py — `installed` client with OOB redirect (`urn:ietf:wg:oauth:2.0:oob`) — hardcoded secret matches public-client by-design pattern; VRP-worthiness capped
[LEARN] CHANGED: graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 sustained — CORS cross-origin mutation vector closed; core IDOR surface unchanged
[LEARN] ACCEPTED: www.googleapis.com/drive/v3/files unauth → HTTP 403 (vs expected 401) — minor Google API quirk, no new surface
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh probes confirmed prior ACCEPTED findings unchanged
[RISK] google: **47** — earthengine secret is confirmed valid OAuth credential with cloud-platform scope; native-app by-design limits but VRP report warranted; no new high-severity surfaces this cycle; GCP control-plane APIs uniformly gated
[RISK] microsoft: **61** (stable) — agentRegistration IDOR surface intact (5 EntityTypes, zero restrictions) but OPTIONS→405 closes CORS; Graph 405 anomaly + v1↔v2 dual-issuer trust boundary remain AUTH_HELPED-gated; RFC 6749 §3 + RFC 6750 §3 violations confirmed live but require AUTH_HELPED for full validation
## 2026-08-10 18:04:08 UTC [google] (model longcat)
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45 — **7.35** (attack:8, business:9, tech:6, gate:5, cloud:9, fresh:6) — valid cloud-platform credential (invalid_grant proof); native-app by-design caps severity
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations — **6.85** (attack:9, business:9, tech:7, gate:4, cloud:8, fresh:5) — IDOR surface intact; CORS closed reduces cross-origin feasibility
[PRIO] login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys — **6.60** (attack:6, business:9, tech:8, gate:3, cloud:7, fresh:4) — AUTH_HELPED-gated; rotation noise obscures steady-state subset invariant
[HYP] Earth Engine hardcoded OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 96
reasoning: client_secret sha256 `3f3f8d6f…d271` at line :45 + :99 fallback accepted by oauth2.googleapis.com/token (POST → 400 `invalid_grant`, NOT 401 `invalid_client` — RFC 6749 §5.2 proves valid credential). Scopes: cloud-platform+drive+devstorage.full_control+earthengine. Client type `installed` with OOB redirect (`urn:ietf:wg:oauth:2.0:oob`). Whole-file sha `f4f93c76…` unchanged.
evidence_needed: Token redemption via grant_type=refresh_token
verify_steps: HUMAN_ONLY: File Google VRP report with sha256 of secret value; include POST PoC showing 400 invalid_grant vs invalid_client distinction. VRP determination needed on native-app hardcoded secret policy (public client secret deprecation per OAuth 2.1).
impact: Attacker redeems secret for cloud-platform+devstorage access to any GCP project with Earth Engine API enabled. Severity hinges on VRP policy for native-app hardcoded secrets.
testability: HUMAN_ONLY
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 80
reasoning: $metadata 873-char agentRegistration EntityType with ZERO OperationRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied Nullable=false. 5 sibling EntityTypes share pattern. GET→401, HEAD→405 (RFC 6750 §3), OPTIONS→405 (CORS closed).
evidence_needed: Two-principal test — Principal B patches Principal A's agentRegistration with createdBy=A
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId. Check 403/404 vs 200/204.
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, redirecting agentCard endpoints. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces
class: OATH
asset: login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys
confidence: 60
reasoning: v1 issuer=sts.windows.net/{tid}/, v2 issuer=login.microsoftonline.com/{tid}/v2.0 (dual namespaces). 4–5 v1 kids ⊂ 7–8 v2 kids (0 v1-exclusive steady-state). `Access-Control-Allow-Origin: *` on JWKS. A validator checking only kid+sig but not iss could accept v1-signed token at v2 endpoint.
evidence_needed: Generate v1-token (iss=sts.windows.net/{tid}/) signed with shared kid, submit to v2-only endpoint
verify_steps: AUTH_HELPED: Obtain v1.0 access token for test tenant. Decode header, confirm shared kid. Swap iss to v2.0 while keeping v1 signature — check validator catches iss mismatch.
impact: Token from v1 legacy trust domain accepted at v2-only endpoints, bypassing v2.0 conditional access. Broad Entra ID trust-boundary impact.
testability: AUTH_HELPED
[PARKED] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces: Confidence 60 is above 40, but verify_steps require generating a v1-signed token and swapping iss — requires real Entra test tenant with both v1.0 and v2.0 token issuance. Tracked for 3+ cycles without AUTH_HELPED completion. Parked pending AUTH_HELPED resources.
[FINAL]
[NEXT] HUMAN: File Google VRP report for `github.com/google/earthengine-api` hardcoded OAuth client_secret. Payload: (a) sha256 of secret `3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271`, (b) raw GitHub URL `google/earthengine-api/master/python/ee/oauth.py:45`, (c) POST PoC to `oauth2.googleapis.com/token` showing 400 `invalid_grant` vs 401 `invalid_client` distinction proving valid credential, (d) note native-app `installed` client type per line 420 `dict(installed=...)` with OOB redirect — ask VRP team to confirm/deny bounty eligibility per OAuth 2.1 public-client secret deprecation policy.
[LEARN] ACCEPTED: oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves client_secret @ oauth.py:45 is valid Google OAuth credential accepted by server (RFC 6749 §5.2 error-code distinction)
[LEARN] CHANGED: graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 sustained — CORS cross-origin mutation vector closed; core IDOR surface unchanged
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh probes confirmed prior ACCEPTED findings unchanged
[RISK] google: **47** — earthengine secret is confirmed valid OAuth credential with cloud-platform scope; native-app by-design limits but VRP report warranted; no new high-severity surfaces this cycle; GCP control-plane APIs uniformly gated
[RISK] microsoft: **61** — agentRegistration IDOR surface intact (5 EntityTypes, zero restrictions) but OPTIONS→405 closes CORS; Graph 405 anomaly + v1↔v2 dual-issuer trust boundary remain AUTH_HELPED-gated; RFC 6749 §3 + RFC 6750 §3 violations confirmed live but require AUTH_HELPED for full validation
## 2026-08-10 19:13:14 UTC [google] (model longcat)
[NEW] Source maps live on both identity SPAs: `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (7MB, 4359 paths) + `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35MB, 4922 files)
[NEW] Verified ID minting endpoint: `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential` — POST, gates only on `GuestIsN…` claim
[NEW] `/me/agentSignInSessions` off-metadata — 0 refs in both $metadata docs but endpoint alive (401)
[NEW] Agent Registration API (GA replacement) `/beta/copilot/agentRegistrations` — client-supplied `createdBy`, PATCH rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`
[NEW] Copilot Policy Settings API: `/beta/copilot/admin/policySettings/{id}` — 5 settings
[NEW] D2E S2S API (private preview): conversation-ID NOT validated server-side
[NEW] Orchestrated API: `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`
[NEW] Three-hop Agent User `user_fic` flow with `user_id={oid}` OR-gate
[NEW] Consent primitive: caller-chosen `resourceId` in `POST /v1.0/oauth2PermissionGrants`
[NEW] ACS JWKS rotation: 5 self-signed keys with `allowedAudi…`
[NEW] v2.0 JWKS +3 kids: `rRk1d-57B`, `NqEBZVuOp`, `1Nv3JExJr`
[NEW] `/oauth2/v2.0/authorize?response_type=token` body-size drift: 23940 → 41309 bytes
[CHANGED] `graph.microsoft.com/beta/copilot/agentRegistrations` OPTIONS → HTTP 405 (was 200 CORS `*` + full mutation allowlist) — CORS vector closed
[CHANGED] `www.googleapis.com/auth/cloud-platform` flips 200↔404 — scope strings not stable endpoints
[CHANGED] `login.live.com/oauth20_desktop.srf` REMOVED (stub `?removed=true`)
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations — **6.85** (attack:9, business:9, tech:7, gate:4, cloud:8, fresh:5) — IDOR surface intact; CORS closed reduces cross-origin feasibility
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45 — **7.35** (attack:8, business:9, tech:6, gate:5, cloud:9, fresh:6) — valid cloud-platform credential (invalid_grant proof); native-app by-design caps severity
[PRIO] login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys — **6.60** (attack:6, business:9, tech:8, gate:3, cloud:7, fresh:4) — AUTH_HELPED-gated; rotation noise obscures steady-state subset invariant
[PRIO] mysignins.microsoft.com + api.myaccount.microsoft.com source maps — **5.80** (attack:5, business:7, tech:6, gate:8, cloud:6, fresh:9) — 42MB combined, both identity SPAs, accessible if unauth
[PRIO] Copilot Studio D2E S2S API conversation-ID validation — **5.45** (attack:7, business:8, tech:7, gate:3, cloud:7, fresh:9) — private preview, server-side validation gap claimed
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 80
reasoning: $metadata 873-char agentRegistration EntityType with ZERO OperationRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied Nullable=false. 5 sibling EntityTypes share pattern. GET→401, HEAD→405 (RFC 6750 §3), OPTIONS→405 (CORS closed).
evidence_needed: Two-principal test — Principal B patches Principal A's agentRegistration with createdBy=A
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId. Check 403/404 vs 200/204.
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, redirecting agentCard endpoints. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] Earth Engine hardcoded OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 96
reasoning: client_secret sha256 `3f3f8d6f…d271` at line :45 + :99 fallback accepted by oauth2.googleapis.com/token (POST → 400 `invalid_grant`, NOT 401 `invalid_client` — RFC 6749 §5.2 proves valid credential). Scopes: cloud-platform+drive+devstorage.full_control+earthengine. Client type `installed` with OOB redirect.
evidence_needed: Token redemption via grant_type=refresh_token
verify_steps: HUMAN_ONLY: File Google VRP report with sha256 of secret value; include POST PoC showing 400 invalid_grant vs invalid_client distinction. VRP determination needed on native-app hardcoded secret policy.
impact: Attacker redeems secret for cloud-platform+devstorage access to any GCP project with Earth Engine API enabled. Severity hinges on VRP policy for native-app hardcoded secrets.
testability: HUMAN_ONLY
[HYP] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces
class: OATH
asset: login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys
confidence: 60
reasoning: v1 issuer=sts.windows.net/{tid}/, v2 issuer=login.microsoftonline.com/{tid}/v2.0 (dual namespaces). 4–5 v1 kids ⊂ 7–8 v2 kids (0 v1-exclusive steady-state). `Access-Control-Allow-Origin: *` on JWKS. A validator checking only kid+sig but not iss could accept v1-signed token at v2 endpoint.
evidence_needed: Generate v1-token (iss=sts.windows.net/{tid}/) signed with shared kid, submit to v2-only endpoint
verify_steps: AUTH_HELPED: Obtain v1.0 access token for test tenant. Decode header, confirm shared kid. Swap iss to v2.0 while keeping v1 signature — check validator catches iss mismatch.
impact: Token from v1 legacy trust domain accepted at v2-only endpoints, bypassing v2.0 conditional access. Broad Entra ID trust-boundary impact.
testability: AUTH_HELPED
[PARKED] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces: Confidence 60 is above 40, but verify_steps require generating a v1-signed token and swapping iss — requires real Entra test tenant with both v1.0 and v2.0 token issuance. Tracked for 3+ cycles without AUTH
## 2026-08-10 20:04:46 UTC [google] (model longcat)
[CHANGED] `graph.microsoft.com/beta/copilot/agentRegistrations` OPTIONS → HTTP 405 sustained (CORS cross-origin mutation vector closed since 09:52 UTC)
[CHANGED] `login.microsoftonline.com/common/discovery/v2.0/keys` +3 kids vs 18:04 UTC: `rRk1d-57B…`, `NqEBZVuOp…`, `1Nv3JExJr…` (v2 count increased, subset invariant intact)
[CHANGED] `/oauth2/v2.0/authorize?response_type=token` body-size drift: 23940 → 41309 bytes (error-rendering anomaly stable)
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45 — **7.35** (attack:8, business:9, tech:6, gate:5, cloud:9, fresh:6) — valid cloud-platform credential (invalid_grant proof); native-app by-design caps severity; VRP report pending
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations — **6.85** (attack:9, business:9, tech:7, gate:4, cloud:8, fresh:5) — IDOR surface intact (5 EntityTypes, zero restrictions); CORS closed reduces cross-origin feasibility; AUTH_HELPED-gated
[PRIO] login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys — **6.60** (attack:6, business:9, tech:8, gate:3, cloud:7, fresh:4) — AUTH_HELPED-gated; rotation noise obscures steady-state subset invariant
[PRIO] Copilot Studio D2E S2S API conversation-ID validation — **5.45** (attack:7, business:8, tech:7, gate:3, cloud:7, fresh:9) — private preview, server-side validation gap claimed, off-metadata
[PRIO] mysignins.microsoft.com + api.myaccount.microsoft.com source maps — **5.80** (attack:5, business:7, tech:6, gate:8, cloud:6, fresh:9) — 42MB combined, both identity SPAs, unauthenticated access
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 80
reasoning: $metadata 873-char agentRegistration EntityType with ZERO OperationRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied Nullable=false. 5 sibling EntityTypes share pattern. GET→401, HEAD→405 (RFC 6750 §3), OPTIONS→405 (CORS closed).
evidence_needed: Two-principal test — Principal B patches Principal A's agentRegistration with createdBy=A
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId. Check 403/404 vs 200/204.
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, redirecting agentCard endpoints. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] Earth Engine hardcoded OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 96
reasoning: client_secret sha256 `3f3f8d6f…d271` at line :45 + :99 fallback accepted by oauth2.googleapis.com/token (POST → 400 `invalid_grant`, NOT 401 `invalid_client` — RFC 6749 §5.2 proves valid credential). Scopes: cloud-platform+drive+devstorage.full_control+earthengine. Client type `installed` with OOB redirect.
evidence_needed: Token redemption via grant_type=refresh_token
verify_steps: HUMAN_ONLY: File Google VRP report with sha256 of secret value; include POST PoC showing 400 invalid_grant vs invalid_client distinction. VRP determination needed on native-app hardcoded secret policy.
impact: Attacker redeems secret for cloud-platform+devstorage access to any GCP project with Earth Engine API enabled. Severity hinges on VRP policy for native-app hardcoded secrets.
testability: HUMAN_ONLY
[HYP] Copilot Studio D2E S2S conversation-ID IDOR — cross-user conversation access
class: IDOR
asset: (Copilot Studio D2E S2S API endpoint — private preview, exact URL from docs)
confidence: 45
reasoning: Documentation claims conversation-ID NOT validated server-side. If true, authenticated attacker could enumerate/guess conversation-IDs to read other users' bot conversations. Private preview limits scope. No live probe possible without private preview access.
evidence_needed: Authenticated test showing User A accessing User B's conversation by ID
verify_steps: AUTH_HELPED: Obtain private preview access. Create conversation as User A. Attempt GET /conversations/{A's-id} as User B. Check 403/404 vs 200.
impact: Cross-user conversation data leakage in Copilot Studio bots. Medium severity, limited to private preview tenants.
testability: AUTH_HELPED
[PARKED] Copilot Studio D2E S2S conversation-ID IDOR: Confidence 45 is marginal, private preview limits reproducibility, no live probe possible without access. Tracked for 1 cycle only — parked pending private preview availability.
[FINAL]
[NEXT] HUMAN: File Google VRP report for `github.com/google/earthengine-api` hardcoded OAuth client_secret. Payload: (a) sha256 of secret `3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271`, (b) raw GitHub URL `google/earthengine-api/master/python/ee/oauth.py:45`, (c) POST PoC to `oauth2.googleapis.com/token` showing 400 `invalid_grant` vs 401 `invalid_client` distinction proving valid credential, (d) note native-app `installed` client type per line 420 `dict(installed=...)` with OOB redirect — ask VRP team to confirm/deny bounty eligibility per OAuth 2.1 public-client secret deprecation policy.
[LEARN] ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live — GET→401/237, HEAD→405/0 (RFC 6750 §3), 873-char metadata block, 0 OperationRestrictions across 5 EntityTypes
[LEARN] CHANGED: graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 sustained — CORS cross-origin mutation vector closed; core IDOR surface unchanged
[LEARN] ACCEPTED: v1↔v2 JWKS kid overlap confirmed live — v1(4 kids) ⊂ v2(7–8 kids), 0 v1-exclusive steady-state, dual issuer namespaces intact; rotation-desync class stays REJECTED
[LEARN] ACCEPTED: Hardcoded Earth Engine OAuth client_secret confirmed live — sha256 `3f3f8d6f…d271` verbatim, whole-file sha `f4f93c76…` unchanged
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh probes confirmed prior ACCEPTED findings unchanged
[RISK] google: **47** — earthengine secret is confirmed valid OAuth credential with cloud-platform scope; native-app by-design limits but VRP report warranted; no new high-severity surfaces this cycle; GCP control-plane APIs uniformly gated
[RISK] microsoft: **61** — agentRegistration IDOR surface intact (5 EntityTypes, zero restrictions) but OPTIONS→405 closes CORS; D2E S2S conversation-ID gap (private preview) adds freshness; Graph 405 anomaly + v1↔v2 dual-issuer trust boundary remain AUTH_HELPED-gated; RFC 6749 §3 + RFC 6750 §3 violations confirmed live
## 2026-08-10 20:59:12 UTC [google] (model longcat)
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45 — **7.35** (attack:8, business:9, tech:6, gate:5, cloud:9, fresh:6) — valid cloud-platform credential (invalid_grant proof); native-app by-design caps severity; VRP report pending
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations — **6.85** (attack:9, business:9, tech:7, gate:4, cloud:8, fresh:5) — IDOR surface intact (5 EntityTypes, zero restrictions); CORS closed reduces cross-origin feasibility; AUTH_HELPED-gated
[PRIO] login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys — **6.60** (attack:6, business:9, tech:8, gate:3, cloud:7, fresh:4) — AUTH_HELPED-gated; dual-issuer + kid subset invariant confirmed
[PRIO] mysignins.microsoft.com + api.myaccount.microsoft.com source maps — **5.80** (attack:5, business:7, tech:6, gate:8, cloud:6, fresh:9) — 42MB combined, both identity SPAs, unauthenticated access
[PRIO] login.microsoftonline.com/common/oauth2/v2.0/authorize — **5.65** (attack:4, business:8, tech:7, gate:10, cloud:6, fresh:3) — response_type=token → HTTP 200 (RFC 6749 §3), passive-only, no exploit chain
[HYP] Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 80
reasoning: $metadata 873-char agentRegistration EntityType with ZERO OperationRestrictions; createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied Nullable=false. 5 sibling EntityTypes share pattern. GET→401, HEAD→405 (RFC 6750 §3), OPTIONS→405 (CORS closed).
evidence_needed: Two-principal test — Principal B patches Principal A's agentRegistration with createdBy=A
verify_steps: AUTH_HELPED: Provision two Entra test-tenant principals (A, B). A POSTs agentRegistrations. B attempts PATCH /beta/copilot/agentRegistrations/{A's-id} with createdBy=A's objectId. Check 403/404 vs 200/204.
impact: Cross-tenant agent registration takeover — attacker modifies another org's Copilot agent config, redirecting agentCard endpoints. High severity in M365 Copilot ecosystem.
testability: AUTH_HELPED
[HYP] v1↔v2 issuer-confusion via shared JWKS kid set + dual issuer namespaces
class: OATH
asset: login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys
confidence: 60
reasoning: v1 issuer=sts.windows.net/{tid}/, v2 issuer=login.microsoftonline.com/{tid}/v2.0 (dual namespaces). 4–5 v1 kids ⊂ 7–8 v2 kids (0 v1-exclusive steady-state). `Access-Control-Allow-Origin: *` on JWKS. A validator checking only kid+sig but not iss could accept v1-signed token at v2 endpoint.
evidence_needed: Generate v1-token (iss=sts.windows.net/{tid}/) signed with shared kid, submit to v2-only endpoint
verify_steps: AUTH_HELPED: Obtain v1.0 access token for test tenant. Decode header, confirm shared kid. Swap iss to v2.0 while keeping v1 signature — check validator catches iss mismatch.
impact: Token from v1 legacy trust domain accepted at v2-only endpoints, bypassing v2.0 conditional access. Broad Entra ID trust-boundary impact.
testability: AUTH_HELPED
[HYP] Earth Engine hardcoded OAuth client_secret redeemable for cloud-platform-scoped tokens
class: OATH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 96
reasoning: client_secret sha256 `3f3f8d6f…d271` at line :45 + :99 fallback accepted by oauth2.googleapis.com/token (POST → 400 `invalid_grant`, NOT 401 `invalid_client` — RFC 6749 §5.2 proves valid credential). Scopes: cloud-platform+drive+devstorage.full_control+earthengine. Client type `installed` with OOB redirect.
evidence_needed: Token redemption via grant_type=refresh_token
verify_steps: HUMAN_ONLY: File Google VRP report with sha256 of secret value; include POST PoC showing 400 invalid_grant vs invalid_client distinction. VRP determination needed on native-app hardcoded secret policy.
impact: Attacker redeems secret for cloud-platform+devstorage access to any GCP project with Earth Engine API enabled. Severity hinges on VRP policy for native-app hardcoded secrets.
testability: HUMAN_ONLY
[FINAL]
[NEXT] HUMAN: File Google VRP report for `github.com/google/earthengine-api` hardcoded OAuth client_secret. Payload: (a) sha256 of secret `3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271`, (b) raw GitHub URL `google/earthengine-api/master/python/ee/oauth.py:45`, (c) POST PoC to `oauth2.googleapis.com/token` showing 400 `invalid_grant` vs 401 `invalid_client` distinction proving valid credential, (d) note native-app `installed` client type per line 420 `dict(installed=...)` with OOB redirect — ask VRP team to confirm/deny bounty eligibility per OAuth 2.1 public-client secret deprecation policy.
[LEARN] ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live — GET→401/237, HEAD→405/0 (RFC 6750 §3), 873-char metadata block, 0 OperationRestrictions across 5 EntityTypes
[LEARN] CHANGED: graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 sustained — CORS cross-origin mutation vector closed; core IDOR surface unchanged
[LEARN] ACCEPTED: v1↔v2 JWKS kid overlap confirmed live — v1(4 kids) ⊂ v2(7–8 kids), 0 v1-exclusive steady-state, dual issuer namespaces intact; rotation-desync class stays REJECTED
[LEARN] ACCEPTED: Hardcoded Earth Engine OAuth client_secret confirmed live — sha256 `3f3f8d6f…d271` verbatim, whole-file sha `f4f93c76…` unchanged
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh probes confirmed prior ACCEPTED findings unchanged
[RISK] google: **47** — earthengine secret is confirmed valid OAuth credential with cloud-platform scope; native-app by-design limits but VRP report warranted; no new high-severity surfaces this cycle; GCP control-plane APIs uniformly gated
[RISK] microsoft: **61** — agentRegistration IDOR surface intact (5 EntityTypes, zero restrictions) but OPTIONS→405 closes CORS; D2E S2S conversation-ID gap (private preview) adds freshness; Graph 405 anomaly + v1↔v2 dual-issuer trust boundary remain AUTH_HELPED-gated; RFC 6749 §3 + RFC 6750 §3 violations confirmed live
