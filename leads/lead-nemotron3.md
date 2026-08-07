
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
