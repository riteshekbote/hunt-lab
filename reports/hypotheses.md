
## RANKED HYPOTHESES 2026-08-07 15:49:41 UTC

## RANKED HYPOTHESES 2026-08-07 16:11:28 UTC
- [75] `graph.microsoft.com/beta/copilot/agentRegistrations`: Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- [55] POST: D2E conversation-ID resumption / cross-app hijack (from reports/hypotheses-bigpickle.txt)
- [45] login.microsoftonline.com: Issuer-confusion / cross-protocol token replay (v1.0 ↔ v2.0) (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token) — tests enumeration of registrations (doc says no
- NEXT(hypotheses-bigpickle.txt): RAG: In microsoft/Agents + @microsoft/agents-copilotstudio-client (both in-scope orgs), grep for CallerIdentityMismatch / CallerIdentityTypeMismatch / D2EAccess
- NEXT(hypotheses-laguna.txt): PROBE: diff v1.0 JWKS vs v2.0 JWKS key IDs —
- LEARN: ACCEPTED v2.0 HTTP-200 error rendering @ login.microsoftonline.com/common/oauth2/v2.0/authorize: unsupported response_type=token returns HTTP 200 (embedded JS e
- LEARN: ACCEPTED Graph API 405 anomaly @ graph.microsoft.com/v1.0: unauth GET/HEAD returns HTTP 405 (not 401), no WWW-Authenticate Bearer, Content-Length 0 — violates R
- LEARN: REJECTED tokeninfo 500 handler anomaly @ oauth2.googleapis.com/tokeninfo: non-reproducible (1×500 then 3×400 on identical malformed id_token); transient fronten

## RANKED HYPOTHESES 2026-08-07 16:42:10 UTC
- [75] `graph.microsoft.com/beta/copilot/agentRegistrations`: Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- [52] POST: OAuth2 permission-grant escalation via caller-chosen resourceId (from reports/hypotheses-bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All`) — tests 
- NEXT(hypotheses-bigpickle.txt): HUMAN: In an onboarded Verified ID test tenant, run the authorized mint test for HYP-2 — synthetic non-employee member with attacker-set `jobTitle`/`department`
- NEXT(hypotheses-laguna.txt): PROBE: GET `https://login.microsoftonline.com/common/discovery/keys` and `https://login.microsoftonline.com/common/discovery/v2.0/keys` — diff `kid` sets for ov
- LEARN: ACCEPTED `oauth2.googleapis.com/tokeninfo` public introspection oracle: accepts `?access_token=` / `?id_token=` query params (no Authorization header), returns 
- LEARN: ACCEPTED `bughunters.google.com` root `/` hardening: HTTP 200, HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
- LEARN: ACCEPTED Graph API 405 anomaly @ `graph.microsoft.com/v1.0`: unauth HEAD/GET to `/v1.0`, `/me`, `/users` → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bea
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering @ `login.microsoftonline.com/common/oauth2/v2.0/authorize`: unsupported `response_type=token` returns HTTP 200 

## RANKED HYPOTHESES 2026-08-07 17:38:33 UTC
- [75] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- [50] GET/POST/PATCH/DELETE: Agent Registration ownership boundary / undocumented collection enumeration (from reports/hypotheses-bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All`) — tests 
- LEARN: ACCEPTED `oauth2.googleapis.com/tokeninfo` public introspection oracle: accepts `?access_token=` / `?id_token=` query params (no Authorization header), returns 
- LEARN: ACCEPTED `bughunters.google.com` root `/` hardening: HTTP 200, HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
- LEARN: ACCEPTED Graph API 405 anomaly @ `graph.microsoft.com/v1.0`: unauth HEAD/GET to `/v1.0`, `/me`, `/users` → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bea
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering @ `login.microsoftonline.com/common/oauth2/v2.0/authorize`: unsupported `response_type=token` returns HTTP 200 

## RANKED HYPOTHESES 2026-08-07 18:29:05 UTC
- [75] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All`) — tests 
- NEXT(hypotheses-laguna.txt): PROBE: GET `https://login.microsoftonline.com/common/discovery/keys` and `https://login.microsoftonline.com/common/discovery/v2.0/keys` — diff `kid` sets for ov
- LEARN: ACCEPTED `oauth2.googleapis.com/tokeninfo` public introspection oracle: accepts `?access_token=` / `?id_token=` query params (no Authorization header), returns 
- LEARN: ACCEPTED `bughunters.google.com` root `/` hardening: HTTP 200, HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
- LEARN: ACCEPTED Graph API 405 anomaly @ `graph.microsoft.com/v1.0`: unauth HEAD/GET to `/v1.0`, `/me`, `/users` → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bea
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering @ `login.microsoftonline.com/common/oauth2/v2.0/authorize`: unsupported `response_type=token` returns HTTP 200 
- LEARN: No new proving-dead classes this cycle
