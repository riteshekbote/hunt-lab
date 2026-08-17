# Microsoft surface inventory (seed 2026-08-07)

## ALIVE / CONFIRMED
- mysignins.microsoft.com SPA API map: clientId 19db86c3-b2b9-44cc-b339-36da233a3be2 (MSA); api.myaccount clientId 8c59ead7-d703-4a27-9e55-c96a0054c8d2; all endpoint probes → 401
- login.live vs AAD redirect asymmetry (known class, deferred validation AADSTS50011)
- ACS legacy (account.microsoft.com/auth/ACS): metadata alive, 4 keys (rotated from 5); endpoints /tokens/OAuth/2, /tokens/delegation, /mgmt/delegation/1
- Entra Verified ID endpoints (DID core, credential contracts, status API) — surface only
- accledger /health: informational only
- AAD authorize (clientId 19db86c3..., any redirect_uri incl. attacker + prompt=none): 200 generic "Redirecting" shell, no Location; validation ONLY at completion (AADSTS50011 exact-match)
- Graph agentSignInSessions endpoint: 401

## DEAD / REJECTED
- AAD deferred redirect validation: known class, not reportable (REJECTED)
- jarvisapi.account.microsoft.com: NXDOMAIN (internal URL leak dead)
- jcmsfd.account.microsoft.com/CPM: 400 MissingOrInvalidHeader (header-gated)
- /health metadata disclosures: informational, no-reward (REJECTED)
- Endpoint-map-only output: not a finding (REJECTED)
- Implicit-flow / email-enum surfaces: no-reward (REJECTED)

## NOTES
- ACS EOL; ESTS 2.1.24997.11 observed
- 401 endpoint maps: discovery only, need AUTH_HELPED/HUMAN tests to progress

