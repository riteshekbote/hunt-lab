
## 2026-08-07 09:09:44 UTC [google] (model nemotron3)
- [UNVALIDATED] TIER0 domains: accounts.google.com, admin.google.com, bard.google.com, chrome.google.com, console.cloud.google.com, gemini.google.com, mail.google.com, myaccount.google.com, passwords.google.com, play.google.com, takeout.google.com, vault.google.com, console.firebase.google.com, issuetracker.google.com, cloudsearch.google.com
- [UNVALIDATED] Cloud IT1 products: AlloyDB, App Engine, Artifact Registry, BigQuery, Cloud Build, Cloud KMS, Cloud Run, Cloud SQL, Cloud Storage, Compute Engine, Firestore, GKE, Secret Manager, Vertex AI, Cloud Console, IAM, VPC, Load Balancing, Pub/Sub, Spanner, Bigtable
- [UNVALIDATED] Historical subdomains from CT: *.corp.google.com, *.sandbox.google.com, *.c.docs.google.com, *.clients*.google.com, *.ext.google.com, *.vp.video.l.google.com, adwords.google.com.*, onex.wifi.google.com, hosted-id.google.com, freezone.google.com, flexpack.google.com

## 2026-08-07 09:40:49 UTC [microsoft] (model nemotron3)
- [UNVALIDATED] OAuth/OIDC**: `accounts.google.com/.well-known/oauth-authorization-server`, `accounts.google.com/.well-known/openid-configuration`, `accounts.google.com/o/oauth2/v2/auth`, `oauth2.googleapis.com/token`, `oauth2.googleapis.com/revoke`, `openidconnect.googleapis.com/v1/userinfo`, `www.googleapis.com/oauth2/v3/certs`
- [UNVALIDATED] TIER0 auth redirects**: `console.cloud.google.com/.well-known/*` → `accounts.google.com/ServiceLogin?service=cloudconsole`, `issuetracker.google.com/*` → search redirect, `admin.google.com` → 204 on `.well-known/*`
- [UNVALIDATED] Vertex AI regional**: `https://{region}-aiplatform.googleapis.com` (30+ regions), `https://aiplatform.mtls.googleapis.com`, `https://aiplatform.googleapis.com/$discovery/rest?version=v1`
- [UNVALIDATED] IAM**: `https://iam.googleapis.com/$discovery/rest?version=v1` — `projects.serviceAccounts.signJwt`, `signBlob`, `generateAccessToken`, `projects.locations.workloadIdentityPools.providers`
- [UNVALIDATED] Access Context Manager**: `https://accesscontextmanager.googleapis.com/$discovery/rest?version=v1` — `accessPolicies.servicePerimeters`, `accessPolicies.accessLevels`, `gcpUserAccessBindings`
- [UNVALIDATED] Binary Authorization**: `https://binaryauthorization.googleapis.com/$discovery/rest?version=v1` — `policy`, `attestors`, `systempolicy.v1`
- [UNVALIDATED] Org Policy**: `https://orgpolicy.googleapis.com/$discovery/rest?version=v2` — `constraints`, `policies`, `customConstraints`
- [UNVALIDATED] Assured Workloads**: `https://assuredworkloads.googleapis.com/$discovery/rest?version=v1` — `workloads`, `violations`, `analyzeWorkloadMove`
- [UNVALIDATED] BeyondCorp**: `https://beyondcorp.googleapis.com/$discovery/rest?version=v1` — `appConnections`, `appConnectors`, `appGateways`, `securityGateways`
- [UNVALIDATED] Agent Identity**: `https://agentidentity.googleapis.com/$discovery/rest?version=v1` — `authProviders`, `authorizations`, `accessSummaries`
- [UNVALIDATED] Cloud Build**: `https://cloudbuild.googleapis.com/$discovery/rest?version=v1` — `projects.triggers`, `projects.workerPools`, `projects.builds`
- [UNVALIDATED] Artifact Registry**: `https://artifactregistry.googleapis.com/$discovery/rest?version=v1` — `repositories`, `packages`, `versions`, `files`

## 2026-08-07 10:38:45 UTC [microsoft] (model nemotron3)
- [UNVALIDATED] STS token exchange**: `https://sts.googleapis.com/v1/token` (not in discovery) — accepts `grant_type=urn:ietf:params:oauth:grant-type:token-exchange`, `subject_token` from WIP provider, `subject_token_type=urn:ietf:params:oauth:token-type:jwt`
- [UNVALIDATED] Vertex AI deploy**: `POST /v1/projects/{project}/locations/{location}/endpoints/{endpoint}:deployModel` with `DeployModelRequest` → `ModelContainerSpec` + `NetworkSpec`
- [UNVALIDATED] WIP provider creation**: `POST /v1/projects/{project}/locations/{location}/workloadIdentityPools/{pool}/providers` with `attributeMapping` CEL + `attributeCondition` CEL
- [UNVALIDATED] Binary Authorization policy**: `GET/POST /v1/projects/{project}/policy` — `defaultAdmissionRule`, `clusterAdmissionRules`, `admissionWhitelistPatterns`
- [UNVALIDATED] Org Policy custom constraint**: `POST /v1/organizations/{org}/customConstraints` with CEL `condition`; `POST /v1/{resource}/policies/{constraint}` with `PolicySpec.rules[].condition` (CEL)
- [UNVALIDATED] Agent Identity auth provider**: `POST /v1/projects/{project}/locations/{location}/authProviders` with `ThreeLeggedOAuth`/`TwoLeggedOAuth`/`ApiKeyParams`/`GeminiEnterpriseAuthProviderParams`
- [UNVALIDATED] Secret Manager rotation trigger**: `POST /v1/projects/{project}/secrets/{secret}/versions:add` — manual version addition bypasses rotation schedule

