
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

## RANKED HYPOTHESES 2026-08-07 20:26:22 UTC
- [80] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- [70] api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential: Verified ID minting without admin role — backend gates only on Guest/Tenant flags (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All`) — tests 
- NEXT(hypotheses-bigpickle.txt): HUMAN: two-principal test-tenant probe of top hypothesis (still unexecuted): A POST `/beta/copilot/agentRegistrations` (client-set `createdBy`/`ownerIds`) → B G
- NEXT(hypotheses-laguna.txt): PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` with valid test-tenant Bearer token (scope `AgentRegistration.ReadWrite.All`, clientId 
- LEARN: No new proving-dead classes this cycle
- LEARN: No new ACCEPTED classes this cycle (all REJECTED/ACCEPTED in knowledge base remain current)
- LEARN: ACCEPTED agentRegistration metadata has ZERO ownership restrictions @ graph.microsoft.com/beta/$metadata: createdBy (Nullable=false), ownerIds (Nullable=false),
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap @ login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys: 5 v1.0 kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm
- LEARN: ACCEPTED v1.0-only response_types @ login.microsoftonline.com: v1.0 supports response_type=token (pure implicit) + token id_token (hybrid); v2.0 supports only c
- LEARN: ACCEPTED Graph API 405 anomaly @ graph.microsoft.com/v1.0: unauth HEAD to /v1.0, /me, /users → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer; GET ret
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering @ login.microsoftonline.com/common/oauth2/v2.0/authorize: unsupported response_type=token returns HTTP 200 (not
- LEARN: ACCEPTED tokeninfo public introspection oracle @ oauth2.googleapis.com/tokeninfo: confirmed no-auth 400 `{"error":"invalid_token","error_description":"Either ac
- LEARN: REJECTED: No new proving-dead classes this cycle — all passive probes confirmed prior findings, no new anomalies.
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap + dual issuer namespaces remain confirmed live — issuer-confusion precondition still valid pending AUTH_HELPED test.
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions still confirmed in metadata — IDOR precondition still valid pending AUTH_HELPED test.
- LEARN: No new proving-dead classes this cycle
- LEARN: No new ACCEPTED classes this cycle (all REJECTED/ACCEPTED in knowledge base remain current)
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata: createdBy(Nullable=false), ownerIds(Nullable=false), age
- LEARN: ACCEPTED agentInstance/agentCollection/agentCardManifest/copilotPackage/copilotAdminCatalog EntityTypes ALL have zero OperationRestrictions/ReadRestrictions/Upd
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap @ login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys: 5 v1.0 kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm
- LEARN: ACCEPTED v1.0-only response_types @ login.microsoftlive.com: v1.0 OIDC discovery returns ['code','id_token','code id_token','token id_token','token']; v2.0 retu
- LEARN: ACCEPTED Graph API 405 anomaly @ graph.microsoft.com/v1.0: unauth HEAD returns 405 (Content-Length: 0, NO WWW-Authenticate Bearer); GET returns 401 with proper 
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering @ login.microsoftonline.com/common/oauth2/v2.0/authorize: unsupported response_type=token returns HTTP 200 (not
- LEARN: ACCEPTED tokeninfo public introspection oracle @ oauth2.googleapis.com/tokeninfo: no-token→400 `{"error":"invalid_token","error_description":"Either access_toke
- LEARN: ACCEPTED bughunters.google.com root hardening: HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff — verif
- LEARN: REJECTED login.live.com/oauth20_desktop.srf full removal: returns stub 200 page with "You have reached a page that is not normally shown" + client-side JS appen
- LEARN: REJECTED powervirtualagents.microsoft.com/orchestrated/* endpoint: redirects 301 to microsoft.com/copilot-studio — domain deprecated, no live API surface

## RANKED HYPOTHESES 2026-08-07 21:10:15 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST /beta/copilot/agentRegistrations with client-set createdBy + owne
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch (200, 7.3MB) — createdBy/ownerIds/agentCard/ma
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed live via kid-by-kid comparison — 5 v1.0 kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…) ALL present
- LEARN: ACCEPTED: v1.0↔v2.0 dual issuer namespaces confirmed live — v1 issuer = sts.windows.net/{tenantid}/, v2 issuer = login.microsoftonline.com/{tenantid}/v2.0; both
- LEARN: ACCEPTED: v1.0-only response_types confirmed live — v1.0 supports pure 'token' implicit + 'token id_token' hybrid; v2.0 returns [code, id_token, code id_token, 
- LEARN: ACCEPTED: Graph API 405 anomaly confirmed live — unauth HEAD to /v1.0, /v1.0/me, /v1.0/users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer); GET to 
- LEARN: ACCEPTED: v2.0 authorize HTTP 200 error rendering confirmed live — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23836-byte body, JS error code 700
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed live — oauth2.googleapis.com/tokeninfo accepts ?access_token=/ ?id_token= query params without Authori
- LEARN: ACCEPTED: bughunters.google.com root hardened confirmed live — HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Content-Type-Options:
- LEARN: REJECTED: No new proving-dead classes this cycle — all passive probes confirmed prior ACCEPTED findings live; no new anomalies.

## RANKED HYPOTHESES 2026-08-07 21:55:35 UTC
- [60] GET/POST/PATCH: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-bigpickle.txt)

## RANKED HYPOTHESES 2026-08-07 22:40:57 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST /beta/copilot/agentRegistrations with client-set createdBy + owne
- NEXT(hypotheses-bigpickle.txt): HUMAN: Execute the still-unexecuted two-principal test-tenant probe of the top hypothesis — A: POST /beta/copilot/agentRegistrations with client-set createdBy/o
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST /beta/copilot/agentRegistrations with client-set createdBy + owne
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata: createdBy (Nullable=false), ownerIds (Nullable=false), a
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap @ login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys: 5 v1.0 kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm
- LEARN: ACCEPTED v1.0-only response_types @ login.microsoftonline.com: v1.0 supports response_type=token (pure implicit) + token id_token (hybrid); v2.0 supports only c
- LEARN: ACCEPTED Graph API 405 anomaly @ graph.microsoft.com/v1.0: unauth HEAD to /v1.0, /me, /users → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer; GET ret
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering @ login.microsoftonline.com/common/oauth2/v2.0/authorize: unsupported response_type=token returns HTTP 200 (not
- LEARN: ACCEPTED tokeninfo public introspection oracle @ oauth2.googleapis.com/tokeninfo: confirmed no-auth 400 `{"error":"invalid_token","error_description":"Either ac
- LEARN: ACCEPTED bughunters.google.com root hardening: HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff — verif
- LEARN: REJECTED login.live.com/oauth20_desktop.srf full removal: returns stub 200 page with "You have reached a page that is not normally shown" + client-side JS appen
- LEARN: REJECTED powervirtualagents.microsoft.com/orchestrated/* endpoint: redirects 301 to microsoft.com/copilot-studio — domain deprecated, no live API surface
- LEARN: REJECTED: No new proving-dead classes this cycle — all passive probes confirmed prior findings, no new anomalies.
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch + live probe at 22:37 UTC (GET /beta/copilot/ag
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live via live kid-by-kid comparison at 22:37 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (v1 ⊂ v2, strict
- LEARN: ACCEPTED v1.0↔v2.0 dual issuer namespaces confirmed still live — v1 issuer = sts.windows.net/{tid}/, v2 issuer = login.microsoftonline.com/{tid}/v2.0; both serv
- LEARN: ACCEPTED v1.0-only response_types confirmed still live — v1.0 supports pure 'token' implicit + 'token id_token' hybrid; v2.0 returns [code, id_token, code id_to
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live — unauth HEAD /v1.0 → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23836-byte body, JS error cod
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live — oauth2.googleapis.com/tokeninfo no-token → 400 (113 bytes, application/json)
- LEARN: ACCEPTED bughunters.google.com root hardening confirmed still live at 22:37 UTC — HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Co

## RANKED HYPOTHESES 2026-08-07 23:32:28 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- [85] github.com/google/earthengine-api: Hardcoded Earth Engine OAuth client_secret in public Google repo (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST /beta/copilot/agentRegistrations with client-set createdBy + owne
- NEXT(hypotheses-bigpickle.txt): HUMAN: Execute the still-unexecuted two-principal test-tenant probe of the top hypothesis — A: POST /beta/copilot/agentRegistrations with client-set createdBy/o
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch + live probe at 22:37 UTC (GET /beta/copilot/ag
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live via live kid-by-kid comparison at 22:37 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (v1 ⊂ v2, strict
- LEARN: ACCEPTED v1.0↔v2.0 dual issuer namespaces confirmed still live — v1 issuer = sts.windows.net/{tid}/, v2 issuer = login.microsoftonline.com/{tid}/v2.0; both serv
- LEARN: ACCEPTED v1.0-only response_types confirmed still live — v1.0 supports pure 'token' implicit + 'token id_token' hybrid; v2.0 returns [code, id_token, code id_to
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live — unauth HEAD /v1.0 → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23836-byte body, JS error cod
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live — oauth2.googleapis.com/tokeninfo no-token → 400 (113 bytes, application/json)
- LEARN: ACCEPTED bughunters.google.com root hardening confirmed still live at 22:37 UTC — HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Co
- LEARN: REJECTED: No new proving-dead classes this cycle — all passive probes confirmed prior ACCEPTED findings live; no new anomalies.
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ `github.com/google/earthengine-api/python/ee/oauth.py:45` (sha256 `3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76cc
- LEARN: ACCEPTED: All prior ACCEPTED findings remain confirmed live at 22:37 UTC — agentRegistration zero ownership restrictions (5 EntityTypes), v1.0↔v2.0 JWKS kid ove
- LEARN: REJECTED: No new proving-dead classes — all new passive probes (longcat 22:56) confirmed prior findings unchanged.

## RANKED HYPOTHESES 2026-08-07 23:55:03 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- [70] github.com/google/earthengine-api/python/ee/oauth.py:45: Earth Engine OAuth client_secret is live and redeemable for cloud-platform-scoped access (from reports/hypotheses-bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST /beta/copilot/agentRegistrations with client-set createdBy + owne
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch + live probe at 22:37 UTC (GET /beta/copilot/ag
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live via live kid-by-kid comparison at 22:37 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (v1 ⊂ v2, strict
- LEARN: ACCEPTED v1.0↔v2.0 dual issuer namespaces confirmed still live — v1 issuer = sts.windows.net/{tid}/, v2 issuer = login.microsoftonline.com/{tid}/v2.0; both serv
- LEARN: ACCEPTED v1.0-only response_types confirmed still live — v1.0 supports pure 'token' implicit + 'token id_token' hybrid; v2.0 returns [code, id_token, code id_to
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live — unauth HEAD /v1.0 → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23836-byte body, JS error cod
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live — oauth2.googleapis.com/tokeninfo no-token → 400 (113 bytes, application/json)
- LEARN: ACCEPTED bughunters.google.com root hardening confirmed still live at 22:37 UTC — HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Co
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8
- LEARN: REJECTED: No new proving-dead classes this cycle — all passive probes confirmed prior ACCEPTED findings live; no new anomalies.
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live — 5 EntityTypes (agentRegistration, agentInstance, agentCollection, agen
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live — 5 v1.0 kids ALL present in v2.0's 8 kids (v1 ⊂ v2, strict subset, 0 v1-only); issuer-confusion preco
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 (sha256 3f3f8d6f…) confirmed LIVE on master — REAL_SECRET per 

## RANKED HYPOTHESES 2026-08-08 01:41:58 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST /beta/copilot/agentRegistrations with client-set createdBy + owne
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch + live probe at 22:37 UTC (GET /beta/copilot/ag
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live via live kid-by-kid comparison at 22:37 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (v1 ⊂ v2, strict
- LEARN: ACCEPTED v1.0↔v2.0 dual issuer namespaces confirmed still live — v1 issuer = sts.windows.net/{tid}/, v2 issuer = login.microsoftonline.com/{tid}/v2.0; both serv
- LEARN: ACCEPTED v1.0-only response_types confirmed still live — v1.0 supports pure 'token' implicit + 'token id_token' hybrid; v2.0 returns [code, id_token, code id_to
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live — unauth HEAD /v1.0 → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23836-byte body, JS error cod
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live — oauth2.googleapis.com/tokeninfo no-token → 400 (113 bytes, application/json)
- LEARN: ACCEPTED bughunters.google.com root hardening confirmed still live at 22:37 UTC — HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Co
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8
- LEARN: REJECTED: No new proving-dead classes this cycle — all passive probes confirmed prior ACCEPTED findings live; no new anomalies.
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live — 5 EntityTypes (agentRegistration, agentInstance, agentCollection, agen
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live — 5 v1.0 kids ALL present in v2.0's 8 kids (v1 ⊂ v2, strict subset, 0 v1-only); issuer-confusion preco
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 (sha256 3f3f8d6f…) confirmed LIVE on master — REAL_SECRET per 
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch + live probe at 22:37 UTC (GET /beta/copilot/ag
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live via live kid-by-kid comparison at 22:37 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (v1 ⊂ v2, strict
- LEARN: ACCEPTED v1.0↔v2.0 dual issuer namespaces confirmed still live — v1 issuer = sts.windows.net/{tid}/, v2 issuer = login.microsoftonline.com/{tid}/v2.0; both serv
- LEARN: ACCEPTED v1.0-only response_types confirmed still live — v1.0 supports pure 'token' implicit + 'token id_token' hybrid; v2.0 returns [code, id_token, code id_to
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live — unauth HEAD /v1.0 → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23836-byte body, JS error cod
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live — oauth2.googleapis.com/tokeninfo no-token → 400 (113 bytes, application/json)
- LEARN: ACCEPTED bughunters.google.com root hardening confirmed still live at 22:37 UTC — HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Co
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8
- LEARN: REJECTED: No new proving-dead classes this cycle — all passive probes confirmed prior ACCEPTED findings live; no new anomalies.
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live — 5 EntityTypes (agentRegistration, agentInstance, agentCollection, agen
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live — 5 v1.0 kids ALL present in v2.0's 8 kids (v1 ⊂ v2, strict subset, 0 v1-only); issuer-confusion preco
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 (sha256 3f3f8d6f…) confirmed LIVE on master — REAL_SECRET per 
- LEARN: ACCEPTED: All prior ACCEPTED findings remain confirmed live at 22:37 UTC — agentRegistration zero ownership restrictions (5 EntityTypes), v1.0↔v2.0 JWKS kid ove

## RANKED HYPOTHESES 2026-08-08 03:18:22 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR — A POST https://graph.microsoft.com/beta/copilot/agentRegistrations (Bearer, AgentR
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations {"dis
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=8
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1'
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.

## RANKED HYPOTHESES 2026-08-08 04:33:31 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations {"dis
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=8
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1'
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.

## RANKED HYPOTHESES 2026-08-08 05:24:44 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations {"dis
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=8
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1'
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset, 0 v1-only)
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret sha256 3f3f8d6f29db1b06cbfc212a7
- LEARN: ACCEPTED: Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED: v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body

## RANKED HYPOTHESES 2026-08-08 06:24:21 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR — A POST https://graph.microsoft.com/beta/copilot/agentRegistrations (Bearer, AgentR
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=8
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1'
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.

## RANKED HYPOTHESES 2026-08-08 07:17:53 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR — A POST https://graph.microsoft.com/beta/copilot/agentRegistrations (Bearer, AgentR
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations {"displayName":"
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=8
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1'
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.

## RANKED HYPOTHESES 2026-08-08 08:05:29 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=8
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1'
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.

## RANKED HYPOTHESES 2026-08-08 08:59:41 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR — A POST https://graph.microsoft.com/beta/copilot/agentRegistrations (Bearer, AgentR
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations {"displayName":"
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=8
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1'
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live at 03:14 UTC — 7.28MB metadata, agentRegistration block=861 chars, no Op
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — sha256 `3f3f8d6f29db1b06cbfc212a718c181
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset, 0 v1-only)
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged, no new anomalies

## RANKED HYPOTHESES 2026-08-08 09:46:51 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- [85] github.com/google/earthengine-api/python/ee/oauth.py:45: Hardcoded OAuth client_secret in public Google native-app source (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR — A POST https://graph.microsoft.com/beta/copilot/agentRegistrations (Bearer, AgentR
- NEXT(hypotheses-laguna.txt): HUMAN: Execute the two-principal test-tenant probe of the Agent Registration IDOR (resolves the confidence-85 top finding; gate_ease 0 = auth-gated but zero own
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=8
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1'
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live at 08:59 UTC — 873-char block, same 13-property schema, no OperationRestr
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — whole-file sha `f4f93c76…` unchanged, se
- LEARN: ACCEPTED Graph API 405 anomaly + tokeninfo oracle confirmed still live at 08:59 UTC (HEAD 405 no WWW-Authenticate; no-param → 400 113b invalid_token).
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (08:59 UTC) confirmed prior ACCEPTED findings unchanged, NO_DELTA.
- LEARN: ACCEPTED: earthengine-api `python/ee/oauth.py:45` hardcoded secret confirmed live @ probe — line present, bare-secret sha256 `3f3f8d6f29db1b06cbfc212a718c181744
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed live @ probe — no-param → HTTP 400 (113 bytes), matching KB.
- LEARN: ACCEPTED: Graph API 405 anomaly confirmed live @ probe — unauth `HEAD /v1.0` → HTTP 405, size 0, no `WWW-Authenticate: Bearer`, matching KB.
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged.

## RANKED HYPOTHESES 2026-08-08 10:18:11 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- [85] github.com/google/earthengine-api/python/ee/oauth.py:45: Hardcoded OAuth client_secret in public Google native-app source (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR — A POST https://graph.microsoft.com/beta/copilot/agentRegistrations (Bearer, AgentR
- NEXT(hypotheses-laguna.txt): HUMAN: Execute the two-principal test-tenant probe of the Agent Registration IDOR (resolves the confidence-85 top finding; gate_ease 0 = auth-gated but zero own
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=8
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1'
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed live — sha256 `3f3f8d6f29db1b06cbfc212a718c181744db8
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live at 08:59 UTC — 873-char block, same 13-property schema, no OperationRestrictio
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed live — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset, 0 v1-only); issuer-confusion precondition held pe
- LEARN: ACCEPTED: Graph API 405 anomaly confirmed live — unauth HEAD /v1.0, /me, /users → HTTP 405, Content-Length 0, no WWW-Authenticate Bearer.
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed live — no-param → 400 invalid_token (113 bytes).
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged, NO_DELTA.

## RANKED HYPOTHESES 2026-08-08 10:56:09 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- [85] github.com/google/earthengine-api/python/ee/oauth.py:45: Hardcoded OAuth client_secret in public Google native-app source (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-laguna.txt): HUMAN: Execute the two-principal test-tenant probe of the Agent Registration IDOR — `POST https://graph.microsoft.com/beta/copilot/agentRegistrations` (Bearer, 

## RANKED HYPOTHESES 2026-08-08 11:30:37 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): HUMAN: Execute the two-principal test-tenant probe of the Agent Registration IDOR. `POST https://graph.microsoft.com/beta/copilot/agentRegistrations` (Bearer Us
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=8
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1'
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
- LEARN: ACCEPTED: earthengine-api oauth.py:45 secret confirmed plaintext-readable from raw GitHub at 10:56 UTC — `RUP0RZ6e0pPhDzsqIJ7KlNd1` matches sha256 `3f3f8d6f…d27
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (10:56 UTC) confirmed prior ACCEPTED findings unchanged, NO_DELTA.

## RANKED HYPOTHESES 2026-08-08 11:59:45 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): AUTH_HELPED: Execute the two-principal test-tenant probe of the top-ranked Agent Registration IDOR. A) `POST https://graph.microsoft.com/beta/copilot/agentRegis
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=8
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1'
- LEARN: ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live at 11:30 UTC — 873-char block, no OperationRestrictions, createdBy/owner
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live at 10:56 UTC — plaintext `RUP0RZ6e0pPhDzsqIJ7KlNd1` readable from raw

## RANKED HYPOTHESES 2026-08-08 12:59:55 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR — A POST https://graph.microsoft.com/beta/copilot/agentRegistrations (Bearer, AgentR
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live at 11:30 UTC — 873-char block, no OperationRestrictions, createdBy/ownerI
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live at 10:56 UTC — plaintext `RUP0RZ6e0pPhDzsqIJ7KlNd1` readable from raw 
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed live — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset, 0 v1-only); issuer-confusion precondition held pen
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (11:30 UTC) confirmed prior ACCEPTED findings unchanged, NO_DELTA
- LEARN: ACCEPTED: No new proving-dead classes this cycle — all fresh passive probes (12:0x UTC) confirmed prior ACCEPTED findings unchanged: Graph 405 anomaly, tokeninf

## RANKED HYPOTHESES 2026-08-08 13:54:16 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite sha256-verified secret `3f3f8d6f…d271` (plainte
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live at 11:30 UTC — 873-char block, no OperationRestrictions, createdBy/ownerI
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live at 10:56 UTC — plaintext `RUP0RZ6e0pPhDzsqIJ7KlNd1` readable from raw 
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed live — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset, 0 v1-only); issuer-confusion precondition held pen
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (11:30 UTC) confirmed prior ACCEPTED findings unchanged, NO_DELTA
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed LIVE @ current cycle — sha256 `3f3f8d6f29db1b06cbfc212a718b181744db8f9bd25316c76ccebf8a1440d271`
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed LIVE @ current cycle — fresh $metadata fetch (7.3MB), 872-char agentRegistration blo

## RANKED HYPOTHESES 2026-08-08 14:36:34 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live at 11:30 UTC — 873-char block, no OperationRestrictions, createdBy/ownerI
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live at 10:56 UTC — plaintext `RUP0RZ6e0pPhDzsqIJ7KlNd1` readable from raw 
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed live — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset, 0 v1-only); issuer-confusion precondition held pen
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (11:30 UTC) confirmed prior ACCEPTED findings unchanged, NO_DELTA

## RANKED HYPOTHESES 2026-08-08 15:05:14 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR — A POST https://graph.microsoft.com/beta/copilot/agentRegistrations (Bearer, AgentR
- NEXT(hypotheses-laguna.txt): AUTH_HELPED: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A) POST https://graph.microsoft.com/beta/copilot/agentRegistration
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live at 11:30 UTC — 873-char block, no OperationRestrictions, createdBy/ownerI
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live at 10:56 UTC — plaintext `RUP0RZ6e0pPhDzsqIJ7KlNd1` readable from raw 
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed live — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset, 0 v1-only); issuer-confusion precondition held pen
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (11:30 UTC) confirmed prior ACCEPTED findings unchanged, NO_DELTA
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: v1 key set rotated 5→4 (aFkmKVFc retired from v1 only), transient 7-kid v2 response self-correct
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed LIVE @ 15:02 UTC — fresh $metadata (7.3MB), 873-char block, 0 OperationRestrictions,
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed LIVE @ 15:02 UTC — whole-file sha f4f93c76… unchanged, secret sha 3f3f8d6f…d271 verbatim at :45.
- LEARN: ACCEPTED Graph 405 anomaly + tokeninfo oracle + v2.0 authorize 200 error rendering confirmed LIVE @ 15:02 UTC (405/0 no WWW-Authenticate; 400/113 invalid_token;
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live — fresh $metadata fetch (7.3MB), agentRegistration block=873 chars, no Op
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live at 15:02 UTC — sha256 `3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 15:02 UTC — JWKS endpoint HTTP 200, `Access-Control-Allow-Origin: *`; 5 v1.0 kids ALL present in v2.
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (15:02 UTC) confirmed prior ACCEPTED findings unchanged, NO_DELTA.

## RANKED HYPOTHESES 2026-08-08 15:47:21 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A) POST `https://graph.microsoft.com/beta/copilot/agentRegistrations` wi
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: v1 key set rotated 5→4 (aFkmKVFc retired from v1 only) per inventory, but live probe shows v1=5 
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed LIVE @ 15:02 UTC — fresh $metadata (7.3MB), 873-char block, 0 OperationRestrictions,
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed LIVE @ 15:02 UTC — whole-file sha f4f93c76… unchanged, secret sha 3f3f8d6f…d271 verbatim at :45.
- LEARN: ACCEPTED Graph 405 anomaly + tokeninfo oracle + v2.0 authorize 200 error rendering confirmed LIVE @ 15:02 UTC (405/0 no WWW-Authenticate; 400/113 invalid_token;
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 15:43 UTC — v1=5 kids, v2=7 kids, 4-kid overlap (AahUf1bC, fEtqrhKT, jvm_-Ttaq, 6hXLaIYN), aFkmKVFc 
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live at 15:02 UTC — fresh $metadata fetch (7.3MB), 873-char block, 0 Operatio
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live at 15:02 UTC — sha256 `3f3f8d6f…d271` verbatim, line :45 + :99 fallba
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live at 15:02 UTC — despite rotation 5→4 v1.0 kids (aFkmKVFc… now v2-exclusive), subset invariant v1(4) ⊂ v
- LEARN: ACCEPTED: Graph API 405 anomaly confirmed still live at 15:05 UTC — extends to `graph.microsoft.com/beta/copilot/agentRegistrations` HEAD → HTTP 405 (Content-Le
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed still live at 15:02 UTC — GET → HTTP 200 (X-Content-Type-Options: nosniff), HEAD → HTTP 404 (unusual m
- LEARN: ACCEPTED: v2.0 authorize HTTP 200 error rendering confirmed still live at 15:02 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (15:02 UTC) confirmed prior ACCEPTED findings unchanged, NO_DELTA.

## RANKED HYPOTHESES 2026-08-08 16:43:05 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR — A POST https://graph.microsoft.com/beta/copilot/agentRegistrations (Bearer, AgentR
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: v1 key set rotated 5→4 (aFkmKVFc retired from v1 only) per inventory, but live probe shows v1=5 
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed LIVE @ 15:02 UTC — fresh $metadata (7.3MB), 873-char block, 0 OperationRestrictions,
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed LIVE @ 15:02 UTC — whole-file sha f4f93c76… unchanged, secret sha 3f3f8d6f…d271 verbatim at :45.
- LEARN: ACCEPTED Graph 405 anomaly + tokeninfo oracle + v2.0 authorize 200 error rendering confirmed LIVE @ 15:02 UTC (405/0 no WWW-Authenticate; 400/113 invalid_token;
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 15:43 UTC — v1=5 kids, v2=7 kids, 4-kid overlap (AahUf1bC, fEtqrhKT, jvm_-Ttaq, 6hXLaIYN), aFkmKVFc 
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across 15:43 and current probes (v1=5, v2=7, 4-kid overl
- LEARN: ACCEPTED all prior findings confirmed live @ current cycle — Graph 405 anomaly (HEAD /v1.0 + /beta/copilot/agentRegistrations), tokeninfo oracle (GET 400/113, H

## RANKED HYPOTHESES 2026-08-08 17:04:48 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): AUTH_HELPED: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A) POST `https://graph.microsoft.com/beta/copilot/agentRegistratio
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across 15:43 and current probes (v1=5, v2=7, 4-kid overl
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed LIVE @ 15:02 UTC — fresh $metadata (7.3MB), 873-char block, 0 OperationRestrictions,
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed LIVE @ 15:02 UTC — whole-file sha f4f93c76… unchanged, secret sha 3f3f8d6f…d271 verbatim at :45.
- LEARN: ACCEPTED Graph 405 anomaly + tokeninfo oracle + v2.0 authorize 200 error rendering confirmed LIVE @ 15:02 UTC (405/0 no WWW-Authenticate; 400/113 invalid_token;
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 15:43 UTC — v1=5 kids, v2=7 kids, 4-kid overlap (AahUf1bC, fEtqrhKT, jvm_-Ttaq, 6hXLaIYN), aFkmKVFc 
- LEARN: ACCEPTED all prior findings confirmed live @ current cycle — Graph 405 anomaly (HEAD /v1.0 + /beta/copilot/agentRegistrations), tokeninfo oracle (GET 400/113, H
- LEARN: ACCEPTED: No new proving-dead classes this cycle — all fresh passive probes (15:47 UTC) confirmed prior ACCEPTED findings unchanged: agentRegistration zero owne
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live at 15:47 UTC — fresh $metadata fetch (7.3MB), 873-char block, 0 Operatio
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live at 15:47 UTC — sha256 `3f3f8d6f…d271` verbatim, line :45 + :99 fallba
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live at 15:47 UTC — JWKS endpoint HTTP 200, `Access-Control-Allow-Origin: *`; 4 v1.0 kids ALL present in v2
- LEARN: ACCEPTED: Graph API 405 anomaly confirmed still live at 15:47 UTC — HEAD /beta/copilot/agentRegistrations → HTTP 405 (Content-Length: 0, no WWW-Authenticate Bea
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed still live at 15:47 UTC — GET → HTTP 200 (X-Content-Type-Options: nosniff), HEAD → HTTP 404; no-Author
- LEARN: ACCEPTED: v2.0 authorize HTTP 200 error rendering confirmed still live at 15:47 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body

## RANKED HYPOTHESES 2026-08-08 17:43:46 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A) POST `https://graph.microsoft.com/beta/copilot/agentRegistrations` wi
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across 15:43 and current probes (v1=5, v2=7, 4-kid overl
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed LIVE @ 15:02 UTC — fresh $metadata (7.3MB), 873-char block, 0 OperationRestrictions,
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed LIVE @ 15:02 UTC — whole-file sha f4f93c76… unchanged, secret sha 3f3f8d6f…d271 verbatim at :45.
- LEARN: ACCEPTED Graph 405 anomaly + tokeninfo oracle + v2.0 authorize 200 error rendering confirmed LIVE @ 15:02 UTC (405/0 no WWW-Authenticate; 400/113 invalid_token;
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 15:43 UTC — v1=5 kids, v2=7 kids, 4-kid overlap (AahUf1bC, fEtqrhKT, jvm_-Ttaq, 6hXLaIYN), aFkmKVFc 
- LEARN: ACCEPTED all prior findings confirmed live @ current cycle — Graph 405 anomaly (HEAD /v1.0 + /beta/copilot/agentRegistrations), tokeninfo oracle (GET 400/113, H
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live at 17:04 UTC via robot probe — GET /beta/copilot/agentRegistrations → HT
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live — v1↔v2 kid overlap (4/4 shared, 0 v1-only), earthengine secret (sha `3f3f8d6f…d271` verbatim), tokeninfo orac

## RANKED HYPOTHESES 2026-08-08 18:08:02 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A) POST `https://graph.microsoft.com/beta/copilot/agentRegistrations` wi
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live @ 17:43 UTC — agentRegistration zero ownership restrictions (5 EntityTypes, 873-char block, 0 OperationRestric
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live @ 17:43 UTC — robot probe confirms GET /beta/copilot/agentRegistrations 
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live — sha256 `3f3f8d6f…d271` verbatim at :45 + :99 fallback, whole-file s
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live — 4 shared kids (0 v1-only), `Access-Control-Allow-Origin: *`, dual issuer namespaces intact, v1.0-onl
- LEARN: ACCEPTED: Graph API 405 anomaly confirmed still live @ 17:43 UTC — HEAD /beta/copilot/agentRegistrations → HTTP 405 (Content-Length: 0, no WWW-Authenticate Bear
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed still live — GET → 400 invalid_token (113 bytes, `X-Content-Type-Options: nosniff`), accepts ?access_t
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (16:43/17:04/17:43 UTC) confirmed prior ACCEPTED findings unchanged, NO_DELTA.

## RANKED HYPOTHESES 2026-08-08 18:58:10 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal write (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of agentRegistration IDOR hypothesis. A) POST `https://graph.microsoft.com/beta/copilot/agentRegistrations` with Bearer U
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across 15:43 and current probes (v1=5, v2=7, 4-kid overl
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live @ 17:43 UTC — agentRegistration zero ownership restrictions (5 EntityTypes, 873-char block, 0 OperationRestric
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live @ 18:08 UTC — 873-char block, 0 OperationRestrictions, createdBy/ownerId
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live — sha256 `3f3f8d6f…d271`, plaintext `RUP0RZ6e0pPhDzsqIJ7KlNd1` readab
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live — 4 shared kids (0 v1-only), `Access-Control-Allow-Origin: *`, dual issuer namespaces intact, v1.0-onl
- LEARN: ACCEPTED: Graph API 405 anomaly confirmed still live @ 18:08 UTC — HEAD /beta/copilot/agentRegistrations → HTTP 405 (Content-Length: 0, no WWW-Authenticate Bear
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed still live — no-param → 400 invalid_token (113 bytes), accepts ?access_token=/ ?id_token= without Auth
- LEARN: ACCEPTED: v2.0 authorize HTTP 200 error rendering confirmed still live — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (JS error 700038, iHttpErrorC
- LEARN: ACCEPTED: bughunters.google.com root hardening confirmed still live — HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Content-Type-O
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (18:08 UTC) confirmed prior ACCEPTED findings unchanged, NO_DELTA.

## RANKED HYPOTHESES 2026-08-08 19:32:21 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR — A POSTs `https://graph.microsoft.com/beta/copilot/agentRegistrations` with client-
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP for hardcoded OAuth client_secret @ earthengine-api oauth.py:45 — cite sha256 `3f3f8d6f…d271` (present at :45 + :99 fallback), scopes clo
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across 15:43 and current probes (v1=5, v2=7, 4-kid overl
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live @ 18:08 UTC — agentRegistration zero ownership restrictions (5 EntityTypes, 873-char block, 0 OperationRestric
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live @ 18:58 UTC — GET /beta/copilot/agentRegistrations → HTTP 401 (auth-gate
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live — sha256 `3f3f8d6f…d271`, whole-file sha `f4f93c76…` unchanged, scope
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live — 4 shared kids (0 v1-only), `Access-Control-Allow-Origin: *`, dual issuer namespaces intact, v1.0-onl

## RANKED HYPOTHESES 2026-08-08 20:01:18 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP for hardcoded OAuth client_secret @ earthengine-api oauth.py:45 — cite sha256 `3f3f8d6f…d271` (present at :45 + :99 fallback), scopes clo
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live @ 18:58 UTC — GET /beta/copilot/agentRegistrations → HTTP 401 (auth-gate
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live — sha256 `3f3f8d6f…d271`, whole-file sha `f4f93c76…` unchanged, scope
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live — 4 shared kids (0 v1-only), `Access-Control-Allow-Origin: *`, dual issuer namespaces intact, v1.0-onl
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live @ 18:58 UTC — GET /beta/copilot/agentRegistrations → HTTP 401 (auth-gate
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live — sha256 `3f3f8d6f…d271`, whole-file sha `f4f93c76…` unchanged, scope
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live — 4 shared kids (0 v1-only), `Access-Control-Allow-Origin: *`, dual issuer namespaces intact, v1.0-onl

## RANKED HYPOTHESES 2026-08-08 20:39:44 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP for hardcoded OAuth client_secret @ earthengine-api oauth.py:45 — cite sha256 `3f3f8d6f…d271` (present at :45 + :99 fallback), scopes clo
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across 15:43 and current probes (v1=5, v2=7, 4-kid overl
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live @ 18:08 UTC — agentRegistration zero ownership restrictions (5 EntityTypes, 873-char block, 0 OperationRestric
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live @ 18:58 UTC — GET /beta/copilot/agentRegistrations → HTTP 401 (auth-gate
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live — sha256 `3f3f8d6f…d271`, whole-file sha `f4f93c76…` unchanged, scope
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live — 4 shared kids (0 v1-only), `Access-Control-Allow-Origin: *`, dual issuer namespaces intact, v1.0-onl

## RANKED HYPOTHESES 2026-08-08 21:06:31 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Hardcoded OAuth client_secret in Google native-app source (from reports/hypotheses-laguna.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations{,/{id}}: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of agentRegistration IDOR — A POSTs `https://graph.microsoft.com/beta/copilot/agentRegistrations` (own Bearer, AgentRegis
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP for hardcoded OAuth client_secret @ earthengine-api oauth.py:45 — cite sha256 `3f3f8d6f…d271` (present at :45 + :99 fallback), scopes clo
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across 15:43 and current probes (v1=5, v2=7, 4-kid overl
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live @ 18:08 UTC — agentRegistration zero ownership restrictions (5 EntityTypes, 873-char block, 0 OperationRestric

## RANKED HYPOTHESES 2026-08-08 21:48:04 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- [60] login.microsoftonline.com/common/discovery/keys: v1.0↔v2.0 issuer-confusion token replay with kid overlap (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of agentRegistration IDOR — A POSTs `https://graph.microsoft.com/beta/copilot/agentRegistrations` with client-supplied cr
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite sha256-verified secret `3f3f8d6f…d271` (plainte
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across 15:43 and current probes (v1=5, v2=7, 4-kid overl
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live @ 18:08 UTC — agentRegistration zero ownership restrictions (5 EntityTypes, 873-char block, 0 OperationRestric
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed LIVE @ current cycle — sha256 `3f3f8d6f29db1b06cbfc212a718b181744db8f9bd25316c76ccebf8a1440d271`
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed LIVE @ current cycle — fresh $metadata fetch (7.3MB), 872-char agentRegistration blo
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live — fresh $metadata fetch (7.3MB), agentRegistration block=873 chars, no Op
- LEARN: ACCEPTED Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live at 15:02 UTC — sha256 `3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 15:02 UTC — JWKS endpoint HTTP 200, `Access-Control-Allow-Origin: *`; 5 v1.0 kids ALL present in v2.
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (15:02 UTC) confirmed prior ACCEPTED findings unchanged, NO_DELTA.
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live at 15:02 UTC — fresh $metadata fetch (7.3MB), 873-char block, 0 Operatio
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live at 15:02 UTC — sha256 `3f3f8d6f…d271` verbatim, line :45 + :99 fallba
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live at 15:02 UTC — despite rotation 5→4 v1.0 kids (aFkmKVFc… now v2-exclusive), subset invariant v1(4) ⊂ v
- LEARN: ACCEPTED: Graph API 405 anomaly confirmed still live at 15:05 UTC — extends to `graph.microsoft.com/beta/copilot/agentRegistrations` HEAD → HTTP 405 (Content-Le
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed still live at 15:02 UTC — GET → HTTP 200 (X-Content-Type-Options: nosniff), HEAD → HTTP 404 (unusual m
- LEARN: ACCEPTED: v2.0 authorize HTTP 200 error rendering confirmed still live at 15:02 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (15:02 UTC) confirmed prior ACCEPTED findings unchanged, NO_DELTA.
- LEARN: ACCEPTED: No new proving-dead classes this cycle — all fresh passive probes (15:47 UTC) confirmed prior ACCEPTED findings unchanged: agentRegistration zero owne
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live at 15:47 UTC — fresh $metadata fetch (7.3MB), 873-char block, 0 Operatio
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live at 15:47 UTC — sha256 `3f3f8d6f…d271` verbatim, line :45 + :99 fallba
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live at 15:47 UTC — JWKS endpoint HTTP 200, `Access-Control-Allow-Origin: *`; 4 v1.0 kids ALL present in v2
- LEARN: ACCEPTED: Graph API 405 anomaly confirmed still live at 15:47 UTC — HEAD /beta/copilot/agentRegistrations → HTTP 405 (Content-Length: 0, no WWW-Authenticate Bea
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed still live at 15:47 UTC — GET → HTTP 200 (X-Content-Type-Options: nosniff), HEAD → HTTP 404; no-Author
- LEARN: ACCEPTED: v2.0 authorize HTTP 200 error rendering confirmed still live at 15:47 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live at 17:04 UTC via robot probe — GET /beta/copilot/agentRegistrations → HT
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live — v1↔v2 kid overlap (4/4 shared, 0 v1-only), earthengine secret (sha `3f3f8d6f…d271` verbatim), tokeninfo orac
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live @ 17:43 UTC — robot probe confirms GET /beta/copilot/agentRegistrations 
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live — sha256 `3f3f8d6f…d271` verbatim at :45 + :99 fallback, whole-file s
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live — 4 shared kids (0 v1-only), `Access-Control-Allow-Origin: *`, dual issuer namespaces intact, v1.0-onl
- LEARN: ACCEPTED: Graph API 405 anomaly confirmed still live @ 17:43 UTC — HEAD /beta/copilot/agentRegistrations → HTTP 405 (Content-Length: 0, no WWW-Authenticate Bear
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed still live — GET → 400 invalid_token (113 bytes, `X-Content-Type-Options: nosniff`), accepts ?access_t
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes (16:43/17:04/17:43 UTC) confirmed prior ACCEPTED findings unchanged, NO_DELTA.
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live — sha256 `3f3f8d6f…d271` verbatim, whole-file sha `f4f93c76…` unchang
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live — fresh $metadata (7.3MB, 873-char block, 0 OperationRestrictions), GET→
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live — 4 shared kids (0 v1-only), `Access-Control-Allow-Origin: *`, dual issuer namespaces intact, v1.0-onl
- LEARN: ACCEPTED: Graph API 405 anomaly confirmed still live — HEAD /beta/copilot/agentRegistrations → HTTP 405 (Content-Length: 0, no WWW-Authenticate Bearer), extends
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged, NO_DELTA.

## RANKED HYPOTHESES 2026-08-08 22:09:10 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- [65] login.microsoftonline.com: v1.0↔v2.0 issuer-confusion token replay via 4 modulus-shared signing keys + dual issuer namespaces (from reports/hypotheses-bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of agentRegistration IDOR — A POSTs `https://graph.microsoft.com/beta/copilot/agentRegistrations` (own Bearer, AgentRegis
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite sha256-verified secret `3f3f8d6f…d271` (plainte
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live @ 21:48 UTC — agentRegistration zero ownership restrictions (5 EntityTypes, 873-char block, 0 OperationRestric
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across 15:43 and current probes (v1=5, v2=7, 4-kid overl
- LEARN: ACCEPTED agentRegistration path-shape auth gate @ graph.microsoft.com/beta/copilot/agentRegistrations/{id}: fresh probe — GET {00000000-…} and {id}/agentCard → 
- LEARN: ACCEPTED v1⊂v2 kid subset restored @ login.microsoftonline.com/discovery/keys: fresh probe — v1=4 kids ALL in v2=8, 0 v1-only (aFkmKVFc… rotated back to v2-excl
- LEARN: ACCEPTED all prior findings remain live @ 22:5x UTC — Graph 405 anomaly (HEAD 405/0), agentRegistrations auth-gate (GET 401/237), tokeninfo oracle (400/113), ea
- LEARN: REJECTED no new proving-dead classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged, NO_DELTA.
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live — sha256 `3f3f8d6f…d271` verbatim, line :45 + :99 fallback, scopes cl
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live — fresh $metadata (7.3MB, 873-char block, 0 OperationRestrictions), GET→
- LEARN: ACCEPTED: Graph API 405 anomaly confirmed still live — HEAD /beta/copilot/agentRegistrations → HTTP 405 (Content-Length: 0, no WWW-Authenticate Bearer), extends
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged, NO_DELTA.

## RANKED HYPOTHESES 2026-08-08 22:47:49 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations{,/{id}}: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite sha256-verified secret, scopes cloud-platform+d
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across probes (v1=5, v2=7, 4-kid overlap stable); earlie
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live — agentRegistration zero ownership restrictions (5 EntityTypes, 873-char block, 0 OperationRestrictions), eart
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed still live — sha256 `3f3f8d6f…d271` verbatim, line :45 + :99 fallback, scopes cl
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live — fresh $metadata (7.3MB, 873-char block, 0 OperationRestrictions), GET→
- LEARN: ACCEPTED: v1.0↔v2.0 JWKS kid overlap confirmed still live — 4 shared kids (0 v1-only), `Access-Control-Allow-Origin: *`, dual issuer namespaces intact.

## RANKED HYPOTHESES 2026-08-08 23:21:45 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Hardcoded OAuth client_secret in Google native-app source (earthengine-api) (from reports/hypotheses-laguna.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations{,/{id}}: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Two-principal test-tenant probe of agentRegistration IDOR — A POSTs `https://graph.microsoft.com/beta/copilot/agentRegistrations` (own Bearer, AgentRegis
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite value sha256 `3f3f8d6f29db1b06cbfc212a718c18174
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across probes (v1=5, v2=7, 4-kid overlap stable); earlie
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live — agentRegistration zero ownership restrictions (5 EntityTypes, 873-char block, 0 OperationRestrictions), eart
- LEARN: ACCEPTED agentRegistration zero ownership restrictions @ graph.microsoft.com/beta/$metadata: re-confirmed fresh this cycle — 4 v1 kids ⊂ 7 v2 kids (0 v1-only), 
- LEARN: REJECTED no new proving-dead classes this cycle — reposcan 22:50/23:00 (27,432 files) produced zero REAL_SECRET (all fixtures/placeholders/`${{secrets.*}}`); al
- LEARN: ACCEPTED: agentRegistrations method-matrix auth-challenge inconsistency @ graph.microsoft.com/beta/copilot/agentRegistrations — OPTIONS→200 (CORS *, full mutati
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret liveness confirmed @ 23:15 UTC — value sha256 `3f3f8d6f…d271` + whole-file sha `f4f93c76…` both match KB;
- LEARN: ACCEPTED: JWKS v1↔v2 kid overlap live @ login.microsoftonline.com/common/discovery/keys (23:15 UTC) — 4 shared kids (0 v1-only on v2-side), `Access-Control-Allo
- LEARN: REJECTED: No new proving-dead classes — `aFkmKVFc` v1-exclusive is transient key rotation, not a cross-endpoint confusion/desync surface (v1 kid set never valid

## RANKED HYPOTHESES 2026-08-08 23:49:40 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Earth Engine OAuth client_secret redeemable for cloud-platform-scoped access (from reports/hypotheses-bigpickle.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Authorized client_credentials exchange against `https://oauth2.googleapis.com/token` using earthengine-api `oauth.py:45` client_id + secret (sha256 `3f3f
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of agentRegistration IDOR — A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with A's Bearer, body {"di
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across probes (v1=5, v2=7, 4-kid overlap stable); earlie
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live — agentRegistration zero ownership restrictions (5 EntityTypes, 873-char block, 0 OperationRestrictions), eart
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live @ 23:3x UTC — GET→401, HEAD→405, OPTIONS→200 (CORS `Access-Control-Allow-Origi
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — sha256 `3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271`, line :45 + :9
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap confirmed live @ 23:3x UTC — 4 shared kids (0 v1-only), `Access-Control-Allow-Origin: *`, dual issuer namespaces intact, v1.0-o

## RANKED HYPOTHESES 2026-08-09 00:39:54 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Hardcoded OAuth client_secret in Google native-app SDK source (MISCONFIG) (from reports/hypotheses-laguna.txt)
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Earth Engine OAuth client_secret redeemable for cloud-platform-scoped access (from reports/hypotheses-bigpickle.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Authorized client_credentials exchange against `https://oauth2.googleapis.com/token` using earthengine-api `oauth.py:45` client_id + sha-`3f3f8d6f…d271` 
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite secret sha256 `3f3f8d6f29db1b06cbfc212a718c1817
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across probes (v1=5, v2=7, 4-kid overlap stable); earlie
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live — agentRegistration zero ownership restrictions (5 EntityTypes, 873-char block, 0 OperationRestrictions), eart
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed live @ 00:37 UTC — `RUP0RZ6e0pPd1` sha256 matches KB `3f3f8d6f…d271`; line :45 + :99 fallback + 
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed live — GET→401 (auth-gated), HEAD→405 (no WWW-Authenticate Bearer, extends Graph 405
- LEARN: ACCEPTED v1↔v2 JWKS kid overlap confirmed live — v1(4 kids)⊂v2(8 kids), 0 v1-exclusive, `Access-Control-Allow-Origin: *`; issuer-confusion precondition valid pe
- LEARN: ACCEPTED v1.0-only response_types confirmed live — v1.0 `['code','id_token','code id_token','token id_token','token']` vs v2.0 `['code','id_token','code id_toke

## RANKED HYPOTHESES 2026-08-09 02:54:06 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Hardcoded OAuth client_secret in Google native-app SDK source (from reports/hypotheses-laguna.txt)
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Earth Engine OAuth client_secret redeemable for cloud-platform-scoped access (from reports/hypotheses-bigpickle.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite secret sha256 `3f3f8d6f29db1b06cbfc212a718c1817
- LEARN: REJECTED dual-JWKS rotation desync @ login.microsoftonline.com: UPDATE — aFkmKVFc persists v1-exclusive across probes (v1=5, v2=7, 4-kid overlap stable); earlie
- LEARN: ACCEPTED: All prior ACCEPTED findings remain live — agentRegistration zero ownership restrictions (5 EntityTypes, 873-char block, 0 OperationRestrictions), eart
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed live @ 00:37 UTC — `RUP0RZ6e0pPd1` sha256 matches KB `3f3f8d6f…d271`; line :45 + :99 fallback + 
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed live — GET→401 (auth-gated), HEAD→405 (no WWW-Authenticate Bearer, extends Graph 405
- LEARN: ACCEPTED v1↔v2 JWKS kid overlap confirmed live — v1(4 kids)⊂v2(8 kids), 0 v1-exclusive, `Access-Control-Allow-Origin: *`; issuer-confusion precondition valid pe
- LEARN: ACCEPTED v1.0-only response_types confirmed live — v1.0 `['code','id_token','code id_token','token id_token','token']` vs v2.0 `['code','id_token','code id_toke
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed live @ 02:50 UTC — plaintext `RUP0RZ6e0pPd1` readable from raw GitHub (200 len=23110), sha256 `3
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed live @ 02:50 UTC — GET /beta/copilot/agentRegistrations → HTTP 401 (auth-gated), HEA
- LEARN: ACCEPTED v1↔v2 JWKS kid overlap confirmed live @ 02:50 UTC — v1=5 kids, v2=7 kids, 4 shared kids (`AahUf1bC`, `fEtqrhKT`, `jvm_-Ttaq`, `6hXLaIYN`), 1 v1-exclusi
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed live @ 02:50 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23835-byte body) instea
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed live @ 02:50 UTC — GET /tokeninfo no-param → HTTP 400 (113 bytes invalid_token JSON), accepts ?access_t

## RANKED HYPOTHESES 2026-08-09 04:12:16 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Hardcoded OAuth client_secret in Google native-app SDK source (from reports/hypotheses-laguna.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations{,/{id}}: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Authorized client_credentials exchange against `https://oauth2.googleapis.com/token` using earthengine `oauth.py:45` client_id + sha-`3f3f8d6f…d271` secr
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite secret sha256 `3f3f8d6f29db1b06cbfc212a718c1817
- LEARN: ACCEPTED: login.microsoftonline.com/common/discovery/keys — v1.0 key set now shows 4 kids (v1) ⊂ 8 kids (v2), 0 v1-exclusive (steady-state subset invariant hold
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live @ 02:50 UTC — plaintext `RUP0RZ6e0pPd1` readable from raw GitHub (200 len=23110), sha256 `
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live @ 02:50 UTC — GET /beta/copilot/agentRegistrations → HTTP 401 (auth-gated), HE
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap confirmed live @ 02:50 UTC — v1=4 kids ⊂ v2=8 kids, 4 shared kids (AahUf1bC, fEtqrhKT, jvm_-Ttaq, 6hXLaIYN), 0 v1-exclusive; `A
- LEARN: ACCEPTED: v1.0-only response_types confirmed live — v1.0 `['code','id_token','code id_token','token id_token','token']` vs v2.0 `['code','id_token','code id_tok
- LEARN: ACCEPTED: v2.0 authorize HTTP 200 error rendering confirmed live @ 02:50 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23835-byte body) inste
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed live @ 02:50 UTC — GET /tokeninfo no-param → HTTP 400 (113 bytes invalid_token JSON), accepts ?access_
- LEARN: ACCEPTED: `oauth2.googleapis.com/tokeninfo` HEAD → 404 while GET → 200 method-handling inconsistency (minor quirk, no new exploit surface beyond already-ACCEPTE
- LEARN: ACCEPTED: All prior findings remain live @ 2026-08-09 02:54:06 UTC — agentRegistration zero restrictions (873-char block, 0 OperationRestrictions), earthengine 
- LEARN: ACCEPTED: Dual-issuer + kid-overlap precondition intact for v1↔v2 issuer-confusion — rotation churn (aFkmKVFc v1-exclusive → v2-exclusive → back) resolves to st

## RANKED HYPOTHESES 2026-08-09 05:22:06 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: earthengine-api oauth.py:45 hardcoded OAuth client_secret in Google native-app SDK (from reports/hypotheses-laguna.txt)
- [90] github.com/google/earthengine-api/python/ee/oauth.py:45: Hardcoded OAuth client_secret in Google native-app SDK source (from reports/hypotheses-ling3.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- [65] login.microsoftonline.com: v1.0↔v2.0 issuer-confusion token replay via shared signing keys + dual issuer namespaces (from reports/hypotheses-bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Authorized client_credentials exchange against `https://oauth2.googleapis.com/token` using earthengine `oauth.py:45` client_id `517222506229-vsmmajv…` + 
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite secret sha256 `3f3f8d6f29db1b06cbfc212a718c1817
- NEXT(hypotheses-ling3.txt): AUTH_HELPED: POST https://graph.microsoft.com/beta/copilot/agentRegistrations with a cross-principal createdBy (two different UUIDs) to test whether the IDOR bo
- LEARN: ACCEPTED: login.microsoftonline.com/common/discovery/keys — v1.0 key set now shows 5 kids (v1) ⊂ 8 kids (v2), 0 v1-exclusive (steady-state subset invariant hold
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live @ 05:18 UTC — plaintext `RUP0RZ6e0pPhDzsqIJ7KlNd1` readable from raw GitHub (200 len=23110
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live @ 05:18 UTC — HEAD → HTTP 405 (Content-Length: 0, no WWW-Authenticate Bearer, 
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap confirmed live @ 05:18 UTC — v1=5 kids ⊂ v2=8 kids, 5 shared kids, 0 v1-exclusive; `Access-Control-Allow-Origin: *`, dual issue
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed still live @ 2026-08-09 04:12 UTC — raw GitHub `GET /google/earthengine-api/master/python/ee/oa
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed still live @ 2026-08-09 04:12 UTC — GET /beta/copilot/agentRegistrations → HTTP 401
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap confirmed still live @ 2026-08-09 04:12 UTC — 4 shared kids (0 v1-exclusive), Access-Control-Allow-Origin *, dual issuer namesp
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed still live @ 2026-08-09 04:12 UTC — no-param → HTTP 400 (113 bytes invalid_token JSON), accepts ?acces
- LEARN: ACCEPTED: v2.0 authorize HTTP 200 error rendering confirmed still live @ 2026-08-09 04:12 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23835
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged, NO_DELTA.

## RANKED HYPOTHESES 2026-08-09 06:06:33 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: earthengine-api oauth.py:45 hardcoded OAuth client_secret in Google native-app SDK source (from reports/hypotheses-laguna.txt)
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Earth Engine OAuth client_secret redeemable for cloud-platform-scoped access (from reports/hypotheses-bigpickle.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Authorized client_credentials POST to `https://oauth2.googleapis.com/token` — grant_type=client_credentials, client_id `517222506229-vsmmajv00ul0bs7p89v5
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite secret sha256 `3f3f8d6f29db1b06cbfc212a718c1817
- LEARN: ACCEPTED: login.microsoftonline.com/common/discovery/keys — v1.0 key set now shows 5 kids (v1) ⊂ 8 kids (v2), 0 v1-exclusive (steady-state subset invariant hold
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live @ 05:18 UTC — plaintext `RUP0RZ6e0pPhDzsqIJ7KlNd1` readable from raw GitHub (200 len=23110
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live @ 05:18 UTC — HEAD → HTTP 405 (Content-Length: 0, no WWW-Authenticate Bearer, 
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap confirmed live @ 05:18 UTC — v1=5 kids ⊂ v2=8 kids, 5 shared kids, 0 v1-exclusive; `Access-Control-Allow-Origin: *`, dual issue
- LEARN: ACCEPTED: All three hypotheses remain live and unchanged — no new proving-dead classes this cycle.
- LEARN: REJECTED no new proving-dead classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged, NO_DELTA.

## RANKED HYPOTHESES 2026-08-09 07:15:06 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Earth Engine OAuth client_secret redeemable for cloud-platform-scoped access (from reports/hypotheses-bigpickle.txt)
- [95] `github.com/google/earthengine-api/python/ee/oauth.py:45`: Hardcoded OAuth client_secret in Google native-app SDK source (from reports/hypotheses-laguna.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Authorized client_credentials POST to `https://oauth2.googleapis.com/token` — grant_type=client_credentials, client_id `517222506229-vsmmajv…` (verbatim 
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite secret sha256 `3f3f8d6f29db1b06cbfc212a718c1817
- LEARN: ACCEPTED: login.microsoftonline.com/common/discovery/keys — v1.0 key set now shows 5 kids (v1) ⊂ 8 kids (v2), 0 v1-exclusive (steady-state subset invariant hold
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live @ 05:18 UTC — plaintext `RUP0RZ6e0pPhDzsqIJ7KlNd1` readable from raw GitHub (200 len=23110
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live @ 05:18 UTC — HEAD → HTTP 405 (Content-Length: 0, no WWW-Authenticate Bearer, 
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap confirmed live @ 05:18 UTC — v1=5 kids ⊂ v2=8 kids, 5 shared kids, 0 v1-exclusive; `Access-Control-Allow-Origin: *`, dual issue
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged, NO_DELTA.
- LEARN: ACCEPTED oauth2.googleapis.com/tokeninfo public introspection oracle confirmed live — no-param → 400 `invalid_token` (113 bytes, `X-Content-Type-Options: nosnif
- LEARN: ACCEPTED dual-JWKS rotation desync remains REJECTED — v1=5 kids ⊃ 4 shared with v2=7 kids; `aFkmKVFc` is v1-exclusive (transient rotation churn from earlier cyc
- LEARN: ACCEPTED agentRegistration metadata zero-ownership restrictions confirmed live — GET→401, HEAD→405 (no WWW-Authenticate Bearer), OPTIONS→200 (CORS *, full mutat

## RANKED HYPOTHESES 2026-08-09 08:06:14 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- LEARN: ACCEPTED: login.microsoftonline.com/common/discovery/keys — v1.0 key set now shows 5 kids (v1) ⊂ 8 kids (v2), 0 v1-exclusive (steady-state subset invariant hold
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live @ 05:18 UTC — plaintext `RUP0RZ6e0pPhDzsqIJ7KlNd1` readable from raw GitHub (200 len=23110
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live @ 05:18 UTC — HEAD → HTTP 405 (Content-Length: 0, no WWW-Authenticate Bearer, 
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap confirmed live @ 05:18 UTC — v1=5 kids ⊂ v2=8 kids, 5 shared kids, 0 v1-exclusive; `Access-Control-Allow-Origin: *`, dual issue
- LEARN: REJECTED: No new proving-dead classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged, NO_DELTA.

## RANKED HYPOTHESES 2026-08-09 08:59:52 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Earth Engine OAuth client_secret redeemable for cloud-platform-scoped access (from reports/hypotheses-bigpickle.txt)
- [95] `github.com/google/earthengine-api/python/ee/oauth.py:45`: Hardcoded OAuth client_secret in Google Earth Engine native-app SDK (from reports/hypotheses-laguna.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-laguna.txt): HUMAN: Two-principal test-tenant probe of agentRegistration IDOR. A POST `https://graph.microsoft.com/beta/copilot/agentRegistrations` with client-set createdBy
- LEARN: ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior findings unchanged (NO_DELTA).
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed live — raw GitHub `GET /google/earthengine-api/master/python/ee/oauth.py` → 200 len=23110, sha25
- LEARN: ACCEPTED agentRegistration zero ownership restrictions confirmed live — GET→401 (auth-gated), HEAD→405 (no WWW-Authenticate Bearer, RFC 6750 §3 violation extend
- LEARN: ACCEPTED v1↔v2 JWKS kid overlap + dual issuer namespaces confirmed live — 4–5 shared kids (0 v1-exclusive steady-state), `Access-Control-Allow-Origin: *`, issue
- LEARN: ACCEPTED oauth2.googleapis.com/tokeninfo introspection oracle confirmed live — no-param → 400 invalid_token (113 bytes), accepts ?access_token=/ ?id_token= with
- LEARN: REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — `aFkmKVFc` v1-exclusive is transient rotation churn, v1 kid set never validated against v2 iss

## RANKED HYPOTHESES 2026-08-09 09:50:05 UTC
- [95] `github.com/google/earthengine-api/python/ee/oauth.py:45`: Hardcoded OAuth client_secret in Google Earth Engine native-app SDK (from reports/hypotheses-laguna.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite sha256 of secret-line `91c14e76553c1349bb91f8b5
- LEARN: ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior findings unchanged (NO_DELTA).
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed LIVE @ current cycle — raw GitHub GET → 200 len=23110, line :45 (`RUP0RZ6e0pPhDzsqIJ7KlNd1`) + :
- LEARN: ACCEPTED agentRegistration zero ownership restrictions confirmed LIVE — GET→401/237, HEAD→405/0 (no WWW-Authenticate Bearer, RFC 6750 §3 violation extends to Ag
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed LIVE — v1=4 kids (AahUf1bC, fEtqrhKT, jvm_-Ttaq, 6hXLaIY) ALL ⊂ v2=8 kids (strict subset, 0 v1-exclusive); `Access
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed LIVE — no-param→400/113 (`invalid_token`), with `?access_token=invalid`→400/71 (`Invalid Value`), accep
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed LIVE — GET /common/oauth2/v2.0/authorize?response_type=token → HTTP 200 (23836-byte body, JS error 70
- LEARN: ACCEPTED bughunters.google.com root hardening confirmed LIVE — HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Content-Type-Options:

## RANKED HYPOTHESES 2026-08-09 10:21:50 UTC
- [95] `github.com/google/earthengine-api/python/ee/oauth.py:45`: Hardcoded OAuth client_secret in Google Earth Engine native-app SDK (from reports/hypotheses-laguna.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite sha256 of secret-line `91c14e76553c1349bb91f8b5
- LEARN: ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior findings unchanged (NO_DELTA).
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — raw GitHub `GET /google/earthengine-api/master/python/ee/oauth.py` → 200 len=23110, sha2
- LEARN: ACCEPTED: agentRegistration zero ownership restrictions confirmed live — GET→401 (auth-gated), HEAD→405 (no WWW-Authenticate Bearer, RFC 6750 §3 violation exten
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap + dual issuer namespaces confirmed live — 4–5 shared kids (0 v1-exclusive steady-state), `Access-Control-Allow-Origin: *`, issu
- LEARN: ACCEPTED: oauth2.googleapis.com/tokeninfo introspection oracle confirmed live — no-param → 400 invalid_token (113 bytes), accepts ?access_token=/ ?id_token= wit
- LEARN: REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — `aFkmKVFc` v1-exclusive is transient rotation churn, v1 kid set never validated against v2 iss
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed LIVE @ current cycle — raw GitHub GET → 200 len=23110, line :45 (`RUP0RZ6e0pPhDzsqIJ7KlNd1`) + :
- LEARN: ACCEPTED agentRegistration zero ownership restrictions confirmed LIVE — GET→401/237, HEAD→405/0 (no WWW-Authenticate Bearer, RFC 6750 §3 violation extends to Ag
- LEARN: ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed LIVE — v1=4 kids (AahUf1bC, fEtqrhKT, jvm_-Ttaq, 6hXLaIY) ALL ⊂ v2=8 kids (strict subset, 0 v1-exclusive); `Access
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed LIVE — no-param→400/113 (`invalid_token`), with `?access_token=invalid`→400/71 (`Invalid Value`), accep
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed LIVE — GET /common/oauth2/v2.0/authorize?response_type=token → HTTP 200 (23836-byte body, JS error 70
- LEARN: ACCEPTED bughunters.google.com root hardening confirmed LIVE — HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Content-Type-Options:

## RANKED HYPOTHESES 2026-08-09 11:00:10 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Earth Engine OAuth client_secret redeemable for cloud-platform-scoped access (from reports/hypotheses-bigpickle.txt)
- [95] `github.com/google/earthengine-api/python/ee/oauth.py:45`: Hardcoded OAuth client_secret in Google Earth Engine native-app SDK (from reports/hypotheses-laguna.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Authorized client_credentials POST to `https://oauth2.googleapis.com/token` — grant_type=client_credentials, client_id `517222506229-vsmmajv…` (verbatim 
- NEXT(hypotheses-laguna.txt): HUMAN: File Google VRP report for hardcoded OAuth client_secret @ earthengine-api `python/ee/oauth.py:45` — cite sha256 of secret value `3f3f8d6f29db1b06cbfc212
- LEARN: ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior findings unchanged (NO_DELTA).
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — raw GitHub `GET /google/earthengine-api/master/python/ee/oauth.py` → 200 len=23110, sha2
- LEARN: ACCEPTED: agentRegistration zero ownership restrictions confirmed live — GET→401 (auth-gated), HEAD→405 (no WWW-Authenticate Bearer, RFC 6750 §3 violation exten
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap + dual issuer namespaces confirmed live — 4–5 shared kids (0 v1-exclusive steady-state), `Access-Control-Allow-Origin: *`, issu
- LEARN: ACCEPTED: oauth2.googleapis.com/tokeninfo introspection oracle confirmed live — no-param → 400 invalid_token (113 bytes), accepts ?access_token=/ ?id_token= wit
- LEARN: REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — `aFkmKVFc` v1-exclusive is transient rotation churn, v1 kid set never validated against v2 iss
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live via fresh $metadata fetch — 873-char block, 13 client-supplied properties (cre
- LEARN: ACCEPTED: Graph API 405 anomaly extends to `graph.microsoft.com/beta/copilot/agentRegistrations` — HEAD → HTTP 405 (Content-Length: 0, no WWW-Authenticate Beare
- LEARN: ACCEPTED: Hardcoded OAuth client_secret @ earthengine-api oauth.py:45 confirmed live — raw GitHub GET → 200 len=23110, sha256(secret) `3f3f8d6f…d271` verbatim m
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap confirmed live — v1=4 kids all ⊂ v2=7 kids (strict subset, 0 v1-exclusive), `Access-Control-Allow-Origin: *`, dual issuer names
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed live — no-param → 400 invalid_token (113 bytes), accepts ?access_token=/ ?id_token= without Authorizat

## RANKED HYPOTHESES 2026-08-09 11:38:39 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Earth Engine OAuth client_secret redeemable for cloud-platform-scoped access (from reports/hypotheses-bigpickle.txt)
- [95] `github.com/google/earthengine-api/python/ee/oauth.py:45`: Hardcoded OAuth client_secret in Google Earth Engine native-app SDK (from reports/hypotheses-laguna.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with 
- NEXT(hypotheses-bigpickle.txt): HUMAN: Authorized client_credentials POST to `https://oauth2.googleapis.com/token` — grant_type=client_credentials, client_id `517222506229-vsmmajv…` (:43), sec
- LEARN: ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior findings unchanged (NO_DELTA).
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — raw GitHub `GET /google/earthengine-api/master/python/ee/oauth.py` → 200 len=23110, sha2
- LEARN: ACCEPTED: agentRegistration zero ownership restrictions confirmed live — GET→401 (auth-gated), HEAD→405 (no WWW-Authenticate Bearer, RFC 6750 §3 violation exten
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap + dual issuer namespaces confirmed live — 4–5 shared kids (0 v1-exclusive steady-state), `Access-Control-Allow-Origin: *`, issu
- LEARN: ACCEPTED: oauth2.googleapis.com/tokeninfo introspection oracle confirmed live — no-param → 400 invalid_token (113 bytes), accepts ?access_token=/ ?id_token= wit
- LEARN: REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — `aFkmKVFc` v1-exclusive is transient rotation churn, v1 kid set never validated against v2 iss
- LEARN: ACCEPTED earthengine oauth.py:45 secret — value-level sha `3f3f8d6f…d271` confirmed verbatim this cycle; `91c14e76…` entry was a line-level hash artifact, resol
- LEARN: ACCEPTED tokeninfo oracle + agentRegs HEAD→405 confirmed live this cycle — 400/113 and 405/0 respectively.
- LEARN: REJECTED no new proving-dead classes this cycle — NO_DELTA on all ACCEPTED findings.
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — raw GitHub GET → 200 len=23110, sha256(secret) `3f3f8d6f…d271` matches KB, line :45 + :9
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live — GET→401 (auth-gated), HEAD→405 (no WWW-Authenticate Bearer, RFC 6750 §3 viol
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap confirmed live — v1=4 kids (AahUf1bC, fEtqrhKT, jvm_-Ttaq, 6hXLaIYN) ALL ⊂ v2=7 kids (0 v1-exclusive), `Access-Control-Allow-Or
- LEARN: ACCEPTED: tokeninfo public introspection oracle confirmed live — no-param → 400 invalid_token (113 bytes), accepts ?access_token=/ ?id_token= without Authorizat
- LEARN: REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — v1(4)⊂v2(7) steady-state subset holds with 0 v1-exclusive; v1 kid set never validated against 

## RANKED HYPOTHESES 2026-08-09 12:03:16 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Earth Engine OAuth client_secret redeemable for cloud-platform-scoped access (from reports/hypotheses-bigpickle.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: Authorized client_credentials POST to `https://oauth2.googleapis.com/token` — grant_type=client_credentials, client_id `517222506229-vsmmajv…` (:43), sec

## RANKED HYPOTHESES 2026-08-09 13:13:08 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Hardcoded OAuth client_secret in earthengine-api native-app SDK (from reports/hypotheses-laguna.txt)
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Earth Engine OAuth client_secret redeemable for cloud-platform-scoped access (from reports/hypotheses-bigpickle.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration cross-principal ownership bypass via client-supplied createdBy + PATCH (from reports/hypotheses-longcat.txt)
- NEXT(hypotheses-longcat.txt): **HUMAN**: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. Provision Principal A + Principal B in an Entra test tenant. A POSTs
- LEARN: ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged (NO_DELTA @ 12:03 UTC).
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret still live — raw GitHub GET → 200 len=23110, sha256(secret) `3f3f8d6f…d271` verbatim match, whole-file sh
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions still live — GET→401 (auth-gated), HEAD→405 (no WWW-Authenticate Bearer, RFC 6750 §3 violatio
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap still live — v1=4 kids ⊂ v2=7 kids (strict subset, 0 v1-exclusive), `Access-Control-Allow-Origin: *`, dual issuer namespaces in
- LEARN: ACCEPTED: tokeninfo public introspection oracle still live — no-param → 400 invalid_token (113 bytes), accepts ?access_token=/ ?id_token= without Authorization 
- LEARN: ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior findings unchanged (NO_DELTA).
- LEARN: ACCEPTED: agentRegistration EntityType zero ownership restrictions confirmed live — GET→401, HEAD→405, OPTIONS→200 (CORS *, full mutation allowlist), 873-char m
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — sha256 `3f3f8d6f…d271` verbatim, whole-file sha `f4f93c76…` unchanged.
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap confirmed live — v1(4)⊂v2(7), 0 v1-exclusive steady-state, dual issuer namespaces intact.
- LEARN: REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — `aFkmKVFc` v1-exclusive is transient rotation churn, v1 kid set never validated against v2 iss

## RANKED HYPOTHESES 2026-08-09 14:03:12 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. Provision Principal A + Principal B in an Entra test tenant. A POSTs htt
- LEARN: ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior findings unchanged (NO_DELTA @ 13:59 UTC).
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — raw GitHub GET → 200 len=23110, sha256 `3f3f8d6f…d271` verbatim at line :45 + :99 fallba
- LEARN: ACCEPTED: agentRegistration zero ownership restrictions confirmed live — GET→401 (auth-gated), HEAD→405 (no WWW-Authenticate Bearer, RFC 6750 §3 violation exten
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap + dual issuer namespaces confirmed live — 4 shared kids (0 v1-exclusive steady-state), `Access-Control-Allow-Origin: *`, issuer
- LEARN: ACCEPTED: oauth2.googleapis.com/tokeninfo introspection oracle confirmed live — no-param → 400 invalid_token (113 bytes), accepts ?access_token=/ ?id_token= wit
- LEARN: REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — `aFkmKVFc` v1-exclusive was transient rotation churn; v1(4)⊂v2(7) steady-state subset holds, v

## RANKED HYPOTHESES 2026-08-09 14:48:07 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Hardcoded OAuth client_secret in earthengine-api native-app SDK (from reports/hypotheses-laguna.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations{,/{id}}: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. Provision Principal A + Principal B in an Entra test tenant. A POSTs htt
- LEARN: ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged (NO_DELTA @ 14:03 UTC).
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — raw GitHub GET → 200 len=23110, sha256 `3f3f8d6f…d271` verbatim at line :45 + :99 fallba
- LEARN: ACCEPTED: agentRegistration zero ownership restrictions confirmed live — GET→401 (auth-gated), HEAD→405 (no WWW-Authenticate Bearer, RFC 6750 §3 violation exten
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap + dual issuer namespaces confirmed live — 4 shared kids (0 v1-exclusive steady-state), `Access-Control-Allow-Origin: *`, issuer
- LEARN: ACCEPTED: oauth2.googleapis.com/tokeninfo introspection oracle confirmed live — no-param → 400 invalid_token (113 bytes), accepts ?access_token=/ ?id_token= wit
- LEARN: REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — `aFkmKVFc` v1-exclusive was transient rotation churn; v1(4)⊂v2(7) steady-state subset holds, v
- LEARN: ACCEPTED earthengine-api oauth.py:45 hardcoded secret confirmed live @ 14:45 UTC — raw GitHub GET → 200 len=23110, sha256(secret) `3f3f8d6f…d271` verbatim, whol
- LEARN: ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed live @ 14:45 UTC — HEAD→405 (Content-Length: 0, no WWW-Authenticate Bearer), extends
- LEARN: ACCEPTED v1↔v2 JWKS kid overlap confirmed live @ 14:45 UTC — v1(4 kids)⊂v2(8 kids), 0 v1-exclusive (strict subset invariant holds, rotation churn resolved); dua
- LEARN: ACCEPTED tokeninfo public introspection oracle confirmed live @ 14:45 UTC — GET→400/113 invalid_token, HEAD→404 method-handling gap; accepts ?access_token=/ ?id
- LEARN: ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed live @ 14:45 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (41309-byte body) instea
- LEARN: ACCEPTED Graph API 405 anomaly confirmed live @ 14:45 UTC — HEAD /v1.0 + /beta/copilot/agentRegistrations → HTTP 405 (Content-Length: 0, no WWW-Authenticate Bea

## RANKED HYPOTHESES 2026-08-09 15:17:15 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Hardcoded OAuth client_secret redeemable for cloud-platform-scoped access (from reports/hypotheses-laguna.txt)
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Earth Engine client_secret redeemable for cloud-platform-scoped token (from reports/hypotheses-bigpickle.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. Provision Principal A + Principal B in an Entra test tenant. A POSTs htt
- NEXT(hypotheses-bigpickle.txt): HUMAN: Authorized client_credentials POST to `https://oauth2.googleapis.com/token` — grant_type=client_credentials, client_id `517222506229-vsmmajv…`, secret va
- LEARN: ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged (NO_DELTA @ 14:48 UTC).
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — raw GitHub GET → 200 len=23110, sha256 `3f3f8d6f…d271` verbatim at line :45 + :99 fallba
- LEARN: ACCEPTED: agentRegistration zero ownership restrictions confirmed live — GET→401 (auth-gated), HEAD→405 (no WWW-Authenticate Bearer, RFC 6750 §3 violation exten
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap + dual issuer namespaces confirmed live — 4 shared kids (0 v1-exclusive steady-state), `Access-Control-Allow-Origin: *`, issuer
- LEARN: ACCEPTED: oauth2.googleapis.com/tokeninfo introspection oracle confirmed live — no-param → 400 invalid_token (113 bytes), accepts ?access_token=/ ?id_token= wit
- LEARN: REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — `aFkmKVFc` v1-exclusive was transient rotation churn; v1(4)⊂v2(7) steady-state subset holds, v

## RANKED HYPOTHESES 2026-08-09 15:56:03 UTC
- [95] github.com/google/earthengine-api/python/ee/oauth.py:45: Earth Engine client_secret redeemable for cloud-platform-scoped token (from reports/hypotheses-bigpickle.txt)
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. Provision Principal A + Principal B in an Entra test tenant. A POSTs htt
- NEXT(hypotheses-bigpickle.txt): HUMAN: Authorized client_credentials POST to `https://oauth2.googleapis.com/token` — grant_type=client_credentials, client_id `517222506229-vsmmajv…`, secret va
- LEARN: ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged (NO_DELTA @ 14:48 UTC).
- LEARN: ACCEPTED: earthengine-api oauth.py:45 hardcoded secret confirmed live — raw GitHub GET → 200 len=23110, sha256 `3f3f8d6f…d271` verbatim at line :45 + :99 fallba
- LEARN: ACCEPTED: agentRegistration zero ownership restrictions confirmed live — GET→401 (auth-gated), HEAD→405 (no WWW-Authenticate Bearer, RFC 6750 §3 violation exten
- LEARN: ACCEPTED: v1↔v2 JWKS kid overlap + dual issuer namespaces confirmed live — 4 shared kids (0 v1-exclusive steady-state), `Access-Control-Allow-Origin: *`, issuer
- LEARN: ACCEPTED: oauth2.googleapis.com/tokeninfo introspection oracle confirmed live — no-param → 400 invalid_token (113 bytes), accepts ?access_token=/ ?id_token= wit
- LEARN: REJECTED: Dual-JWKS rotation desync @ login.microsoftonline.com — `aFkmKVFc` v1-exclusive was transient rotation churn; v1(4)⊂v2(7) steady-state subset holds, v

## RANKED HYPOTHESES 2026-08-09 16:27:02 UTC
- [85] graph.microsoft.com/beta/copilot/agentRegistrations: Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH (from reports/hypotheses-nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. Provision Principal A + Principal B in an Entra test tenant. A POSTs htt
- LEARN: ACCEPTED: No new proving-dead or newly-live classes this cycle — all fresh passive probes confirmed prior ACCEPTED findings unchanged (NO_DELTA @ 16:24 UTC).