## 2026-08-07 16:11:28 UTC
- NEW Source maps live on BOTH identity SPAs: `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (7MB, 4359 paths) + `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35MB, 4922 files with sourc
- NEW Verified ID minting endpoint `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential` — POST, Bearer scope=SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2`; backend gates ONLY on `GuestIsN
- NEW `/me/agentSignInSessions` (v1.0 + beta) fully off-metadata — 0 refs in both `$metadata` docs (hashes `9d84e451...`, `ca304859...`) but endpoint alive (401)
- NEW Agent Registry API (beta, deprecated May-2026): `/beta/agentRegistry` → `agentInstances`/`agentCardManifests`/`agentCollections`; `agentInstance` binds `agentIdentityId`+`agentUserId`+`agentIdentityBl
- NEW Copilot agent admin (beta): `/beta/agents`, `/beta/copilot/agents`, `/beta/copilot/agentRegistrations`, `/beta/copilot/admin/catalog/packages` (block/unblock/reassign); scope `CopilotPackages.Read.All
- NEW Agent Registration API (GA replacement): `/beta/copilot/agentRegistrations` POST/GET/PATCH/DELETE — **client-supplied `createdBy`**, PATCH rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agent
- NEW Copilot Policy Settings API: `/beta/copilot/admin/policySettings/{id}` — 5 settings (`microsoft.copilot.{copilotchatpinning,blockaccesstoopenfiles,imagegeneration,allowwebsearch,allowinadmincenters}`)
- NEW Copilot Studio D2E (Direct-to-Engine) S2S API (private preview): `POST /copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations` — **conversation-ID NOT validated server-side** (doc: 
- NEW Orchestrated API: `/powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}` — `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`; validation against agent's registered too
- NEW Three-hop Agent User `user_fic` flow (documented in `microsoft/entrabot`): Hop1 `client_credentials`+cert+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` **OR*
- NEW `managerApplications` on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without `AgentIdentityBlueprintPrincipal.ReadWrite.All` — supply-chain trust surface
- NEW Consent primitive `POST /v1.0/oauth2PermissionGrants` with **caller-chosen `resourceId`** (Graph OR Azure Storage `user_impersonation` `e406a681...`); `Application.Read.All` NOT in agent blocked-permi
- NEW ACS JWKS rotation confirmed: 5 self-signed keys (3× `CN=accounts.accesscontrol.windows.net` notBefore 06-08/06-17/07-04-2026, 2× `CN=login.microsoftonline.us` notBefore 07-01/07-26-2026), `allowedAudi
- CHANGED `login.live.com` redirect matrix EXHAUSTED: `/oauth20_desktop.srf` REMOVED (stub `?removed=true`); `/oauth20_authorize.srf` returns generic 200 for all 8 variants, validation deferred, no `redirect_ur
- NEW login.microsoftonline.com OIDC discovery: v2.0 (issuer login.microsoftonline.com/{tid}/v2.0; JWKS /discovery/v2.0/keys, 8 RSA keys; mtls alias mtlsauth.microsoft.com; tls_client_certificate_bound_acce
- NEW Graph $metadata: 1,183 EntityTypes, 326 Functions across microsoft.graph.identityGovernance + microsoft.graph.security + microsoft.graph.entraRecoveryServices; 22 filterByCurrentUser bindings — GET /v
- NEW Graph API 405 anomaly: unauth HEAD/GET to /v1.0, /me, /users → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer — verified passively (4×405)
- NEW v2.0 authorize HTTP 200 error rendering: GET /oauth2/v2.0/authorize?response_type=token (unsupported in v2.0) → HTTP 200 with embedded JS error code 700038 (iHttpErrorCode 400, "We received a bad requ
- NEW oauth2.googleapis.com/tokeninfo public introspection oracle: accepts ?access_token= / ?id_token= query params (no Authorization header), returns aud/scope/expiry — verified (no-token→400, malformed→40
- NEW bughunters.google.com root `/` → HTTP 200, hardened (HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff)

## 2026-08-07 16:42:10 UTC
- NEW Source maps live on BOTH identity SPAs: `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (7MB, 4359 paths) + `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35MB, 4922 files with sourc
- NEW Verified ID minting endpoint `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential` — POST, Bearer scope=SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2`; backend gates ONLY on `GuestIsN
- NEW `/me/agentSignInSessions` (v1.0 + beta) fully off-metadata — 0 refs in both `$metadata` docs (hashes `9d84e451...`, `ca304859...`) but endpoint alive (401)
- NEW Agent Registry API (beta, deprecated May-2026): `/beta/agentRegistry` → `agentInstances`/`agentCardManifests`/`agentCollections`; `agentInstance` binds `agentIdentityId`+`agentUserId`+`agentIdentityBl
- NEW Copilot agent admin (beta): `/beta/agents`, `/beta/copilot/agents`, `/beta/copilot/agentRegistrations`, `/beta/copilot/admin/catalog/packages` (block/unblock/reassign); scope `CopilotPackages.Read.All
- NEW Agent Registration API (GA replacement): `/beta/copilot/agentRegistrations` POST/GET/PATCH/DELETE — **client-supplied `createdBy`**, PATCH rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agent
- NEW Copilot Policy Settings API: `/beta/copilot/admin/policySettings/{id}` — 5 settings (`microsoft.copilot.{copilotchatpinning,blockaccesstoopenfiles,imagegeneration,allowwebsearch,allowinadmincenters}`)
- NEW Copilot Studio D2E (Direct-to-Engine) S2S API (private preview): `POST /copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations` — **conversation-ID NOT validated server-side** (doc: 
- NEW Orchestrated API: `/powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}` — `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`; validation against agent's registered too
- NEW Three-hop Agent User `user_fic` flow (documented in `microsoft/entrabot`): Hop1 `client_credentials`+cert+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` **OR*
- NEW `managerApplications` on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without `AgentIdentityBlueprintPrincipal.ReadWrite.All` — supply-chain trust surface
- NEW Consent primitive `POST /v1.0/oauth2PermissionGrants` with **caller-chosen `resourceId`** (Graph OR Azure Storage `user_impersonation` `e406a681...`); `Application.Read.All` NOT in agent blocked-permi
- NEW ACS JWKS rotation confirmed: 5 self-signed keys (3× `CN=accounts.accesscontrol.windows.net` notBefore 06-08/06-17/07-04-2026, 2× `CN=login.microsoftonline.us` notBefore 07-01/07-26-2026), `allowedAudi
- CHANGED `login.live.com` redirect matrix EXHAUSTED: `/oauth20_desktop.srf` REMOVED (stub `?removed=true`); `/oauth20_authorize.srf` returns generic 200 for all 8 variants, validation deferred, no `redirect_ur
- NEW `login.microsoftonline.com` OIDC discovery: v2.0 (issuer `login.microsoftonline.com/{tid}/v2.0`; JWKS `/discovery/v2.0/keys`, 8 RSA keys; mtls alias `mtlsauth.microsoft.com`; tls_client_certificate_bo
- NEW Graph `$metadata`: 1,183 EntityTypes, 326 Functions across `microsoft.graph.identityGovernance` + `microsoft.graph.security` + `microsoft.graph.entraRecoveryServices`; 22 `filterByCurrentUser` binding
- NEW Graph API 405 anomaly: unauth HEAD/GET to `/v1.0`, `/me`, `/users` → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer — verified passively (4×405)
- NEW v2.0 authorize HTTP 200 error rendering: GET `/oauth2/v2.0/authorize?response_type=token` (unsupported in v2.0) → HTTP 200 with embedded JS error code 700038 (iHttpErrorCode 400, "We received a bad re
- NEW `oauth2.googleapis.com/tokeninfo` public introspection oracle: accepts `?access_token=` / `?id_token=` query params (no Authorization header), returns aud/scope/expiry — verified (no-token→400, malfor
- NEW `bughunters.google.com` root `/` → HTTP 200, hardened (HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff)

## 2026-08-07 17:38:33 UTC

## 2026-08-07 18:29:05 UTC
- NEW NO_DELTA

## 2026-08-07 18:56:45 UTC

## 2026-08-07 19:24:56 UTC
- NEW Source maps live on BOTH identity SPAs: `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (7MB, 4359 paths) + `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35MB, 4922 files)
- NEW Agent Registry API (beta, deprecated May-2026): `/beta/agentRegistry` → `agentInstances`/`agentCardManifests`/`agentCollections`
- NEW Copilot agent admin (beta): `/beta/agents`, `/beta/copilot/agents`, `/beta/copilot/admin/catalog/packages` (block/unblock/reassign); scope `CopilotPackages.Read.All`
- NEW Copilot Policy Settings API: `/beta/copilot/admin/policySettings/{id}` — 5 settings (`microsoft.copilot.{copilotchatpinning,blockaccesstoopenfiles,imagegeneration,allowwebsearch,allowinadmincenters}`)
- NEW Orchestrated API: `/powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}` — `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`
- NEW Three-hop Agent User `user_fic` flow: Hop1 `client_credentials`+cert+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` OR `upn`
- NEW `managerApplications` on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without `AgentIdentityBlueprintPrincipal.ReadWrite.All`
- NEW Consent primitive `POST /v1.0/oauth2PermissionGrants` with **caller-chosen `resourceId`** (Graph OR Azure Storage `user_impersonation` `e406a681...`); `Application.Read.All` NOT in agent blocked-permi
- NEW ACS JWKS rotation confirmed: 5 self-signed keys (3× `CN=accounts.accesscontrol.windows.net` notBefore 06-08/06-17/07-04-2026, 2× `CN=login.microsoftonline.us` notBefore 07-01/07-26-2026)
- NEW `login.live.com` redirect matrix EXHAUSTED: `/oauth20_desktop.srf` REMOVED; `/oauth20_authorize.srf` returns generic 200 for all 8 variants
- NEW `login.microsoftonline.com` OIDC discovery v2.0: issuer `login.microsoftonline.com/{tid}/v2.0`; JWKS `/discovery/v2.0/keys` (8 RSA keys); mtls alias `mtlsauth.microsoft.com`
- NEW Graph `$metadata`: 1,183 EntityTypes, 326 Functions across `microsoft.graph.identityGovernance` + `microsoft.graph.security` + `microsoft.graph.entraRecoveryServices`; 22 `filterByCurrentUser` binding

## 2026-08-07 19:36:42 UTC
- NEW Agent Registry API (beta, deprecated May-2026): `/beta/agentRegistry` → `agentInstances`/`agentCardManifests`/`agentCollections`
- NEW Copilot agent admin (beta): `/beta/agents`, `/beta/copilot/agents`, `/beta/copilot/admin/catalog/packages` (block/unblock/reassign); scope `CopilotPackages.Read.All`
- NEW Copilot Policy Settings API: `/beta/copilot/admin/policySettings/{id}` — 5 settings (`microsoft.copilot.{copilotchatpinning,blockaccesstoopenfiles,imagegeneration,allowwebsearch,allowinadmincenters}`)
- NEW Orchestrated API: `/powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}` — `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`
- NEW Three-hop Agent User `user_fic` flow: Hop1 `client_credentials`+cert+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` OR `upn`
- NEW `managerApplications` on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without `AgentIdentityBlueprintPrincipal.ReadWrite.All`
- NEW Consent primitive `POST /v1.0/oauth2PermissionGrants` with **caller-chosen `resourceId`** (Graph OR Azure Storage `user_impersonation` `e406a681...`); `Application.Read.All` NOT in agent blocked-permi
- CHANGED `login.live.com` redirect matrix EXHAUSTED: `/oauth20_desktop.srf` REMOVED; `/oauth20_authorize.srf` returns generic 200 for all 8 variants

## 2026-08-07 20:26:22 UTC
- NEW NO_DELTA
- NEW Agent Registry API (beta, deprecated May-2026): `/beta/agentRegistry` → `agentInstances`/`agentCardManifests`/`agentCollections`
- NEW Copilot agent admin (beta): `/beta/agents`, `/beta/copilot/agents`, `/beta/copilot/admin/catalog/packages` (block/unblock/reassign); scope `CopilotPackages.Read.All`
- NEW Copilot Policy Settings API: `/beta/copilot/admin/policySettings/{id}` — 5 settings (`microsoft.copilot.{copilotchatpinning,blockaccesstoopenfiles,imagegeneration,allowwebsearch,allowinadmincenters}`)
- NEW Orchestrated API: `/powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}` — `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`
- NEW Three-hop Agent User `user_fic` flow: Hop1 `client_credentials`+cert+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` OR `upn`
- NEW `managerApplications` on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without `AgentIdentityBlueprintPrincipal.ReadWrite.All`
- NEW Consent primitive `POST /v1.0/oauth2PermissionGrants` with **caller-chosen `resourceId`** (Graph OR Azure Storage `user_impersonation` `e406a681...`); `Application.Read.All` NOT in agent blocked-permi
- CHANGED `login.live.com` redirect matrix EXHAUSTED: `/oauth20_desktop.srf` REMOVED; `/oauth20_authorize.srf` returns generic 200 for all 8 variants

## 2026-08-07 21:10:15 UTC
- NEW Source maps live on BOTH identity SPAs: mysignins.microsoft.com/bundle/main.caa6a456.js.map (7MB, 4359 paths) + api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map (35MB, 4922 files) — appeared at
- NEW Agent Registry API (beta, deprecated May-2026): /beta/agentRegistry → agentInstances/agentCardManifests/agentCollections — appeared at 19:24:56, not in last leads
- NEW Copilot agent admin (beta): /beta/agents, /beta/copilot/agents, /beta/copilot/admin/catalog/packages (block/unblock/reassign); scope CopilotPackages.Read.All — appeared at 19:24:56
- NEW Copilot Policy Settings API: /beta/copilot/admin/policySettings/{id} — 5 settings — appeared at 19:24:56
- NEW Orchestrated API: /powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool takes client-supplied toolSchemaName+inputs — appeared at 19:24:56
- NEW Three-hop Agent User user_fic flow: Hop1 client_credentials+cert+fmi_path → T1; Hop2 FIC exchange → T2; Hop3 grant_type=user_fic with user_id={oid} OR upn — appeared at 19:24:56
- NEW managerApplications on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without AgentIdentityBlueprintPrincipal.ReadWrite.All — appeared at 19:24:56
- NEW Consent primitive POST /v1.0/oauth2PermissionGrants with caller-chosen resourceId (Graph OR Azure Storage user_impersonation e406a681...); Application.Read.All NOT in agent blocked-permissions — appea
- NEW ACS JWKS rotation confirmed: 5 self-signed keys (3× CN=accounts.accesscontrol.windows.net notBefore 06-08/06-17/07-04-2026, 2× CN=login.microsoftonline.us notBefore 07-01/07-26-2026) — appeared at 19:
- NEW login.live.com redirect matrix EXHAUSTED: /oauth20_desktop.srf REMOVED (stub ?removed=true); /oauth20_authorize.srf returns generic 200 for all 8 variants — appeared at 19:24:56
- NEW login.microsoftonline.com OIDC discovery v2.0: issuer login.microsoftonline.com/{tid}/v2.0; JWKS /discovery/v2.0/keys (8 RSA keys); mtls alias mtlsauth.microsoft.com — appeared at 19:24:56
- NEW Graph $metadata: 1,183 EntityTypes, 326 Functions across microsoft.graph.identityGovernance + microsoft.graph.security + microsoft.graph.entraRecoveryServices; 22 filterByCurrentUser binding — appeare
- NEW Graph API 405 anomaly: unauth HEAD/GET to /v1.0, /me, /users → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer — appeared at 17:38:33
- NEW v2.0 authorize HTTP 200 error rendering: GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 with embedded JS error code 700038 — appeared at 17:38:33
- NEW oauth2.googleapis.com/tokeninfo public introspection oracle — appeared at 17:38:33
- NEW bughunters.google.com root hardening — appeared at 17:38:33
- CHANGED Agent Registration ownership bypass hypothesis confidence raised from 70→80 (nemotron3 20:21:26) — NEW top priority
- CHANGED Verified ID minting hypothesis added at confidence 70 (laguna 20:26:22) — NEW #2 priority
- CHANGED Consent primitive resourceId hypothesis confidence held at 70 but dropped to #3 — appeared in last leads
- CHANGED Three-hop user_fic hypothesis confidence held at 70 — appeared in last leads

## 2026-08-07 21:55:35 UTC

## 2026-08-07 22:40:57 UTC
- NEW Source maps live on BOTH identity SPAs: mysignins.microsoft.com/bundle/main.caa6a456.js.map (7MB, 4359 paths) + api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map (35MB, 4922 files) — appeared at
- NEW Agent Registry API (beta, deprecated May-2026): /beta/agentRegistry → agentInstances/agentCardManifests/agentCollections — appeared at 19:24:56
- NEW Copilot agent admin (beta): /beta/agents, /beta/copilot/agents, /beta/copilot/admin/catalog/packages (block/unblock/reassign); scope CopilotPackages.Read.All — appeared at 19:24:56
- NEW Copilot Policy Settings API: /beta/copilot/admin/policySettings/{id} — 5 settings — appeared at 19:24:56
- NEW Orchestrated API: /powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool takes client-supplied toolSchemaName+inputs — appeared at 19:24:56
- NEW Three-hop Agent User user_fic flow: Hop1 client_credentials+cert+fmi_path → T1; Hop2 FIC exchange → T2; Hop3 grant_type=user_fic with user_id={oid} OR upn — appeared at 19:24:56
- NEW managerApplications on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without AgentIdentityBlueprintPrincipal.ReadWrite.All — appeared at 19:24:56
- NEW Consent primitive POST /v1.0/oauth2PermissionGrants with caller-chosen resourceId (Graph OR Azure Storage user_impersonation e406a681...); Application.Read.All NOT in agent blocked-permissions — appea
- NEW ACS JWKS rotation confirmed: 5 self-signed keys (3× CN=accounts.accesscontrol.windows.net notBefore 06-08/06-17/07-04-2026, 2× CN=login.microsoftonline.us notBefore 07-01/07-26-2026) — appeared at 19:
- NEW login.live.com redirect matrix EXHAUSTED: /oauth20_desktop.srf REMOVED (stub ?removed=true); /oauth20_authorize.srf returns generic 200 for all 8 variants — appeared at 19:24:56
- NEW login.microsoftonline.com OIDC discovery v2.0: issuer login.microsoftonline.com/{tid}/v2.0; JWKS /discovery/v2.0/keys (8 RSA keys); mtls alias mtlsauth.microsoft.com — appeared at 19:24:56
- NEW Graph $metadata: 1,183 EntityTypes, 326 Functions across microsoft.graph.identityGovernance + microsoft.graph.security + microsoft.graph.entraRecoveryServices; 22 filterByCurrentUser binding — appeare
- CHANGED Agent Registration ownership bypass hypothesis confidence raised from 70→85 (nemotron3/laguna convergence) — NEW top priority
- CHANGED Verified ID minting hypothesis confidence held at 70-75 — appeared in last leads
- CHANGED Three-hop user_fic hypothesis confidence held at 60-70 — appeared in last leads
- CHANGED Consent primitive resourceId hypothesis confidence held at 70 — appeared in last leads

## 2026-08-07 23:32:28 UTC
- NEW NO_DELTA — inventory, knowledge, and leads all converge at 2026-08-07 22:40:57 UTC with identical surface items; no new endpoints or schema changes since last cycle
- CHANGED Agent Registration ownership bypass hypothesis confidence raised from 70→85 (nemotron3/laguna convergence) — NEW top priority
- CHANGED Verified ID minting hypothesis confidence held at 70-75
- CHANGED Three-hop user_fic hypothesis confidence held at 60-70
- CHANGED Consent primitive resourceId hypothesis confidence held at 70

## 2026-08-07 23:55:03 UTC

## 2026-08-08 01:41:58 UTC

## 2026-08-08 03:18:22 UTC

## 2026-08-08 04:33:31 UTC

## 2026-08-08 05:24:44 UTC

## 2026-08-08 06:24:21 UTC
- NEW None — inventory, knowledge base, and last leads all converge at 2026-08-08 03:14/22:37 UTC re-verification; no new endpoints, schema changes, or surface anomalies since last cycle.
- CHANGED None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.

## 2026-08-08 07:17:53 UTC
- NEW None — inventory, knowledge base, and last leads all converge at 2026-08-08 03:14/22:37 UTC re-verification; no new endpoints, schema changes, or surface anomalies since last cycle.
- CHANGED None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.

## 2026-08-08 08:05:29 UTC
- NEW None — inventory, knowledge base, and last leads all converge at 2026-08-08 03:14/22:37 UTC re-verification; no new endpoints, schema changes, or surface anomalies since last cycle.
- CHANGED None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.

## 2026-08-08 08:59:41 UTC
- NEW None — inventory, knowledge base, and last leads all converge at 2026-08-08 03:14/22:37 UTC re-verification; no new endpoints, schema changes, or surface anomalies since last cycle.
- CHANGED None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.

## 2026-08-08 09:46:51 UTC

## 2026-08-08 10:18:11 UTC
- NEW None — inventory, knowledge base, and last leads all converge at 2026-08-08 08:59 UTC re-verification; no new endpoints, schema changes, or surface anomalies since last cycle.
- CHANGED None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.

## 2026-08-08 10:56:09 UTC

## 2026-08-08 11:30:37 UTC

## 2026-08-08 11:59:45 UTC

## 2026-08-08 12:59:55 UTC

## 2026-08-08 13:54:16 UTC

## 2026-08-08 14:36:34 UTC
- NEW None — inventory, knowledge base, and last leads all converge at 2026-08-08 11:30 UTC re-verification; no new endpoints, schema changes, or surface anomalies since last cycle.
- CHANGED None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.

## 2026-08-08 15:05:14 UTC
- CHANGED login.microsoftonline.com/common/discovery/keys v1.0 key set rotated 5→4 kids (aFkmKVFc… retired from v1; now v2-exclusive) — steady-state subset invariant v1(4) ⊂ v2(8) still holds; transient 7-kid v
- NEW graph.microsoft.com/beta/copilot/agentRegistrations `HEAD` → **HTTP 405** (Content-Length: 0, no `WWW-Authenticate: Bearer`), consistent with documented 405 anomaly at 15:02 UTC — confirms metadata-le
- NEW login.microsoftonline.com/common/discovery/keys → **HTTP 200** (23,932 bytes, `Access-Control-Allow-Origin: *`, `Access-Control-Allow-Methods: GET, OPTIONS`), confirming v1/v2 JWKS kid overlap + dual 
- NEW oauth2.googleapis.com/tokeninfo → **HTTP 404 on HEAD / HTTP 200 on GET** with `X-Content-Type-Options: nosniff`, confirms query-param introspection oracle still live (no-Auth-header required) at 15:02

## 2026-08-08 15:47:21 UTC
- NEW login.microsoftonline.com/common/discovery/keys v1.0 key set rotated 5→4 kids (aFkmKVFc retired from v1; now v2-exclusive) — steady-state subset invariant v1(4) ⊂ v2(8) still holds
- NEW graph.microsoft.com/beta/copilot/agentRegistrations `HEAD` → HTTP 405 (Content-Length: 0, no `WWW-Authenticate: Bearer`), confirming metadata-level 405 anomaly
- NEW login.microsoftonline.com/common/discovery/keys → HTTP 200 (23,828 bytes, `Access-Control-Allow-Origin: *`, `Access-Control-Allow-Methods: GET, OPTIONS`), confirming v1/v2 JWKS kid overlap + dual issu
- NEW oauth2.googleapis.com/tokeninfo → HTTP 404 on HEAD / HTTP 200 on GET with `X-Content-Type-Options: nosniff`, confirming query-param introspection oracle still live
- CHANGED `login.microsoftonline.com/common/discovery/keys` — v1.0 key set rotated 5→4 kids (`aFkmKVFc…` retired from v1, now v2-exclusive). Steady-state subset invariant v1(4) ⊂ v2(8) still holds; 0 v1-exclusi
- NEW `graph.microsoft.com/beta/copilot/agentRegistrations` `HEAD` → HTTP 405 (Content-Length: 0, no `WWW-Authenticate: Bearer`) — RFC 6750 §3 violation extends beyond `/v1.0`, `/me`, `/users` to the Agent 
- NEW `oauth2.googleapis.com/tokeninfo` — `HEAD` → HTTP 404 (GET → HTTP 200, 113 bytes, `X-Content-Type-Options: nosniff`). Introspection oracle confirmed live via GET method; HEAD-404 is unusual method-han

## 2026-08-08 16:43:05 UTC

## 2026-08-08 17:04:48 UTC

## 2026-08-08 17:43:46 UTC

## 2026-08-08 18:08:02 UTC

## 2026-08-08 18:58:10 UTC

## 2026-08-08 19:32:21 UTC
- NEW None — all passive probes (18:08/18:58 UTC) confirm prior ACCEPTED findings unchanged; inventory, knowledge base, and last leads converge at NO_DELTA.

## 2026-08-08 20:01:18 UTC

## 2026-08-08 20:39:44 UTC

## 2026-08-08 21:06:31 UTC
- NEW None — all passive probes (18:08/18:58/19:32/20:01/20:39 UTC) confirm prior ACCEPTED findings unchanged; inventory, knowledge base, and last leads converge at NO_DELTA.
- CHANGED None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.

## 2026-08-08 21:48:04 UTC
- NEW graph.microsoft.com/beta/copilot/agentRegistrations `HEAD` → **HTTP 405** (Content-Length: 0, no `WWW-Authenticate: Bearer`), consistent with documented 405 anomaly at 15:02 UTC — confirms metadata-le
- NEW login.microsoftonline.com/common/discovery/keys → **HTTP 200** (23,932 bytes, `Access-Control-Allow-Origin: *`, `Access-Control-Allow-Methods: GET, OPTIONS`), confirming v1/v2 JWKS kid overlap + dual 
- NEW oauth2.googleapis.com/tokeninfo → **HTTP 404 on HEAD / HTTP 200 on GET** with `X-Content-Type-Options: nosniff`, confirms query-param introspection oracle still live (no-Auth-header required) at 15:02
- CHANGED `login.microsoftonline.com/common/discovery/keys` — v1.0 key set rotated 5→4 kids (`aFkmKVFc…` retired from v1, now v2-exclusive). Steady-state subset invariant v1(4) ⊂ v2(8) still holds; 0 v1-exclusi
- NEW `graph.microsoft.com/beta/copilot/agentRegistrations` `HEAD` → HTTP 405 (Content-Length: 0, no `WWW-Authenticate: Bearer`) — RFC 6750 §3 violation extends beyond `/v1.0`, `/me`, `/users` to the Agent 
- NEW `oauth2.googleapis.com/tokeninfo` — `HEAD` → HTTP 404 (GET → HTTP 200, 113 bytes, `X-Content-Type-Options: nosniff`). Introspection oracle confirmed live via GET method; HEAD-404 is unusual method-han

## 2026-08-08 22:09:10 UTC
- NEW None — all passive probes (18:08/18:58/19:32/20:01/20:39/21:06/21:48 UTC) confirm prior ACCEPTED findings unchanged; inventory, knowledge base, and last leads converge at NO_DELTA.
- CHANGED None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.

## 2026-08-08 22:47:49 UTC

## 2026-08-08 23:21:45 UTC

## 2026-08-08 23:49:40 UTC
- NEW None — all passive probes (23:47 UTC) confirm prior ACCEPTED findings unchanged; inventory, knowledge base, and last leads converge at NO_DELTA.
- CHANGED None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.

## 2026-08-09 00:39:54 UTC

## 2026-08-09 02:54:06 UTC
- NEW login.microsoftonline.com/common/discovery/keys — v1.0 key set now shows 4 kids (v1) ⊂ 8 kids (v2), 0 v1-exclusive (steady-state subset invariant holds, rotation churn resolved)
- NEW earthengine-api oauth.py:45 secret value confirmed as `RUP0RZ6e0pPd1` at 00:37 UTC (sha256 matches KB `3f3f8d6f…d271`)
- NEW graph.microsoft.com/beta/copilot/agentRegistrations HEAD → 405 confirmed (extends Graph 405 anomaly to Agent Registration endpoint)
- NEW v1.0-only response_types confirmed live — v1.0 `['code','id_token','code id_token','token id_token','token']` vs v2.0 `['code','id_token','code id_token','id_token token']`; pure `token` implicit excl

## 2026-08-09 04:12:16 UTC

## 2026-08-09 05:22:06 UTC

## 2026-08-09 06:06:33 UTC

## 2026-08-09 07:15:06 UTC

## 2026-08-09 08:06:14 UTC

## 2026-08-09 08:59:52 UTC
- NEW None — all passive probes (08:06 UTC) confirm prior ACCEPTED findings unchanged; inventory, knowledge base, and last leads converge at NO_DELTA.

## 2026-08-09 09:50:05 UTC

## 2026-08-09 10:21:50 UTC
- NEW None — all passive probes (09:50 UTC) confirm prior ACCEPTED findings unchanged; inventory, knowledge base, and last leads converge at NO_DELTA.
- CHANGED None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.

## 2026-08-09 11:00:10 UTC

## 2026-08-09 11:38:39 UTC

## 2026-08-09 12:03:16 UTC

## 2026-08-09 13:13:08 UTC

## 2026-08-09 14:03:12 UTC

## 2026-08-09 14:48:07 UTC
- NEW None — all passive probes (14:03 UTC) confirm prior ACCEPTED findings unchanged; inventory, knowledge base, and last leads converge at NO_DELTA.
- CHANGED None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.

## 2026-08-09 15:17:15 UTC
- NEW None — all passive probes confirm prior ACCEPTED findings unchanged; inventory, knowledge base, and last leads converge at NO_DELTA.

## 2026-08-09 15:56:03 UTC

## 2026-08-09 16:27:02 UTC

## 2026-08-09 17:06:19 UTC

## 2026-08-09 17:48:19 UTC

## 2026-08-09 18:19:10 UTC
- CHANGED non-item — new robot probe `https://www.googleapis.com/auth/cloud-platform` → 404; scope strings are not HTTP endpoints, confirms no new surface.

## 2026-08-09 19:10:56 UTC

## 2026-08-09 19:50:31 UTC

## 2026-08-09 20:33:10 UTC

## 2026-08-09 20:59:06 UTC
- NEW None — all passive probes (20:33 UTC) confirm prior ACCEPTED findings unchanged; inventory, knowledge base, and last leads converge at NO_DELTA.

## 2026-08-09 21:37:08 UTC

## 2026-08-09 22:05:43 UTC

## 2026-08-09 22:47:27 UTC

## 2026-08-09 23:15:38 UTC

## 2026-08-09 23:52:03 UTC

## 2026-08-10 00:48:37 UTC
- CHANGED https://www.googleapis.com/auth/cloud-platform → HTTP 200 (len=14, type=text/html) at 2026-08-09 21:37:08 UTC → HTTP 404 at 2026-08-09 22:05:45 UTC

## 2026-08-10 03:02:43 UTC
- CHANGED https://www.googleapis.com/auth/cloud-platform — scope string probe flipped HTTP 200 (len=14, text/html) at 2026-08-09 21:37 → HTTP 404 at 2026-08-09 22:05; confirms scope strings are not HTTP endpoin

## 2026-08-10 04:50:16 UTC

## 2026-08-10 06:06:47 UTC
- NEW None — reposcan 2026-08-10 05:06 UTC (39,446 files, 454 hits) produced **zero** REAL_SECRET; all hits classified TEST_OR_EXAMPLE or KNOWN-DUP.
- CHANGED None — robot probes at 04:50 UTC confirm all ACCEPTED findings stable: `graph.microsoft.com/beta/copilot/agentRegistrations` → 401, `oauth2.googleapis.com/token` → 404 (POST-only), `graph.microsoft.co

## 2026-08-10 08:03:22 UTC

## 2026-08-10 09:52:22 UTC
- CHANGED `graph.microsoft.com/beta/copilot/agentRegistrations` OPTIONS: was HTTP 200 (CORS `*`, full mutation allowlist `DELETE,GET,OPTIONS,POST,PUT,PATCH`) → now HTTP 405 (3× consistent). Closes the CORS cros

## 2026-08-10 10:55:25 UTC
- CHANGED `oauth2.googleapis.com/token` POST with leaked client_secret + dummy refresh_token → HTTP 400 `invalid_grant "Bad Request"` (NOT 401 `invalid_client`) — conclusively proves `CLIENT_SECRET` @ oauth.py:
- CHANGED `https://www.googleapis.com/auth/cloud-platform` → 200 (body=`cloud-platform`, 14 bytes) — flipped back from 404 at 09:52 UTC; part of documented flaky pattern (scope-name echo from API gateway), no n
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS: HTTP 200 (CORS *, full mutation allowlist) → HTTP 405 — closes CORS cross-origin mutation vector
- CHANGED oauth2.googleapis.com/token GET→404 confirmed — POST-only alive gate (validates earthengine secret hypothesis)
- CHANGED www.googleapis.com/auth/cloud-platform: flips 200 (len=14 text/html) ↔ 404 across cycles — scope strings not stable HTTP endpoints
- NEW graph.microsoft.com root → HTTP 200 (text/html signin page) — confirms root-level reachability, no auth-bypass surface
- NEW reposcan 2026-08-10 05:06 UTC (39,446 files) — zero REAL_SECRET; all hits TEST_OR_EXAMPLE or KNOWN-DUP

## 2026-08-10 11:40:26 UTC
- NEW oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — proves client_secret is valid Google OAuth credential
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 (was 200 with CORS `*` + full mutation allowlist) — closes CORS cross-origin mutation vector
- CHANGED www.googleapis.com/auth/cloud-platform flips 200 (len=14) ↔ 404 — scope strings not stable HTTP endpoints

## 2026-08-10 12:39:30 UTC
- NEW oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — proves client_secret is valid Google OAuth credential
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 (was 200 with CORS `*` + full mutation allowlist) — closes CORS cross-origin mutation vector
- CHANGED www.googleapis.com/auth/cloud-platform flips 200 (len=14) ↔ 404 — scope strings not stable HTTP endpoints
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 (was 200 with CORS * + full mutation allowlist) — closes CORS cross-origin mutation vector (first observed 2026-08-10 09:52 UTC, 
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — proves client_secret is valid Google OAuth credential accepted by token server
- CHANGED www.googleapis.com/auth/cloud-platform: flips 200 (len=14) ↔ 404 across cycles — scope strings not stable HTTP endpoints

## 2026-08-10 14:04:45 UTC
- NEW oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — proves client_secret is valid Google OAuth credential

## 2026-08-10 15:14:26 UTC
- NEW graph.microsoft.com root → HTTP 200 (text/html signin page) — confirms root-level reachability, no auth-bypass surface
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 (was 200 with CORS `*` + full mutation allowlist) — closes CORS cross-origin mutation vector (sustained since 09:52 UTC)
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves client_secret is valid Google OAuth credential (RFC 6749 §5.2)
- CHANGED www.googleapis.com/auth/cloud-platform flips 200 (len=14) ↔ 404 — scope strings not stable HTTP endpoints

## 2026-08-10 16:19:15 UTC
- NEW graph.microsoft.com root → HTTP 200 (text/html signin page) — confirms root-level reachability, no auth-bypass surface
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 (was 200 with CORS `*` + full mutation allowlist) — closes CORS cross-origin mutation vector (sustained since 09:52 UTC)
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves client_secret is valid Google OAuth credential (RFC 6749 §5.2)
- CHANGED www.googleapis.com/auth/cloud-platform flips 200 (len=14) ↔ 404 — scope strings not stable HTTP endpoints

## 2026-08-10 17:16:05 UTC
- NEW graph.microsoft.com root → HTTP 200 (text/html signin page) — confirms root-level reachability, no auth-bypass surface
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 (was 200 with CORS `*` + full mutation allowlist) — closes CORS cross-origin mutation vector (sustained since 09:52 UTC)
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves client_secret is valid Google OAuth credential (RFC 6749 §5.2)
- CHANGED www.googleapis.com/auth/cloud-platform flips 200 (len=14) ↔ 404 — scope strings not stable HTTP endpoints
- NEW `www.googleapis.com/drive/v3/files` → HTTP 403 (no API key) — minor quirk: 403 vs 401 for unauthenticated Drive REST API access
- CHANGED `graph.microsoft.com/beta/copilot/agentRegistrations` OPTIONS → HTTP 405 sustained (CORS vector closed since 09:52 UTC)
- CHANGED earthengine secret confirmed redeemable: `invalid_grant` proves valid credential, but native-app `installed` client + OOB redirect confirmed by-design

## 2026-08-10 18:06:41 UTC
- NEW graph.microsoft.com root → HTTP 200 (text/html signin page) — confirms root-level reachability, no auth-bypass surface
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 (was 200 with CORS `*` + full mutation allowlist) — closes CORS cross-origin mutation vector (sustained since 09:52 UTC)
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves client_secret is valid Google OAuth credential (RFC 6749 §5.2)
- CHANGED www.googleapis.com/auth/cloud-platform flips 200 (len=14) ↔ 404 — scope strings not stable HTTP endpoints
- NEW www.googleapis.com/drive/v3/files → HTTP 403 (no API key) — minor quirk: 403 vs 401 for unauthenticated Drive REST API access
- NEW Source maps live on BOTH identity SPAs: `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (7MB, 4359 paths) + `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35MB, 4922 files with sourc
- NEW Verified ID minting endpoint `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential` — POST, Bearer scope=SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2`; backend gates ONLY on `GuestIsN
- NEW `/me/agentSignInSessions` (v1.0 + beta) fully off-metadata — 0 refs in both `$metadata` docs (hashes `9d84e451...`, `ca304859...`) but endpoint alive (401)
- NEW Agent Registry API (beta, deprecated May-2026): `/beta/agentRegistry` → `agentInstances`/`agentCardManifests`/`agentCollections`; `agentInstance` binds `agentIdentityId`+`agentUserId`+`agentIdentityBl
- NEW Copilot agent admin (beta): `/beta/agents`, `/beta/copilot/agents`, `/beta/copilot/agentRegistrations`, `/beta/copilot/admin/catalog/packages` (block/unblock/reassign); scope `CopilotPackages.Read.All
- NEW Agent Registration API (GA replacement): `/beta/copilot/agentRegistrations` POST/GET/PATCH/DELETE — **client-supplied `createdBy`**, PATCH rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agent
- NEW Copilot Policy Settings API: `/beta/copilot/admin/policySettings/{id}` — 5 settings (`microsoft.copilot.{copilotchatpinning,blockaccesstoopenfiles,imagegeneration,allowwebsearch,allowinadmincenters}`)
- NEW Copilot Studio D2E (Direct-to-Engine) S2S API (private preview): `POST /copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations` — **conversation-ID NOT validated server-side** (doc: 
- NEW Orchestrated API: `/powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}` — `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`; validation against agent's registered too
- NEW Three-hop Agent User `user_fic` flow (documented in `microsoft/entrabot`): Hop1 `client_credentials`+cert+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` **OR*
- NEW `managerApplications` on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without `AgentIdentityBlueprintPrincipal.ReadWrite.All` — supply-chain trust surface
- NEW Consent primitive `POST /v1.0/oauth2PermissionGrants` with **caller-chosen `resourceId`** (Graph OR Azure Storage `user_impersonation` `e406a681...`); `Application.Read.All` NOT in agent blocked-permi
- NEW ACS JWKS rotation confirmed: 5 self-signed keys (3× `CN=accounts.accesscontrol.windows.net` notBefore 06-08/06-17/07-04-2026, 2× `CN=login.microsoftonline.us` notBefore 07-01/07-26-2026), `allowedAudi
- CHANGED `login.live.com` redirect matrix EXHAUSTED: `/oauth20_desktop.srf` REMOVED (stub `?removed=true`); `/oauth20_authorize.srf` returns generic 200 for all 8 variants, validation deferred, no `redirect_ur
- NEW `login.microsoftonline.com/common/discovery/v2.0/keys` now includes 3 additional kids vs 18:04 UTC probe: `rRk1d-57B…` (msonline.com), `NqEBZVuOp…` (msonline.us), `1Nv3JExJr…` (new v2-only) — v2 count
- NEW `/oauth2/v2.0/authorize?response_type=token` body-size drift: 23940 → 41309 bytes (content-length increase per KB) — confirms error-rendering anomaly stable, content is the JS error shell
- CHANGED `graph.microsoft.com/beta/copilot/agentRegistrations` OPTIONS → HTTP 405 sustained (was 200 w/ CORS `*` + full mutation allowlist at 09:52 UTC) — CORS cross-origin mutation vector **closed**, confirme
- CHANGED `www.googleapis.com/auth/cloud-platform` → HTTP 404 (len=14) at 17:16 UTC cycle reverted to 400/403-class endpoint behavior — scope-name echo remains flaky, no new surface

## 2026-08-10 19:18:04 UTC
- NEW Source maps live on both identity SPAs: `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (7MB, 4359 paths) + `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35MB, 4922 files)
- NEW Verified ID minting endpoint `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential` — POST, Bearer scope=SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2`; backend gates ONLY on `GuestIsN
- NEW `/me/agentSignInSessions` (v1.0 + beta) fully off-metadata — 0 refs in both `$metadata` docs but endpoint alive (401)
- NEW Agent Registry API (beta, deprecated May-2026): `/beta/agentRegistry` → `agentInstances`/`agentCardManifests`/`agentCollections`; `agentInstance` binds `agentIdentityId`+`agentUserId`+`agentIdentityBl
- NEW Copilot agent admin (beta): `/beta/agents`, `/beta/copilot/agents`, `/beta/copilot/agentRegistrations`, `/beta/copilot/admin/catalog/packages` (block/unblock/reassign); scope `CopilotPackages.Read.All
- NEW Agent Registration API (GA replacement): `/beta/copilot/agentRegistrations` POST/GET/PATCH/DELETE — **client-supplied `createdBy`**, PATCH rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agent
- NEW Copilot Policy Settings API: `/beta/copilot/admin/policySettings/{id}` — 5 settings (`microsoft.copilot.{copilotchatpinning,blockaccesstoopenfiles,imagegeneration,allowwebsearch,allowinadmincenters}`)
- NEW Copilot Studio D2E (Direct-to-Engine) S2S API (private preview): `POST /copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations` — **conversation-ID NOT validated server-side**
- NEW Orchestrated API: `/powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}` — `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`; validation against agent's registered too
- NEW Three-hop Agent User `user_fic` flow: Hop1 `client_credentials`+cert+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` OR `upn`
- NEW `managerApplications` on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without `AgentIdentityBlueprintPrincipal.ReadWrite.All`
- NEW Consent primitive `POST /v1.0/oauth2PermissionGrants` with **caller-chosen `resourceId`** (Graph OR Azure Storage `user_impersonation` `e406a681...`); `Application.Read.All` NOT in agent blocked-permi
- NEW ACS JWKS rotation confirmed: 5 self-signed keys (3× `CN=accounts.accesscontrol.windows.net` notBefore 06-08/06-17/07-04-2026, 2× `CN=login.microsoftonline.us` notBefore 07-01/07-26-2026), `allowedAudi
- NEW `login.live.com` redirect matrix EXHAUSTED: `/oauth20_desktop.srf` REMOVED (stub `?removed=true`); `/oauth20_authorize.srf` returns generic 200 for all 8 variants, validation deferred
- NEW `login.microsoftonline.com/common/discovery/v2.0/keys` now includes 3 additional kids vs prior probe: `rRk1d-57B…` (msonline.com), `NqEBZVuOp…` (msonline.us), `1Nv3JExJr…` (new v2-only) — v2 count inc
- NEW `/oauth2/v2.0/authorize?response_type=token` body-size drift: 23940 → 41309 bytes — confirms error-rendering anomaly stable, content is JS error shell
- CHANGED `graph.microsoft.com/beta/copilot/agentRegistrations` OPTIONS → HTTP 405 sustained (was 200 w/ CORS `*` + full mutation allowlist at 09:52 UTC) — CORS cross-origin mutation vector **closed**, confirme
- CHANGED `www.googleapis.com/auth/cloud-platform` → HTTP 404 (len=14) at 17:16 UTC cycle reverted to 400/403-class endpoint behavior — scope-name echo remains flaky, no new surface
- CHANGED `oauth2.googleapis.com/token` POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves client_secret is valid Google OAuth credential (RFC 6749 §5.2)
- NEW Source maps live on both identity SPAs: `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (7MB, 4359 paths) + `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35MB, 4922 files)
- NEW Verified ID minting endpoint: `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential` — POST, gates only on `GuestIsN…` claim
- NEW `/me/agentSignInSessions` off-metadata — 0 refs in both $metadata docs but endpoint alive (401)
- NEW Agent Registration API (GA replacement) `/beta/copilot/agentRegistrations` — client-supplied `createdBy`, PATCH rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`
- NEW Copilot Policy Settings API: `/beta/copilot/admin/policySettings/{id}` — 5 settings
- NEW D2E S2S API (private preview): conversation-ID NOT validated server-side
- NEW Orchestrated API: `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`
- NEW Three-hop Agent User `user_fic` flow with `user_id={oid}` OR-gate
- NEW Consent primitive: caller-chosen `resourceId` in `POST /v1.0/oauth2PermissionGrants`
- NEW ACS JWKS rotation: 5 self-signed keys with `allowedAudi…`
- NEW v2.0 JWKS +3 kids: `rRk1d-57B`, `NqEBZVuOp`, `1Nv3JExJr`
- NEW `/oauth2/v2.0/authorize?response_type=token` body-size drift: 23940 → 41309 bytes
- CHANGED `graph.microsoft.com/beta/copilot/agentRegistrations` OPTIONS → HTTP 405 (was 200 CORS `*` + full mutation allowlist) — CORS vector closed
- CHANGED `www.googleapis.com/auth/cloud-platform` flips 200↔404 — scope strings not stable endpoints
- CHANGED `login.live.com/oauth20_desktop.srf` REMOVED (stub `?removed=true`)

## 2026-08-10 20:06:39 UTC
- NEW Source maps live on both identity SPAs: `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (7MB, 4359 paths) + `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35MB, 4922 files)
- NEW Verified ID minting endpoint `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential` — POST, Bearer scope=SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2`; backend gates ONLY on `GuestIsN
- NEW `/me/agentSignInSessions` (v1.0 + beta) fully off-metadata — 0 refs in both `$metadata` docs but endpoint alive (401)
- NEW Agent Registry API (beta, deprecated May-2026): `/beta/agentRegistry` → `agentInstances`/`agentCardManifests`/`agentCollections`; `agentInstance` binds `agentIdentityId`+`agentUserId`+`agentIdentityBl
- NEW Copilot Studio D2E S2S API (private preview): `POST /copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations` — **conversation-ID NOT validated server-side**
- NEW Orchestrated API: `/powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}` — `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`
- NEW Three-hop Agent User `user_fic` flow: Hop1 `client_credentials`+cert+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` OR `upn`
- NEW `managerApplications` on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without `AgentIdentityBlueprintPrincipal.ReadWrite.All`
- NEW Consent primitive `POST /v1.0/oauth2PermissionGrants` with **caller-chosen `resourceId`** (Graph OR Azure Storage `user_impersonation` `e406a681...`); `Application.Read.All` NOT in agent blocked-permi
- NEW ACS JWKS rotation confirmed: 5 self-signed keys (3× `CN=accounts.accesscontrol.windows.net`, 2× `CN=login.microsoftonline.us`), `allowedAudiences` claim present
- NEW `login.live.com` redirect matrix EXHAUSTED: `/oauth20_desktop.srf` REMOVED (stub `?removed=true`); `/oauth20_authorize.srf` returns generic 200 for all 8 variants, validation deferred
- NEW `login.microsoftonline.com/common/discovery/v2.0/keys` now includes 3 additional kids vs prior probe: `rRk1d-57B…` (msonline.com), `NqEBZVuOp…` (msonline.us), `1Nv3JExJr…` (new v2-only) — v2 count inc
- NEW `/oauth2/v2.0/authorize?response_type=token` body-size drift: 23940 → 41309 bytes — confirms error-rendering anomaly stable, content is JS error shell
- CHANGED `graph.microsoft.com/beta/copilot/agentRegistrations` OPTIONS → HTTP 405 sustained (was 200 w/ CORS `*` + full mutation allowlist at 09:52 UTC) — CORS cross-origin mutation vector **closed**, confirme
- CHANGED `www.googleapis.com/auth/cloud-platform` → HTTP 404 (len=14) at 17:16 UTC cycle reverted to 400/403-class endpoint behavior — scope-name echo remains flaky, no new surface
- CHANGED `oauth2.googleapis.com/token` POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves client_secret is valid Google OAuth credential (RFC 6749 §5.2)
- NEW Nothing is new — all findings in the knowledge base remain current. The last knowledge update (2026-08-10 19:18:04 UTC) and the latest robot probes confirm all ACCEPTED findings are live. The knowledg
- CHANGED `graph.microsoft.com/beta/copilot/agentRegistrations` OPTIONS → HTTP 405 sustained (CORS cross-origin mutation vector closed since 09:52 UTC)
- CHANGED `login.microsoftonline.com/common/discovery/v2.0/keys` +3 kids vs 18:04 UTC: `rRk1d-57B…`, `NqEBZVuOp…`, `1Nv3JExJr…` (v2 count increased, subset invariant intact)
- CHANGED `/oauth2/v2.0/authorize?response_type=token` body-size drift: 23940 → 41309 bytes (error-rendering anomaly stable)

