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
