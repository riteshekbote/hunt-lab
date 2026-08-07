
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

## RANKED HYPOTHESES 2026-08-07 18:56:45 UTC
- [75] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- [65] https://{envhost}/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations: Copilot Studio D2E conversation-ID validation bypass — server does not validate conversation IDs (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All`) — tests 
- NEXT(hypotheses-laguna.txt): PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All`) — tests 
- LEARN: ACCEPTED `oauth2.googleapis.com/tokeninfo` public introspection oracle: accepts `?access_token=` / `?id_token=` query params (no Authorization header), returns 
- LEARN: ACCEPTED `bughunters.google.com` root `/` hardening: HTTP 200, HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
- LEARN: ACCEPTED Graph API 405 anomaly @ `graph.microsoft.com/v1.0`: unauth HEAD/GET to `/v1.0`, `/me`, `/users` → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bea
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering @ `login.microsoftonline.com/common/oauth2/v2.0/authorize`: unsupported `response_type=token` returns HTTP 200 
- LEARN: No new proving-dead classes this cycle
- LEARN: ACCEPTED `oauth2.googleapis.com/tokeninfo` public introspection oracle: accepts `?access_token=` / `?id_token=` query params (no Authorization header), returns 
- LEARN: ACCEPTED `bughunters.google.com` root `/` hardening: HTTP 200, HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
- LEARN: ACCEPTED Graph API 405 anomaly @ `graph.microsoft.com/v1.0`: unauth HEAD/GET to `/v1.0`, `/me`, `/users` → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bea
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering @ `login.microsoftonline.com/common/oauth2/v2.0/authorize`: unsupported `response_type=token` returns HTTP 200 

## RANKED HYPOTHESES 2026-08-07 19:24:56 UTC
- [82] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-laguna.txt)
- [75] api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential: Verified ID minting via caller-chosen claims — backend only checks Guest/Tenant flags (from reports/hypotheses-nemotron3.txt)
- [50] GET/POST/PATCH/DELETE: Agent Registration ownership boundary / collection enumeration (carry-forward, differentiated sub-claim = enumeration) (from reports/hypotheses-bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All`) — tests 
- NEXT(hypotheses-bigpickle.txt): HUMAN: two-principal test-tenant probe (top-ranked, unexecuted): A POST /beta/copilot/agentRegistrations (createdBy/ownerIds client-set) → B GET /beta/copilot/a
- NEXT(hypotheses-laguna.txt): PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` with valid test-tenant Bearer token (scope `AgentRegistration.ReadWrite.All`, clientId 
- LEARN: ACCEPTED `oauth2.googleapis.com/tokeninfo` public introspection oracle: accepts `?access_token=` / `?id_token=` query params (no Authorization header), returns 
- LEARN: ACCEPTED `bughunters.google.com` root `/` hardening: HTTP 200, HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
- LEARN: ACCEPTED Graph API 405 anomaly @ `graph.microsoft.com/v1.0`: unauth HEAD/GET to `/v1.0`, `/me`, `/users` → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bea
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering @ `login.microsoftonline.com/common/oauth2/v2.0/authorize`: unsupported `response_type=token` returns HTTP 200 
- LEARN: No new proving-dead classes this cycle
- LEARN: REJECTED dual-JWKS rotation desync / alg-confusion @ login.microsoftonline.com/discovery/keys: verified live — v1 kid set (5) is a strict subset of v2.0 (8), al
- LEARN: REJECTED dual-JWKS rotation desync / alg-confusion @ login.microsoftonline.com/discovery/keys: verified live — v1 kid set (5) is a strict subset of v2.0 (8), al
- LEARN: REJECTED dual-JWKS rotation desync / alg-confusion @ login.microsoftonline.com/discovery/keys: verified live — v1 kid set (5) is a strict subset of v2.0 (8), al
- LEARN: ACCEPTED agentRegistration metadata has ZERO ownership restrictions @ graph.microsoft.com/beta/$metadata: createdBy (Nullable=false), ownerIds (Nullable=false),
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap @ login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys: 5 v1.0 kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm
- LEARN: ACCEPTED v1.0-only response_types @ login.microsoftonline.com: v1.0 supports response_type=token (pure implicit) + token id_token (hybrid); v2.0 supports only c
- LEARN: ACCEPTED Graph API 405 anomaly @ graph.microsoft.com/v1.0: unauth HEAD to /v1.0, /me, /users → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer; GET ret
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering @ login.microsoftonline.com/common/oauth2/v2.0/authorize: unsupported response_type=token returns HTTP 200 (not
- LEARN: ACCEPTED tokeninfo public introspection oracle @ oauth2.googleapis.com/tokeninfo: confirmed no-auth 400 `{"error":"invalid_token","error_description":"Either ac

## RANKED HYPOTHESES 2026-08-07 19:36:42 UTC
- [82] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-laguna.txt)
- [70] login.microsoftonline.com/{tid}/oauth2/v2.0/token: Three-hop Agent User `user_fic` Hop3 `user_id` parameter allows arbitrary user impersonation (from reports/hypotheses-nemotron3.txt)
- [55] POST/GET/PATCH: Agent Registration ownership boundary bypass via client-controlled createdBy + PATCH rewrite (from reports/hypotheses-bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET `https://graph.microsoft.com/beta/agentRegistry/agentInstances` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All` or `Cop
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of the top-ranked hypothesis (agentRegistration IDOR). A POST /beta/copilot/agentRegistrations (client-set createdBy + ow
- LEARN: No new proving-dead classes this cycle
- LEARN: No new ACCEPTED classes this cycle (all REJECTED/ACCEPTED in knowledge base remain current)
- LEARN: REJECTED: No new proving-dead classes this cycle — all passive probes confirmed prior findings, no new anomalies.
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap + dual issuer namespaces remain confirmed live — issuer-confusion precondition still valid pending AUTH_HELPED test.
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions still confirmed in metadata — IDOR precondition still valid pending AUTH_HELPED test.