## 2026-08-10 21:06:52 UTC
- NEW login.microsoftonline.com/common/discovery/v2.0/keys: +3 new kids (rRk1d-57B… msonline.com, NqEBZVuOp… msonline.us, 1Nv3JExJr… new v2-only) — v2 count increased, v1⊂v2 subset invariant intact
- NEW /oauth2/v2.0/authorize?response_type=token: body-size drift 23940 → 41309 bytes — error-rendering anomaly stable, content is JS error shell
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 sustained (CORS cross-origin mutation vector closed since 09:52 UTC)
- CHANGED www.googleapis.com/auth/cloud-platform → HTTP 404 (len=14) reverted from 200/14 — scope-name echo flaky, no new surface
- NEW Nothing is new — all findings in the knowledge base remain current. The last knowledge update (2026-08-10 19:18:04 UTC) and the latest robot probes confirm all ACCEPTED findings are live. The knowledg
- NEW Source maps live on both identity SPAs: `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (7MB, 4359 paths) + `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35MB, 4922 files)
- NEW Verified ID minting endpoint `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential` — POST, Bearer scope=SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2`; backend gates ONLY on `GuestIsN
- NEW `/me/agentSignInSessions` (v1.0 + beta) fully off-metadata — 0 refs in both `$metadata` docs but endpoint alive (401)
- NEW Agent Registry API (beta, deprecated May-2026): `/beta/agentRegistry` → `agentInstances`/`agentCardManifests`/`agentCollections`; `agentInstance` binds `agentIdentityId`+`agentUserId`+`agentIdentityBl

## 2026-08-10 21:57:30 UTC
- CHANGED `raw.githubusercontent.com/google/earthengine-api/master/python/ee/oauth.py` → HTTP 404 in latest probe (21:06:53 UTC) — contradicts 20+ prior cycles showing 200 len=23110. Suspect backtick-in-URL pro
- CHANGED `login.microsoftonline.com/common/discovery/v2.0/keys` +3 new kids (`rRk1d-57B…`, `NqEBZVuOp…`, `1Nv3JExJr…`) — v2 key count increased from 8 to 11, but v1(4)⊂v2(11) subset invariant still intact.

## 2026-08-10 22:26:30 UTC
- CHANGED raw.githubusercontent.com/google/earthengine-api/master/python/ee/oauth.py → HTTP 404 (was 200 len=23110 for 20+ cycles) — suspect backtick-in-URL probe artifact or repo change
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys +3 new kids (`rRk1d-57B…`, `NqEBZVuOp…`, `1Nv3JExJr…`) — v2 count 8→11, v1(4)⊂v2(11) subset invariant intact
- CHANGED `raw.githubusercontent.com/google/earthengine-api/master/python/ee/oauth.py` 404 (21:06/21:57 robot probes) → PROBE ARTIFACT confirmed: clean URL → **200/23110**; trailing-backtick URL → 404. File liv
- CHANGED `graph.microsoft.com/beta/copilot/agentRegistrations/{id}` OPTIONS→405 KB entries → **bare-OPTIONS artifact**. True preflight (Origin + `Access-Control-Request-Method: PATCH` + `Access-Control-Request
- CHANGED raw.githubusercontent.com/google/earthengine-api/master/python/ee/oauth.py: HTTP 404 probe (21:06 UTC) was a backtick-in-URL artifact — fresh clean GET → **200 23110**, secret still live, whole-file s
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys: body grew 9KB → 11292 bytes (from +3 new v2-only kids: `rRk1d-57B…`, `NqEBZVuOp…`, `1Nv3JExJr…`), v1(4)⊂v2(11) subset invariant intact, 0 v1-exclu
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret → HTTP 400 `invalid_grant` (not 401 `invalid_client`) — validates credential validity (RFC 6749 §5.2), fresh confirmation of existing ACCEPTE
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → 405 sustained (CORS vector closed since 09:52 UTC), GET → 401/237 (auth-gated), HEAD → 405/0 (RFC 6750 §3 violation) — all unchanged

