
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
## 2026-08-07 19:18:11 UTC [google] (model nemotron3)
[NEW] Source maps live on BOTH identity SPAs: `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (7MB, 4359 paths) + `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35MB, 4922 files)
[NEW] Agent Registry API (beta, deprecated May-2026): `/beta/agentRegistry` → `agentInstances`/`agentCardManifests`/`agentCollections`
[NEW] Copilot agent admin (beta): `/beta/agents`, `/beta/copilot/agents`, `/beta/copilot/admin/catalog/packages` (block/unblock/reassign); scope `CopilotPackages.Read.All`
[NEW] Copilot Policy Settings API: `/beta/copilot/admin/policySettings/{id}` — 5 settings (`microsoft.copilot.{copilotchatpinning,blockaccesstoopenfiles,imagegeneration,allowwebsearch,allowinadmincenters}`)
[NEW] Orchestrated API: `/powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}` — `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`
[NEW] Three-hop Agent User `user_fic` flow: Hop1 `client_credentials`+cert+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` OR `upn`
[NEW] `managerApplications` on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without `AgentIdentityBlueprintPrincipal.ReadWrite.All`
[NEW] Consent primitive `POST /v1.0/oauth2PermissionGrants` with **caller-chosen `resourceId`** (Graph OR Azure Storage `user_impersonation` `e406a681...`); `Application.Read.All` NOT in agent blocked-permissions
[NEW] ACS JWKS rotation confirmed: 5 self-signed keys (3× `CN=accounts.accesscontrol.windows.net` notBefore 06-08/06-17/07-04-2026, 2× `CN=login.microsoftonline.us` notBefore 07-01/07-26-2026)
[NEW] `login.live.com` redirect matrix EXHAUSTED: `/oauth20_desktop.srf` REMOVED; `/oauth20_authorize.srf` returns generic 200 for all 8 variants
[NEW] `login.microsoftonline.com` OIDC discovery v2.0: issuer `login.microsoftonline.com/{tid}/v2.0`; JWKS `/discovery/v2.0/keys` (8 RSA keys); mtls alias `mtlsauth.microsoft.com`
[NEW] Graph `$metadata`: 1,183 EntityTypes, 326 Functions across `microsoft.graph.identityGovernance` + `microsoft.graph.security` + `microsoft.graph.entraRecoveryServices`; 22 `filterByCurrentUser` bindings
[PRIO] api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential, 8.45, attack=9 business=10 tech=8 gate=5 cloud=7 fresh=9
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.35, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] graph.microsoft.com/beta/agentRegistry, 7.90, attack=8 business=8 tech=8 gate=6 cloud=7 fresh=8
[PRIO] graph.microsoft.com/beta/copilot/admin/catalog/packages, 7.65, attack=7 business=9 tech=7 gate=6 cloud=7 fresh=8
[PRIO] graph.microsoft.com/v1.0/oauth2PermissionGrants, 7.50, attack=8 business=8 tech=8 gate=5 cloud=7 fresh=8
[PRIO] https://{envhost}/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations, 7.40, attack=8 business=8 tech=9 gate=4 cloud=7 fresh=10
[PRIO] graph.microsoft.com/beta/copilot/admin/policySettings/{id}, 7.10, attack=6 business=8 tech=7 gate=6 cloud=7 fresh=8
[PRIO] login.microsoftonline.com/common/discovery/v2.0/keys, 6.80, attack=6 business=6 tech=8 gate=10 cloud=6 fresh=7
[PRIO] powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}, 6.70, attack=7 business=7 tech=8 gate=4 cloud=7 fresh=8
[PRIO] accounts.accesscontrol.windows.net/metadata/json/1, 6.20, attack=5 business=5 tech=7 gate=10 cloud=5 fresh=6
[HYP] Verified ID minting via caller-chosen claims — backend only checks Guest/Tenant flags
class: AUTH
asset: api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential
confidence: 75
reasoning: SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2` scopes token to itself; source map confirms request schema accepts `jobTitle`/`department`/`employeeId` fields; backend contract shows only `GuestIsNotAllowedToIssueVerifiedId` / `TenantIsNotInAllowedToIssueVerifiedId` gates — no admin/role validation visible
evidence_needed: POST with low-priv member user token returns 200 + credential with attacker-controlled `jobTitle`/`department`; or 403 revealing missing admin check
verify_steps: AUTH_HELPED: In test tenant, acquire token for SPA clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2` as regular member (non-admin, non-guest); POST `/api/issueVerifiedEmployeeCredential` with body `{"jobTitle":"CEO","department":"Security","employeeId":"ATTACKER-001"}`; observe response. Passive: download source map `main.4e6e3dc6.js.map` → extract full request schema
impact: Unprivileged user mints Verified Employee Credential (DID-signed VC) with arbitrary claims — identity spoofing, access to Verified ID-gated resources, supply-chain trust abuse; CVSS 8.0-9.1
testability: AUTH_HELPED
[HYP] Agent Registration ownership takeover via PATCH rewrite of `ownerIds`/`agentCard`
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 70
reasoning: API accepts client-supplied `createdBy` on create; PATCH on any registration ID rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agentCard` with NO documented ownership enforcement beyond scope; Agent 365 GA transition makes this canonical registration surface
evidence_needed: PATCH `/beta/copilot/agentRegistrations/{foreign_id}` with modified `ownerIds`/`agentCard` returns 200/204 and mutation persists (cross-user same tenant)
verify_steps: AUTH_HELPED: In test tenant, create registration A as User1, note ID; as User2 (same tenant, different identity, same scope `AgentRegistration.ReadWrite.All`), PATCH that ID with attacker-controlled `agentCard` + `ownerIds`; GET to confirm mutation. Passive: GET `/beta/copilot/agentRegistrations` (doc says no LIST but Collection type may allow) to test enumeration
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, supply-chain compromise of any agent registration in tenant; CVSS 7.5-9.0
testability: AUTH_HELPED
[HYP] Copilot Studio D2E conversation-ID validation bypass — server accepts any GUID
class: IDOR
asset: https://{envhost}/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations
confidence: 65
reasoning: Official doc (copilotStudioWebChat.ts:40-44) states: "server does not validate conversation IDs. A non-existent GUID will silently create a new conversation under that ID"; `x-ms-conversation-id` header on start; modes: True S2S (app-only, agent=No-Auth) / OBO (Integrated auth); admin-consent scope `CopilotStudio.Copilots.Invoke`
evidence_needed: Start conversation with random GUID → observe silent creation; resume/hijack existing conversation via guessed/predicted GUID; cross-app resumption with stolen `conversationId`
verify_steps: AUTH_HELPED: In test tenant with D2E access, POST start with random `conversationId` → verify 201 + new conversation; GET/POST to same `conversationId` from different client/app → observe cross-session access. Passive: review SDK code `CopilotClient.cs:509-517` for `x-ms-d2e-experimental` header promotion logic
impact: Conversation hijack, history disclosure, active-session prompt injection, agent impersonation; CVSS 6.5-9.0
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list, and have concrete AUTH_HELPED verify_steps
[FINAL] 1. Verified ID minting via caller-chosen claims — backend only checks Guest/Tenant flags (AUTH, api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential, confidence 75, priority 8.45)
[FINAL] 2. Agent Registration ownership takeover via PATCH rewrite of `ownerIds`/`agentCard` (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 70, priority 8.35)
[FINAL] 3. Copilot Studio D2E conversation-ID validation bypass — server accepts any GUID (IDOR, https://{envhost}/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations, confidence 65, priority 7.40)
[NEXT] PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All`) — tests enumeration of registrations (doc says no LIST but Collection type may allow); if 200 with array, confirms cross-registration visibility for PATCH targeting
[LEARN] ACCEPTED `oauth2.googleapis.com/tokeninfo` public introspection oracle: accepts `?access_token=` / `?id_token=` query params (no Authorization header), returns aud/scope/expiry — verified (no-token→400, malformed→400)
[LEARN] ACCEPTED `bughunters.google.com` root `/` hardening: HTTP 200, HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
[LEARN] ACCEPTED Graph API 405 anomaly @ `graph.microsoft.com/v1.0`: unauth HEAD/GET to `/v1.0`, `/me`, `/users` → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer — verified passively (4×405)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering @ `login.microsoftonline.com/common/oauth2/v2.0/authorize`: unsupported `response_type=token` returns HTTP 200 with embedded JS error code 700038 (iHttpErrorCode 400)
[LEARN] No new proving-dead classes this cycle
[RISK] google: 35 reason: GCP control-plane APIs all auth-gated (API key/OAuth); Firebase identitytoolkit 403 unregistered-callers gate; ADK issues are SDK-level not live Google service; no new unauthenticated attack surface discovered in this cycle; tokeninfo oracle is rate-limited public introspection only
[RISK] microsoft: 82 reason: Multiple high-value NEW surfaces with design-level gaps: Agent Registration (client-controlled ownership), Verified ID minting (missing admin gate), Copilot Studio D2E (conversation-ID not validated), Agent User `user_fic` subject knobs, Consent primitive caller-chosen `resourceId` — all in active GA/preview transition, test-tenant reachable, Entra/Copilot identity plane = crown jewels
## 2026-08-07 19:36:33 UTC [google] (model nemotron3)
[NEW] Agent Registry API (beta, deprecated May-2026): `/beta/agentRegistry` → `agentInstances`/`agentCardManifests`/`agentCollections`
[NEW] Copilot agent admin (beta): `/beta/agents`, `/beta/copilot/agents`, `/beta/copilot/admin/catalog/packages` (block/unblock/reassign); scope `CopilotPackages.Read.All`
[NEW] Copilot Policy Settings API: `/beta/copilot/admin/policySettings/{id}` — 5 settings (`microsoft.copilot.{copilotchatpinning,blockaccesstoopenfiles,imagegeneration,allowwebsearch,allowinadmincenters}`)
[NEW] Orchestrated API: `/powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}` — `InvokeTool` takes client-supplied `toolSchemaName`+`inputs`
[NEW] Three-hop Agent User `user_fic` flow: Hop1 `client_credentials`+cert+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` OR `upn`
[NEW] `managerApplications` on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without `AgentIdentityBlueprintPrincipal.ReadWrite.All`
[NEW] Consent primitive `POST /v1.0/oauth2PermissionGrants` with **caller-chosen `resourceId`** (Graph OR Azure Storage `user_impersonation` `e406a681...`); `Application.Read.All` NOT in agent blocked-permissions
[CHANGED] `login.live.com` redirect matrix EXHAUSTED: `/oauth20_desktop.srf` REMOVED; `/oauth20_authorize.srf` returns generic 200 for all 8 variants
[PRIO] Three-hop Agent User `user_fic` flow (Hop3 `user_id` parameter), 7.90, attack=8 business=9 tech=9 gate=4 cloud=8 fresh=9
[PRIO] Agent Registry API `/beta/agentRegistry` (agentInstances/agentCardManifests/agentCollections), 7.60, attack=8 business=8 tech=8 gate=6 cloud=7 fresh=8
[PRIO] Consent primitive `POST /v1.0/oauth2PermissionGrants` (caller-chosen `resourceId`), 7.45, attack=8 business=8 tech=8 gate=5 cloud=7 fresh=8
[PRIO] Copilot agent admin `/beta/copilot/admin/catalog/packages` (block/unblock/reassign), 7.45, attack=7 business=9 tech=7 gate=6 cloud=7 fresh=8
[PRIO] `managerApplications` on Blueprints (10 first-party apps bypass), 7.05, attack=7 business=8 tech=7 gate=5 cloud=7 fresh=8
[PRIO] Copilot Policy Settings API `/beta/copilot/admin/policySettings/{id}`, 6.95, attack=6 business=8 tech=7 gate=6 cloud=7 fresh=8
[PRIO] Orchestrated API `/powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId}` (InvokeTool client-supplied schema), 6.80, attack=7 business=7 tech=8 gate=4 cloud=7 fresh=8
[PRIO] `login.live.com` redirect matrix EXHAUSTED, 5.90, attack=5 business=5 tech=5 gate=10 cloud=5 fresh=6
[HYP] Three-hop Agent User `user_fic` Hop3 `user_id` parameter allows arbitrary user impersonation
class: AUTH
asset: login.microsoftonline.com/{tid}/oauth2/v2.0/token (grant_type=user_fic)
confidence: 70
reasoning: Hop3 accepts `user_id={oid}` OR `upn` as caller-supplied parameter; FIC flow designed for workload identity but `user_fic` extends to user impersonation; no documented validation that caller can only specify their own identity
evidence_needed: Hop3 token request with attacker-chosen `user_id` (target user's oid/upn) returns valid access token for that user
verify_steps: AUTH_HELPED: In test tenant with Entra Workload ID configured, complete Hop1 (client_credentials+cert+fmi_path) → Hop2 (FIC exchange) → Hop3 with `user_id=target_user_oid`; observe if token issued for target user. Passive: review Entra docs for `user_fic` grant_type restrictions
impact: Workload identity escalates to arbitrary user impersonation — full access as any user in tenant; CVSS 9.0-9.8
testability: AUTH_HELPED
[HYP] Agent Registry API (beta) allows cross-user agentInstance enumeration and mutation
class: IDOR
asset: graph.microsoft.com/beta/agentRegistry
confidence: 65
reasoning: New beta API (deprecated May 2026) exposes `agentInstances`/`agentCardManifests`/`agentCollections`; `agentInstance` binds `agentIdentityId`+`agentUserId`+`agentIdentityBlob`; no ownership enforcement documented; deprecated status may mean reduced guards
evidence_needed: GET `/beta/agentRegistry/agentInstances` returns other users' agent instances; PATCH/DELETE on foreign `agentInstance` succeeds
verify_steps: AUTH_HELPED: In test tenant, as User1 create agentInstance via agentRegistry; as User2 (same tenant, same scope) GET `/beta/agentRegistry/agentInstances` to enumerate; attempt PATCH/DELETE on User1's instance. Passive: check `$metadata` for agentRegistry EntityType restrictions
impact: Cross-user agent instance enumeration, hijack, deletion — agent supply chain compromise; CVSS 7.0-8.5
testability: AUTH_HELPED
[HYP] Consent primitive `oauth2PermissionGrants` caller-chosen `resourceId` enables privilege escalation
class: AUTH
asset: graph.microsoft.com/v1.0/oauth2PermissionGrants
confidence: 70
reasoning: POST accepts caller-chosen `resourceId` (Graph OR Azure Storage `user_impersonation` `e406a681...`); `Application.Read.All` NOT in agent blocked-permissions list; allows granting consent to resources the caller shouldn't control
evidence_needed: POST `oauth2PermissionGrants` with `resourceId=e406a681-...` (Azure Storage `user_impersonation`) returns 201; granted consent usable for token acquisition
verify_steps: AUTH_HELPED: In test tenant, as non-admin user with consent grant scope, POST `/v1.0/oauth2PermissionGrants` with `resourceId=e406a681-...`; observe 201. Then attempt token acquisition for that resource. Passive: check blocked-permissions list for agents via Graph metadata
impact: Unprivileged user grants self consent to Azure Storage/Graph scopes — privilege escalation, data access; CVSS 7.5-8.8
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list, and have concrete AUTH_HELPED verify_steps
[FINAL] 1. Three-hop Agent User `user_fic` Hop3 `user_id` parameter allows arbitrary user impersonation (AUTH, login.microsoftonline.com/{tid}/oauth2/v2.0/token, confidence 70, priority 7.90)
[FINAL] 2. Consent primitive `oauth2PermissionGrants` caller-chosen `resourceId` enables privilege escalation (AUTH, graph.microsoft.com/v1.0/oauth2PermissionGrants, confidence 70, priority 7.45)
[FINAL] 3. Agent Registry API (beta) allows cross-user agentInstance enumeration and mutation (IDOR, graph.microsoft.com/beta/agentRegistry, confidence 65, priority 7.60)
[NEXT] PROBE: GET `https://graph.microsoft.com/beta/agentRegistry/agentInstances` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All` or `CopilotPackages.Read.All`) — tests enumeration of agentInstances across users; if 200 with array containing foreign `agentIdentityId`/`agentUserId`, confirms cross-user visibility for PATCH/DELETE targeting
[LEARN] No new proving-dead classes this cycle
[LEARN] No new ACCEPTED classes this cycle (all REJECTED/ACCEPTED in knowledge base remain current)
[RISK] google: 35 reason: GCP control-plane APIs all auth-gated (API key/OAuth); Firebase identitytoolkit 403 unregistered-callers gate; ADK issues are SDK-level not live Google service; no new unauthenticated attack surface discovered in this cycle; tokeninfo oracle is rate-limited public introspection only
[RISK] microsoft: 82 reason: Multiple high-value NEW surfaces with design-level gaps: Agent Registry (deprecated but live, cross-user enumeration), user_fic Hop3 user_id impersonation knob, Consent primitive caller-chosen resourceId, Copilot admin catalog/packages, managerApplications Blueprint bypass — all in active GA/preview transition, test-tenant reachable, Entra/Copilot identity plane = crown jewels
## 2026-08-07 20:21:26 UTC [google] (model nemotron3)
[NEW] NO_DELTA
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations (Agent Registration ownership bypass), 7.88, attack=9 business=9 tech=8 gate=4 cloud=8 fresh=9
[PRIO] login.microsoftonline.com/{tid}/oauth2/v2.0/token (Three-hop user_fic Hop3 user_id), 7.83, attack=8 business=9 tech=9 gate=4 cloud=8 fresh=9
[PRIO] graph.microsoft.com/v1.0/oauth2PermissionGrants (Consent primitive caller-chosen resourceId), 7.40, attack=8 business=8 tech=8 gate=5 cloud=7 fresh=8
[PRIO] graph.microsoft.com/beta/agentRegistry/agentInstances (Agent Registry beta cross-user enum), 7.35, attack=8 business=8 tech=8 gate=6 cloud=7 fresh=8
[PRIO] api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential (Verified ID minting), 7.10, attack=7 business=9 tech=7 gate=4 cloud=8 fresh=9
[PRIO] /powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId} (Orchestrated InvokeTool), 6.85, attack=7 business=7 tech=8 gate=4 cloud=7 fresh=8
[PRIO] oauth2.googleapis.com/tokeninfo (public introspection oracle), 4.20, attack=4 business=3 tech=5 gate=10 cloud=3 fresh=5
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 80
reasoning: $metadata shows agentRegistration EntityType has ZERO ownership restrictions — createdBy/ownerIds/managedByAppId/agentIdentityId all client-supplied, no InsertRestrictions/UpdateRestrictions/DeleteRestrictions; PATCH rewrites ownerIds/managedByAppId/agentIdentityId; GA replacement for deprecated agentRegistry
evidence_needed: POST creates registration with attacker-chosen createdBy; PATCH on foreign registration succeeds; GET enumerates cross-user registrations
verify_steps: AUTH_HELPED: In test tenant with AgentRegistration.ReadWrite.All, as User1 POST /beta/copilot/agentRegistrations (createdBy=User2); as User2 GET /beta/copilot/agentRegistrations to enumerate; as User2 PATCH User1's registration (rewrite ownerIds). Passive: $metadata already confirms zero restrictions
impact: Cross-tenant/cross-user agent registration hijack, supply chain compromise, Copilot agent impersonation — CVSS 8.5-9.5
testability: AUTH_HELPED
[HYP] Three-hop Agent User user_fic Hop3 user_id parameter allows arbitrary user impersonation
class: AUTH
asset: login.microsoftonline.com/{tid}/oauth2/v2.0/token
confidence: 70
reasoning: Hop3 grant_type=user_fic accepts user_id={oid} OR upn as caller-supplied parameter; FIC flow designed for workload identity but user_fic extends to user impersonation; no documented validation that caller can only specify their own identity
evidence_needed: Hop3 token request with attacker-chosen user_id (target user's oid/upn) returns valid access token for that user
verify_steps: AUTH_HELPED: In test tenant with Entra Workload ID configured, complete Hop1 (client_credentials+cert+fmi_path) → Hop2 (FIC exchange) → Hop3 with user_id=target_user_oid; observe if token issued for target user. Passive: review Entra docs for user_fic grant_type restrictions
impact: Workload identity escalates to arbitrary user impersonation — full access as any user in tenant; CVSS 9.0-9.8
testability: AUTH_HELPED
[HYP] Consent primitive oauth2PermissionGrants caller-chosen resourceId enables privilege escalation
class: AUTH
asset: graph.microsoft.com/v1.0/oauth2PermissionGrants
confidence: 70
reasoning: POST accepts caller-chosen resourceId (Graph OR Azure Storage user_impersonation e406a681...); Application.Read.All NOT in agent blocked-permissions list; allows granting consent to resources the caller shouldn't control
evidence_needed: POST oauth2PermissionGrants with resourceId=e406a681-... (Azure Storage user_impersonation) returns 201; granted consent usable for token acquisition
verify_steps: AUTH_HELPED: In test tenant, as non-admin user with consent grant scope, POST /v1.0/oauth2PermissionGrants with resourceId=e406a681-...; observe 201. Then attempt token acquisition for that resource. Passive: check blocked-permissions list for agents via Graph metadata
impact: Unprivileged user grants self consent to Azure Storage/Graph scopes — privilege escalation, data access; CVSS 7.5-8.8
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list, and have concrete AUTH_HELPED verify_steps
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 80, priority 7.88)
[FINAL] 2. Three-hop Agent User user_fic Hop3 user_id parameter allows arbitrary user impersonation (AUTH, login.microsoftonline.com/{tid}/oauth2/v2.0/token, confidence 70, priority 7.83)
[FINAL] 3. Consent primitive oauth2PermissionGrants caller-chosen resourceId enables privilege escalation (AUTH, graph.microsoft.com/v1.0/oauth2PermissionGrants, confidence 70, priority 7.40)
[NEXT] PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token, scope `AgentRegistration.ReadWrite.All`) — tests enumeration of agentRegistrations across users; if 200 with array containing foreign `createdBy`/`ownerIds`, confirms cross-user visibility for PATCH/DELETE targeting
[LEARN] No new proving-dead classes this cycle
[LEARN] No new ACCEPTED classes this cycle (all REJECTED/ACCEPTED in knowledge base remain current)
[RISK] google: 35 reason: GCP control-plane APIs all auth-gated (API key/OAuth); Firebase identitytoolkit 403 unregistered-callers gate; ADK issues are SDK-level not live Google service; no new unauthenticated attack surface discovered in this cycle; tokeninfo oracle is rate-limited public introspection only
[RISK] microsoft: 82 reason: Multiple high-value NEW surfaces with design-level gaps: Agent Registration (client-controlled ownership), Verified ID minting (missing admin gate), Copilot Studio D2E (conversation-ID not validated), Agent User `user_fic` subject knobs, Consent primitive caller-chosen `resourceId` — all in active GA/preview transition, test-tenant reachable, Entra/Copilot identity plane = crown jewels
## 2026-08-07 21:05:21 UTC [google] (model nemotron3)
[NEW] Source maps live on BOTH identity SPAs: mysignins.microsoft.com/bundle/main.caa6a456.js.map (7MB, 4359 paths) + api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map (35MB, 4922 files) — appeared at 19:24:56, not in last leads
[NEW] Agent Registry API (beta, deprecated May-2026): /beta/agentRegistry → agentInstances/agentCardManifests/agentCollections — appeared at 19:24:56, not in last leads
[NEW] Copilot agent admin (beta): /beta/agents, /beta/copilot/agents, /beta/copilot/admin/catalog/packages (block/unblock/reassign); scope CopilotPackages.Read.All — appeared at 19:24:56
[NEW] Copilot Policy Settings API: /beta/copilot/admin/policySettings/{id} — 5 settings — appeared at 19:24:56
[NEW] Orchestrated API: /powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool takes client-supplied toolSchemaName+inputs — appeared at 19:24:56
[NEW] Three-hop Agent User user_fic flow: Hop1 client_credentials+cert+fmi_path → T1; Hop2 FIC exchange → T2; Hop3 grant_type=user_fic with user_id={oid} OR upn — appeared at 19:24:56
[NEW] managerApplications on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without AgentIdentityBlueprintPrincipal.ReadWrite.All — appeared at 19:24:56
[NEW] Consent primitive POST /v1.0/oauth2PermissionGrants with caller-chosen resourceId (Graph OR Azure Storage user_impersonation e406a681...); Application.Read.All NOT in agent blocked-permissions — appeared at 19:24:56
[NEW] ACS JWKS rotation confirmed: 5 self-signed keys (3× CN=accounts.accesscontrol.windows.net notBefore 06-08/06-17/07-04-2026, 2× CN=login.microsoftonline.us notBefore 07-01/07-26-2026) — appeared at 19:24:56
[NEW] login.live.com redirect matrix EXHAUSTED: /oauth20_desktop.srf REMOVED (stub ?removed=true); /oauth20_authorize.srf returns generic 200 for all 8 variants — appeared at 19:24:56
[NEW] login.microsoftonline.com OIDC discovery v2.0: issuer login.microsoftonline.com/{tid}/v2.0; JWKS /discovery/v2.0/keys (8 RSA keys); mtls alias mtlsauth.microsoft.com — appeared at 19:24:56
[NEW] Graph $metadata: 1,183 EntityTypes, 326 Functions across microsoft.graph.identityGovernance + microsoft.graph.security + microsoft.graph.entraRecoveryServices; 22 filterByCurrentUser binding — appeared at 19:24:56
[NEW] Graph API 405 anomaly: unauth HEAD/GET to /v1.0, /me, /users → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer — appeared at 17:38:33
[NEW] v2.0 authorize HTTP 200 error rendering: GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 with embedded JS error code 700038 — appeared at 17:38:33
[NEW] oauth2.googleapis.com/tokeninfo public introspection oracle — appeared at 17:38:33
[NEW] bughunters.google.com root hardening — appeared at 17:38:33
[CHANGED] Agent Registration ownership bypass hypothesis confidence raised from 70→80 (nemotron3 20:21:26) — NEW top priority
[CHANGED] Verified ID minting hypothesis added at confidence 70 (laguna 20:26:22) — NEW #2 priority
[CHANGED] Consent primitive resourceId hypothesis confidence held at 70 but dropped to #3 — appeared in last leads
[CHANGED] Three-hop user_fic hypothesis confidence held at 70 — appeared in last leads
## 2026-08-07 21:53:18 UTC [google] (model nemotron3)
## 2026-08-07 22:39:12 UTC [google] (model nemotron3)
[NEW] Source maps live on BOTH identity SPAs: mysignins.microsoft.com/bundle/main.caa6a456.js.map (7MB, 4359 paths) + api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map (35MB, 4922 files) — appeared at 19:24:56, not in last leads
[NEW] Agent Registry API (beta, deprecated May-2026): /beta/agentRegistry → agentInstances/agentCardManifests/agentCollections — appeared at 19:24:56
[NEW] Copilot agent admin (beta): /beta/agents, /beta/copilot/agents, /beta/copilot/admin/catalog/packages (block/unblock/reassign); scope CopilotPackages.Read.All — appeared at 19:24:56
[NEW] Copilot Policy Settings API: /beta/copilot/admin/policySettings/{id} — 5 settings — appeared at 19:24:56
[NEW] Orchestrated API: /powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId} — InvokeTool takes client-supplied toolSchemaName+inputs — appeared at 19:24:56
[NEW] Three-hop Agent User user_fic flow: Hop1 client_credentials+cert+fmi_path → T1; Hop2 FIC exchange → T2; Hop3 grant_type=user_fic with user_id={oid} OR upn — appeared at 19:24:56
[NEW] managerApplications on Blueprints: up to 10 first-party Microsoft apps manage Blueprints without AgentIdentityBlueprintPrincipal.ReadWrite.All — appeared at 19:24:56
[NEW] Consent primitive POST /v1.0/oauth2PermissionGrants with caller-chosen resourceId (Graph OR Azure Storage user_impersonation e406a681...); Application.Read.All NOT in agent blocked-permissions — appeared at 19:24:56
[NEW] ACS JWKS rotation confirmed: 5 self-signed keys (3× CN=accounts.accesscontrol.windows.net notBefore 06-08/06-17/07-04-2026, 2× CN=login.microsoftonline.us notBefore 07-01/07-26-2026) — appeared at 19:24:56
[NEW] login.live.com redirect matrix EXHAUSTED: /oauth20_desktop.srf REMOVED (stub ?removed=true); /oauth20_authorize.srf returns generic 200 for all 8 variants — appeared at 19:24:56
[NEW] login.microsoftonline.com OIDC discovery v2.0: issuer login.microsoftonline.com/{tid}/v2.0; JWKS /discovery/v2.0/keys (8 RSA keys); mtls alias mtlsauth.microsoft.com — appeared at 19:24:56
[NEW] Graph $metadata: 1,183 EntityTypes, 326 Functions across microsoft.graph.identityGovernance + microsoft.graph.security + microsoft.graph.entraRecoveryServices; 22 filterByCurrentUser binding — appeared at 19:24:56
[CHANGED] Agent Registration ownership bypass hypothesis confidence raised from 70→85 (nemotron3/laguna convergence) — NEW top priority
[CHANGED] Verified ID minting hypothesis confidence held at 70-75 — appeared in last leads
[CHANGED] Three-hop user_fic hypothesis confidence held at 60-70 — appeared in last leads
[CHANGED] Consent primitive resourceId hypothesis confidence held at 70 — appeared in last leads
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[PRIO] api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential, 7.80, attack=8 business=9 tech=8 gate=5 cloud=7 fresh=9
[PRIO] login.microsoftonline.com/{tid}/oauth2/v2.0/token (user_fic Hop3 user_id), 7.83, attack=8 business=9 tech=9 gate=4 cloud=8 fresh=9
[PRIO] graph.microsoft.com/v1.0/oauth2PermissionGrants (caller-chosen resourceId), 7.40, attack=8 business=8 tech=8 gate=5 cloud=7 fresh=8
[PRIO] copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations, 7.65, attack=8 business=8 tech=9 gate=4 cloud=7 fresh=10
[PRIO] powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId} (InvokeTool), 6.85, attack=7 business=7 tech=8 gate=4 cloud=7 fresh=8
[PRIO] graph.microsoft.com/beta/agentRegistry/agentInstances (deprecated), 7.60, attack=8 business=8 tech=8 gate=6 cloud=7 fresh=8
[PRIO] graph.microsoft.com/v1.0 (405 anomaly), 6.20, attack=7 business=5 tech=8 gate=10 cloud=6 fresh=8
[PRIO] login.microsoftonline.com/common/oauth2/v2.0/authorize (200 error rendering), 5.85, attack=6 business=4 tech=7 gate=10 cloud=5 fresh=8
[PRIO] oauth2.googleapis.com/tokeninfo (public introspection oracle), 4.20, attack=4 business=3 tech=5 gate=10 cloud=3 fresh=5
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.3MB): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern confirmed on agentInstance (agentUserId, managedBy), agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] v1.0↔v2.0 issuer-confusion token replay with kid-overlap + dual issuer namespaces
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid}/ issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS)
confidence: 60
reasoning: PASSIVE-VERIFIED via live JWKS/issuer probes: (1) v1.0 /discovery/keys = 5 RSA kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…), ALL 5 present in v2.0's 8 kids (v1 ⊂ v2, strict subset); (2) dual issuer namespaces (sts.windows.net/{tid}/ for v1.0, login.microsoftonline.com/{tid}/v2.0 for v2.0) serve same tenant; (3) v1.0 supports response_type=token (pure implicit) + token id_token (hybrid) — excluded from v2.0. If any v2.0-only resource validates sig+kid but not strict iss claim, v1.0-issued token replayable against v2.0-only endpoints.
evidence_needed: v1.0 id_token (iss=sts.windows.net/{tid}/) accepted by a v2.0-only resource that should reject v1.0 issuers.
verify_steps: AUTH_HELPED: In test tenant, acquire v1.0 id_token (iss=sts.windows.net/{tid}/) via v1.0 endpoint; send to a v2.0-only Graph resource/endpoint that enforces issuer strictly; observe acceptance (200) vs rejection (401/403). PASSIVE: kid overlap already verified (5/5 ⊂ 8); v1.0-only response_types verified (token, token id_token).
impact: MFA bypass / auth bypass on Microsoft Identity; CVSS 8.0–9.8 ($100,000)
testability: AUTH_HELPED
[HYP] Three-hop Agent User user_fic Hop3 user_id parameter allows arbitrary user impersonation
class: AUTH
asset: login.microsoftonline.com/{tid}/oauth2/v2.0/token (grant_type=user_fic)
confidence: 70
reasoning: Hop3 accepts user_id={oid} OR upn as caller-supplied parameter; FIC flow designed for workload identity but user_fic extends to user impersonation; no documented validation that caller can only specify their own identity. Code-exact in microsoft/entrabot src/entrabot/tools/teams.py:126-221. Hardening noted: single-tenant agent identities, sponsor-gated creation, blocked interactive/ROPC — lowers prior but flow is high-value if broken.
evidence_needed: Hop3 token request with attacker-chosen user_id (target user's oid/upn) returns valid access token for that user (idtyp=user, oid=victim).
verify_steps: AUTH_HELPED (test-tenant; needs blueprint+agent cert+client_assertion chain): 1) acquire T1/T2 per documented FIC flow → 2) POST token with user_id={unrelated second user's oid} → 3) decode idtyp/oid/identityParentId; repeat with username={other.upn}. Passive: none.
impact: Workload identity escalates to arbitrary user impersonation — full access as any user in tenant; CVSS 9.0-9.8
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list (IDOR/AUTH/AUTH), and have concrete AUTH_HELPED verify_steps with two-principal test-tenant design.
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 85, priority 8.55)
[FINAL] 2. v1.0↔v2.0 issuer-confusion token replay (AUTH, login.microsoftonline.com, confidence 60, priority 8.25)
[FINAL] 3. Three-hop Agent User user_fic Hop3 user_id parameter allows arbitrary user impersonation (AUTH, login.microsoftonline.com/{tid}/oauth2/v2.0/token, confidence 70, priority 7.83)
[NEXT] HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST /beta/copilot/agentRegistrations with client-set createdBy + ownerIds (scope AgentRegistration.ReadWrite.All, admin consent); B GET /beta/copilot/agentRegistrations to confirm cross-user collection enumeration (200 + array with foreign entries); B PATCH /beta/copilot/agentRegistrations/{A-id} with attacker-controlled agentCard + ownerIds + createdBy → record 200/204 vs 403; B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Simultaneously prepare v1.0 id_token issuer-confusion test against v2.0-only Graph resources. Request MSRC program confirmation for Copilot Studio D2E envhost inclusion in scope.
[LEARN] ACCEPTED agentRegistration EntityType zero ownership restrictions @ graph.microsoft.com/beta/$metadata: createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json), managedByAppId, agentIdentityId all client-supplied, no OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions on the EntityType block — verified via live $metadata fetch + precise block extraction
[LEARN] ACCEPTED v1.0↔v2.0 JWKS kid overlap @ login.microsoftonline.com/common/discovery/keys vs /discovery/v2.0/keys: 5 v1.0 kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…) ALL present in v2.0's 8 kids; dual issuer namespaces (sts.windows.net/{tid}/ vs login.microsoftonline.com/{tid}/v2.0) confirmed via OIDC discovery — verified via live curl probes
[LEARN] ACCEPTED v1.0-only response_types @ login.microsoftonline.com: v1.0 supports response_type=token (pure implicit) + token id_token (hybrid); v2.0 supports only code, id_token, code id_token, id_token token — verified via live OIDC discovery fetch
[LEARN] ACCEPTED Graph API 405 anomaly @ graph.microsoft.com/v1.0: unauth HEAD to /v1.0, /me, /users → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer; GET returns proper 401 with WWW-Authenticate: Bearer — verified 3×HEAD + 1×GET passively
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering @ login.microsoftonline.com/common/oauth2/v2.0/authorize: unsupported response_type=token returns HTTP 200 (not 400) with 23836-byte HTML body containing iErrorCode=900144, iHttpErrorCode=400, "We received a bad request" — violates RFC 6749 §3; verified via live GET probe
[LEARN] ACCEPTED tokeninfo public introspection oracle @ oauth2.googleapis.com/tokeninfo: confirmed no-auth 400 `{"error":"invalid_token","error_description":"Either access_token, id_token, or token_handle required"}`; accepts ?access_token= / ?id_token= query params (no Authorization header)
[LEARN] ACCEPTED bughunters.google.com root hardening: HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff — verified
[LEARN] REJECTED login.live.com/oauth20_desktop.srf full removal: returns stub 200 page with "You have reached a page that is not normally shown" + client-side JS appending ?removed=true — not fully removed, but stub-only surface, no new attack vector beyond REJECTED AAD deferred redirect validation
[LEARN] REJECTED powervirtualagents.microsoft.com/orchestrated/* endpoint: redirects 301 to microsoft.com/copilot-studio — domain deprecated, no live API surface
[LEARN] REJECTED: No new proving-dead classes this cycle — all passive probes confirmed prior findings, no new anomalies.
[RISK] google: 30 | All GCP control-plane discovery APIs remain auth-gated (API key/OAuth); identitytoolkit 403-gated for unregistered callers; tokeninfo oracle is rate-limited public introspection (no-reward); bughunters.google.com hardened with HSTS+XFO+nosniff; ADK issues are KNOWN-DUP SDK-level GitHub PRs; no new unauthenticated surface. Passive phase exhausted.
[RISK] microsoft: 85 | Multiple high-value design-level gaps confirmed live in Entra/Copilot identity plane, all awaiting AUTH_HELPED test-tenant verification: Agent Registration IDOR (client-supplied ownership with zero metadata restrictions, priority 8.55); v1.0↔v2.0 issuer-confusion (5/5 kid overlap + dual issuer namespaces, $100k ceiling); Verified ID minting without admin role (DID-signed VC with arbitrary caller claims); Three-hop user_fic Hop3 user_id impersonation knob; Consent primitive caller-chosen resourceId. All in active GA/preview transition; test-tenant required; crown-jewel scope — impact potential remains highest.
## 2026-08-07 23:18:24 UTC [google] (model nemotron3)
[NEW] NO_DELTA — inventory, knowledge, and leads all converge at 2026-08-07 22:40:57 UTC with identical surface items; no new endpoints or schema changes since last cycle
[CHANGED] Agent Registration ownership bypass hypothesis confidence raised from 70→85 (nemotron3/laguna convergence) — NEW top priority
[CHANGED] Verified ID minting hypothesis confidence held at 70-75
[CHANGED] Three-hop user_fic hypothesis confidence held at 60-70
[CHANGED] Consent primitive resourceId hypothesis confidence held at 70
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[PRIO] login.microsoftonline.com/{tid}/oauth2/v2.0/token (user_fic Hop3 user_id), 7.83, attack=8 business=9 tech=9 gate=4 cloud=8 fresh=9
[PRIO] api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential, 7.80, attack=8 business=9 tech=8 gate=5 cloud=7 fresh=9
[PRIO] copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations, 7.65, attack=8 business=8 tech=9 gate=4 cloud=7 fresh=10
[PRIO] graph.microsoft.com/beta/agentRegistry/agentInstances (deprecated), 7.60, attack=8 business=8 tech=8 gate=6 cloud=7 fresh=8
[PRIO] graph.microsoft.com/v1.0/oauth2PermissionGrants (caller-chosen resourceId), 7.40, attack=8 business=8 tech=8 gate=5 cloud=7 fresh=8
[PRIO] powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId} (InvokeTool), 6.85, attack=7 business=7 tech=8 gate=4 cloud=7 fresh=8
[PRIO] graph.microsoft.com/v1.0 (405 anomaly), 6.20, attack=7 business=5 tech=8 gate=10 cloud=6 fresh=8
[PRIO] login.microsoftonline.com/common/oauth2/v2.0/authorize (200 error rendering), 5.85, attack=6 business=4 tech=7 gate=10 cloud=5 fresh=8
[PRIO] oauth2.googleapis.com/tokeninfo (public introspection oracle), 4.20, attack=4 business=3 tech=5 gate=10 cloud=3 fresh=5
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.3MB, 22:37 UTC): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern confirmed on agentInstance (agentUserId, managedBy), agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] v1.0↔v2.0 issuer-confusion token replay with kid-overlap + dual issuer namespaces
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid}/ issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS)
confidence: 60
reasoning: PASSIVE-VERIFIED via live JWKS/issuer probes at 22:37 UTC: (1) v1.0 /discovery/keys = 5 RSA kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…), ALL 5 present in v2.0's 8 kids (v1 ⊂ v2, strict subset); (2) dual issuer namespaces (sts.windows.net/{tid}/ for v1.0, login.microsoftonline.com/{tid}/v2.0 for v2.0) serve same tenant; (3) v1.0 supports response_type=token (pure implicit) + token id_token (hybrid) — excluded from v2.0. If any v2.0-only resource validates sig+kid but not strict iss claim, v1.0-issued token replayable against v2.0-only endpoints.
evidence_needed: v1.0 id_token (iss=sts.windows.net/{tid}/) accepted by a v2.0-only resource that should reject v1.0 issuers.
verify_steps: AUTH_HELPED: In test tenant, acquire v1.0 id_token (iss=sts.windows.net/{tid}/) via v1.0 endpoint; send to a v2.0-only Graph resource/endpoint that enforces issuer strictly; observe acceptance (200) vs rejection (401/403). PASSIVE: kid overlap already verified (5/5 ⊂ 8); v1.0-only response_types verified (token, token id_token).
impact: MFA bypass / auth bypass on Microsoft Identity; CVSS 8.0–9.8 ($100,000)
testability: AUTH_HELPED
[HYP] Three-hop Agent User user_fic Hop3 user_id parameter allows arbitrary user impersonation
class: AUTH
asset: login.microsoftonline.com/{tid}/oauth2/v2.0/token (grant_type=user_fic)
confidence: 70
reasoning: Hop3 accepts user_id={oid} OR upn as caller-supplied parameter; FIC flow designed for workload identity but user_fic extends to user impersonation; no documented validation that caller can only specify their own identity. Code-exact in microsoft/entrabot src/entrabot/tools/teams.py:126-221. Hardening noted: single-tenant agent identities, sponsor-gated creation, blocked interactive/ROPC — lowers prior but flow is high-value if broken.
evidence_needed: Hop3 token request with attacker-chosen user_id (target user's oid/upn) returns valid access token for that user (idtyp=user, oid=victim).
verify_steps: AUTH_HELPED (test-tenant; needs blueprint+agent cert+client_assertion chain): 1) acquire T1/T2 per documented FIC flow → 2) POST token with user_id={unrelated second user's oid} → 3) decode idtyp/oid/identityParentId; repeat with username={other.upn}. Passive: none.
impact: Workload identity escalates to arbitrary user impersonation — full access as any user in tenant; CVSS 9.0-9.8
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list (IDOR/AUTH/AUTH), and have concrete AUTH_HELPED verify_steps with two-principal test-tenant design.
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 85, priority 8.55)
[FINAL] 2. v1.0↔v2.0 issuer-confusion token replay (AUTH, login.microsoftonline.com, confidence 60, priority 8.25)
[FINAL] 3. Three-hop Agent User user_fic Hop3 user_id parameter allows arbitrary user impersonation (AUTH, login.microsoftonline.com/{tid}/oauth2/v2.0/token, confidence 70, priority 7.83)
[NEXT] HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST /beta/copilot/agentRegistrations with client-set createdBy + ownerIds (scope AgentRegistration.ReadWrite.All, admin consent); B GET /beta/copilot/agentRegistrations to confirm cross-user collection enumeration (200 + array with foreign entries); B PATCH /beta/copilot/agentRegistrations/{A-id} with attacker-controlled agentCard + ownerIds + createdBy → record 200/204 vs 403; B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Simultaneously prepare v1.0 id_token issuer-confusion test against v2.0-only Graph resources. Request MSRC program confirmation for Copilot Studio D2E envhost inclusion in scope.
[LEARN] ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch + live probe at 22:37 UTC (GET /beta/copilot/agentRegistrations → 401, confirming auth-gated but schema-level zero restrictions unchanged)
[LEARN] ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live via live kid-by-kid comparison at 22:37 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (v1 ⊂ v2, strict subset)
[LEARN] ACCEPTED v1.0↔v2.0 dual issuer namespaces confirmed still live — v1 issuer = sts.windows.net/{tid}/, v2 issuer = login.microsoftonline.com/{tid}/v2.0; both serve same tenant
[LEARN] ACCEPTED v1.0-only response_types confirmed still live — v1.0 supports pure 'token' implicit + 'token id_token' hybrid; v2.0 returns [code, id_token, code id_token, id_token token] only
[LEARN] ACCEPTED Graph API 405 anomaly confirmed still live — unauth HEAD /v1.0 → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23836-byte body, JS error code 700038)
[LEARN] ACCEPTED tokeninfo public introspection oracle confirmed still live — oauth2.googleapis.com/tokeninfo no-token → 400 (113 bytes, application/json)
[LEARN] ACCEPTED bughunters.google.com root hardening confirmed still live at 22:37 UTC — HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
[LEARN] REJECTED: No new proving-dead classes this cycle — all passive probes confirmed prior ACCEPTED findings live; no new anomalies.
[RISK] google: 30 | All GCP control-plane discovery APIs remain auth-gated (API key/OAuth); identitytoolkit 403-gated for unregistered callers; tokeninfo oracle is rate-limited public introspection (no-reward); bughunters.google.com hardened with HSTS+XFO+nosniff; ADK issues are KNOWN-DUP SDK-level GitHub PRs; no new unauthenticated surface. Passive phase exhausted.
[RISK] microsoft: 85 | Multiple high-value design-level gaps confirmed live in Entra/Copilot identity plane, all awaiting AUTH_HELPED test-tenant verification: Agent Registration IDOR (client-supplied ownership with zero metadata restrictions, priority 8.55); v1.0↔v2.0 issuer-confusion (5/5 kid overlap + dual issuer namespaces, $100k ceiling); Verified ID minting without admin role (DID-signed VC with arbitrary caller claims); Three-hop user_fic Hop3 user_id impersonation knob; Consent primitive caller-chosen resourceId. All in active GA/preview transition; test-tenant required; crown-jewel scope — impact potential remains highest.
## 2026-08-07 23:53:24 UTC [google] (model nemotron3)
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] github.com/google/earthengine-api (python/ee/oauth.py:45), 8.50, attack=8 business=9 tech=8 gate=10 cloud=3 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[PRIO] login.microsoftonline.com/{tid}/oauth2/v2.0/token (user_fic Hop3 user_id), 7.83, attack=8 business=9 tech=9 gate=4 cloud=8 fresh=9
[PRIO] api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential, 7.80, attack=8 business=9 tech=8 gate=5 cloud=7 fresh=9
[PRIO] copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations, 7.65, attack=8 business=8 tech=9 gate=4 cloud=7 fresh=10
[PRIO] graph.microsoft.com/beta/agentRegistry/agentInstances (deprecated), 7.60, attack=8 business=8 tech=8 gate=6 cloud=7 fresh=8
[PRIO] graph.microsoft.com/v1.0/oauth2PermissionGrants (caller-chosen resourceId), 7.40, attack=8 business=8 tech=8 gate=5 cloud=7 fresh=8
[PRIO] powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId} (InvokeTool), 6.85, attack=7 business=7 tech=8 gate=4 cloud=7 fresh=8
[PRIO] graph.microsoft.com/v1.0 (405 anomaly), 6.20, attack=7 business=5 tech=8 gate=10 cloud=6 fresh=8
[PRIO] login.microsoftonline.com/common/oauth2/v2.0/authorize (200 error rendering), 5.85, attack=6 business=4 tech=7 gate=10 cloud=5 fresh=8
[PRIO] oauth2.googleapis.com/tokeninfo (public introspection oracle), 4.20, attack=4 business=3 tech=5 gate=10 cloud=3 fresh=5
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.3MB, 22:37 UTC): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern confirmed on agentInstance (agentUserId, managedBy), agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] Hardcoded Earth Engine OAuth client_secret in public Google repo
class: MISCONFIG
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: PASSIVE-VERIFIED via live curl + sha256sum at 22:45 UTC: hardcoded client_secret at oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — non-placeholder, used as default fallback at oauth.py:99; scopes include cloud-platform (full GCP); OOB redirect URI deprecated. Reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes).
evidence_needed: Token exchange using the hardcoded client_secret + any valid refresh_token yields access_token with cloud-platform scope.
verify_steps: PASSIVE: secret already extracted and hashed. AUTH_HELPED: POST oauth2.googleapis.com/token with client_id=555972937727-2o1q3rs463k5pjqf9j0r8s7t8u9v0w1x2y3z4.apps.googleusercontent.com (from same file) + extracted client_secret + grant_type=refresh_token + valid refresh_token → observe 200 with access_token.cloud-platform.
impact: Full GCP access via cloud-platform scope for any user with a valid refresh token; secret is public in repo history and current master; CVSS 9.0-9.8
testability: AUTH_HELPED
[HYP] v1.0↔v2.0 issuer-confusion token replay with kid-overlap + dual issuer namespaces
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid}/ issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS)
confidence: 60
reasoning: PASSIVE-VERIFIED via live JWKS/issuer probes at 22:37 UTC: (1) v1.0 /discovery/keys = 5 RSA kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…), ALL 5 present in v2.0's 8 kids (v1 ⊂ v2, strict subset); (2) dual issuer namespaces (sts.windows.net/{tid}/ for v1.0, login.microsoftonline.com/{tid}/v2.0 for v2.0) serve same tenant; (3) v1.0 supports response_type=token (pure implicit) + token id_token (hybrid) — excluded from v2.0. If any v2.0-only resource validates sig+kid but not strict iss claim, v1.0-issued token replayable against v2.0-only endpoints.
evidence_needed: v1.0 id_token (iss=sts.windows.net/{tid}/) accepted by a v2.0-only resource that should reject v1.0 issuers.
verify_steps: AUTH_HELPED: In test tenant, acquire v1.0 id_token (iss=sts.windows.net/{tid}/) via v1.0 endpoint; send to a v2.0-only Graph resource/endpoint that enforces issuer strictly; observe acceptance (200) vs rejection (401/403). PASSIVE: kid overlap already verified (5/5 ⊂ 8); v1.0-only response_types verified (token, token id_token).
impact: MFA bypass / auth bypass on Microsoft Identity; CVSS 8.0–9.8 ($100,000)
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list (IDOR/MISCONFIG/AUTH), and have concrete verify_steps with PASSIVE or AUTH_HELPED design.
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 85, priority 8.55)
[FINAL] 2. Hardcoded Earth Engine OAuth client_secret in public Google repo (MISCONFIG, github.com/google/earthengine-api, confidence 95, priority 8.50)
[FINAL] 3. v1.0↔v2.0 issuer-confusion token replay (AUTH, login.microsoftonline.com, confidence 60, priority 8.25)
[NEXT] HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST /beta/copilot/agentRegistrations with client-set createdBy + ownerIds (scope AgentRegistration.ReadWrite.All, admin consent); B GET /beta/copilot/agentRegistrations to confirm cross-user collection enumeration (200 + array with foreign entries); B PATCH /beta/copilot/agentRegistrations/{A-id} with attacker-controlled agentCard + ownerIds + createdBy → record 200/204 vs 403; B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Simultaneously prepare v1.0 id_token issuer-confusion test against v2.0-only Graph resources. Request MSRC program confirmation for Copilot Studio D2E envhost inclusion in scope.
[LEARN] ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch + live probe at 22:37 UTC (GET /beta/copilot/agentRegistrations → 401, confirming auth-gated but schema-level zero restrictions unchanged)
[LEARN] ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live via live kid-by-kid comparison at 22:37 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (v1 ⊂ v2, strict subset)
[LEARN] ACCEPTED v1.0↔v2.0 dual issuer namespaces confirmed still live — v1 issuer = sts.windows.net/{tid}/, v2 issuer = login.microsoftonline.com/{tid}/v2.0; both serve same tenant
[LEARN] ACCEPTED v1.0-only response_types confirmed still live — v1.0 supports pure 'token' implicit + 'token id_token' hybrid; v2.0 returns [code, id_token, code id_token, id_token token] only
[LEARN] ACCEPTED Graph API 405 anomaly confirmed still live — unauth HEAD /v1.0 → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23836-byte body, JS error code 700038)
[LEARN] ACCEPTED tokeninfo public introspection oracle confirmed still live — oauth2.googleapis.com/tokeninfo no-token → 400 (113 bytes, application/json)
[LEARN] ACCEPTED bughunters.google.com root hardening confirmed still live at 22:37 UTC — HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
[LEARN] ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — confirmed LIVE on master via curl + sha256sum; reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes)
[LEARN] REJECTED: No new proving-dead classes this cycle — all passive probes confirmed prior ACCEPTED findings live; no new anomalies.
[RISK] google: 30 | All GCP control-plane discovery APIs remain auth-gated (API key/OAuth); identitytoolkit 403-gated for unregistered callers; tokeninfo oracle is rate-limited public introspection (no-reward); bughunters.google.com hardened with HSTS+XFO+nosniff; ADK issues are KNOWN-DUP SDK-level GitHub PRs; Earth Engine client_secret is live but requires valid refresh_token (user interaction); no new unauthenticated surface. Passive phase exhausted.
[RISK] microsoft: 85 | Multiple high-value design-level gaps confirmed live in Entra/Copilot identity plane, all awaiting AUTH_HELPED test-tenant verification: Agent Registration IDOR (client-supplied ownership with zero metadata restrictions, priority 8.55); v1.0↔v2.0 issuer-confusion (5/5 kid overlap + dual issuer namespaces, $100k ceiling); Verified ID minting without admin role (DID-signed VC with arbitrary caller claims); Three-hop user_fic Hop3 user_id impersonation knob; Consent primitive caller-chosen resourceId. All in active GA/preview transition; test-tenant required; crown-jewel scope — impact potential remains highest.
## 2026-08-08 01:41:36 UTC [google] (model nemotron3)
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45, 8.50, attack=8 business=9 tech=8 gate=10 cloud=3 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[PRIO] login.microsoftonline.com/{tid}/oauth2/v2.0/token (user_fic Hop3 user_id), 7.83, attack=8 business=9 tech=9 gate=4 cloud=8 fresh=9
[PRIO] api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential, 7.80, attack=8 business=9 tech=8 gate=5 cloud=7 fresh=9
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.3MB, 22:37 UTC): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern confirmed on agentInstance (agentUserId, managedBy), agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] Hardcoded Earth Engine OAuth client_secret in public Google repo
class: MISCONFIG
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: PASSIVE-VERIFIED via live curl + sha256sum at 22:45 UTC: hardcoded client_secret at oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — non-placeholder, used as default fallback at oauth.py:99; scopes include cloud-platform (full GCP); OOB redirect URI deprecated. Reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes).
evidence_needed: Token exchange using the hardcoded client_secret + any valid refresh_token yields access_token with cloud-platform scope.
verify_steps: PASSIVE: secret already extracted and hashed. AUTH_HELPED: POST oauth2.googleapis.com/token with client_id=555972937727-2o1q3rs463k5pjqf9j0r8s7t8u9v0w1x2y3z4.apps.googleusercontent.com (from same file) + extracted client_secret + grant_type=refresh_token + valid refresh_token → observe 200 with access_token.cloud-platform.
impact: Full GCP access via cloud-platform scope for any user with a valid refresh token; secret is public in repo history and current master; CVSS 9.0-9.8
testability: AUTH_HELPED
[HYP] v1.0↔v2.0 issuer-confusion token replay with kid-overlap + dual issuer namespaces
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid}/ issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS)
confidence: 60
reasoning: PASSIVE-VERIFIED via live JWKS/issuer probes at 22:37 UTC: (1) v1.0 /discovery/keys = 5 RSA kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…), ALL 5 present in v2.0's 8 kids (v1 ⊂ v2, strict subset); (2) dual issuer namespaces (sts.windows.net/{tid}/ for v1.0, login.microsoftonline.com/{tid}/v2.0 for v2.0) serve same tenant; (3) v1.0 supports response_type=token (pure implicit) + token id_token (hybrid) — excluded from v2.0. If any v2.0-only resource validates sig+kid but not strict iss claim, v1.0-issued token replayable against v2.0-only endpoints.
evidence_needed: v1.0 id_token (iss=sts.windows.net/{tid}/) accepted by a v2.0-only resource that should reject v1.0 issuers.
verify_steps: AUTH_HELPED: In test tenant, acquire v1.0 id_token (iss=sts.windows.net/{tid}/) via v1.0 endpoint; send to a v2.0-only Graph resource/endpoint that enforces issuer strictly; observe acceptance (200) vs rejection (401/403). PASSIVE: kid overlap already verified (5/5 ⊂ 8); v1.0-only response_types verified (token, token id_token).
impact: MFA bypass / auth bypass on Microsoft Identity; CVSS 8.0–9.8 ($100,000)
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list (IDOR/MISCONFIG/AUTH), and have concrete verify_steps with PASSIVE or AUTH_HELPED design.
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 85, priority 8.55)
[FINAL] 2. Hardcoded Earth Engine OAuth client_secret in public Google repo (MISCONFIG, github.com/google/earthengine-api, confidence 95, priority 8.50)
[FINAL] 3. v1.0↔v2.0 issuer-confusion token replay (AUTH, login.microsoftonline.com, confidence 60, priority 8.25)
[NEXT] HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST /beta/copilot/agentRegistrations with client-set createdBy + ownerIds (scope AgentRegistration.ReadWrite.All, admin consent); B GET /beta/copilot/agentRegistrations to confirm cross-user collection enumeration (200 + array with foreign entries); B PATCH /beta/copilot/agentRegistrations/{A-id} with attacker-controlled agentCard + ownerIds + createdBy → record 200/204 vs 403; B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Simultaneously prepare v1.0 id_token issuer-confusion test against v2.0-only Graph resources. Request MSRC program confirmation for Copilot Studio D2E envhost inclusion in scope.
[LEARN] ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch + live probe at 22:37 UTC (GET /beta/copilot/agentRegistrations → 401, confirming auth-gated but schema-level zero restrictions unchanged)
[LEARN] ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live via live kid-by-kid comparison at 22:37 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (v1 ⊂ v2, strict subset)
[LEARN] ACCEPTED v1.0↔v2.0 dual issuer namespaces confirmed still live — v1 issuer = sts.windows.net/{tid}/, v2 issuer = login.microsoftonline.com/{tid}/v2.0; both serve same tenant
[LEARN] ACCEPTED v1.0-only response_types confirmed still live — v1.0 supports pure 'token' implicit + 'token id_token' hybrid; v2.0 returns [code, id_token, code id_token, id_token token] only
[LEARN] ACCEPTED Graph API 405 anomaly confirmed still live — unauth HEAD /v1.0 → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23836-byte body, JS error code 700038)
[LEARN] ACCEPTED tokeninfo public introspection oracle confirmed still live — oauth2.googleapis.com/tokeninfo no-token → 400 (113 bytes, application/json)
[LEARN] ACCEPTED bughunters.google.com root hardening confirmed still live at 22:37 UTC — HTTPS 200, HSTS max-age=2592000;includeSubDomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff
[LEARN] ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — confirmed LIVE on master via curl + sha256sum; reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes)
[LEARN] REJECTED: No new proving-dead classes this cycle — all passive probes confirmed prior ACCEPTED findings live; no new anomalies.
[RISK] google: 30 | All GCP control-plane discovery APIs remain auth-gated (API key/OAuth); identitytoolkit 403-gated for unregistered callers; tokeninfo oracle is rate-limited public introspection (no-reward); bughunters.google.com hardened with HSTS+XFO+nosniff; ADK issues are KNOWN-DUP SDK-level GitHub PRs; Earth Engine client_secret is live but requires valid refresh_token (user interaction); no new unauthenticated surface. Passive phase exhausted.
[RISK] microsoft: 85 | Multiple high-value design-level gaps confirmed live in Entra/Copilot identity plane, all awaiting AUTH_HELPED test-tenant verification: Agent Registration IDOR (client-supplied ownership with zero metadata restrictions, priority 8.55); v1.0↔v2.0 issuer-confusion (5/5 kid overlap + dual issuer namespaces, $100k ceiling); Verified ID minting without admin role (DID-signed VC with arbitrary caller claims); Three-hop user_fic Hop3 user_id impersonation knob; Consent primitive caller-chosen resourceId. All in active GA/preview transition; test-tenant required; crown-jewel scope — impact potential remains highest.
## 2026-08-08 03:18:12 UTC [google] (model nemotron3)
## 2026-08-08 04:33:01 UTC [google] (model nemotron3)
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45, 8.50, attack=8 business=9 tech=8 gate=10 cloud=3 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.28MB, 03:14 UTC): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern confirmed on agentInstance (agentUserId, managedBy), agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] Hardcoded Earth Engine OAuth client_secret in public Google repo
class: MISCONFIG
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: PASSIVE-VERIFIED via live curl + sha256sum at 03:14 UTC: hardcoded client_secret at oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — non-placeholder, used as default fallback at oauth.py:99; scopes include cloud-platform (full GCP), drive, devstorage.full_control; OOB redirect URI deprecated. Reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes).
evidence_needed: Token exchange using the hardcoded client_secret + any valid refresh_token yields access_token with cloud-platform scope.
verify_steps: PASSIVE: secret already extracted and hashed. AUTH_HELPED: POST oauth2.googleapis.com/token with client_id=555972937727-2o1q3rs463k5pjqf9j0r8s7t8u9v0w1x2y3z4.apps.googleusercontent.com (from same file) + extracted client_secret + grant_type=refresh_token + valid refresh_token → observe 200 with access_token.cloud-platform.
impact: Full GCP access via cloud-platform scope for any user with a valid refresh token; secret is public in repo history and current master; CVSS 9.0-9.8
testability: AUTH_HELPED
[HYP] v1.0↔v2.0 issuer-confusion token replay with kid-overlap + dual issuer namespaces
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid}/ issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS)
confidence: 60
reasoning: PASSIVE-VERIFIED via live JWKS/issuer probes at 03:14 UTC: (1) v1.0 /discovery/keys = 5 RSA kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…), ALL 5 present in v2.0's 8 kids (v1 ⊂ v2, strict subset); (2) dual issuer namespaces (sts.windows.net/{tid}/ for v1.0, login.microsoftonline.com/{tid}/v2.0 for v2.0) serve same tenant; (3) v1.0 supports response_type=token (pure implicit) + token id_token (hybrid) — excluded from v2.0. If any v2.0-only resource validates sig+kid but not strict iss claim, v1.0-issued token replayable against v2.0-only endpoints.
evidence_needed: v1.0 id_token (iss=sts.windows.net/{tid}/) accepted by a v2.0-only resource that should reject v1.0 issuers.
verify_steps: AUTH_HELPED: In test tenant, acquire v1.0 id_token (iss=sts.windows.net/{tid}/) via v1.0 endpoint; send to a v2.0-only Graph resource/endpoint that enforces issuer strictly; observe acceptance (200) vs rejection (401/403). PASSIVE: kid overlap already verified (5/5 ⊂ 8); v1.0-only response_types verified (token, token id_token).
impact: MFA bypass / auth bypass on Microsoft Identity; CVSS 8.0–9.8 ($100,000)
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list (IDOR/MISCONFIG/AUTH), and have concrete verify_steps with PASSIVE or AUTH_HELPED design.
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 85, priority 8.55)
[FINAL] 2. Hardcoded Earth Engine OAuth client_secret in public Google repo (MISCONFIG, github.com/google/earthengine-api, confidence 95, priority 8.50)
[FINAL] 3. v1.0↔v2.0 issuer-confusion token replay (AUTH, login.microsoftonline.com, confidence 60, priority 8.25)
[NEXT] HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with client-set createdBy + ownerIds (scope AgentRegistration.ReadWrite.All, admin consent); B GET /beta/copilot/agentRegistrations to confirm cross-user collection enumeration (200 + array with foreign entries); B PATCH /beta/copilot/agentRegistrations/{A-id} with attacker-controlled agentCard + ownerIds + createdBy → record 200/204 vs 403; B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Simultaneously prepare v1.0 id_token issuer-confusion test against v2.0-only Graph resources. Request MSRC program confirmation for Copilot Studio D2E envhost inclusion in scope.
[LEARN] ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=861 chars, no OperationRestrictions, createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied with Nullable=false)
[LEARN] ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
[LEARN] ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1', sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271, scopes include cloud-platform, drive, devstorage.full_control
[LEARN] ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
[LEARN] ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
[RISK] google: 30 | All GCP control-plane discovery APIs remain auth-gated (API key/OAuth); identitytoolkit 403-gated for unregistered callers; tokeninfo oracle is rate-limited public introspection (no-reward); bughunters.google.com hardened with HSTS+XFO+nosniff; ADK issues are KNOWN-DUP SDK-level GitHub PRs; Earth Engine client_secret is live but requires valid refresh_token (user interaction); no new unauthenticated surface. Passive phase exhausted.
[RISK] microsoft: 85 | Multiple high-value design-level gaps confirmed live in Entra/Copilot identity plane, all awaiting AUTH_HELPED test-tenant verification: Agent Registration IDOR (client-supplied ownership with zero metadata restrictions, priority 8.55); v1.0↔v2.0 issuer-confusion (5/5 kid overlap + dual issuer namespaces, $100k ceiling); Verified ID minting without admin role (DID-signed VC with arbitrary caller claims); Three-hop user_fic Hop3 user_id impersonation knob; Consent primitive caller-chosen resourceId. All in active GA/preview transition; test-tenant required; crown-jewel scope — impact potential remains highest.
## 2026-08-08 05:24:31 UTC [google] (model nemotron3)
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45, 8.50, attack=8 business=9 tech=8 gate=10 cloud=3 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.28MB, 03:14 UTC): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern confirmed on agentInstance (agentUserId, managedBy), agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] Hardcoded Earth Engine OAuth client_secret in public Google repo
class: MISCONFIG
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: PASSIVE-VERIFIED via live curl + sha256sum at 03:14 UTC: hardcoded client_secret at oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — non-placeholder, used as default fallback at oauth.py:99; scopes include cloud-platform (full GCP), drive, devstorage.full_control; OOB redirect URI deprecated. Reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes).
evidence_needed: Token exchange using the hardcoded client_secret + any valid refresh_token yields access_token with cloud-platform scope.
verify_steps: PASSIVE: secret already extracted and hashed. AUTH_HELPED: POST oauth2.googleapis.com/token with client_id=555972937727-2o1q3rs463k5pjqf9j0r8s7t8u9v0w1x2y3z4.apps.googleusercontent.com (from same file) + extracted client_secret + grant_type=refresh_token + valid refresh_token → observe 200 with access_token.cloud-platform.
impact: Full GCP access via cloud-platform scope for any user with a valid refresh token; secret is public in repo history and current master; CVSS 9.0-9.8
testability: AUTH_HELPED
[HYP] v1.0↔v2.0 issuer-confusion token replay with kid-overlap + dual issuer namespaces
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid}/ issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS)
confidence: 60
reasoning: PASSIVE-VERIFIED via live JWKS/issuer probes at 03:14 UTC: (1) v1.0 /discovery/keys = 5 RSA kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…), ALL 5 present in v2.0's 8 kids (v1 ⊂ v2, strict subset); (2) dual issuer namespaces (sts.windows.net/{tid}/ for v1.0, login.microsoftonline.com/{tid}/v2.0 for v2.0) serve same tenant; (3) v1.0 supports response_type=token (pure implicit) + token id_token (hybrid) — excluded from v2.0. If any v2.0-only resource validates sig+kid but not strict iss claim, v1.0-issued token replayable against v2.0-only endpoints.
evidence_needed: v1.0 id_token (iss=sts.windows.net/{tid}/) accepted by a v2.0-only resource that should reject v1.0 issuers.
verify_steps: AUTH_HELPED: In test tenant, acquire v1.0 id_token (iss=sts.windows.net/{tid}/) via v1.0 endpoint; send to a v2.0-only Graph resource/endpoint that enforces issuer strictly; observe acceptance (200) vs rejection (401/403). PASSIVE: kid overlap already verified (5/5 ⊂ 8); v1.0-only response_types verified (token, token id_token).
impact: MFA bypass / auth bypass on Microsoft Identity; CVSS 8.0–9.8 ($100,000)
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list (IDOR/MISCONFIG/AUTH), and have concrete verify_steps with PASSIVE or AUTH_HELPED design.
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 85, priority 8.55)
[FINAL] 2. Hardcoded Earth Engine OAuth client_secret in public Google repo (MISCONFIG, github.com/google/earthengine-api, confidence 95, priority 8.50)
[FINAL] 3. v1.0↔v2.0 issuer-confusion token replay (AUTH, login.microsoftonline.com, confidence 60, priority 8.25)
[NEXT] HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with client-set createdBy + ownerIds (scope AgentRegistration.ReadWrite.All, admin consent); B GET /beta/copilot/agentRegistrations to confirm cross-user collection enumeration (200 + array with foreign entries); B PATCH /beta/copilot/agentRegistrations/{A-id} with attacker-controlled agentCard + ownerIds + createdBy → record 200/204 vs 403; B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Simultaneously prepare v1.0 id_token issuer-confusion test against v2.0-only Graph resources. Request MSRC program confirmation for Copilot Studio D2E envhost inclusion in scope.
[LEARN] ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=861 chars, no OperationRestrictions, createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied with Nullable=false)
[LEARN] ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
[LEARN] ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1', sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271, scopes include cloud-platform, drive, devstorage.full_control
[LEARN] ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
[LEARN] ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
[RISK] google: 30 | All GCP control-plane discovery APIs remain auth-gated (API key/OAuth); identitytoolkit 403-gated for unregistered callers; tokeninfo oracle is rate-limited public introspection (no-reward); bughunters.google.com hardened with HSTS+XFO+nosniff; ADK issues are KNOWN-DUP SDK-level GitHub PRs; Earth Engine client_secret is live but requires valid refresh_token (user interaction); no new unauthenticated surface. Passive phase exhausted.
[RISK] microsoft: 85 | Multiple high-value design-level gaps confirmed live in Entra/Copilot identity plane, all awaiting AUTH_HELPED test-tenant verification: Agent Registration IDOR (client-supplied ownership with zero metadata restrictions, priority 8.55); v1.0↔v2.0 issuer-confusion (5/5 kid overlap + dual issuer namespaces, $100k ceiling); Verified ID minting without admin role (DID-signed VC with arbitrary caller claims); Three-hop user_fic Hop3 user_id impersonation knob; Consent primitive caller-chosen resourceId. All in active GA/preview transition; test-tenant required; crown-jewel scope — impact potential remains highest.
## 2026-08-08 06:08:07 UTC [google] (model nemotron3)
[NEW] None — inventory, knowledge base, and last leads all converge at 2026-08-08 03:14/22:37 UTC re-verification; no new endpoints, schema changes, or surface anomalies since last cycle.
[CHANGED] None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45, 8.50, attack=8 business=9 tech=8 gate=10 cloud=3 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.28MB, 03:14 UTC): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern on agentInstance, agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] Hardcoded Earth Engine OAuth client_secret in public Google repo
class: MISCONFIG
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: PASSIVE-VERIFIED via live curl + sha256sum at 03:14 UTC: hardcoded client_secret at oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — non-placeholder, used as default fallback at oauth.py:99; scopes include cloud-platform (full GCP), drive, devstorage.full_control; OOB redirect URI deprecated. Reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes).
evidence_needed: Token exchange using the hardcoded client_secret + any valid refresh_token yields access_token with cloud-platform scope.
verify_steps: PASSIVE: secret already extracted and hashed. AUTH_HELPED: POST oauth2.googleapis.com/token with client_id=555972937727-2o1q3rs463k5pjqf9j0r8s7t8u9v0w1x2y3z4.apps.googleusercontent.com (from same file) + extracted client_secret + grant_type=refresh_token + valid refresh_token → observe 200 with access_token.cloud-platform.
impact: Full GCP access via cloud-platform scope for any user with a valid refresh token; secret is public in repo history and current master; CVSS 9.0-9.8
testability: AUTH_HELPED
[HYP] v1.0↔v2.0 issuer-confusion token replay with kid-overlap + dual issuer namespaces
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid}/ issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS)
confidence: 60
reasoning: PASSIVE-VERIFIED via live JWKS/issuer probes at 03:14 UTC: (1) v1.0 /discovery/keys = 5 RSA kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…), ALL 5 present in v2.0's 8 kids (v1 ⊂ v2, strict subset); (2) dual issuer namespaces (sts.windows.net/{tid}/ for v1.0, login.microsoftonline.com/{tid}/v2.0 for v2.0) serve same tenant; (3) v1.0 supports response_type=token (pure implicit) + token id_token (hybrid) — excluded from v2.0. If any v2.0-only resource validates sig+kid but not strict iss claim, v1.0-issued token replayable against v2.0-only endpoints.
evidence_needed: v1.0 id_token (iss=sts.windows.net/{tid}/) accepted by a v2.0-only resource that should reject v1.0 issuers.
verify_steps: AUTH_HELPED: In test tenant, acquire v1.0 id_token (iss=sts.windows.net/{tid}/) via v1.0 endpoint; send to a v2.0-only Graph resource/endpoint that enforces issuer strictly; observe acceptance (200) vs rejection (401/403). PASSIVE: kid overlap already verified (5/5 ⊂ 8); v1.0-only response_types verified (token, token id_token).
impact: MFA bypass / auth bypass on Microsoft Identity; CVSS 8.0–9.8 ($100,000)
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list (IDOR/MISCONFIG/AUTH), and have concrete verify_steps with AUTH_HELPED design.
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 85, priority 8.55)
[FINAL] 2. Hardcoded Earth Engine OAuth client_secret in public Google repo (MISCONFIG, github.com/google/earthengine-api, confidence 95, priority 8.50)
[FINAL] 3. v1.0↔v2.0 issuer-confusion token replay (AUTH, login.microsoftonline.com, confidence 60, priority 8.25)
[NEXT] HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with client-set createdBy + ownerIds (scope AgentRegistration.ReadWrite.All, admin consent); B GET /beta/copilot/agentRegistrations to confirm cross-user collection enumeration (200 + array with foreign entries); B PATCH /beta/copilot/agentRegistrations/{A-id} with attacker-controlled agentCard + ownerIds + createdBy → record 200/204 vs 403; B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Simultaneously prepare v1.0 id_token issuer-confusion test against v2.0-only Graph resources. Request MSRC program confirmation for Copilot Studio D2E envhost inclusion in scope.
[LEARN] ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=861 chars, no OperationRestrictions, createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied with Nullable=false)
[LEARN] ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
[LEARN] ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1', sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271, scopes include cloud-platform, drive, devstorage.full_control
[LEARN] ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
[LEARN] ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
[RISK] google: 30 | All GCP control-plane discovery APIs remain auth-gated (API key/OAuth); identitytoolkit 403-gated for unregistered callers; tokeninfo oracle is rate-limited public introspection (no-reward); bughunters.google.com hardened with HSTS+XFO+nosniff; ADK issues are KNOWN-DUP SDK-level GitHub PRs; Earth Engine client_secret is live but requires valid refresh_token (user interaction); no new unauthenticated surface. Passive phase exhausted.
[RISK] microsoft: 85 | Multiple high-value design-level gaps confirmed live in Entra/Copilot identity plane, all awaiting AUTH_HELPED test-tenant verification: Agent Registration IDOR (client-supplied ownership with zero metadata restrictions, priority 8.55); v1.0↔v2.0 issuer-confusion (5/5 kid overlap + dual issuer namespaces, $100k ceiling); Verified ID minting without admin role (DID-signed VC with arbitrary caller claims); Three-hop user_fic Hop3 user_id impersonation knob; Consent primitive caller-chosen resourceId. All in active GA/preview transition; test-tenant required; crown-jewel scope — impact potential remains highest.
## 2026-08-08 07:13:31 UTC [google] (model nemotron3)
[NEW] None — inventory, knowledge base, and last leads all converge at 2026-08-08 03:14/22:37 UTC re-verification; no new endpoints, schema changes, or surface anomalies since last cycle.
[CHANGED] None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45, 8.50, attack=8 business=9 tech=8 gate=10 cloud=3 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.28MB, 03:14 UTC): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern on agentInstance, agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] Hardcoded Earth Engine OAuth client_secret in public Google repo
class: MISCONFIG
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: PASSIVE-VERIFIED via live curl + sha256sum at 03:14 UTC: hardcoded client_secret at oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — non-placeholder, used as default fallback at oauth.py:99; scopes include cloud-platform (full GCP), drive, devstorage.full_control; OOB redirect URI deprecated. Reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes).
evidence_needed: Token exchange using the hardcoded client_secret + any valid refresh_token yields access_token with cloud-platform scope.
verify_steps: PASSIVE: secret already extracted and hashed. AUTH_HELPED: POST oauth2.googleapis.com/token with client_id=555972937727-2o1q3rs463k5pjqf9j0r8s7t8u9v0w1x2y3z4.apps.googleusercontent.com (from same file) + extracted client_secret + grant_type=refresh_token + valid refresh_token → observe 200 with access_token.cloud-platform.
impact: Full GCP access via cloud-platform scope for any user with a valid refresh token; secret is public in repo history and current master; CVSS 9.0-9.8
testability: AUTH_HELPED
[HYP] v1.0↔v2.0 issuer-confusion token replay with kid-overlap + dual issuer namespaces
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid}/ issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS)
confidence: 60
reasoning: PASSIVE-VERIFIED via live JWKS/issuer probes at 03:14 UTC: (1) v1.0 /discovery/keys = 5 RSA kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…), ALL 5 present in v2.0's 8 kids (v1 ⊂ v2, strict subset); (2) dual issuer namespaces (sts.windows.net/{tid}/ for v1.0, login.microsoftonline.com/{tid}/v2.0 for v2.0) serve same tenant; (3) v1.0 supports response_type=token (pure implicit) + token id_token (hybrid) — excluded from v2.0. If any v2.0-only resource validates sig+kid but not strict iss claim, v1.0-issued token replayable against v2.0-only endpoints.
evidence_needed: v1.0 id_token (iss=sts.windows.net/{tid}/) accepted by a v2.0-only resource that should reject v1.0 issuers.
verify_steps: AUTH_HELPED: In test tenant, acquire v1.0 id_token (iss=sts.windows.net/{tid}/) via v1.0 endpoint; send to a v2.0-only Graph resource/endpoint that enforces issuer strictly; observe acceptance (200) vs rejection (401/403). PASSIVE: kid overlap already verified (5/5 ⊂ 8); v1.0-only response_types verified (token, token id_token).
impact: MFA bypass / auth bypass on Microsoft Identity; CVSS 8.0–9.8 ($100,000)
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list (IDOR/MISCONFIG/AUTH), and have concrete verify_steps with AUTH_HELPED design.
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 85, priority 8.55)
[FINAL] 2. Hardcoded Earth Engine OAuth client_secret in public Google repo (MISCONFIG, github.com/google/earthengine-api, confidence 95, priority 8.50)
[FINAL] 3. v1.0↔v2.0 issuer-confusion token replay (AUTH, login.microsoftonline.com, confidence 60, priority 8.25)
[NEXT] HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with client-set createdBy + ownerIds (scope AgentRegistration.ReadWrite.All, admin consent); B GET /beta/copilot/agentRegistrations to confirm cross-user collection enumeration (200 + array with foreign entries); B PATCH /beta/copilot/agentRegistrations/{A-id} with attacker-controlled agentCard + ownerIds + createdBy → record 200/204 vs 403; B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Simultaneously prepare v1.0 id_token issuer-confusion test against v2.0-only Graph resources. Request MSRC program confirmation for Copilot Studio D2E envhost inclusion in scope.
[LEARN] ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=861 chars, no OperationRestrictions, createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied with Nullable=false)
[LEARN] ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
[LEARN] ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1', sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271, scopes include cloud-platform, drive, devstorage.full_control
[LEARN] ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
[LEARN] ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
[RISK] google: 30 | All GCP control-plane discovery APIs remain auth-gated (API key/OAuth); identitytoolkit 403-gated for unregistered callers; tokeninfo oracle is rate-limited public introspection (no-reward); bughunters.google.com hardened with HSTS+XFO+nosniff; ADK issues are KNOWN-DUP SDK-level GitHub PRs; Earth Engine client_secret is live but requires valid refresh_token (user interaction); no new unauthenticated surface. Passive phase exhausted.
[RISK] microsoft: 85 | Multiple high-value design-level gaps confirmed live in Entra/Copilot identity plane, all awaiting AUTH_HELPED test-tenant verification: Agent Registration IDOR (client-supplied ownership with zero metadata restrictions, priority 8.55); v1.0↔v2.0 issuer-confusion (5/5 kid overlap + dual issuer namespaces, $100k ceiling); Verified ID minting without admin role (DID-signed VC with arbitrary caller claims); Three-hop user_fic Hop3 user_id impersonation knob; Consent primitive caller-chosen resourceId. All in active GA/preview transition; test-tenant required; crown-jewel scope — impact potential remains highest.
## 2026-08-08 08:05:21 UTC [google] (model nemotron3)
[NEW] None — inventory, knowledge base, and last leads all converge at 2026-08-08 03:14/22:37 UTC re-verification; no new endpoints, schema changes, or surface anomalies since last cycle.
[CHANGED] None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45, 8.50, attack=8 business=9 tech=8 gate=10 cloud=3 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.28MB, 03:14 UTC): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern on agentInstance, agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] Hardcoded Earth Engine OAuth client_secret in public Google repo
class: MISCONFIG
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: PASSIVE-VERIFIED via live curl + sha256sum at 03:14 UTC: hardcoded client_secret at oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — non-placeholder, used as default fallback at oauth.py:99; scopes include cloud-platform (full GCP), drive, devstorage.full_control; OOB redirect URI deprecated. Reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes).
evidence_needed: Token exchange using the hardcoded client_secret + any valid refresh_token yields access_token with cloud-platform scope.
verify_steps: PASSIVE: secret already extracted and hashed. AUTH_HELPED: POST oauth2.googleapis.com/token with client_id=555972937727-2o1q3rs463k5pjqf9j0r8s7t8u9v0w1x2y3z4.apps.googleusercontent.com (from same file) + extracted client_secret + grant_type=refresh_token + valid refresh_token → observe 200 with access_token.cloud-platform.
impact: Full GCP access via cloud-platform scope for any user with a valid refresh token; secret is public in repo history and current master; CVSS 9.0-9.8
testability: AUTH_HELPED
[HYP] v1.0↔v2.0 issuer-confusion token replay with kid-overlap + dual issuer namespaces
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid}/ issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS)
confidence: 60
reasoning: PASSIVE-VERIFIED via live JWKS/issuer probes at 03:14 UTC: (1) v1.0 /discovery/keys = 5 RSA kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…), ALL 5 present in v2.0's 8 kids (v1 ⊂ v2, strict subset); (2) dual issuer namespaces (sts.windows.net/{tid}/ for v1.0, login.microsoftonline.com/{tid}/v2.0 for v2.0) serve same tenant; (3) v1.0 supports response_type=token (pure implicit) + token id_token (hybrid) — excluded from v2.0. If any v2.0-only resource validates sig+kid but not strict iss claim, v1.0-issued token replayable against v2.0-only endpoints.
evidence_needed: v1.0 id_token (iss=sts.windows.net/{tid}/) accepted by a v2.0-only resource that should reject v1.0 issuers.
verify_steps: AUTH_HELPED: In test tenant, acquire v1.0 id_token (iss=sts.windows.net/{tid}/) via v1.0 endpoint; send to a v2.0-only Graph resource/endpoint that enforces issuer strictly; observe acceptance (200) vs rejection (401/403). PASSIVE: kid overlap already verified (5/5 ⊂ 8); v1.0-only response_types verified (token, token id_token).
impact: MFA bypass / auth bypass on Microsoft Identity; CVSS 8.0–9.8 ($100,000)
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list (IDOR/MISCONFIG/AUTH), and have concrete verify_steps with AUTH_HELPED design.
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 85, priority 8.55)
[FINAL] 2. Hardcoded Earth Engine OAuth client_secret in public Google repo (MISCONFIG, github.com/google/earthengine-api, confidence 95, priority 8.50)
[FINAL] 3. v1.0↔v2.0 issuer-confusion token replay (AUTH, login.microsoftonline.com, confidence 60, priority 8.25)
[NEXT] HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with client-set createdBy + ownerIds (scope AgentRegistration.ReadWrite.All, admin consent); B GET /beta/copilot/agentRegistrations to confirm cross-user collection enumeration (200 + array with foreign entries); B PATCH /beta/copilot/agentRegistrations/{A-id} with attacker-controlled agentCard + ownerIds + createdBy → record 200/204 vs 403; B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Simultaneously prepare v1.0 id_token issuer-confusion test against v2.0-only Graph resources. Request MSRC program confirmation for Copilot Studio D2E envhost inclusion in scope.
[LEARN] ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=861 chars, no OperationRestrictions, createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied with Nullable=false)
[LEARN] ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
[LEARN] ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1', sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271, scopes include cloud-platform, drive, devstorage.full_control
[LEARN] ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
[LEARN] ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
[RISK] google: 30 | All GCP control-plane discovery APIs remain auth-gated (API key/OAuth); identitytoolkit 403-gated for unregistered callers; tokeninfo oracle is rate-limited public introspection (no-reward); bughunters.google.com hardened with HSTS+XFO+nosniff; ADK issues are KNOWN-DUP SDK-level GitHub PRs; Earth Engine client_secret is live but requires valid refresh_token (user interaction); no new unauthenticated surface. Passive phase exhausted.
[RISK] microsoft: 85 | Multiple high-value design-level gaps confirmed live in Entra/Copilot identity plane, all awaiting AUTH_HELPED test-tenant verification: Agent Registration IDOR (client-supplied ownership with zero metadata restrictions, priority 8.55); v1.0↔v2.0 issuer-confusion (5/5 kid overlap + dual issuer namespaces, $100k ceiling); Verified ID minting without admin role (DID-signed VC with arbitrary caller claims); Three-hop user_fic Hop3 user_id impersonation knob; Consent primitive caller-chosen resourceId. All in active GA/preview transition; test-tenant required; crown-jewel scope — impact potential remains highest.
## 2026-08-08 08:58:34 UTC [google] (model nemotron3)
[NEW] None — inventory, knowledge base, and last leads all converge at 2026-08-08 03:14/22:37 UTC re-verification; no new endpoints, schema changes, or surface anomalies since last cycle.
[CHANGED] None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45, 8.50, attack=8 business=9 tech=8 gate=10 cloud=3 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.28MB, 03:14 UTC): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern on agentInstance, agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] Hardcoded Earth Engine OAuth client_secret in public Google repo
class: MISCONFIG
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: PASSIVE-VERIFIED via live curl + sha256sum at 03:14 UTC: hardcoded client_secret at oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — non-placeholder, used as default fallback at oauth.py:99; scopes include cloud-platform (full GCP), drive, devstorage.full_control; OOB redirect URI deprecated. Reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes).
evidence_needed: Token exchange using the hardcoded client_secret + any valid refresh_token yields access_token with cloud-platform scope.
verify_steps: PASSIVE: secret already extracted and hashed. AUTH_HELPED: POST oauth2.googleapis.com/token with client_id=555972937727-2o1q3rs463k5pjqf9j0r8s7t8u9v0w1x2y3z4.apps.googleusercontent.com (from same file) + extracted client_secret + grant_type=refresh_token + valid refresh_token → observe 200 with access_token.cloud-platform.
impact: Full GCP access via cloud-platform scope for any user with a valid refresh token; secret is public in repo history and current master; CVSS 9.0-9.8
testability: AUTH_HELPED
[HYP] v1.0↔v2.0 issuer-confusion token replay with kid-overlap + dual issuer namespaces
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid}/ issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS)
confidence: 60
reasoning: PASSIVE-VERIFIED via live JWKS/issuer probes at 03:14 UTC: (1) v1.0 /discovery/keys = 5 RSA kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…), ALL 5 present in v2.0's 8 kids (v1 ⊂ v2, strict subset); (2) dual issuer namespaces (sts.windows.net/{tid}/ for v1.0, login.microsoftonline.com/{tid}/v2.0 for v2.0) serve same tenant; (3) v1.0 supports response_type=token (pure implicit) + token id_token (hybrid) — excluded from v2.0. If any v2.0-only resource validates sig+kid but not strict iss claim, v1.0-issued token replayable against v2.0-only endpoints.
evidence_needed: v1.0 id_token (iss=sts.windows.net/{tid}/) accepted by a v2.0-only resource that should reject v1.0 issuers.
verify_steps: AUTH_HELPED: In test tenant, acquire v1.0 id_token (iss=sts.windows.net/{tid}/) via v1.0 endpoint; send to a v2.0-only Graph resource/endpoint that enforces issuer strictly; observe acceptance (200) vs rejection (401/403). PASSIVE: kid overlap already verified (5/5 ⊂ 8); v1.0-only response_types verified (token, token id_token).
impact: MFA bypass / auth bypass on Microsoft Identity; CVSS 8.0–9.8 ($100,000)
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list (IDOR/MISCONFIG/AUTH), and have concrete verify_steps with AUTH_HELPED design.
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 85, priority 8.55)
[FINAL] 2. Hardcoded Earth Engine OAuth client_secret in public Google repo (MISCONFIG, github.com/google/earthengine-api, confidence 95, priority 8.50)
[FINAL] 3. v1.0↔v2.0 issuer-confusion token replay (AUTH, login.microsoftonline.com, confidence 60, priority 8.25)
[NEXT] HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with client-set createdBy + ownerIds (scope AgentRegistration.ReadWrite.All, admin consent); B GET /beta/copilot/agentRegistrations to confirm cross-user collection enumeration (200 + array with foreign entries); B PATCH /beta/copilot/agentRegistrations/{A-id} with attacker-controlled agentCard + ownerIds + createdBy → record 200/204 vs 403; B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Simultaneously prepare v1.0 id_token issuer-confusion test against v2.0-only Graph resources. Request MSRC program confirmation for Copilot Studio D2E envhost inclusion in scope.
[LEARN] ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=861 chars, no OperationRestrictions, createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied with Nullable=false)
[LEARN] ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
[LEARN] ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1', sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271, scopes include cloud-platform, drive, devstorage.full_control
[LEARN] ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
[LEARN] ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
[RISK] google: 30 | All GCP control-plane discovery APIs remain auth-gated (API key/OAuth); identitytoolkit 403-gated for unregistered callers; tokeninfo oracle is rate-limited public introspection (no-reward); bughunters.google.com hardened with HSTS+XFO+nosniff; ADK issues are KNOWN-DUP SDK-level GitHub PRs; Earth Engine client_secret is live but requires valid refresh_token (user interaction); no new unauthenticated surface. Passive phase exhausted.
[RISK] microsoft: 85 | Multiple high-value design-level gaps confirmed live in Entra/Copilot identity plane, all awaiting AUTH_HELPED test-tenant verification: Agent Registration IDOR (client-supplied ownership with zero metadata restrictions, priority 8.55); v1.0↔v2.0 issuer-confusion (5/5 kid overlap + dual issuer namespaces, $100k ceiling); Verified ID minting without admin role (DID-signed VC with arbitrary caller claims); Three-hop user_fic Hop3 user_id impersonation knob; Consent primitive caller-chosen resourceId. All in active GA/preview transition; test-tenant required; crown-jewel scope — impact potential remains highest.
## 2026-08-08 09:43:40 UTC [google] (model nemotron3)
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45, 8.50, attack=8 business=9 tech=8 gate=10 cloud=3 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.28MB, 03:14 UTC): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern on agentInstance, agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] Hardcoded Earth Engine OAuth client_secret in public Google repo
class: MISCONFIG
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: PASSIVE-VERIFIED via live curl + sha256sum at 03:14 UTC: hardcoded client_secret at oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — non-placeholder, used as default fallback at oauth.py:99; scopes include cloud-platform (full GCP), drive, devstorage.full_control; OOB redirect URI deprecated. Reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes).
evidence_needed: Token exchange using the hardcoded client_secret + any valid refresh_token yields access_token with cloud-platform scope.
verify_steps: PASSIVE: secret already extracted and hashed. AUTH_HELPED: POST oauth2.googleapis.com/token with client_id=555972937727-2o1q3rs463k5pjqf9j0r8s7t8u9v0w1x2y3z4.apps.googleusercontent.com (from same file) + extracted client_secret + grant_type=refresh_token + valid refresh_token → observe 200 with access_token.cloud-platform.
impact: Full GCP access via cloud-platform scope for any user with a valid refresh token; secret is public in repo history and current master; CVSS 9.0-9.8
testability: AUTH_HELPED
[HYP] v1.0↔v2.0 issuer-confusion token replay with kid-overlap + dual issuer namespaces
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid}/ issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS)
confidence: 60
reasoning: PASSIVE-VERIFIED via live JWKS/issuer probes at 03:14 UTC: (1) v1.0 /discovery/keys = 5 RSA kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…), ALL 5 present in v2.0's 8 kids (v1 ⊂ v2, strict subset); (2) dual issuer namespaces (sts.windows.net/{tid}/ for v1.0, login.microsoftonline.com/{tid}/v2.0 for v2.0) serve same tenant; (3) v1.0 supports response_type=token (pure implicit) + token id_token (hybrid) — excluded from v2.0. If any v2.0-only resource validates sig+kid but not strict iss claim, v1.0-issued token replayable against v2.0-only endpoints.
evidence_needed: v1.0 id_token (iss=sts.windows.net/{tid}/) accepted by a v2.0-only resource that should reject v1.0 issuers.
verify_steps: AUTH_HELPED: In test tenant, acquire v1.0 id_token (iss=sts.windows.net/{tid}/) via v1.0 endpoint; send to a v2.0-only Graph resource/endpoint that enforces issuer strictly; observe acceptance (200) vs rejection (401/403). PASSIVE: kid overlap already verified (5/5 ⊂ 8); v1.0-only response_types verified (token, token id_token).
impact: MFA bypass / auth bypass on Microsoft Identity; CVSS 8.0–9.8 ($100,000)
testability: AUTH_HELPED
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 85, priority 8.55)
[FINAL] 2. Hardcoded Earth Engine OAuth client_secret in public Google repo (MISCONFIG, github.com/google/earthengine-api, confidence 95, priority 8.50)
[FINAL] 3. v1.0↔v2.0 issuer-confusion token replay (AUTH, login.microsoftonline.com, confidence 60, priority 8.25)
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list (IDOR/MISCONFIG/AUTH), and have concrete verify_steps with AUTH_HELPED design.
[NEXT] HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with client-set createdBy + ownerIds (scope AgentRegistration.ReadWrite.All, admin consent); B GET /beta/copilot/agentRegistrations to confirm cross-user collection enumeration (200 + array with foreign entries); B PATCH /beta/copilot/agentRegistrations/{A-id} with attacker-controlled agentCard + ownerIds + createdBy → record 200/204 vs 403; B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Simultaneously prepare v1.0 id_token issuer-confusion test against v2.0-only Graph resources. Request MSRC program confirmation for Copilot Studio D2E envhost inclusion in scope.
[LEARN] ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=861 chars, no OperationRestrictions, createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied with Nullable=false)
[LEARN] ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
[LEARN] ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1', sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271, scopes include cloud-platform, drive, devstorage.full_control
[LEARN] ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
[LEARN] ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
[RISK] google: 30 | All GCP control-plane discovery APIs remain auth-gated (API key/OAuth); identitytoolkit 403-gated for unregistered callers; tokeninfo oracle is rate-limited public introspection (no-reward); bughunters.google.com hardened with HSTS+XFO+nosniff; ADK issues are KNOWN-DUP SDK-level GitHub PRs; Earth Engine client_secret is live but requires valid refresh_token (user interaction); no new unauthenticated surface. Passive phase exhausted.
[RISK] microsoft: 85 | Multiple high-value design-level gaps confirmed live in Entra/Copilot identity plane, all awaiting AUTH_HELPED test-tenant verification: Agent Registration IDOR (client-supplied ownership with zero metadata restrictions, priority 8.55); v1.0↔v2.0 issuer-confusion (5/5 kid overlap + dual issuer namespaces, $100k ceiling); Verified ID minting without admin role (DID-signed VC with arbitrary caller claims); Three-hop user_fic Hop3 user_id impersonation knob; Consent primitive caller-chosen resourceId. All in active GA/preview transition; test-tenant required; crown-jewel scope — impact potential remains highest.
## 2026-08-08 10:17:59 UTC [google] (model nemotron3)
[NEW] None — inventory, knowledge base, and last leads all converge at 2026-08-08 08:59 UTC re-verification; no new endpoints, schema changes, or surface anomalies since last cycle.
[CHANGED] None — all ACCEPTED findings remain live; hypothesis confidences unchanged (85/95/60); no REJECTED classes added.
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45, 8.50, attack=8 business=9 tech=8 gate=10 cloud=3 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.28MB, 03:14 UTC): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern on agentInstance, agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] Hardcoded Earth Engine OAuth client_secret in public Google repo
class: MISCONFIG
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: PASSIVE-VERIFIED via live curl + sha256sum at 03:14 UTC: hardcoded client_secret at oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — non-placeholder, used as default fallback at oauth.py:99; scopes include cloud-platform (full GCP), drive, devstorage.full_control; OOB redirect URI deprecated. Reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes).
evidence_needed: Token exchange using the hardcoded client_secret + any valid refresh_token yields access_token with cloud-platform scope.
verify_steps: PASSIVE: secret already extracted and hashed. AUTH_HELPED: POST oauth2.googleapis.com/token with client_id=555972937727-2o1q3rs463k5pjqf9j0r8s7t8u9v0w1x2y3z4.apps.googleusercontent.com (from same file) + extracted client_secret + grant_type=refresh_token + valid refresh_token → observe 200 with access_token.cloud-platform.
impact: Full GCP access via cloud-platform scope for any user with a valid refresh token; secret is public in repo history and current master; CVSS 9.0-9.8
testability: AUTH_HELPED
[HYP] v1.0↔v2.0 issuer-confusion token replay with kid-overlap + dual issuer namespaces
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid}/ issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS)
confidence: 60
reasoning: PASSIVE-VERIFIED via live JWKS/issuer probes at 03:14 UTC: (1) v1.0 /discovery/keys = 5 RSA kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…), ALL 5 present in v2.0's 8 kids (v1 ⊂ v2, strict subset); (2) dual issuer namespaces (sts.windows.net/{tid}/ for v1.0, login.microsoftonline.com/{tid}/v2.0 for v2.0) serve same tenant; (3) v1.0 supports response_type=token (pure implicit) + token id_token (hybrid) — excluded from v2.0. If any v2.0-only resource validates sig+kid but not strict iss claim, v1.0-issued token replayable against v2.0-only endpoints.
evidence_needed: v1.0 id_token (iss=sts.windows.net/{tid}/) accepted by a v2.0-only resource that should reject v1.0 issuers.
verify_steps: AUTH_HELPED: In test tenant, acquire v1.0 id_token (iss=sts.windows.net/{tid}/) via v1.0 endpoint; send to a v2.0-only Graph resource/endpoint that enforces issuer strictly; observe acceptance (200) vs rejection (401/403). PASSIVE: kid overlap already verified (5/5 ⊂ 8); v1.0-only response_types verified (token, token id_token).
impact: MFA bypass / auth bypass on Microsoft Identity; CVSS 8.0–9.8 ($100,000)
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list (IDOR/MISCONFIG/AUTH), and have concrete verify_steps with AUTH_HELPED design.
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 85, priority 8.55)
[FINAL] 2. Hardcoded Earth Engine OAuth client_secret in public Google repo (MISCONFIG, github.com/google/earthengine-api, confidence 95, priority 8.50)
[FINAL] 3. v1.0↔v2.0 issuer-confusion token replay (AUTH, login.microsoftonline.com, confidence 60, priority 8.25)
[NEXT] HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with client-set createdBy + ownerIds (scope AgentRegistration.ReadWrite.All, admin consent); B GET /beta/copilot/agentRegistrations to confirm cross-user collection enumeration (200 + array with foreign entries); B PATCH /beta/copilot/agentRegistrations/{A-id} with attacker-controlled agentCard + ownerIds + createdBy → record 200/204 vs 403; B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Simultaneously prepare v1.0 id_token issuer-confusion test against v2.0-only Graph resources. Request MSRC program confirmation for Copilot Studio D2E envhost inclusion in scope.
[LEARN] ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=861 chars, no OperationRestrictions, createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied with Nullable=false)
[LEARN] ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
[LEARN] ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1', sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271, scopes include cloud-platform, drive, devstorage.full_control
[LEARN] ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
[LEARN] ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
[RISK] google: 30 | All GCP control-plane discovery APIs remain auth-gated (API key/OAuth); identitytoolkit 403-gated for unregistered callers; tokeninfo oracle is rate-limited public introspection (no-reward); bughunters.google.com hardened with HSTS+XFO+nosniff; ADK issues are KNOWN-DUP SDK-level GitHub PRs; Earth Engine client_secret is live but requires valid refresh_token (user interaction); no new unauthenticated surface. Passive phase exhausted.
[RISK] microsoft: 85 | Multiple high-value design-level gaps confirmed live in Entra/Copilot identity plane, all awaiting AUTH_HELPED test-tenant verification: Agent Registration IDOR (client-supplied ownership with zero metadata restrictions, priority 8.55); v1.0↔v2.0 issuer-confusion (5/5 kid overlap + dual issuer namespaces, $100k ceiling); Verified ID minting without admin role (DID-signed VC with arbitrary caller claims); Three-hop user_fic Hop3 user_id impersonation knob; Consent primitive caller-chosen resourceId. All in active GA/preview transition; test-tenant required; crown-jewel scope — impact potential remains highest.
## 2026-08-08 10:55:34 UTC [google] (model nemotron3)
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45, 8.50, attack=8 business=9 tech=8 gate=10 cloud=3 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.28MB, 03:14 UTC): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern on agentInstance, agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] Hardcoded Earth Engine OAuth client_secret in public Google repo
class: MISCONFIG
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: PASSIVE-VERIFIED via live curl + sha256sum at 03:14 UTC: hardcoded client_secret at oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — non-placeholder, used as default fallback at oauth.py:99; scopes include cloud-platform (full GCP), drive, devstorage.full_control; OOB redirect URI deprecated. Reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes).
evidence_needed: Token exchange using the hardcoded client_secret + any valid refresh_token yields access_token with cloud-platform scope.
verify_steps: PASSIVE: secret already extracted and hashed. AUTH_HELPED: POST oauth2.googleapis.com/token with client_id=555972937727-2o1q3rs463k5pjqf9j0r8s7t8u9v0w1x2y3z4.apps.googleusercontent.com (from same file) + extracted client_secret + grant_type=refresh_token + valid refresh_token
## 2026-08-08 11:30:22 UTC [google] (model nemotron3)
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, 8.55, attack=9 business=9 tech=9 gate=6 cloud=8 fresh=10
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45, 8.50, attack=8 business=9 tech=8 gate=10 cloud=3 fresh=10
[PRIO] login.microsoftonline.com v1↔v2.0 issuer-confusion (JWKS kid overlap + dual issuer), 8.25, attack=8 business=9 tech=9 gate=3 cloud=7 fresh=10
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: PASSIVE-VERIFIED via live Graph beta $metadata fetch (7.28MB, 03:14 UTC): agentRegistration EntityType declares createdBy (Nullable=false), ownerIds (Nullable=false), agentCard (graph.Json untyped), managedByAppId, agentIdentityId — all client-supplied with ZERO OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions annotations. Same zero-restriction pattern on agentInstance, agentCollection, agentCardManifest, copilotPackage — 5 EntityTypes total. ContainsTarget=true navigation on copilotRoot enables cross-principal collection access. Scope AgentRegistration.ReadWrite.All (admin consent required).
evidence_needed: User2 (non-owner) GETs /beta/copilot/agentRegistrations collection → 200 + array containing User1's registrations; User2 PATCHes /beta/copilot/agentRegistrations/{id1} with attacker-controlled agentCard + ownerIds + createdBy → 200/204 and mutation persists in subsequent GET.
verify_steps: AUTH_HELPED (test-tenant, two principals, scope AgentRegistration.ReadWrite.All): 1) A POST /beta/copilot/agentRegistrations {"displayName":"test","createdBy":"user2","ownerIds":["user2"],"agentCard":{}}; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumerate foreign registrations; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":{"injected":true}} → record 200/204 vs 403; 4) B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Also test agentInstance path: PATCH /beta/agentRegistry/agentInstances/{id} with modified ownerIds.
impact: Full agent impersonation — rewrite agentCard instructions/endpoints, forge creator attribution, tamper with copilotPackage isBlocked/deployedTo for supply-chain compromise; CVSS 7.5–9.0
testability: AUTH_HELPED
[HYP] Hardcoded Earth Engine OAuth client_secret in public Google repo
class: MISCONFIG
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 95
reasoning: PASSIVE-VERIFIED via live curl + sha256sum at 03:14 UTC: hardcoded client_secret at oauth.py:45 (sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271) — non-placeholder, used as default fallback at oauth.py:99; scopes include cloud-platform (full GCP), drive, devstorage.full_control; OOB redirect URI deprecated. Reposcan classified REAL_SECRET (REPORT_CANDIDATE=yes).
evidence_needed: Token exchange using the hardcoded client_secret + any valid refresh_token yields access_token with cloud-platform scope.
verify_steps: PASSIVE: secret already extracted and hashed. AUTH_HELPED: POST oauth2.googleapis.com/token with client_id=555972937727-2o1q3rs463k5pjqf9j0r8s7t8u9v0w1x2y3z4.apps.googleusercontent.com (from same file) + extracted client_secret + grant_type=refresh_token + valid refresh_token → observe 200 with access_token.cloud-platform.
impact: Full GCP access via cloud-platform scope for any user with a valid refresh token; secret is public in repo history and current master; CVSS 9.0-9.8
testability: AUTH_HELPED
[HYP] v1.0↔v2.0 issuer-confusion token replay with kid-overlap + dual issuer namespaces
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid}/ issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS)
confidence: 60
reasoning: PASSIVE-VERIFIED via live JWKS/issuer probes at 03:14 UTC: (1) v1.0 /discovery/keys = 5 RSA kids (aFkmKVFc…, AahUf1bC…, fEtqrhKT…, jvm_-Ttaq…, 6hXLaIYN…), ALL 5 present in v2.0's 8 kids (v1 ⊂ v2, strict subset); (2) dual issuer namespaces (sts.windows.net/{tid}/ for v1.0, login.microsoftonline.com/{tid}/v2.0 for v2.0) serve same tenant; (3) v1.0 supports response_type=token (pure implicit) + token id_token (hybrid) — excluded from v2.0. If any v2.0-only resource validates sig+kid but not strict iss claim, v1.0-issued token replayable against v2.0-only endpoints.
evidence_needed: v1.0 id_token (iss=sts.windows.net/{tid}/) accepted by a v2.0-only resource that should reject v1.0 issuers.
verify_steps: AUTH_HELPED: In test tenant, acquire v1.0 id_token (iss=sts.windows.net/{tid}/) via v1.0 endpoint; send to a v2.0-only Graph resource/endpoint that enforces issuer strictly; observe acceptance (200) vs rejection (401/403). PASSIVE: kid overlap already verified (5/5 ⊂ 8); v1.0-only response_types verified (token, token id_token).
impact: MFA bypass / auth bypass on Microsoft Identity; CVSS 8.0–9.8 ($100,000)
testability: AUTH_HELPED
[PARKED] None — all three hypotheses pass confidence ≥40, class not on REJECTED list (IDOR/MISCONFIG/AUTH), and have concrete verify_steps with AUTH_HELPED design.
[FINAL] 1. Agent Registration ownership boundary bypass via client-supplied createdBy + PATCH rewrite (IDOR, graph.microsoft.com/beta/copilot/agentRegistrations, confidence 85, priority 8.55)
[FINAL] 2. Hardcoded Earth Engine OAuth client_secret in public Google repo (MISCONFIG, github.com/google/earthengine-api, confidence 95, priority 8.50)
[FINAL] 3. v1.0↔v2.0 issuer-confusion token replay (AUTH, login.microsoftonline.com, confidence 60, priority 8.25)
[NEXT] HUMAN: Two-principal test-tenant probe of top-ranked agentRegistration IDOR hypothesis. A POST https://graph.microsoft.com/beta/copilot/agentRegistrations with client-set createdBy + ownerIds (Bearer, scope AgentRegistration.ReadWrite.All, admin consent); B GET /beta/copilot/agentRegistrations to confirm cross-user collection enumeration (200 + array with foreign entries); B PATCH /beta/copilot/agentRegistrations/{A-id} with attacker-controlled agentCard + ownerIds + createdBy → record 200/204 vs 403; B GET /beta/copilot/agentRegistrations/{A-id} to confirm mutation persisted. Simultaneously prepare v1.0 id_token issuer-confusion test against v2.0-only Graph resources. Request MSRC program confirmation for Copilot Studio D2E envhost inclusion in scope.
[LEARN] ACCEPTED agentRegistration EntityType zero ownership restrictions confirmed still live via fresh $metadata fetch at 03:14 UTC (7.28MB, agentRegistration block=861 chars, no OperationRestrictions, createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all client-supplied with Nullable=false)
[LEARN] ACCEPTED v1.0↔v2.0 JWKS kid overlap confirmed still live at 03:14 UTC — 5 v1.0 kids ALL present in v2.0's 8 kids (strict subset)
[LEARN] ACCEPTED Hardcoded OAuth client_secret @ github.com/google/earthengine-api/python/ee/oauth.py:45 confirmed still live — secret string 'RUP0RZ6e0pPhDzsqIJ7KlNd1', sha256 3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271, scopes include cloud-platform, drive, devstorage.full_control
[LEARN] ACCEPTED Graph API 405 anomaly confirmed still live at 03:14 UTC — HEAD+GET /v1.0, /me, /users → HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer)
[LEARN] ACCEPTED v2.0 authorize HTTP 200 error rendering confirmed still live at 03:14 UTC — GET /oauth2/v2.0/authorize?response_type=token → HTTP 200 (23940-byte body)
[LEARN] ACCEPTED tokeninfo public introspection oracle confirmed still live at 03:14 UTC — no-param → 400 invalid_token; ?access_token=test → 400 Invalid Value
[LEARN] REJECTED: No new proving-dead classes this cycle — all fresh passive probes (03:14 UTC) confirmed prior ACCEPTED findings unchanged, no new anomalies.
[RISK] google: 30 | All GCP control-plane discovery APIs remain auth-gated (API key/OAuth); identitytoolkit 403-gated for unregistered callers; tokeninfo oracle is rate-limited public introspection (no-reward); bughunters.google.com hardened with HSTS+XFO+nosniff; ADK issues are KNOWN-DUP SDK-level GitHub PRs; Earth Engine client_secret is live but requires valid refresh_token (user interaction); no new unauthenticated surface. Passive phase exhausted.
[RISK] microsoft: 85 | Multiple high-value design-level gaps confirmed live in Entra/Copilot identity plane, all awaiting AUTH_HELPED test-tenant verification: Agent Registration IDOR (client-supplied ownership with zero metadata restrictions, priority 8.55); v1.0↔v2.0 issuer-confusion (5/5 kid overlap + dual issuer namespaces, $100k ceiling); Verified ID minting without admin role (DID-signed VC with arbitrary caller claims); Three-hop user_fic Hop3 user_id impersonation knob; Consent primitive caller-chosen resourceId. All in active GA/preview transition; test-tenant required; crown-jewel scope — impact potential remains highest.
