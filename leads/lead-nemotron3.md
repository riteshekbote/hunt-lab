
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