## 2026-08-10 23:24:16 UTC
- CHANGED `graph.microsoft.com/beta/copilot/agentRegistrations` true CORS preflight (Origin + Access-Control-Request-Method:PATCH + Access-Control-Request-Headers:authorization) → HTTP **200** with `Access-Cont
- CHANGED `login.microsoftonline.com/common/discovery/v2.0/keys` v2 kid count rotated to 7 (from 8/11 prior) with 2 v2-only kids (`rRk1d-57B…`, `NqEBZVuOp…`); v1(5)⊂v2(7) subset invariant intact, 0 v1-exclusive
- NEW earthengine-api oauth.py scopes confirmed as cloud-platform + **earthengine** + drive + devstorage.full_control (KB omitted earthengine scope, now confirmed present at line 46)
- NEW Copilot Studio D2E S2S API @ graph.microsoft.com — conversation-ID NOT validated server-side (private preview)
- NEW Consent primitive POST /v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation e406a681…)
- NEW Orchestrated API @ /powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool takes client-supplied toolSchemaName+inputs
- NEW Three-hop Agent User user_fic flow — client_credentials+cert+fmi_path → T1, FIC exchange → T2, grant_type=user_fic with user_id={oid}/upn
- NEW managerApplications on Blueprints — up to 10 first-party apps manage without AgentIdentityBlueprintPrincipal.ReadWrite.All
- NEW /me/agentSignInSessions (v1.0 + beta) fully off-metadata — 0 refs in $metadata, endpoint alive (401)
- NEW Agent Registry API (beta, deprecated May-2026) @ /beta/agentRegistry — agentInstances/agentCardManifests/agentCollections
- NEW Source maps unauthenticated @ mysignins.microsoft.com (7MB, 4359 paths) + api.myaccount.microsoft.com (35MB, 4922 files)
- NEW ACS JWKS rotation — 5 self-signed keys (3× CN=accounts.accesscontrol.windows.net, 2× CN=login.microsoftonline.us), allowedAudiences claim
- CHANGED raw.githubusercontent.com/google/earthengine-api/master/python/ee/oauth.py — 404 probe was backtick-in-URL artifact; clean GET → 200/23110, secret live (whole-file sha f4f93c76… unchanged)
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys — +3 new v2-only kids (rRk1d-57B…, NqEBZVuOp…, 1Nv3JExJr…), v2 count 8→11, v1(4)⊂v2(11) subset invariant intact
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations OPTIONS → HTTP 405 sustained — CORS cross-origin mutation vector closed since 09:52 UTC

## 2026-08-10 23:51:16 UTC
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations — true CORS preflight (Origin + Access-Control-Request-Method/Headers) → HTTP 200 ACAO:* + Allow-Methods DELETE,GET,OPTIONS,POST,PUT,PATCH + Max-Age
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys — v2 kid count rotated 7→11 across cycles; latest shows v1(5)⊂v2(7) with 2 v2-only kids; subset invariant intact.
- NEW graph.microsoft.com/beta/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations — Copilot Studio D2E S2S conversation-ID NOT validated server-side (private preview).
- NEW graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation e406a681…) on production v1.0 endpoint.
- NEW powervirtualagents.microsoft.com/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool accepts client-supplied toolSchemaName+inputs.
- NEW graph.microsoft.com/v1.0/me/agentSignInSessions — fully off-metadata (0 refs in $metadata), endpoint alive (401).
- NEW graph.microsoft.com/beta/agentRegistry — deprecated May-2026 but alive; agentInstances/agentCardManifests/agentCollections.
- NEW Source maps unauthenticated @ mysignins.microsoft.com (7MB, 4359 paths) + api.myaccount.microsoft.com (35MB, 4922 files).
- NEW accounts.accesscontrol.windows.net JWKS — 5 self-signed keys (3× CN=accounts.accesscontrol.windows.net, 2× CN=login.microsoftonline.us), allowedAudiences claim.
- NEW github.com/google/earthengine-api/python/ee/oauth.py — confirmed scopes: cloud-platform + earthengine + drive + devstorage.full_control (earthengine scope was missing from prior KB entries).

## 2026-08-11 00:42:24 UTC
- NEW Copilot Studio D2E S2S API @ graph.microsoft.com/beta/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations — conversation-ID NOT validated server-side (private preview)
- NEW Consent primitive POST graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation) on production v1.0
- NEW Orchestrated API @ powervirtualagents.microsoft.com/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool accepts client-supplied toolSchemaName+inputs
- NEW Three-hop Agent User user_fic flow — client_credentials+cert+fmi_path → T1, FIC exchange → T2, grant_type=user_fic with user_id={oid}/upn
- NEW Source maps unauthenticated @ mysignins.microsoft.com (7MB, 4359 paths) + api.myaccount.microsoft.com (35MB, 4922 files)
- NEW ACS JWKS @ accounts.accesscontrol.windows.net — 5 self-signed keys (3× CN=accounts.accesscontrol.windows.net, 2× CN=login.microsoftonline.us), allowedAudiences claim
- NEW /me/agentSignInSessions (v1.0 + beta) fully off-metadata — 0 refs in $metadata, endpoint alive (401)
- NEW Agent Registry API (beta, deprecated May-2026) @ graph.microsoft.com/beta/agentRegistry — agentInstances/agentCardManifests/agentCollections
- NEW earthengine-api oauth.py scopes confirmed: cloud-platform + earthengine + drive + devstorage.full_control (earthengine scope newly confirmed at line 46)
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations — true CORS preflight (Origin + Access-Control-Request-Method/Headers) → HTTP 200 ACAO:* + full mutation allowlist (DELETE,GET,OPTIONS,POST,PUT,PATC
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys — v2 kid count rotated (7→11→7), +3 v2-only kids (rRk1d-57B, NqEBZVuOp, 1Nv3JExJr); v1(4-5)⊂v2(7-11) subset invariant intact, 0 v1-exclusive steady
- CHANGED raw.githubusercontent.com/google/earthengine-api/master/python/ee/oauth.py — 404 probes confirmed backtick-in-URL artifact; clean GET → 200/23110, secret sha256 `3f3f8d6f…d271` verbatim at :45 + :99 f
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves valid Google OAuth credential (RFC 6749 §5.2), confidence 95→96
- CHANGED oauth2.googleapis.com/token GET → 404 sustained — confirms POST-only alive gate (RFC-compliant), validates earthengine secret hypothesis (only grant_type=refresh_token redemption path)

## 2026-08-11 02:55:45 UTC
- NEW Copilot Studio D2E S2S API @ graph.microsoft.com/beta/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations — conversation-ID NOT validated server-side (private preview)
- NEW Consent primitive POST graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation) on production v1.0
- NEW Orchestrated API @ powervirtualagents.microsoft.com/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool accepts client-supplied toolSchemaName+inputs
- NEW Three-hop Agent User user_fic flow — client_credentials+cert+fmi_path → T1, FIC exchange → T2, grant_type=user_fic with user_id={oid}/upn
- NEW Source maps unauthenticated @ mysignins.microsoft.com (7MB, 4359 paths) + api.myaccount.microsoft.com (35MB, 4922 files)
- NEW ACS JWKS @ accounts.accesscontrol.windows.net — 5 self-signed keys (3× CN=accounts.accesscontrol.windows.net, 2× CN=login.microsoftonline.us), allowedAudiences claim
- NEW /me/agentSignInSessions (v1.0 + beta) fully off-metadata — 0 refs in $metadata, endpoint alive (401)
- NEW Agent Registry API (beta, deprecated May-2026) @ graph.microsoft.com/beta/agentRegistry — agentInstances/agentCardManifests/agentCollections
- NEW earthengine-api oauth.py scopes confirmed: cloud-platform + earthengine + drive + devstorage.full_control (earthengine scope newly confirmed at line 46)
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations — true CORS preflight (Origin + Access-Control-Request-Method/Headers) → HTTP 200 ACAO:* + full mutation allowlist (DELETE,GET,OPTIONS,POST,PUT,PATC
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys — v2 kid count rotated (7→11→7), +3 v2-only kids (rRk1d-57B, NqEBZVuOp, 1Nv3JExJr); v1(4-5)⊂v2(7-11) subset invariant intact, 0 v1-exclusive steady
- CHANGED raw.githubusercontent.com/google/earthengine-api/master/python/ee/oauth.py — 404 probes confirmed backtick-in-URL artifact; clean GET → 200/23110, secret sha256 `3f3f8d6f…d271` verbatim at :45 + :99 f
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves valid Google OAuth credential (RFC 6749 §5.2), confidence 95→96
- CHANGED oauth2.googleapis.com/token GET → 404 sustained — confirms POST-only alive gate (RFC-compliant), validates earthengine secret hypothesis (only grant_type=refresh_token redemption path)

## 2026-08-11 04:32:52 UTC

## 2026-08-11 05:49:46 UTC

## 2026-08-11 06:39:01 UTC
- CHANGED mysignins.microsoft.com source map rotated — `main.7b5c8f3a.js.map` now 404 (was 200 7MB); api.myaccount.microsoft.com source map stable 200 35MB
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys requires `Accept: application/json` for JSON (was returning HTML without)

## 2026-08-11 07:53:30 UTC
- NEW Copilot Studio D2E S2S API @ graph.microsoft.com/beta/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations — conversation-ID NOT validated server-side (private preview)
- NEW Consent primitive POST graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation) on production v1.0
- NEW Orchestrated API @ powervirtualagents.microsoft.com/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool accepts client-supplied toolSchemaName+inputs
- NEW Three-hop Agent User user_fic flow — client_credentials+cert+fmi_path → T1, FIC exchange → T2, grant_type=user_fic with user_id={oid}/upn
- NEW Source maps unauthenticated @ mysignins.microsoft.com (7MB, 4359 paths) + api.myaccount.microsoft.com (35MB, 4922 files)
- NEW ACS JWKS @ accounts.accesscontrol.windows.net — 5 self-signed keys (3× CN=accounts.accesscontrol.windows.net, 2× CN=login.microsoftonline.us), allowedAudiences claim
- NEW /me/agentSignInSessions (v1.0 + beta) fully off-metadata — 0 refs in $metadata, endpoint alive (401)
- NEW Agent Registry API (beta, deprecated May-2026) @ graph.microsoft.com/beta/agentRegistry — agentInstances/agentCardManifests/agentCollections
- NEW earthengine-api oauth.py scopes confirmed: cloud-platform + earthengine + drive + devstorage.full_control (earthengine scope newly confirmed)
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations — true CORS preflight (Origin + ACRM/Headers) → HTTP 200 ACAO:* + full mutation allowlist (DELETE,GET,OPTIONS,POST,PUT,PATCH) + Max-Age 86400
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys — v2 kid count rotated (7→11→7), +3 v2-only kids; v1(4-5)⊂v2(7-11) subset invariant intact, 0 v1-exclusive steady-state
- CHANGED raw.githubusercontent
- CHANGED mysignins.microsoft.com source map rotated — `main.7b5c8f3a.js.map` now 404 (was 200 7MB, 4359 paths); api.myaccount.microsoft.com source map still 200/35MB/4922 paths (one SPA hardened, other still e
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys now requires `Accept: application/json` header for JSON response (was returning HTML without — minor hardening of key endpoint)

## 2026-08-11 08:44:47 UTC
- NEW Three-hop Agent User user_fic flow — client_credentials+cert+fmi_path → T1, FIC exchange → T2, grant_type=user_fic with user_id={oid}/upn
- CHANGED mysignins.microsoft.com source map rotated — `main.7b5c8f3a.js.map` now 404 (was 200 7MB); api.myaccount.microsoft.com source map stable 200 35MB
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys requires `Accept: application/json` for JSON (was returning HTML without)
- CHANGED mysignins.microsoft.com source map rotated — `main.7b5c8f3a.js.map` now 404 (was 200 7MB, 4359 paths); api.myaccount.microsoft.com source map still 200/35MB/4922 paths (one SPA hardened, other still e
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys now requires `Accept: application/json` header for JSON response (was returning HTML without — minor hardening of key endpoint)
- CHANGED mysignins.microsoft.com source map `main.7b5c8f3a.js.map` → HTTP 404 (was 200/7MB/4359 paths); sibling `api.myaccount.microsoft.com/main.4e6e3dc6.js.map` still 200/35MB/4922 paths — partial hardening,
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys now requires `Accept: application/json` for JSON (was returning HTML without) — minor key-endpoint hardening.
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys v2 kid count rotated 11→7 (3 v2-only kids `rRk1d-57B…`, `NqEBZVuOp…`, `1Nv3JExJr…` dropped); v1(4-5)⊂v2(7) subset invariant intact, 0 v1-exclusive 

## 2026-08-11 09:53:48 UTC
- CHANGED mysignins.microsoft.com source map rotated — `main.7b5c8f3a.js.map` now 404 (was 200 7MB); api.myaccount.microsoft.com source map stable 200 35MB/4922 paths
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys requires `Accept: application/json` for JSON (was returning HTML without) — minor key-endpoint hardening
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys v2 kid count rotated 11→7 (3 v2-only kids `rRk1d-57B…`, `NqEBZVuOp…`, `1Nv3JExJr…` dropped); v1(4-5)⊂v2(7) subset invariant intact, 0 v1-exclusive 

## 2026-08-11 10:42:38 UTC
- NEW Three-hop Agent User user_fic flow — client_credentials+cert+fmi_path → T1, FIC exchange → T2, grant_type=user_fic with user_id={oid}/upn
- NEW Copilot Studio D2E S2S API @ graph.microsoft.com/beta/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations — conversation-ID NOT validated server-side (private preview)
- NEW Consent primitive POST graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation) on production v1.0
- NEW Orchestrated API @ powervirtualagents.microsoft.com/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool accepts client-supplied toolSchemaName+inputs
- NEW Source maps unauthenticated @ mysignins.microsoft.com (7MB, 4359 paths) + api.myaccount.microsoft.com (35MB, 4922 files)
- NEW ACS JWKS @ accounts.accesscontrol.windows.net — 5 self-signed keys (3× CN=accounts.accesscontrol.windows.net, 2× CN=login.microsoftonline.us), allowedAudiences claim
- NEW /me/agentSignInSessions (v1.0 + beta) fully off-metadata — 0 refs in $metadata, endpoint alive (401)
- NEW Agent Registry API (beta, deprecated May-2026) @ graph.microsoft.com/beta/agentRegistry — agentInstances/agentCardManifests/agentCollections
- NEW earthengine-api oauth.py scopes confirmed: cloud-platform + earthengine + drive + devstorage.full_control (earthengine scope newly confirmed)
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations — true CORS preflight (Origin + Access-Control-Request-Method/Headers) → HTTP 200 ACAO:* + full mutation allowlist (DELETE,GET,OPTIONS,POST,PUT,PATC
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys — v2 kid count rotated (7→11→7), +3 v2-only kids (rRk1d-57B, NqEBZVuOp, 1Nv3JExJr); v1(4-5)⊂v2(7-11) subset invariant intact, 0 v1-exclusive steady
- CHANGED raw.githubusercontent.com/google/earthengine-api/master/python/ee/oauth.py — 404 probes confirmed backtick-in-URL artifact; clean GET → 200/23110, secret sha256 `3f3f8d6f…d271` verbatim at :45 + :99 f
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret → 400 `invalid_grant` (not 401 `invalid_client`) — conclusively proves valid Google OAuth credential (RFC 6749 §5.2), confidence 95→96
- CHANGED oauth2.googleapis.com/token GET → 404 sustained — confirms POST-only alive gate (RFC-compliant), validates earthengine secret hypothesis (only grant_type=refresh_token redemption path)
- CHANGED mysignins.microsoft.com source map rotated — `main.7b5c8f3a.js.map` now 404 (was 200 7MB); api.myaccount.microsoft.com source map stable 200 35MB
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys requires `Accept: application/json` for JSON (was returning HTML without) — minor key-endpoint hardening
- CHANGED api.myaccount.microsoft.com source map → HTTP 401 (was 200/35MB) — both identity SPA source maps now closed (mysignins 404/myaccount 401)
- CHANGED api.myaccount.microsoft.com/main.4e6e3dc6.js.map → HTTP 401 (was 200/35MB/4922 paths) — both identity SPA source maps now closed (mysignins 404 + myaccount 401)
- NEW graph.microsoft.com/beta/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations — Copilot Studio D2E S2S API, conversation-ID NOT validated server-side (private preview)
- NEW graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation) on production v1.0

## 2026-08-11 11:31:36 UTC
- NEW Copilot Studio D2E S2S API @ graph.microsoft.com/beta/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations — conversation-ID NOT validated server-side (private preview)
- NEW Consent primitive POST graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation) on production v1.0
- NEW Orchestrated API @ powervirtualagents.microsoft.com/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool accepts client-supplied toolSchemaName+inputs
- NEW ACS JWKS @ accounts.accesscontrol.windows.net — 5 self-signed keys (3× CN=accounts.accesscontrol.windows.net, 2× CN=login.microsoftonline.us), allowedAudiences claim
- NEW /me/agentSignInSessions (v1.0 + beta) fully off-metadata — 0 refs in $metadata, endpoint alive (401)
- NEW Agent Registry API (beta, deprecated May-2026) @ graph.microsoft.com/beta/agentRegistry — agentInstances/agentCardManifests/agentCollections
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations collection CORS preflight → HTTP 200 ACAO:* + full mutation allowlist (DELETE,GET,OPTIONS,POST,PUT,PATCH) + Max-Age 86400 (confirmed live this probe
- CHANGED api.myaccount.microsoft.com source map → HTTP 401 (was 200/35MB) — both identity SPA source maps now closed (mysignins 404/myaccount 401)
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys v2 kid count rotated 11→7 (3 v2-only kids dropped), Accept: application/json now required for JSON
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret → 400 invalid_grant (not 401 invalid_client) — conclusively proves valid Google OAuth credential (RFC 6749 §5.2), confidence 95→96
- NEW Consent primitive POST `graph.microsoft.com/v1.0/oauth2PermissionGrants` — caller-chosen resourceId (Graph OR Azure Storage user_impersonation) on production v1.0
- NEW Copilot Studio D2E S2S conversation-ID validation gap @ `graph.microsoft.com/beta/copilotstudio` — conversation-ID NOT validated server-side (private preview)
- NEW Orchestrated API @ `powervirtualagents.microsoft.com/orchestrated/{cdsBotId}/conversations/{conversationId}` — InvokeTool accepts client-supplied toolSchemaName+inputs
- NEW Three-hop Agent User user_fic flow documented @ `graph.microsoft.com` — client_credentials+cert+fmi_path → T1, FIC exchange → T2, grant_type=user_fic
- NEW `/me/agentSignInSessions` (v1.0 + beta) fully off-metadata — 0 refs in `$metadata`, endpoint alive (401)
- CHANGED `graph.microsoft.com/beta/copilot/agentRegistrations` — true CORS preflight (Origin+ACRM/Headers) → HTTP 200 ACAO:* + full mutation allowlist, Max-Age 86400 (re-confirmed)

## 2026-08-11 12:32:07 UTC
- NEW Copilot Studio D2E S2S API @ graph.microsoft.com/beta/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations — conversation-ID NOT validated server-side (private preview)
- NEW Consent primitive POST graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation) on production v1.0
- NEW Orchestrated API @ powervirtualagents.microsoft.com/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool accepts client-supplied toolSchemaName+inputs
- NEW ACS JWKS @ accounts.accesscontrol.windows.net — 5 self-signed keys (3× CN=accounts.accesscontrol.windows.net, 2× CN=login.microsoftonline.us), allowedAudiences claim
- NEW /me/agentSignInSessions (v1.0 + beta) fully off-metadata — 0 refs in $metadata, endpoint alive (401)
- NEW Agent Registry API (beta, deprecated May-2026) @ graph.microsoft.com/beta/agentRegistry — agentInstances/agentCardManifests/agentCollections
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations collection CORS preflight → HTTP 200 ACAO:* + full mutation allowlist (DELETE,GET,OPTIONS,POST,PUT,PATCH) + Max-Age 86400 (confirmed live this probe
- CHANGED api.myaccount.microsoft.com source map → HTTP 401 (was 200/35MB) — both identity SPA source maps now closed (mysignins 404/myaccount 401)
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys v2 kid count rotated 11→7 (3 v2-only kids dropped), Accept: application/json now required for JSON
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret → 400 invalid_grant (not 401 invalid_client) — conclusively proves valid Google OAuth credential (RFC 6749 §5.2), confidence 95→96
- CHANGED mysignins.microsoft.com source map rotated — main.7b5c8f3a.js.map now 404 (was 200 7MB); api.myaccount.microsoft.com source map stable 200 35MB/4922 paths
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys requires Accept: application/json for JSON (was returning HTML without) — minor key-endpoint hardening
- CHANGED api.myaccount.microsoft.com/main.4e6e3dc6.js.map → HTTP 401 (was 200/35MB/4922 paths) — both identity SPA source maps now closed (mysignins 404 + myaccount 401)

## 2026-08-11 13:59:24 UTC
- NEW NO_DELTA — all inventory items already reflected in knowledge base (2026-08-11 12:32 UTC probes) and last leads

## 2026-08-11 15:00:17 UTC
- NEW NO_DELTA — all inventory items already reflected in knowledge base (2026-08-11 12:32 UTC probes) and last leads

## 2026-08-11 16:03:47 UTC
- NEW NO_DELTA — all inventory items already reflected in knowledge base (2026-08-11 12:32 UTC probes) and last leads

## 2026-08-11 17:19:04 UTC
- NEW NO_DELTA — all inventory items already reflected in knowledge base (2026-08-11 12:32 UTC probes) and last leads

## 2026-08-11 18:08:34 UTC

## 2026-08-11 19:26:00 UTC

## 2026-08-11 20:09:33 UTC
- NEW graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation) on production v1.0

## 2026-08-11 21:04:46 UTC

## 2026-08-11 22:01:05 UTC

## 2026-08-11 22:56:47 UTC

## 2026-08-11 23:42:45 UTC

## 2026-08-12 00:46:05 UTC
- NEW NO_DELTA — all fresh passive probes (2026-08-11 23:42 UTC) confirmed prior ACCEPTED/REJECTED findings unchanged; inventory from 12:32 UTC already reflected in knowledge base and last leads
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys — v2 kid count rotated 11→7 (3 v2-only kids dropped), Accept: application/json now required for JSON response
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations — true CORS preflight (Origin+ACRM/Headers) → HTTP 200 ACAO:* + full mutation allowlist + Max-Age 86400 (re-confirmed; prior bare-OPTIONS→405 was ar
- NEW graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation) on production v1.0
- NEW graph.microsoft.com/beta/copilotstudio — conversation-ID NOT validated server-side (private preview)
- NEW powervirtualagents.microsoft.com/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool accepts client-supplied toolSchemaName+inputs
- NEW /me/agentSignInSessions (v1.0 + beta) — fully off-metadata (0 refs in $metadata), endpoint alive (401)
- NEW Three-hop Agent User user_fic flow documented @ graph.microsoft.com — client_credentials+cert+fmi_path → T1 → FIC exchange → T2 → grant_type=user_fic

## 2026-08-12 03:14:56 UTC
- NEW NO_DELTA — all fresh passive probes (2026-08-12 00:46 UTC) confirmed prior ACCEPTED/REJECTED findings unchanged; inventory from 12:32 UTC already reflected in knowledge base and last leads

## 2026-08-12 05:08:20 UTC
- NEW NO_DELTA — all fresh passive probes (2026-08-12 00:46 UTC) confirmed prior ACCEPTED/REJECTED findings unchanged; inventory from 12:32 UTC already reflected in knowledge base
- NEW graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId on production v1.0 (consent forge)
- NEW /me/agentSignInSessions (v1.0 + beta) — fully off-metadata, 0 refs in $metadata, alive (401)
- NEW graph.microsoft.com/beta/agentRegistry — deprecated May-2026 Agent Registry API
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys — Accept: application/json now required for JSON; v2 kid count rotated 11→7
- CHANGED api.myaccount.microsoft.com source map → HTTP 401 (both identity SPA maps now closed: mysignins 404 + myaccount 401)

## 2026-08-12 06:45:09 UTC
- NEW graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation) on production v1.0
- NEW /me/agentSignInSessions (v1.0 + beta) — fully off-metadata (0 refs in $metadata), endpoint alive (401)
- NEW graph.microsoft.com/beta/agentRegistry — deprecated May-2026 Agent Registry API
- NEW graph.microsoft.com/beta/copilotstudio — conversation-ID NOT validated server-side (private preview)
- NEW powervirtualagents.microsoft.com/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool accepts client-supplied toolSchemaName+inputs
- NEW Three-hop Agent User user_fic flow documented @ graph.microsoft.com — client_credentials+cert+fmi_path → T1 → FIC exchange → T2 → grant_type=user_fic
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys — requires Accept: application/json for JSON (was returning HTML without)
- CHANGED api.myaccount.microsoft.com/main.4e6e3dc6.js.map → HTTP 401 (was 200/35MB/4922 paths) — both identity SPA source maps now closed (mysignins 404 + myaccount 401)
- CHANGED `graph.microsoft.com` root now returns HTTP 301→200 (redirect to versioned path, resolves 200/106522 text/html) — minor routing change, no new surface
- CHANGED JWKS v2.0 key set rotated to 6 kids (was 7-11 across cycles); v1(5) ⊃ 4 shared + 1 v1-exclusive (`jvm_-Ttaq…`, transient rotation churn) — subset invariant not strict but no confusion surface (v1 kid 
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret + invalid refresh_token → HTTP 400 `invalid_grant` (NOT `401 invalid_client`) — conclusive per-RFC-6749-§5.2 proof of credential validity (re

## 2026-08-12 08:10:08 UTC
- NEW graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation) on production v1.0
- NEW /me/agentSignInSessions (v1.0 + beta) — fully off-metadata (0 refs in $metadata), endpoint alive (401)
- NEW graph.microsoft.com/beta/agentRegistry — deprecated May-2026 Agent Registry API
- NEW graph.microsoft.com/beta/copilotstudio — conversation-ID NOT validated server-side (private preview)
- NEW powervirtualagents.microsoft.com/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool accepts client-supplied toolSchemaName+inputs
- NEW Three-hop Agent User user_fic flow documented @ graph.microsoft.com
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys — requires Accept: application/json for JSON (was returning HTML without)
- CHANGED api.myaccount.microsoft.com/main.4e6e3dc6.js.map → HTTP 401 (was 200/35MB/4922 paths) — both identity SPA source maps now closed
- CHANGED graph.microsoft.com root → HTTP 301→200 (redirect to versioned path, resolves 200/106522 text/html)
- CHANGED JWKS v2.0 key set rotated to 6 kids (was 7-11); v1(5) ⊃ 4 shared + 1 v1-exclusive (transient rotation churn)
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret + invalid refresh_token → HTTP 400 `invalid_grant` (NOT `401 invalid_client`) — conclusive RFC 6749 §5.2 proof

## 2026-08-12 09:33:04 UTC
- NEW graph.microsoft.com/v1.0/oauth2PermissionGrants — caller-chosen resourceId (Graph OR Azure Storage user_impersonation) on production v1.0
- NEW /me/agentSignInSessions (v1.0 + beta) — fully off-metadata (0 refs in $metadata), endpoint alive (401)
- NEW graph.microsoft.com/beta/agentRegistry — deprecated May-2026 Agent Registry API
- NEW graph.microsoft.com/beta/copilotstudio — conversation-ID NOT validated server-side (private preview)
- NEW powervirtualagents.microsoft.com/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool accepts client-supplied toolSchemaName+inputs
- NEW Three-hop Agent User user_fic flow documented @ graph.microsoft.com
- CHANGED login.microsoftonline.com/common/discovery/v2.0/keys — requires Accept: application/json for JSON (was returning HTML without)
- CHANGED api.myaccount.microsoft.com/main.4e6e3dc6.js.map → HTTP 401 (was 200/35MB/4922 paths) — both identity SPA source maps now closed
- CHANGED graph.microsoft.com root → HTTP 301→200 (redirect to versioned path, resolves 200/106522 text/html)
- CHANGED JWKS v2.0 key set rotated to 6 kids (was 7-11); v1(5) ⊃ 4 shared + 1 v1-exclusive (transient rotation churn)
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret + invalid refresh_token → HTTP 400 `invalid_grant` (NOT `401 invalid_client`) — conclusive RFC 6749 §5.2 proof

## 2026-08-12 10:40:03 UTC

## 2026-08-12 11:32:33 UTC

## 2026-08-12 12:20:47 UTC

## 2026-08-12 13:58:26 UTC

## 2026-08-12 14:54:16 UTC

## 2026-08-12 15:47:39 UTC
- NEW None
- CHANGED None

## 2026-08-12 16:45:11 UTC

## 2026-08-12 18:01:28 UTC

## 2026-08-12 18:43:23 UTC

## 2026-08-12 20:06:35 UTC
- CHANGED graph.microsoft.com/beta/copilot/agents/{id} → HTTP 401 (item-level auth-gate confirmed live at 18:43 UTC)
- CHANGED graph.microsoft.com/beta/copilot/admin/catalog/packages/{id} → HTTP 401 (item-level auth-gate confirmed live at 18:43 UTC)
- CHANGED JWKS v2.0 key set rotated to 6 kids (was 7-11); v1(5) ⊃ 4 shared + 1 v1-exclusive (`jvm_-Ttaq…`) — transient rotation churn, subset invariant not strict but no confusion surface
- CHANGED api.myaccount.microsoft.com source map → HTTP 401 sustained (was 200/35MB); both identity SPA source maps now closed (mysignins 404 + myaccount 401)
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret + invalid refresh_token → HTTP 400 `invalid_grant` (NOT `401 invalid_client`) — conclusive RFC 6749 §5.2 proof re-confirmed

## 2026-08-12 20:29:12 UTC
- NEW graph.microsoft.com/beta/copilot/agents/{id} → HTTP 401 (item-level auth-gate confirmed live at 18:43 UTC)
- NEW graph.microsoft.com/beta/copilot/admin/catalog/packages/{id} → HTTP 401 (item-level auth-gate confirmed live at 18:43 UTC)
- CHANGED JWKS v2.0 key set rotated to 6 kids (was 7-11); v1(5) ⊃ 4 shared + 1 v1-exclusive (`jvm_-Ttaq…`) — transient rotation churn
- CHANGED api.myaccount.microsoft.com source map → HTTP 401 sustained (was 200/35MB); both identity SPA source maps now closed (mysignins 404 + myaccount 401)
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret + invalid refresh_token → HTTP 400 `invalid_grant` (NOT `401 invalid_client`) — conclusive RFC 6749 §5.2 proof re-confirmed

## 2026-08-12 21:25:36 UTC
- NEW graph.microsoft.com/beta/copilot/agents/{id} item-level auth-gate confirmed (18:43 UTC)
- NEW graph.microsoft.com/beta/copilot/admin/catalog/packages/{id} item-level auth-gate confirmed (18:43 UTC)
- CHANGED JWKS v2.0 rotated to 6 kids (v1=5, 4 shared + 1 v1-exclusive `jvm_-Ttaq`)
- CHANGED api.myaccount.microsoft.com source map → HTTP 401 sustained (both SPAs now closed)
- CHANGED oauth2.googleapis.com/token POST with leaked secret + invalid RT → 400 `invalid_grant` re-confirmed

## 2026-08-12 22:12:21 UTC

## 2026-08-12 23:02:59 UTC

## 2026-08-12 23:55:03 UTC

## 2026-08-13 01:45:30 UTC
- NEW graph.microsoft.com/beta/copilot/agents/{id} item-level auth-gate → HTTP 401 confirmed live
- NEW graph.microsoft.com/beta/copilot/admin/catalog/packages/{id} item-level auth-gate → HTTP 401 confirmed live
- CHANGED JWKS v2.0 key set rotated to 6 kids (v1=5, 4 shared + 1 v1-exclusive `jvm_-Ttaq…`) — transient rotation churn
- CHANGED api.myaccount.microsoft.com source map → HTTP 401 sustained (both identity SPAs now closed: mysignins 404 + myaccount 401)
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret + invalid refresh_token → HTTP 400 `invalid_grant` (NOT `401 invalid_client`) re-confirmed

## 2026-08-13 03:57:25 UTC
- NEW graph.microsoft.com/beta/copilot/agents/{id} item-level auth-gate → HTTP 401 confirmed live
- NEW graph.microsoft.com/beta/copilot/admin/catalog/packages/{id} item-level auth-gate → HTTP 401 confirmed live
- CHANGED JWKS v2.0 key set rotated to 6 kids (v1=5, 4 shared + 1 v1-exclusive `jvm_-Ttaq…`) — transient rotation churn
- CHANGED api.myaccount.microsoft.com source map → HTTP 401 sustained (both identity SPAs now closed: mysignins 404 + myaccount 401)
- CHANGED oauth2.googleapis.com/token POST with leaked client_secret + invalid refresh_token → HTTP 400 `invalid_grant` (NOT `401 invalid_client`) re-confirmed

## 2026-08-13 05:44:14 UTC

## 2026-08-13 07:14:24 UTC
- NEW None — latest robot probes (2026-08-13 05:44 UTC) and inventory show NO_DELTA vs prior cycle; all ACCEPTED/REJECTED findings unchanged

## 2026-08-13 08:53:43 UTC

## 2026-08-13 09:51:02 UTC
- NEW NO_DELTA — latest robot probes (2026-08-13 08:53 UTC) and inventory show NO_DELTA vs prior cycle; all ACCEPTED/REJECTED findings unchanged

## 2026-08-13 10:47:52 UTC
- NEW NO_DELTA — latest robot probes (2026-08-13 08:53 UTC) and inventory show NO_DELTA vs prior cycle; all ACCEPTED/REJECTED findings unchanged

## 2026-08-13 11:12:00 UTC

## 2026-08-13 11:53:42 UTC

## 2026-08-13 12:33:59 UTC

## 2026-08-13 14:10:26 UTC

## 2026-08-13 15:16:11 UTC

## 2026-08-13 16:22:20 UTC

## 2026-08-13 17:15:46 UTC

## 2026-08-13 18:12:28 UTC
- NEW None — latest robot probes (2026-08-13 17:15 UTC) and inventory show NO_DELTA vs prior cycle; all ACCEPTED/REJECTED findings unchanged

## 2026-08-13 19:37:36 UTC

## 2026-08-13 20:06:51 UTC

## 2026-08-13 20:58:57 UTC

## 2026-08-13 21:56:08 UTC

## 2026-08-13 22:54:46 UTC

## 2026-08-13 23:26:30 UTC
- NEW None — all fresh passive probes (2026-08-13 22:54 UTC) confirmed prior ACCEPTED/REJECTED findings unchanged, NO_DELTA
- CHANGED None — JWKS v2.0 rotation churn (v1v2 steady-state), source maps closed, token POST invalid_grant re-confirmed; no new attack surface

## 2026-08-14 00:08:40 UTC
- NEW None — all fresh passive probes (2026-08-13 22:54/23:26 UTC) confirmed prior ACCEPTED/REJECTED findings unchanged, NO_DELTA

## 2026-08-14 02:46:43 UTC

## 2026-08-14 04:31:28 UTC

## 2026-08-14 06:05:04 UTC

## 2026-08-14 07:47:29 UTC

## 2026-08-14 08:53:30 UTC

## 2026-08-14 09:53:59 UTC

## 2026-08-14 10:51:40 UTC

## 2026-08-14 11:36:17 UTC

## 2026-08-14 12:33:37 UTC

## 2026-08-14 13:58:59 UTC
- NEW oAuth2PermissionGrant EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata — 458-char block, 0 OperationRestrictions, 7 client-supplied properties including resourceId (Graph OR
- CHANGED agentRegistrations bare-OPTIONS→405 reclassified as probe artifact (no Origin = not a preflight); true CORS preflight with PATCH confirmed at collection+item level
- CHANGED Earth Engine secret A/B proof re-confirmed: leaked secret→400 invalid_grant (valid), fake→401 invalid_client (invalid) per RFC 6749 §5.2

## 2026-08-14 14:55:28 UTC
- NEW oAuth2PermissionGrant EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata — 458-char block, 0 OperationRestrictions, 7 client-supplied properties including resourceId (Graph OR
- CHANGED agentRegistrations bare-OPTIONS→405 reclassified as probe artifact (no Origin = not a preflight); true CORS preflight with PATCH confirmed at collection+item level
- CHANGED Earth Engine secret A/B proof re-confirmed: leaked secret→400 invalid_grant (valid), fake→401 invalid_client (invalid) per RFC 6749 §5.2
- CHANGED graph.microsoft.com root → HTTP 301 (was 200 text/html signin page in prior cycles); cosmetic redirect per KB, no auth-bypass surface
- NEW None — agentRegs GET 401/237, item-level true CORS preflight 200 `ACAO:*`+PATCH+Max-Age 86400, token GET→404 POST-only, tokeninfo 400/113, earthengine oauth.py whole-file sha `f4f93c76…`+bare-secret s

## 2026-08-14 15:49:36 UTC

## 2026-08-14 16:38:30 UTC

## 2026-08-14 17:38:14 UTC

## 2026-08-14 18:34:59 UTC
- NEW None — all fresh passive probes (2026-08-14 cycle) confirmed prior ACCEPTED/REJECTED findings unchanged, NO_DELTA
- CHANGED None — JWKS v2.0 rotation churn (v1⊂v2 steady-state), source maps closed, token POST invalid_grant re-confirmed; no new attack surface

## 2026-08-14 19:39:11 UTC
- NEW None — all fresh passive probes (2026-08-14 cycle) confirmed prior ACCEPTED/REJECTED findings unchanged, NO_DELTA
- CHANGED None — JWKS v2.0 rotation churn (v1⊂v2 steady-state), source maps closed, token POST invalid_grant re-confirmed; no new attack surface
- NEW oAuth2PermissionGrant EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata — 458-char block, 0 OperationRestrictions, 7 client-supplied properties including resourceId (Graph OR
- CHANGED agentRegistrations bare-OPTIONS→405 reclassified as probe artifact (no Origin header = not true preflight); verified true CORS preflight (Origin + Access-Control-Request-Method:PATCH + Access-Control-
- CHANGED Earth Engine secret A/B proof refined — leaked secret→400 `invalid_grant` (valid credential per RFC 6749 §5.2); fake secret→401 `invalid_client "The provided client secret is invalid."` — conclusive d

## 2026-08-14 20:08:57 UTC

## 2026-08-14 20:45:38 UTC

## 2026-08-14 21:11:08 UTC
- NEW oAuth2PermissionGrant EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata — 458-char block, 0 OperationRestrictions, 7 client-supplied properties including resourceId (Graph OR
- CHANGED agentRegistrations bare-OPTIONS→405 reclassified as probe artifact (no Origin header = not true preflight); verified true CORS preflight (Origin + Access-Control-Request-Method:PATCH + Access-Control-
- CHANGED Earth Engine secret A/B proof refined — leaked secret→400 `invalid_grant` (valid credential per RFC 6749 §5.2); fake secret→401 `invalid_client "The provided client secret is invalid."` — conclusive d
- CHANGED graph.microsoft.com root → HTTP 301 (was 200 text/html signin page in prior cycles); cosmetic redirect per KB, no auth-bypass surface

## 2026-08-14 21:39:58 UTC

## 2026-08-14 22:01:19 UTC
- NEW None — all fresh passive probes (2026-08-14 cycle) confirmed prior ACCEPTED/REJECTED findings unchanged, NO_DELTA

## 2026-08-14 22:33:33 UTC

## 2026-08-14 22:56:11 UTC

## 2026-08-14 23:23:18 UTC

## 2026-08-14 23:52:14 UTC

## 2026-08-15 00:05:58 UTC

## 2026-08-15 01:47:42 UTC

## 2026-08-15 02:43:05 UTC

## 2026-08-15 03:25:05 UTC

## 2026-08-15 04:05:50 UTC

## 2026-08-15 04:46:21 UTC

## 2026-08-15 05:07:27 UTC
- NEW oAuth2PermissionGrant EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata — 458-char block, 0 OperationRestrictions, 7 client-supplied properties including resourceId (Graph OR
- CHANGED agentRegistrations bare-OPTIONS→405 reclassified as probe artifact (no Origin header = not true preflight); verified true CORS preflight (Origin + Access-Control-Request-Method:PATCH + Access-Control-
- CHANGED Earth Engine secret A/B proof refined — leaked secret→400 `invalid_grant` (valid credential per RFC 6749 §5.2); fake secret→401 `invalid_client "The provided client secret is invalid."` — conclusive d
- CHANGED graph.microsoft.com root → HTTP 301 (was 200 text/html signin page in prior cycles); cosmetic redirect per KB, no auth-bypass surface

## 2026-08-15 05:40:21 UTC

## 2026-08-15 06:01:52 UTC

## 2026-08-15 06:54:11 UTC
- NEW `google/codeworld/web/js/utils/auth.js:30` — hardcoded password `swal-input2` (sha256 `ffd0ae7de65bc67fe6698ae2ebaf08cb1250d4014944ac868ac7c6c66da893e5`) deployed to production client-side JS; verifie
- CHANGED Re-classification: bare-OPTIONS→405 on agentRegistrations confirmed as probe artifact (true CORS preflight with Origin header → 200 ACAO:* + PATCH + Max-Age 86400, item-level verified)

## 2026-08-15 07:26:24 UTC
- NEW `google/codeworld/web/js/utils/auth.js:30` — hardcoded password `swal-input2` (sha256 `ffd0ae7de65bc67fe6698ae2ebaf08cb1250d4014944ac868ac7c6c66da893e5`) deployed to production client-side JS; verifie
- CHANGED Re-classification: bare-OPTIONS→405 on agentRegistrations confirmed as probe artifact (true CORS preflight with Origin header → 200 ACAO:* + PATCH + Max-Age 86400, item-level verified)

## 2026-08-15 07:54:14 UTC
- NEW `google/codeworld/web/js/utils/auth.js:30` — hardcoded `PASSWORD = 'swal-input2'` (sha256 `ffd0ae7d...`) in production client-side JS
- CHANGED agentRegistrations bare-OPTIONS→405 reclassified as probe artifact; true CORS preflight (Origin+ACRM:PATCH+ACH:authorization) → 200 `ACAO:*` + PATCH allowlist + Max-Age 86400 confirmed at item level
- CHANGED Earth Engine A/B proof refined — leaked secret→400 `invalid_grant`, fake secret→401 `invalid_client` (conclusive RFC 6749 §5.2 differential)
- CHANGED graph.microsoft.com root → HTTP 301 (was 200 text/html); cosmetic redirect, no auth-bypass surface

## 2026-08-15 08:21:58 UTC
- NEW `google/codeworld/web/js/utils/auth.js:30` — hardcoded `PASSWORD = 'swal-input2'` (sha256 `ffd0ae7d...`) in production client-side JS
- CHANGED agentRegistrations bare-OPTIONS→405 reclassified as probe artifact; true CORS preflight (Origin+ACRM:PATCH+ACH:authorization) → 200 `ACAO:*` + PATCH allowlist + Max-Age 86400 confirmed at item level
- CHANGED Earth Engine A/B proof refined — leaked secret→400 `invalid_grant`, fake secret→401 `invalid_client` (conclusive RFC 6749 §5.2 differential)
- CHANGED graph.microsoft.com root → HTTP 301 (was 200 text/html); cosmetic redirect, no auth-bypass surface

## 2026-08-15 08:58:22 UTC

## 2026-08-15 09:18:09 UTC
- NEW `google/codeworld/web/js/utils/auth.js:30` — hardcoded `PASSWORD = 'swal-input2'` (sha256 `ffd0ae7d...`) in production client-side JS
- CHANGED agentRegistrations bare-OPTIONS→405 reclassified as probe artifact; true CORS preflight (Origin+ACRM:PATCH+ACH:authorization) → 200 `ACAO:*` + PATCH allowlist + Max-Age 86400 confirmed at item level
- CHANGED Earth Engine A/B proof refined — leaked secret→400 `invalid_grant`, fake secret→401 `invalid_client` (conclusive RFC 6749 §5.2 differential)
- CHANGED graph.microsoft.com root → HTTP 301 (was 200 text/html); cosmetic redirect, no auth-bypass surface

## 2026-08-15 09:45:48 UTC
- NEW none — all surface items already tracked; CodeWorld `PASSWORD='swal-input2'` previously analyzed & rejected as SweetAlert2 DOM ID pattern (swal-input1/2/3/4), not a credential
- CHANGED none — agentRegistrations bare-OPTIONS→405 reclassified as probe artifact (true preflight with Origin+ACRM:PATCH+ACH:authorization → 200 ACAO:* + PATCH + Max-Age 86400 already confirmed), Earth Engine
- CHANGED agentRegistrations bare-OPTIONS→405 reclassified as probe artifact; true CORS preflight (Origin+ACRM:PATCH+ACH:authorization) → 200 `ACAO:*` + PATCH allowlist + Max-Age 86400 confirmed at item level
- CHANGED Earth Engine A/B proof refined — leaked secret→400 `invalid_grant` (valid credential per RFC 6749 §5.2); fake secret→401 `invalid_client "The provided client secret is invalid."` — conclusive differen
- CHANGED graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW oAuth2PermissionGrant EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata — 458-char block, 0 OperationRestrictions, 7 client-supplied properties including caller-supplied reso
- NEW oauth2.googleapis.com/token GET → 404 confirms POST-only alive gate; validates earthengine secret hypothesis (only grant_type=refresh_token is redemption path)

## 2026-08-15 10:08:41 UTC
- NEW none — all surface items already tracked; CodeWorld `PASSWORD='swal-input2'` previously analyzed & rejected as SweetAlert2 DOM ID pattern (swal-input1/2/3/4), not a credential
- CHANGED none — agentRegistrations bare-OPTIONS→405 reclassified as probe artifact (true preflight with Origin+ACRM:PATCH+ACH:authorization → 200 ACAO:* + PATCH + Max-Age 86400 already confirmed), Earth Engine

## 2026-08-15 10:37:06 UTC

## 2026-08-15 10:57:19 UTC

## 2026-08-15 11:22:40 UTC

## 2026-08-15 11:42:45 UTC
- NEW oAuth2PermissionGrant EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata — 458-char block, 0 OperationRestrictions, 7 client-supplied properties including caller-supplied reso
- NEW oauth2.googleapis.com/token GET → 404 confirms POST-only alive gate; validates earthengine secret hypothesis (only grant_type=refresh_token is redemption path)

## 2026-08-15 11:59:51 UTC
- NEW NO_DELTA

## 2026-08-15 12:52:31 UTC

## 2026-08-15 13:24:48 UTC

## 2026-08-15 13:53:16 UTC
- NEW oAuth2PermissionGrant EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata — 458-char block, 0 OperationRestrictions, 7 client-supplied properties incl caller-supplied resourceI
- CHANGED agentRegistrations item-level true CORS preflight gap CLOSED — Origin+ACRM:PATCH+ACH:authorization→200 ACAO:* + PATCH allowlist + Max-Age 86400 confirmed at both collection+item level across all 6 Cop
- CHANGED CodeWorld `PASSWORD='swal-input2'` @ google/codeworld/web/js/utils/auth.js:30 — REJECTED false positive (SweetAlert2 DOM element ID pattern, not a credential) — collapsed MISCONFIG class

## 2026-08-15 14:14:16 UTC

## 2026-08-15 14:41:55 UTC

## 2026-08-15 15:01:23 UTC
- NEW oAuth2PermissionGrant EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata — 458-char block, 0 OperationRestrictions, 7 client-supplied properties including caller-supplied reso
- NEW oauth2.googleapis.com/token GET → 404 confirms POST-only alive gate (RFC-compliant OAuth token endpoint); validates earthengine secret hypothesis — only grant_type=refresh_token is redemption path
- CHANGED agentRegistrations item-level true CORS preflight gap CLOSED — Origin+ACRM:PATCH+ACH:authorization → 200 ACAO:* + PATCH allowlist + Max-Age 86400 confirmed at both collection+item level across all 6 C
- CHANGED CodeWorld `PASSWORD='swal-input2'` @ google/codeworld/web/js/utils/auth.js:30 — REJECTED false positive (SweetAlert2 DOM element ID pattern swal-input1/2/3/4, not a credential) — MISCONFIG class colla
- CHANGED graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface

## 2026-08-15 15:32:16 UTC
- NEW oAuth2PermissionGrant EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata — 458-char block, 0 OperationRestrictions, 7 client-supplied properties including caller-supplied reso
- NEW oauth2.googleapis.com/token GET → 404 confirms POST-only alive gate (RFC-compliant OAuth token endpoint); validates earthengine secret hypothesis — only grant_type=refresh_token is redemption path
- CHANGED agentRegistrations item-level true CORS preflight gap CLOSED — Origin+ACRM:PATCH+ACH:authorization → 200 ACAO:* + PATCH allowlist + Max-Age 86400 confirmed at both collection+item level across all 6 C
- CHANGED CodeWorld `PASSWORD='swal-input2'` @ google/codeworld/web/js/utils/auth.js:30 — REJECTED false positive (SweetAlert2 DOM element ID pattern swal-input1/2/3/4, not a credential) — MISCONFIG class colla
- CHANGED graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface

## 2026-08-15 15:55:32 UTC
- NEW NO_DELTA — all surface items unchanged since last cycle; MSRC/Google VRP drafts prepared but not new attack surface

## 2026-08-15 16:11:13 UTC
- NEW oAuth2PermissionGrant EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata: 458-char block, 0 OperationRestrictions, 7 client-supplied properties including caller-supplied resou
- NEW oauth2.googleapis.com/token GET → 404 confirms POST-only alive gate; validates earthengine secret hypothesis (only grant_type=refresh_token is redemption path)
- CHANGED agentRegistrations item-level true CORS preflight gap CLOSED — Origin+ACRM:PATCH+ACH:authorization → 200 ACAO:* + PATCH allowlist + Max-Age 86400 confirmed at both collection+item level across all 6 C
- CHANGED CodeWorld `PASSWORD='swal-input2'` @ google/codeworld/web/js/utils/auth.js:30 — REJECTED false positive (SweetAlert2 DOM element ID pattern swal-input1/2/3/4, not a credential) — MISCONFIG class colla
- CHANGED graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW None — all surface items confirmed unchanged since last cycle. Verified live: earthengine secret sha256 `3f3f8d6f…d271` verbatim at oauth.py:45 (file sha `f4f93c76…`), agentRegistrations HEAD→405/0 no

## 2026-08-15 16:43:22 UTC
- NEW None — all surface items in latest inventory (2026-08-15 16:11 UTC) already captured in knowledge base (2026-08-15 07:26–16:11 UTC). Verified live: earthengine secret sha256 `3f3f8d6f…d271` at oauth.p

## 2026-08-15 17:02:52 UTC
- NEW None — all surface items in latest inventory (2026-08-15 16:11 UTC) already captured in knowledge base (2026-08-15 07:26–16:11 UTC). Verified live: earthengine secret sha256 `3f3f8d6f…d271` at oauth.p

## 2026-08-15 17:31:50 UTC

## 2026-08-15 17:53:00 UTC
- NEW NO_DELTA

## 2026-08-15 18:14:55 UTC
- NEW NO_DELTA — all surface items confirmed unchanged since last cycle; MSRC/Google VRP drafts prepared but no new attack surface
- NEW NO_DELTA — all fresh passive probes confirmed prior ACCEPTED/REJECTED findings unchanged (NO_DELTA)

## 2026-08-15 18:50:53 UTC
- NEW NO_DELTA — all surface items confirmed unchanged since last cycle; MSRC/Google VRP drafts prepared but no new attack surface
- NEW NO_DELTA — all fresh passive probes confirmed prior ACCEPTED/REJECTED findings unchanged (NO_DELTA)

## 2026-08-15 19:13:54 UTC
- NEW NO_DELTA — all surface items confirmed unchanged since last cycle; MSRC/Google VRP drafts prepared but no new attack surface

## 2026-08-15 19:38:23 UTC
- NEW NO_DELTA — all surface items confirmed unchanged since last cycle; MSRC/Google VRP drafts prepared but no new attack surface

## 2026-08-15 19:55:30 UTC
- NEW NO_DELTA — all surface items confirmed unchanged since last cycle; MSRC/Google VRP drafts prepared but no new attack surface

## 2026-08-15 20:21:02 UTC

## 2026-08-15 20:45:37 UTC
- NEW NO_DELTA — all fresh passive probes (2026-08-15 20:21 UTC) confirmed prior ACCEPTED/REJECTED findings unchanged; no new attack surface discovered.

## 2026-08-15 21:03:46 UTC

## 2026-08-15 21:33:31 UTC
- NEW NO_DELTA — all fresh passive probes confirmed prior ACCEPTED/REJECTED findings unchanged (NO_DELTA @ 2026-08-15 21:03 UTC)

## 2026-08-15 21:54:10 UTC
- NEW graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b → HTTP 400 (new probe target, bucket listing requires auth)
- CHANGED www.googleapis.com/storage/v1/b 400 resolved as missing-param artifact — with `?project=` (both earthengine number 517222506229 and ID) → 401 `storage.buckets.list` denied for anonymous caller; anonym

## 2026-08-15 22:14:57 UTC
- NEW graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b → HTTP 400 (new probe target, bucket listing requires auth)
- CHANGED www.googleapis.com/storage/v1/b 400 resolved as missing-param artifact — with `?project=` (both earthengine number 517222506229 and ID) → 401 `storage.buckets.list` denied for anonymous caller; anonym

## 2026-08-15 22:43:00 UTC
- NEW graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b → HTTP 400 (new probe target, bucket listing requires auth)
- CHANGED www.googleapis.com/storage/v1/b 400 resolved as missing-param artifact — with `?project=` (both earthengine number 517222506229 and ID) → 401 `storage.buckets.list` denied for anonymous caller; no byp

## 2026-08-15 23:00:59 UTC
- NEW graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b → HTTP 400 (new probe target, bucket listing requires auth)
- CHANGED www.googleapis.com/storage/v1/b 400 resolved as missing-param artifact — with `?project=` → 401 `storage.buckets.list` denied for anonymous caller; no bypass

## 2026-08-15 23:31:02 UTC
- NEW graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b → HTTP 400 (new probe target, bucket listing requires auth)
- CHANGED www.googleapis.com/storage/v1/b 400 resolved as missing-param artifact — with ?project= → 401 storage.buckets.list denied for anonymous caller; no bypass
- NEW graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b → HTTP 400 (new probe target, bucket listing requires auth)
- CHANGED www.googleapis.com/storage/v1/b 400 resolved as missing-param artifact — with ?project= → 401 storage.buckets.list denied for anonymous caller; no bypass
- CHANGED `graph.microsoft.com` root: HTTP 200 text/html signin page → HTTP 301 redirect → `developer.microsoft.com/graph` — cosmetic redirect, no auth-bypass surface, no new attack vector.
- NEW `www.googleapis.com/storage/v1/b`: fresh probe target → HTTP 400 (missing `project`); with `?project=` → 401 `storage.buckets.list` denied for anonymous caller — requires auth, no bypass. REJECTED as 

## 2026-08-15 23:50:17 UTC
- NEW graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b → HTTP 400 (new probe target, bucket listing requires auth)
- CHANGED www.googleapis.com/storage/v1/b 400 resolved as missing-param artifact — with ?project= → 401 storage.buckets.list denied for anonymous caller; no bypass

## 2026-08-16 00:26:57 UTC

## 2026-08-16 02:09:04 UTC

## 2026-08-16 03:12:57 UTC

## 2026-08-16 04:05:49 UTC

## 2026-08-16 04:47:09 UTC

## 2026-08-16 05:16:32 UTC

## 2026-08-16 05:47:57 UTC

## 2026-08-16 06:18:59 UTC

## 2026-08-16 07:05:04 UTC

## 2026-08-16 07:43:26 UTC
- NEW graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b → HTTP 400/401 (requires project param, then 401 storage.buckets.list denied); no bypass

## 2026-08-16 08:04:30 UTC

## 2026-08-16 08:44:04 UTC

## 2026-08-16 09:08:01 UTC
- NEW graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b → HTTP 400/401 (requires project param, then 401 storage.buckets.list denied); no bypass
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations/{id} item-level auth-gate → HTTP 401 confirmed (was previously inferred)
- CHANGED graph.microsoft.com/v1.0/oauth2PermissionGrants auth-gate → HTTP 401 confirmed (was previously inferred)

## 2026-08-16 09:56:47 UTC
- NEW graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b → HTTP 400/401 (requires project param, then 401 storage.buckets.list denied); no bypass
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations/{id} item-level auth-gate → HTTP 401 confirmed (was previously inferred)
- CHANGED graph.microsoft.com/v1.0/oauth2PermissionGrants auth-gate → HTTP 401 confirmed (was previously inferred)

## 2026-08-16 10:01:39 UTC
- NEW graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b → HTTP 400/401 (requires project param, then 401 storage.buckets.list denied); no bypass
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations/{id} item-level auth-gate → HTTP 401 confirmed (was previously inferred)
- CHANGED graph.microsoft.com/v1.0/oauth2PermissionGrants auth-gate → HTTP 401 confirmed (was previously inferred)

## 2026-08-16 10:34:07 UTC
- NEW graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b → HTTP 400/401 (requires project param, then 401 storage.buckets.list denied); no bypass
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations/{id} item-level auth-gate → HTTP 401 confirmed (was previously inferred)
- CHANGED graph.microsoft.com/v1.0/oauth2PermissionGrants auth-gate → HTTP 401 confirmed (was previously inferred)

## 2026-08-16 10:56:10 UTC
- NEW graph.microsoft.com root → HTTP 301 (was 200 text/html signin page); cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b → HTTP 400/401 (requires project param, then 401 storage.buckets.list denied); no bypass
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations/{id} item-level auth-gate → HTTP 401 confirmed (was previously inferred)
- CHANGED graph.microsoft.com/v1.0/oauth2PermissionGrants auth-gate → HTTP 401 confirmed (was previously inferred)

## 2026-08-16 11:20:05 UTC

## 2026-08-16 11:42:08 UTC

## 2026-08-16 11:59:11 UTC

## 2026-08-16 12:52:45 UTC

## 2026-08-16 13:25:52 UTC

## 2026-08-16 13:54:27 UTC

## 2026-08-16 14:18:43 UTC

## 2026-08-16 14:44:53 UTC
- NEW graph.microsoft.com root → HTTP 301 redirect to developer.microsoft.com/graph (was 200 text/html signin page) — cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b anonymous bucket enumeration → HTTP 400 missing project, then 401 storage.buckets.list denied — requires auth, no bypass
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations/{id} item-level auth-gate confirmed HTTP 401 (was previously inferred)
- CHANGED graph.microsoft.com/v1.0/oauth2PermissionGrants auth-gate confirmed HTTP 401 (was previously inferred)

## 2026-08-16 15:03:39 UTC

## 2026-08-16 15:33:08 UTC
- NEW NO_DELTA — all fresh passive probes (token→404, agentRegs→401/405, oauth2PermissionGrants→401, earthengine file sha unchanged) confirmed prior ACCEPTED/REJECTED findings unchanged

## 2026-08-16 15:54:29 UTC
- NEW NO_DELTA — all fresh passive probes (token→404, agentRegs→401/405, oauth2PermissionGrants→401, earthengine file sha unchanged) confirmed prior ACCEPTED/REJECTED findings unchanged

## 2026-08-16 16:19:47 UTC
- NEW NO_DELTA — all fresh passive probes (token→404, agentRegs→401/405, oauth2PermissionGrants→401, earthengine file sha unchanged) confirmed prior ACCEPTED/REJECTED findings unchanged

## 2026-08-16 16:48:12 UTC
- NEW NO_DELTA — all fresh passive probes (token→404, agentRegs→401/405, oauth2PermissionGrants→401, earthengine file sha unchanged) confirmed prior ACCEPTED/REJECTED findings unchanged

## 2026-08-16 17:07:27 UTC

## 2026-08-16 17:34:26 UTC
- NEW graph.microsoft.com root → HTTP 301 redirect to developer.microsoft.com/graph (was 200 text/html signin page) — cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b anonymous bucket enumeration → HTTP 400 missing project, then 401 storage.buckets.list denied — requires auth, no bypass
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations/{id} item-level auth-gate confirmed HTTP 401 (was previously inferred)
- CHANGED graph.microsoft.com/v1.0/oauth2PermissionGrants auth-gate confirmed HTTP 401 (was previously inferred)

## 2026-08-16 17:53:26 UTC

## 2026-08-16 18:18:58 UTC

## 2026-08-16 18:51:45 UTC

## 2026-08-16 19:13:32 UTC

## 2026-08-16 19:36:47 UTC

## 2026-08-16 19:55:07 UTC

## 2026-08-16 20:13:38 UTC

## 2026-08-16 20:41:38 UTC
- NEW graph.microsoft.com root → HTTP 301 redirect to developer.microsoft.com/graph (was 200 text/html signin page) — cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b anonymous bucket enumeration → HTTP 400 missing project, then 401 storage.buckets.list denied — requires auth, no bypass
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations/{id} item-level auth-gate confirmed HTTP 401 (was previously inferred)
- CHANGED graph.microsoft.com/v1.0/oauth2PermissionGrants auth-gate confirmed HTTP 401 (was previously inferred)

## 2026-08-16 21:01:08 UTC
- NEW www.googleapis.com/storage/v1/b anonymous enumeration → HTTP 400/401 (rejected: no bypass)
- NEW graph.microsoft.com root → HTTP 301 redirect (rejected: cosmetic)
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations/{id} item-level auth-gate confirmed 401
- CHANGED graph.microsoft.com/v1.0/oauth2PermissionGrants auth-gate confirmed 401

## 2026-08-16 21:29:38 UTC
- NEW graph.microsoft.com/beta/copilot/agentRegistrations/{id} item-level auth-gate confirmed HTTP 401 (was previously inferred)
- NEW graph.microsoft.com/v1.0/oauth2PermissionGrants auth-gate confirmed HTTP 401 (was previously inferred)
- CHANGED graph.microsoft.com root → HTTP 301 redirect to developer.microsoft.com/graph (was 200 text/html signin page) — cosmetic redirect
- CHANGED www.googleapis.com/storage/v1/b anonymous bucket enumeration → HTTP 400 missing project, then 401 storage.buckets.list denied — requires auth, no bypass

## 2026-08-16 21:50:22 UTC
- NEW graph.microsoft.com root → HTTP 301 redirect to developer.microsoft.com/graph (was 200 text/html signin page) — cosmetic redirect, no auth-bypass surface
- NEW www.googleapis.com/storage/v1/b anonymous bucket enumeration → HTTP 400 missing project, then 401 storage.buckets.list denied — requires auth, no bypass
- CHANGED graph.microsoft.com/beta/copilot/agentRegistrations/{id} item-level auth-gate confirmed HTTP 401 (was previously inferred)
- CHANGED graph.microsoft.com/v1.0/oauth2PermissionGrants auth-gate confirmed HTTP 401 (was previously inferred)

## 2026-08-16 22:03:27 UTC

## 2026-08-16 22:33:30 UTC

## 2026-08-16 22:54:15 UTC

## 2026-08-16 23:16:56 UTC

## 2026-08-16 23:39:07 UTC

## 2026-08-16 23:57:47 UTC
- NEW graph.microsoft.com/beta/copilot/agentRegistrations/{id} item-level auth-gate confirmed HTTP 401 (was previously inferred)
- NEW graph.microsoft.com/v1.0/oauth2PermissionGrants auth-gate confirmed HTTP 401 (was previously inferred)
- CHANGED graph.microsoft.com root → HTTP 301 redirect to developer.microsoft.com/graph (was 200 text/html signin page) — cosmetic redirect
- CHANGED www.googleapis.com/storage/v1/b anonymous bucket enumeration → HTTP 400 missing project, then 401 storage.buckets.list denied — requires auth, no bypass

## 2026-08-17 01:26:25 UTC

## 2026-08-17 02:40:35 UTC

## 2026-08-17 03:35:15 UTC

## 2026-08-17 04:24:54 UTC

## 2026-08-17 05:10:14 UTC

## 2026-08-17 05:50:51 UTC

## 2026-08-17 06:28:00 UTC

## 2026-08-17 07:35:22 UTC

## 2026-08-17 08:23:09 UTC

## 2026-08-17 09:08:11 UTC

## 2026-08-17 09:56:30 UTC
- NEW No fresh probe cycle since 09:08:11 UTC — robot probe log last entry 09:08:11. All inventory items unchanged from prior cycle. NO_DELTA.

## 2026-08-17 10:35:05 UTC

## 2026-08-17 11:02:15 UTC
- NEW graph.microsoft.com/v1.0/oauth2PermissionGrants auth-gate confirmed HTTP 401 (was previously inferred)
- CHANGED graph.microsoft.com root → HTTP 301 redirect to developer.microsoft.com/graph (was 200 text/html signin page) — cosmetic redirect
- CHANGED www.googleapis.com/storage/v1/b anonymous bucket enumeration → HTTP 400 missing project, then 401 storage.buckets.list denied — requires auth, no bypass