## 2026-08-07 11:31:46 UTC [google] (model nemotron3)
- [UNVALIDATED] STS token exchange**: `POST https://sts.googleapis.com/v1/token` — `grant_type=urn:ietf:params:oauth:grant-type:token-exchange`, `subject_token` (external IdP JWT), `subject_token_type=urn:ietf:params:oauth:token-type:jwt`, `requested_token_type=urn:ietf:params:oauth:token-type:access_token`
- [UNVALIDATED] Vertex AI endpoint deploy**: `POST /v1/projects/{project}/locations/{location}/endpoints/{endpoint}:deployModel` — body: `DeployModelRequest` with `model`, `deployedModel`, `dedicatedResources`, `automaticResources`
- [UNVALIDATED] WIP provider config**: `GET /v1/projects/{project}/locations/{location}/workloadIdentityPools/{pool}/providers/{provider}` — returns `attributeMapping`, `attributeCondition`, `oidc`/`aws`/`saml` config
- [UNVALIDATED] Binary Authz policy**: `GET /v1/projects/{project}/policy` — returns full policy with `defaultAdmissionRule.evaluationMode`, `clusterAdmissionRules`, `admissionWhitelistPatterns`
- [UNVALIDATED] Org Policy effective policy**: `GET /v1/{resource}/policies/{constraint}:getEffectivePolicy` — shows computed policy after inheritance
- [UNVALIDATED] Agent Identity auth providers**: `GET /v1/projects/{project}/locations/{location}/authProviders` — lists `AuthProvider` with `allowedScopes`, `blockedScopes`, `workloadIds`

## 2026-08-07 12:32:42 UTC [google] (model nemotron3)
- [UNVALIDATED] Cloud Build private pool**: `GET /v1/projects/{project}/locations/{location}/workerPools/{workerPool}` — returns `privatePoolV1Config.networkConfig` (VPC peering) + `privateServiceConnect`
- [UNVALIDATED] Cloud Build trigger webhook**: `POST /v1/projects/{project}/locations/{location}/triggers/{trigger}:webhook` — accepts JSON payload to trigger build
- [UNVALIDATED] Artifact Registry virtual repo upstreams**: `GET /v1/projects/{project}/locations/{location}/repositories/{repo}?view=FULL` — returns `virtualRepositoryConfig.upstreamPolicies[]` with priority order
- [UNVALIDATED] Artifact Registry remote repo config**: `GET /v1/projects/{project}/locations/{location}/repositories/{repo}` — returns `remoteRepositoryConfig` with `upstreamCredentials`, `disableUpstreamValidation`, per-format settings
- [UNVALIDATED] Artifact Registry cleanup policies**: `GET /v1/projects/{project}/locations/{location}/repositories/{repo}/rules` — lists `CleanupPolicy` with DELETE/KEEP actions
- [UNVALIDATED] Assured Workloads compliance**: `GET /v1/organizations/{org}/locations/{location}/workloads/{workload}:analyzeWorkloadMove` — validates resource move
- [UNVALIDATED] BeyondCorp connector health**: `POST /v1/projects/{project}/locations/{location}/appConnectors/{connector}:reportStatus` — connector self-reports status
- [UNVALIDATED] Cloud KMS key version import**: `POST /v1/projects/{project}/locations/{location}/keyRings/{keyRing}/cryptoKeys/{key}/cryptoKeyVersions:import` — imports wrapped key material
- [UNVALIDATED] Cloud Run service ingress**: `GET /v1/projects/{project}/locations/{location}/services/{service}` — `metadata.annotations['run.googleapis.com/ingress']` + `status.conditions` for ingress status
- [UNVALIDATED] Cloud Run binary authorization**: `metadata.annotations['run.googleapis.com/binary-authorization']` — BREAKGLASS allows unverified images
- [UNVALIDATED] Cloud Run custom audiences**: `metadata.annotations['run.googleapis.com/custom-audiences']` — OIDC audience override for service-to-service auth

