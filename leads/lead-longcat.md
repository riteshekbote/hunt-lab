
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