## 2026-08-07 15:41:41 UTC [microsoft] (model nemotron3)
- [UNVALIDATED] `POST /v1/projects/{project}/locations/{location}/triggers/{trigger}:run` — manual trigger with `RepoSource.substitutions` override
- [UNVALIDATED] `GET /v1/projects/{project}/locations/{location}/repositories/{repo}?view=FULL` — virtual repo upstreams + credentials
- [UNVALIDATED] `GET /v1/projects/{project}/policy` (Binary Authz) — admission rules, attestors, global policy mode
- [UNVALIDATED] `GET /v1/projects/{project}/locations/{location}/services/{service}` — Cloud Run annotations (breakglass, custom-audiences, ingress)
- [UNVALIDATED] `GET /v1/projects/{project}/locations/{location}/workerPools/{workerPool}` — private pool dual network config
## 2026-08-07 16:04:34 UTC [google] (model nemotron3)
[NEW] Source maps live on BOTH identity SPAs: `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (7MB, 4359 paths) + `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35MB, 4922 files with sourcesContent) — verified 200, no Cache-Control
[NEW] Verified ID minting endpoint `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential` — POST, Bearer scope=SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2`; backend gates ONLY on `GuestIsNotAllowedToIssueVerifiedId` / `TenantIsNotInAllowedToIssueVerifiedId` (no admin-role check visible in client contract)
[NEW] `/me/agentSignInSessions` (v1.0 + beta) fully off-metadata — 0 refs in both `$metadata` docs (hashes `9d84e451...`, `ca304859...`) but endpoint alive (401)
[NEW] Agent Registry API (beta, deprecated May-2026): `/beta/agentRegistry` → `agentInstances`/`agentCardManifests`/`agentCollections`; `agentInstance` binds `agentIdentityId`+`agentUserId`+`agentIdentityBlueprintId`, JWS-signed agent cards (ES256, `did:web`); perms `AgentInstance.ReadWrite.All` + `Agent Registry Administrator` role
[NEW] Copilot agent admin (beta): `/beta/agents`, `/beta/copilot/agents`, `/beta/copilot/agentRegistrations`, `/beta/copilot/admin/catalog/packages` (block/unblock/reassign); scope `CopilotPackages.Read.All` (no admin consent), requires Agent 365 license + AI admin/Global admin
[NEW] Agent Registration API (GA replacement): `/beta/copilot/agentRegistrations` POST/GET/PATCH/DELETE — **client-supplied `createdBy`**, PATCH rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agentCard` of ANY registration; scope `AgentRegistration.ReadWrite.All` (admin consent required)
[NEW] Copilot Policy Settings API: `/beta/copilot/admin/policySettings/{id}` — 5 settings (`microsoft.copilot.{copilotchatpinning,blockaccesstoopenfiles,imagegeneration,allowwebsearch,allowinadmincenters}`); stored in Exchange arbitration mailbox `Organization Partition_PolicyService_c2ada927-a9e2-4564-aae2-70775a2fa0af`
[NEW] Copilot Studio D2E (Direct-to-Engine) S2S API (private preview): `POST /copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations` — **conversation-ID NOT validated server-side** (doc: "non-existent GUID silently creates new conversation"); perm `CopilotStudio.Copilots.Invoke` on Power Platform API `8578e004-a5c6-46e7-913e-12f58912df43` (admin consent required)
[NEW] Orchestrated API: `/powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}` — `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`; validation against agent's registered tools unproven
[NEW] Three-hop Agent User `user_fic` flow (documented in `microsoft/entrabot`): Hop1 `client_credentials`+cert+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` **OR** `username={upn}` + `requested_token_use=on_behalf_of` → delegated `idtyp=user` token
[NEW] `managerApplications` on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without `AgentIdentityBlueprintPrincipal.ReadWrite.All` — supply-chain trust surface
[NEW] Consent primitive `POST /v1.0/oauth2PermissionGrants` with **caller-chosen `resourceId`** (Graph OR Azure Storage `user_impersonation` `e406a681...`); `Application.Read.All` NOT in agent blocked-permissions table
[NEW] ACS JWKS rotation confirmed: 5 self-signed keys (3× `CN=accounts.accesscontrol.windows.net` notBefore 06-08/06-17/07-04-2026, 2× `CN=login.microsoftonline.us` notBefore 07-01/07-26-2026), `allowedAudiences` field removed (tenant-enum patch)
[CHANGED] `login.live.com` redirect matrix EXHAUSTED: `/oauth20_desktop.srf` REMOVED (stub `?removed=true`); `/oauth20_authorize.srf` returns generic 200 for all 8 variants, validation deferred, no `redirect_uri` echo — passive signal gone
[PRIO] Agent Registration API, 8.65, attack=9 biz=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] Verified ID minting, 7.85, attack=8 biz=9 tech=8 gate=5 cloud=7 fresh=9
[PRIO] Copilot Studio D2E, 7.55, attack=8 biz=8 tech=9 gate=4 cloud=7 fresh=10
[HYP] Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite
class: IDOR
asset: `graph.microsoft.com/beta/copilot/agentRegistrations`
confidence: 75
reasoning: API accepts client-supplied `createdBy` on create; PATCH on any registration ID rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agentCard` with NO documented ownership enforcement beyond scope; Agent 365 GA transition makes this the canonical registration surface
evidence_needed: PATCH `/beta/copilot/agentRegistrations/{foreign_id}` with modified `ownerIds`/`agentCard` returns 200/204 and mutation persists (cross-tenant or cross-user)
verify_steps: AUTH_HELPED: In test tenant, create registration A as User1, note ID; as User2 (same tenant, different identity), PATCH that ID with attacker-controlled `agentCard` + `ownerIds`; GET to confirm mutation. Passive: GET `/beta/copilot/agentRegistrations` (no-list doc) to test enumeration
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, supply-chain compromise of any agent registration in tenant; CVSS 7.5-9.0
testability: AUTH_HELPED
[HYP] Verified ID minting without admin role — backend gates only on Guest/Tenant flags
class: AUTH
asset: `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential`
confidence: 70
reasoning: SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2` scopes token to itself; backend contract shows only `GuestIsNotAllowedToIssueVerifiedId` / `TenantIsNotInAllowedToIssueVerifiedId` checks — no admin/role validation visible in client schema (source map confirms); Entra Verified ID = high-value employee credential
evidence_needed: POST with low-priv user token (non-admin, non-guest, tenant allowed) returns 200/204 + credential; or 403 with error revealing missing admin check
verify_steps: AUTH_HELPED: In test tenant, acquire token for SPA clientId as regular member user (non-admin); POST `/api/issueVerifiedEmployeeCredential` (empty body per source map); observe response. Passive: download source map `main.4e6e3dc6.js.map` → extract request schema for endpoint
impact: Unprivileged user mints Verified Employee Credential (DID-signed VC) — identity spoofing, access to Verified ID-gated resources; CVSS 7.1-9.1
testability: AUTH_HELPED
[HYP] Copilot Studio D2E conversation-ID validation bypass — server does not validate conversation IDs
class: IDOR
asset: `https://{envhost}/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations`
confidence: 65
reasoning: Official doc (copilotStudioWebChat.ts:40-44) states: "server does not validate conversation IDs. A non-existent GUID will silently create a new conversation under that ID"; `x-ms-conversation-id` header on start; modes: True S2S (app-only, agent=No-Auth) / OBO (Integrated auth); admin-consent scope `CopilotStudio.Copilots.Invoke`
evidence_needed: Start conversation with random GUID → observe silent creation; resume/hijack existing conversation via guessed/predicted GUID; cross-app resumption with stolen `conversationId`
verify_steps: AUTH_HELPED: In test tenant with D2E access, POST start with random `conversationId` → verify 201 + new conversation; GET/POST to same `conversationId` from different client/app → observe cross-session access. Passive: review SDK code `CopilotClient.cs:509-517` for `x-ms-d2e-experimental` header promotion logic
impact: Conversation hijack, history disclosure, active-session prompt injection, agent impersonation; CVSS 6.5-9.0
testability: AUTH_HELPED
[PARKED] Agent User `user_fic` Hop3 subject-validation gap: confidence 60 but verify_steps requires test tenant with Agent ID + blueprint + agent user provisioned (multi-hop flow); no passive path; class AUTH but overlaps with known Microsoft Entra Agent ID hardening docs (blocked interactive flows, sponsor-gated) — may be mitigated by design
[PARKED] Agent Registry `agentCard` JWS trust: confidence 55; `microsoft/agents` SDK has 0 JWS/did/agentcard hits — M365 Agent SDK does NOT implement card signature verification per code review; but Agent Registry API is DEPRECATED (May-2026) in favor of Agent Registration; testing deprecated surface is lower priority
[PARKED] Source maps as standalone finding: confidence 40; triage confirmed "supporting asset only" — source maps alone are best-practice/informational class, not reportable without chained vuln
[FINAL] 1. Agent Registration ownership boundary bypass (IDOR) — 8.65 priority
[FINAL] 2. Verified ID minting without admin role (AUTH) — 7.85 priority  
[FINAL] 3. Copilot Studio D2E conversation-ID validation bypass (IDOR) — 7.55 priority
[NEXT] PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token) — tests enumeration of registrations (doc says no LIST but Collection type may allow); if 200 with array, confirms cross-registration visibility for PATCH targeting
[RISK] google: 35 reason: GCP control-plane APIs all auth-gated (API key/OAuth); Firebase identitytoolkit 403 unregistered-callers gate; ADK issues are SDK-level not live Google service; no new unauthenticated attack surface discovered in this cycle
[RISK] microsoft: 82 reason: Multiple high-value NEW surfaces with design-level gaps: Agent Registration (client-controlled ownership), Verified ID minting (missing admin gate), Copilot Studio D2E (conversation-ID not validated), Agent User `user_fic` subject knobs — all in active GA/preview transition, test-tenant reachable, Entra/Copilot identity plane = crown jewels
## 2026-08-07 16:39:58 UTC [google] (model nemotron3)
[NEW] Source maps live on BOTH identity SPAs: `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (7MB, 4359 paths) + `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35MB, 4922 files with sourcesContent) — verified 200, no Cache-Control
[NEW] Verified ID minting endpoint `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential` — POST, Bearer scope=SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2`; backend gates ONLY on `GuestIsNotAllowedToIssueVerifiedId` / `TenantIsNotInAllowedToIssueVerifiedId` (no admin-role check visible in client contract)
[NEW] `/me/agentSignInSessions` (v1.0 + beta) fully off-metadata — 0 refs in both `$metadata` docs (hashes `9d84e451...`, `ca304859...`) but endpoint alive (401)
[NEW] Agent Registry API (beta, deprecated May-2026): `/beta/agentRegistry` → `agentInstances`/`agentCardManifests`/`agentCollections`; `agentInstance` binds `agentIdentityId`+`agentUserId`+`agentIdentityBlueprintId`, JWS-signed agent cards (ES256, `did:web`); perms `AgentInstance.ReadWrite.All` + `Agent Registry Administrator` role
[NEW] Copilot agent admin (beta): `/beta/agents`, `/beta/copilot/agents`, `/beta/copilot/agentRegistrations`, `/beta/copilot/admin/catalog/packages` (block/unblock/reassign); scope `CopilotPackages.Read.All` (no admin consent), requires Agent 365 license + AI admin/Global admin
[NEW] Agent Registration API (GA replacement): `/beta/copilot/agentRegistrations` POST/GET/PATCH/DELETE — **client-supplied `createdBy`**, PATCH rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agentCard` of ANY registration; scope `AgentRegistration.ReadWrite.All` (admin consent required)
[NEW] Copilot Policy Settings API: `/beta/copilot/admin/policySettings/{id}` — 5 settings (`microsoft.copilot.{copilotchatpinning,blockaccesstoopenfiles,imagegeneration,allowwebsearch,allowinadmincenters}`); stored in Exchange arbitration mailbox `Organization Partition_PolicyService_c2ada927-a9e2-4564-aae2-70775a2fa0af`
[NEW] Copilot Studio D2E (Direct-to-Engine) S2S API (private preview): `POST /copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations` — **conversation-ID NOT validated server-side** (doc: "non-existent GUID silently creates new conversation"); perm `CopilotStudio.Copilots.Invoke` on Power Platform API `8578e004-a5c6-46e7-913e-12f58912df43` (admin consent required)
[NEW] Orchestrated API: `/powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}` — `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`; validation against agent's registered tools unproven
[NEW] Three-hop Agent User `user_fic` flow (documented in `microsoft/entrabot`): Hop1 `client_credentials`+cert+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` **OR** `username={upn}` + `requested_token_use=on_behalf_of` → delegated `idtyp=user` token
[NEW] `managerApplications` on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without `AgentIdentityBlueprintPrincipal.ReadWrite.All` — supply-chain trust surface
[NEW] Consent primitive `POST /v1.0/oauth2PermissionGrants` with **caller-chosen `resourceId`** (Graph OR Azure Storage `user_impersonation` `e406a681...`); `Application.Read.All` NOT in agent blocked-permissions table
[NEW] ACS JWKS rotation confirmed: 5 self-signed keys (3× `CN=accounts.accesscontrol.windows.net` notBefore 06-08/06-17/07-04-2026, 2× `CN=login.microsoftonline.us` notBefore 07-01/07-26-2026), `allowedAudiences` field removed (tenant-enum patch)
[CHANGED] `login.live.com` redirect matrix EXHAUSTED: `/oauth20_desktop.srf` REMOVED (stub `?removed=true`); `/oauth20_authorize.srf` returns generic 200 for all 8 variants, validation deferred, no `redirect_uri` echo — passive signal gone
[NEW] `login.microsoftonline.com` OIDC discovery: v2.0 (issuer `login.microsoftonline.com/{tid}/v2.0`; JWKS `/discovery/v2.0/keys`, 8 RSA keys; mtls alias `mtlsauth.microsoft.com`; tls_client_certificate_bound_access_tokens)
[NEW] Graph `$metadata`: 1,183 EntityTypes, 326 Functions across `microsoft.graph.identityGovernance` + `microsoft.graph.security` + `microsoft.graph.entraRecoveryServices`; 22 `filterByCurrentUser` bindings
[NEW] Graph API 405 anomaly: unauth HEAD/GET to `/v1.0`, `/me`, `/users` → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer — verified passively (4×405)
[NEW] v2.0 authorize HTTP 200 error rendering: GET `/oauth2/v2.0/authorize?response_type=token` (unsupported in v2.0) → HTTP 200 with embedded JS error code 700038 (iHttpErrorCode 400, "We received a bad request")
[NEW] `oauth2.googleapis.com/tokeninfo` public introspection oracle: accepts `?access_token=` / `?id_token=` query params (no Authorization header), returns aud/scope/expiry — verified (no-token→400, malformed→400)
[NEW] `bughunters.google.com` root `/` → HTTP 200, hardened (HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff)
[PRIO] `graph.microsoft.com/beta/copilot/agentRegistrations`, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential`, 7.80, attack=8 business=9 tech=8 gate=5 cloud=7 fresh=9
[PRIO] `copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations`, 7.65, attack=8 business=8 tech=9 gate=4 cloud=7 fresh=10
[PRIO] `powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}`, 6.70, attack=7 business=7 tech=8 gate=4 cloud=6 fresh=8
[PRIO] `login.microsoftonline.com` (user_fic three-hop), 6.90, attack=7 business=8 tech=8 gate=3 cloud=7 fresh=8
[PRIO] `graph.microsoft.com/v1.0/oauth2PermissionGrants`, 6.70, attack=7 business=7 tech=7 gate=5 cloud=6 fresh=8
[HYP] Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite
class: IDOR
asset: `graph.microsoft.com/beta/copilot/agentRegistrations`
confidence: 75
reasoning: API accepts client-supplied `createdBy` on create; PATCH on any registration ID rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agentCard` with NO documented ownership enforcement beyond scope; Agent 365 GA transition makes this the canonical registration surface
evidence_needed: PATCH `/beta/copilot/agentRegistrations/{foreign_id}` with modified `ownerIds`/`agentCard` returns 200/204 and mutation persists (cross-tenant or cross-user)
verify_steps: AUTH_HELPED: In test tenant, create registration A as User1, note ID; as User2 (same tenant, different identity), PATCH that ID with attacker-controlled `agentCard` + `ownerIds`; GET to confirm mutation. Passive: GET `/beta/copilot/agentRegistrations` (no-list doc) to test enumeration
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, supply-chain compromise of any agent registration in tenant; CVSS 7.5-9.0
testability: AUTH_HELPED
[HYP] Verified ID minting without admin role — backend gates only on Guest/Tenant flags
class: AUTH
asset: `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential`
confidence: 70
reasoning: SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2` scopes token to itself; backend contract shows only `GuestIsNotAllowedToIssueVerifiedId` / `TenantIsNotInAllowedToIssueVerifiedId` checks — no admin/role validation visible in client schema (source map confirms); Entra Verified ID = high-value employee credential
evidence_needed: POST with low-priv user token (non-admin, non-guest, tenant allowed) returns 200/204 + credential; or 403 with error revealing missing admin check
verify_steps: AUTH_HELPED: In test tenant, acquire token for SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2` as regular member user (non-admin); POST `/api/issueVerifiedEmployeeCredential` (empty body per source map); observe response. Passive: download source map `main.4e6e3dc6.js.map` → extract request schema for endpoint
impact: Unprivileged user mints Verified Employee Credential (DID-signed VC) — identity spoofing, access to Verified ID-gated resources; CVSS 7.1-9.1
testability: AUTH_HELPED
[HYP] Copilot Studio D2E conversation-ID validation bypass — server does not validate conversation IDs
class: IDOR
asset: `https://{envhost}/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations`
confidence: 65
reasoning: Official doc (copilotStudioWebChat.ts:40-44) states: "server does not validate conversation IDs. A non-existent GUID will silently create a new conversation under that ID"; `x-ms-conversation-id` header on start; modes: True S2S (app-only, agent=No-Auth) / OBO (Integrated auth); admin-consent scope `CopilotStudio.Copilots.Invoke`
evidence_needed: Start conversation with random GUID → observe silent creation; resume/hijack existing conversation via guessed/predicted GUID; cross-app resumption with stolen `conversationId`
verify_steps: AUTH_HELPED: In test tenant with D2E access, POST start with random `conversationId` → verify 201 + new conversation; GET/POST to same `conversationId` from different client/app → observe cross-session access. Passive: review SDK code `CopilotClient.cs:509-517` for `x-ms-d2e-experimental` header promotion logic
impact: Conversation hijack, history disclosure, active-session prompt injection, agent impersonation; CVSS 6.5-9.0
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list, and have concrete AUTH_HELPED verify_steps
[FINAL] 1. Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite (IDOR, `graph.microsoft.com/beta/copilot/agentRegistrations`, confidence 75, priority 8.55)
[FINAL] 2. Verified ID minting without admin role — backend gates only on Guest/Tenant flags (AUTH, `api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential`, confidence 70, priority 7.80)
[FINAL] 3. Copilot Studio D2E conversation-ID validation bypass — server does not validate conversation IDs (IDOR, `copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations`, confidence 65, priority 7.65)
[NEXT] PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All`) — tests enumeration of registrations (doc says no LIST but Collection type may allow); if 200 with array, confirms cross-registration visibility for PATCH targeting
[LEARN] ACCEPTED `oauth2.googleapis.com/tokeninfo` public introspection oracle: accepts `?access_token=` / `?id_token=` query params (no Authorization header), returns aud/scope/expiry — verified (no-token→400, malformed→400)
[LEARN] ACCEPTED `bughunters.google.com` root `/` hardening: HTTP 200, HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
[LEARN] ACCEPTED Graph API 405 anomaly @ `graph.microsoft.com/v1.0`: unauth HEAD/GET to `/v1.0`, `/me`, `/users` → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer — verified passively (4×405)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering @ `login.microsoftonline.com/common/oauth2/v2.0/authorize`: unsupported `response_type=token` returns HTTP 200 with embedded JS error code 700038 (iHttpErrorCode 400)
[RISK] google: 35 reason: GCP control-plane APIs all auth-gated (API key/OAuth); Firebase identitytoolkit 403 unregistered-callers gate; ADK issues are SDK-level not live Google service; no new unauthenticated attack surface discovered in this cycle; tokeninfo oracle is rate-limited public introspection only
[RISK] microsoft: 82 reason: Multiple high-value NEW surfaces with design-level gaps: Agent Registration (client-controlled ownership), Verified ID minting (missing admin gate), Copilot Studio D2E (conversation-ID not validated), Agent User `user_fic` subject knobs — all in active GA/preview transition, test-tenant reachable, Entra/Copilot identity plane = crown jewels
## 2026-08-07 17:33:16 UTC [google] (model nemotron3)
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential, 7.80, attack=8 business=9 tech=8 gate=5 cloud=7 fresh=9
[PRIO] copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations, 7.65, attack=8 business=8 tech=9 gate=4 cloud=7 fresh=10
[HYP] Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 75
reasoning: API accepts client-supplied `createdBy` on create; PATCH on any registration ID rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agentCard` with NO documented ownership enforcement beyond scope; Agent 365 GA transition makes this the canonical registration surface
evidence_needed: PATCH `/beta/copilot/agentRegistrations/{foreign_id}` with modified `ownerIds`/`agentCard` returns 200/204 and mutation persists (cross-tenant or cross-user)
verify_steps: AUTH_HELPED: In test tenant, create registration A as User1, note ID; as User2 (same tenant, different identity), PATCH that ID with attacker-controlled `agentCard` + `ownerIds`; GET to confirm mutation. Passive: GET `/beta/copilot/agentRegistrations` (no-list doc) to test enumeration
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, supply-chain compromise of any agent registration in tenant; CVSS 7.5-9.0
testability: AUTH_HELPED
[HYP] Verified ID minting without admin role — backend gates only on Guest/Tenant flags
class: AUTH
asset: api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential
confidence: 70
reasoning: SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2` scopes token to itself; backend contract shows only `GuestIsNotAllowedToIssueVerifiedId` / `TenantIsNotInAllowedToIssueVerifiedId` checks — no admin/role validation visible in client schema (source map confirms); Entra Verified ID = high-value employee credential
evidence_needed: POST with low-priv user token (non-admin, non-guest, tenant allowed) returns 200/204 + credential; or 403 with error revealing missing admin check
verify_steps: AUTH_HELPED: In test tenant, acquire token for SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2` as regular member user (non-admin); POST `/api/issueVerifiedEmployeeCredential` (empty body per source map); observe response. Passive: download source map `main.4e6e3dc6.js.map` → extract request schema for endpoint
impact: Unprivileged user mints Verified Employee Credential (DID-signed VC) — identity spoofing, access to Verified ID-gated resources; CVSS 7.1-9.1
testability: AUTH_HELPED
[HYP] Copilot Studio D2E conversation-ID validation bypass — server does not validate conversation IDs
class: IDOR
asset: https://{envhost}/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations
confidence: 65
reasoning: Official doc (copilotStudioWebChat.ts:40-44) states: "server does not validate conversation IDs. A non-existent GUID will silently create a new conversation under that ID"; `x-ms-conversation-id` header on start; modes: True S2S (app-only, agent=No-Auth) / OBO (Integrated auth); admin-consent scope `CopilotStudio.Copilots.Invoke`
evidence_needed: Start conversation with random GUID → observe silent creation; resume/hijack existing conversation via guessed/predicted GUID; cross-app resumption with stolen `conversationId`
verify_steps: AUTH_HELPED: In test tenant with D2E access, POST start with random `conversationId` → verify 201 + new conversation; GET/POST to same `conversationId` from different client/app → observe cross-session access. Passive: review SDK code `CopilotClient.cs:509-517` for `x-ms-d2e-experimental` header promotion logic
impact: Conversation hijack, history disclosure, active-session prompt injection, agent impersonation; CVSS 6.5-9.0
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list, and have concrete AUTH_HELPED verify_steps
[FINAL] 1. Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 75, priority 8.55)
[FINAL] 2. Verified ID minting without admin role — backend gates only on Guest/Tenant flags (AUTH, api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential, confidence 70, priority 7.80)
[FINAL] 3. Copilot Studio D2E conversation-ID validation bypass — server does not validate conversation IDs (IDOR, copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations, confidence 65, priority 7.65)
[NEXT] PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All`) — tests enumeration of registrations (doc says no LIST but Collection type may allow); if 200 with array, confirms cross-registration visibility for PATCH targeting
[LEARN] ACCEPTED `oauth2.googleapis.com/tokeninfo` public introspection oracle: accepts `?access_token=` / `?id_token=` query params (no Authorization header), returns aud/scope/expiry — verified (no-token→400, malformed→400)
[LEARN] ACCEPTED `bughunters.google.com` root `/` hardening: HTTP 200, HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
[LEARN] ACCEPTED Graph API 405 anomaly @ `graph.microsoft.com/v1.0`: unauth HEAD/GET to `/v1.0`, `/me`, `/users` → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer — verified passively (4×405)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering @ `login.microsoftonline.com/common/oauth2/v2.0/authorize`: unsupported `response_type=token` returns HTTP 200 with embedded JS error code 700038 (iHttpErrorCode 400)
[RISK] google: 35 reason: GCP control-plane APIs all auth-gated (API key/OAuth); Firebase identitytoolkit 403 unregistered-callers gate; ADK issues are SDK-level not live Google service; no new unauthenticated attack surface discovered in this cycle; tokeninfo oracle is rate-limited public introspection only
[RISK] microsoft: 82 reason: Multiple high-value NEW surfaces with design-level gaps: Agent Registration (client-controlled ownership), Verified ID minting (missing admin gate), Copilot Studio D2E (conversation-ID not validated), Agent User `user_fic` subject knobs — all in active GA/preview transition, test-tenant reachable, Entra/Copilot identity plane = crown jewels
## 2026-08-07 18:28:25 UTC [google] (model nemotron3)
[NEW] NO_DELTA
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential, 7.80, attack=8 business=9 tech=8 gate=5 cloud=7 fresh=9
[PRIO] copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations, 7.65, attack=8 business=8 tech=9 gate=4 cloud=7 fresh=10
[PRIO] graph.microsoft.com/v1.0 (405 anomaly), 6.20, attack=7 business=5 tech=8 gate=10 cloud=6 fresh=8
[PRIO] login.microsoftonline.com/common/oauth2/v2.0/authorize (200 error rendering), 5.85, attack=6 business=4 tech=7 gate=10 cloud=5 fresh=8
[PRIO] oauth2.googleapis.com/tokeninfo (public introspection), 5.40, attack=6 business=4 tech=6 gate=10 cloud=4 fresh=7
[PRIO] graph.microsoft.com/v1.0/oauth2PermissionGrants (caller-chosen resourceId), 6.90, attack=8 business=7 tech=8 gate=5 cloud=7 fresh=9
[PRIO] login.microsoftonline.com (user_fic three-hop), 7.10, attack=8 business=8 tech=9 gate=4 cloud=8 fresh=9
[PRIO] powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId} (InvokeTool), 6.55, attack=7 business=6 tech=8 gate=4 cloud=7 fresh=9
[PRIO] graph.microsoft.com/beta/agentIdentityBlueprints (managerApplications), 6.30, attack=7 business=7 tech=7 gate=4 cloud=8 fresh=9
[HYP] Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 75
reasoning: API accepts client-supplied `createdBy` on create; PATCH on any registration ID rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agentCard` with NO documented ownership enforcement beyond scope; Agent 365 GA transition makes this the canonical registration surface
evidence_needed: PATCH `/beta/copilot/agentRegistrations/{foreign_id}` with modified `ownerIds`/`agentCard` returns 200/204 and mutation persists (cross-tenant or cross-user)
verify_steps: AUTH_HELPED: In test tenant, create registration A as User1, note ID; as User2 (same tenant, different identity), PATCH that ID with attacker-controlled `agentCard` + `ownerIds`; GET to confirm mutation. Passive: GET `/beta/copilot/agentRegistrations` (no-list doc) to test enumeration
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, supply-chain compromise of any agent registration in tenant; CVSS 7.5-9.0
testability: AUTH_HELPED
[HYP] Verified ID minting without admin role — backend gates only on Guest/Tenant flags
class: AUTH
asset: api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential
confidence: 70
reasoning: SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2` scopes token to itself; backend contract shows only `GuestIsNotAllowedToIssueVerifiedId` / `TenantIsNotInAllowedToIssueVerifiedId` checks — no admin/role validation visible in client schema (source map confirms); Entra Verified ID = high-value employee credential
evidence_needed: POST with low-priv user token (non-admin, non-guest, tenant allowed) returns 200/204 + credential; or 403 with error revealing missing admin check
verify_steps: AUTH_HELPED: In test tenant, acquire token for SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2` as regular member user (non-admin); POST `/api/issueVerifiedEmployeeCredential` (empty body per source map); observe response. Passive: download source map `main.4e6e3dc6.js.map` → extract request schema for endpoint
impact: Unprivileged user mints Verified Employee Credential (DID-signed VC) — identity spoofing, access to Verified ID-gated resources; CVSS 7.1-9.1
testability: AUTH_HELPED
[HYP] Copilot Studio D2E conversation-ID validation bypass — server does not validate conversation IDs
class: IDOR
asset: https://{envhost}/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations
confidence: 65
reasoning: Official doc (copilotStudioWebChat.ts:40-44) states: "server does not validate conversation IDs. A non-existent GUID will silently create a new conversation under that ID"; `x-ms-conversation-id` header on start; modes: True S2S (app-only, agent=No-Auth) / OBO (Integrated auth); admin-consent scope `CopilotStudio.Copilots.Invoke`
evidence_needed: Start conversation with random GUID → observe silent creation; resume/hijack existing conversation via guessed/predicted GUID; cross-app resumption with stolen `conversationId`
verify_steps: AUTH_HELPED: In test tenant with D2E access, POST start with random `conversationId` → verify 201 + new conversation; GET/POST to same `conversationId` from different client/app → observe cross-session access. Passive: review SDK code `CopilotClient.cs:509-517` for `x-ms-d2e-experimental` header promotion logic
impact: Conversation hijack, history disclosure, active-session prompt injection, agent impersonation; CVSS 6.5-9.0
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list, and have concrete AUTH_HELPED verify_steps
[FINAL] 1. Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 75, priority 8.55)
[FINAL] 2. Verified ID minting without admin role — backend gates only on Guest/Tenant flags (AUTH, api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential, confidence 70, priority 7.80)
[FINAL] 3. Copilot Studio D2E conversation-ID validation bypass — server does not validate conversation IDs (IDOR, copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations, confidence 65, priority 7.65)
[NEXT] PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All`) — tests enumeration of registrations (doc says no LIST but Collection type may allow); if 200 with array, confirms cross-registration visibility for PATCH targeting
[LEARN] ACCEPTED `oauth2.googleapis.com/tokeninfo` public introspection oracle: accepts `?access_token=` / `?id_token=` query params (no Authorization header), returns aud/scope/expiry — verified (no-token→400, malformed→400)
[LEARN] ACCEPTED `bughunters.google.com` root `/` hardening: HTTP 200, HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
[LEARN] ACCEPTED Graph API 405 anomaly @ `graph.microsoft.com/v1.0`: unauth HEAD/GET to `/v1.0`, `/me`, `/users` → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer — verified passively (4×405)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering @ `login.microsoftonline.com/common/oauth2/v2.0/authorize`: unsupported `response_type=token` returns HTTP 200 with embedded JS error code 700038 (iHttpErrorCode 400)
[RISK] google: 35 reason: GCP control-plane APIs all auth-gated (API key/OAuth); Firebase identitytoolkit 403 unregistered-callers gate; ADK issues are SDK-level not live Google service; no new unauthenticated attack surface discovered in this cycle; tokeninfo oracle is rate-limited public introspection only
[RISK] microsoft: 82 reason: Multiple high-value NEW surfaces with design-level gaps: Agent Registration (client-controlled ownership), Verified ID minting (missing admin gate), Copilot Studio D2E (conversation-ID not validated), Agent User `user_fic` subject knobs — all in active GA/preview transition, test-tenant reachable, Entra/Copilot identity plane = crown jewels
## 2026-08-07 18:48:13 UTC [google] (model nemotron3)
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential, 7.80, attack=8 business=9 tech=8 gate=5 cloud=7 fresh=9
[PRIO] copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations, 7.65, attack=8 business=8 tech=9 gate=4 cloud=7 fresh=10
[HYP] Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 75
reasoning: API accepts client-supplied `createdBy` on create; PATCH on any registration ID rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agentCard` with NO documented ownership enforcement beyond scope; Agent 365 GA transition makes this the canonical registration surface
evidence_needed: PATCH `/beta/copilot/agentRegistrations/{foreign_id}` with modified `ownerIds`/`agentCard` returns 200/204 and mutation persists (cross-tenant or cross-user)
verify_steps: AUTH_HELPED: In test tenant, create registration A as User1, note ID; as User2 (same tenant, different identity), PATCH that ID with attacker-controlled `agentCard` + `ownerIds`; GET to confirm mutation. Passive: GET `/beta/copilot/agentRegistrations` (no-list doc) to test enumeration
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, supply-chain compromise of any agent registration in tenant; CVSS 7.5-9.0
testability: AUTH_HELPED
[HYP] Verified ID minting without admin role — backend gates only on Guest/Tenant flags
class: AUTH
asset: api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential
confidence: 70
reasoning: SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2` scopes token to itself; backend contract shows only `GuestIsNotAllowedToIssueVerifiedId` / `TenantIsNotInAllowedToIssueVerifiedId` checks — no admin/role validation visible in client schema (source map confirms); Entra Verified ID = high-value employee credential
evidence_needed: POST with low-priv user token (non-admin, non-guest, tenant allowed) returns 200/204 + credential; or 403 with error revealing missing admin check
verify_steps: AUTH_HELPED: In test tenant, acquire token for SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2` as regular member user (non-admin); POST `/api/issueVerifiedEmployeeCredential` (empty body per source map); observe response. Passive: download source map `main.4e6e3dc6.js.map` → extract request schema for endpoint
impact: Unprivileged user mints Verified Employee Credential (DID-signed VC) — identity spoofing, access to Verified ID-gated resources; CVSS 7.1-9.1
testability: AUTH_HELPED
[HYP] Copilot Studio D2E conversation-ID validation bypass — server does not validate conversation IDs
class: IDOR
asset: https://{envhost}/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations
confidence: 65
reasoning: Official doc (copilotStudioWebChat.ts:40-44) states: "server does not validate conversation IDs. A non-existent GUID will silently create a new conversation under that ID"; `x-ms-conversation-id` header on start; modes: True S2S (app-only, agent=No-Auth) / OBO (Integrated auth); admin-consent scope `CopilotStudio.Copilots.Invoke`
evidence_needed: Start conversation with random GUID → observe silent creation; resume/hijack existing conversation via guessed/predicted GUID; cross-app resumption with stolen `conversationId`
verify_steps: AUTH_HELPED: In test tenant with D2E access, POST start with random `conversationId` → verify 201 + new conversation; GET/POST to same `conversationId` from different client/app → observe cross-session access. Passive: review SDK code `CopilotClient.cs:509-517` for `x-ms-d2e-experimental` header promotion logic
impact: Conversation hijack, history disclosure, active-session prompt injection, agent impersonation; CVSS 6.5-9.0
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list, and have concrete AUTH_HELPED verify_steps
[FINAL] 1. Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 75, priority 8.55)
[FINAL] 2. Verified ID minting without admin role — backend gates only on Guest/Tenant flags (AUTH, api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential, confidence 70, priority 7.80)
[FINAL] 3. Copilot Studio D2E conversation-ID validation bypass — server does not validate conversation IDs (IDOR, copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations, confidence 65, priority 7.65)
[NEXT] PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All`) — tests enumeration of registrations (doc says no LIST but Collection type may allow); if 200 with array, confirms cross-registration visibility for PATCH targeting
[LEARN] ACCEPTED `oauth2.googleapis.com/tokeninfo` public introspection oracle: accepts `?access_token=` / `?id_token=` query params (no Authorization header), returns aud/scope/expiry — verified (no-token→400, malformed→400)
[LEARN] ACCEPTED `bughunters.google.com` root `/` hardening: HTTP 200, HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
[LEARN] ACCEPTED Graph API 405 anomaly @ `graph.microsoft.com/v1.0`: unauth HEAD/GET to `/v1.0`, `/me`, `/users` → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer — verified passively (4×405)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering @ `login.microsoftonline.com/common/oauth2/v2.0/authorize`: unsupported `response_type=token` returns HTTP 200 with embedded JS error code 700038 (iHttpErrorCode 400)
[LEARN] No new proving-dead classes this cycle
[RISK] google: 35 reason: GCP control-plane APIs all auth-gated (API key/OAuth); Firebase identitytoolkit 403 unregistered-callers gate; ADK issues are SDK-level not live Google service; no new unauthenticated attack surface discovered in this cycle; tokeninfo oracle is rate-limited public introspection only
[RISK] microsoft: 82 reason: Multiple high-value NEW surfaces with design-level gaps: Agent Registration (client-controlled ownership), Verified ID minting (missing admin gate), Copilot Studio D2E (conversation-ID not validated), Agent User `user_fic` subject knobs — all in active GA/preview transition, test-tenant reachable, Entra/Copilot identity plane = crown jewels
