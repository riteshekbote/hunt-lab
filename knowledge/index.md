# Knowledge Base — verified learnings (RAG for all models)

> Appended by triage + verification cycles. Models MUST read this before generating hypotheses.
> Rules: never propose a class on the REJECTED list; never duplicate KNOWN-DUP items; use ALIVE surface facts.

## REJECTED CLASSES (verified dead, do not propose)
- 2026-08-07 REJECTED Firebase config reads (getProjects/getProjectConfig) @ identitytoolkit.googleapis.com: 403 "unregistered callers" gate; config public-by-design anyway.
- 2026-08-07 REJECTED delegatedProjectNumber cross-project lookup @ accounts:lookup: field DEPRECATED, documented as caller's own requesting app (Firebase V1 migration), IAM firebaseauth.users.get required on targetProjectId resource. Resurrection ONLY via authorized two-project token test.
- 2026-08-07 REJECTED AAD deferred redirect_uri validation @ login.microsoftonline.com: verified normal+prompt=none both return generic 200 shell, no Location, no inline error; check fires only at auth-completion (AADSTS50011 exact match). Cosmetic timing difference vs login.live.com fast-fail. No code-to-attacker possible passively.
- 2026-08-07 REJECTED legacy ACS @ accounts.accesscontrol.windows.net: EOL platform, 4 signing keys, 3 endpoints, no new surface.
- 2026-08-07 REJECTED internal backend URL leaks in SPA config: jarvisapi.account.microsoft.com NXDOMAIN; jcmsfd.account.microsoft.com/CPM alive but 400 MissingOrInvalidHeader gate.
- 2026-08-07 REJECTED /health metadata leaks (accledger.azure.com pod name + image tag): info-only, banner class.
- 2026-08-07 REJECTED email/phone enumeration differential (Firebase sendOobCode): on Google's no-reward list.
- 2026-08-07 REJECTED endpoint maps with all-401 responses (mysignins API map): inventory is not a finding without a bypass.
- 2026-08-07 REJECTED implicit-flow / client-registration hygiene (accounts.google.com): documented client allow-list model.
- 2026-08-07 REJECTED Algolia DocSearch apiKey in docusaurus config (google/xrblocks): search-only key, public by design.

## KNOWN DUPLICATES (do not re-report)
- ADK /run_sse OAuth client_secret to untrusted clients: google/adk-python issue #2128, closed 2025-07-23.
- ADK hardcoded ya29.* OAuth token in git history: google/adk-python issue #5520, closed 2026-04-28, issuetracker 504158909, token redacted.
- CVE-2026-47391, Benchikh OAuth, Tenable SSRF: already public/fixed (from 10:16 triage).

## ALIVE SURFACE FACTS (verified)
- mysignins.microsoft.com SPA: MSAL app clientId 19db86c3-b2b9-44cc-b339-36da233a3be2, backend api.mysignins.microsoft.com, region header x-ms-mysignins-region: westus2. API map: /api/session/*, /api/authenticationmethods/*, /api/signIns, /api/password/*, /api/captcha/*, /api/signOutEverywhere — all 401/POST-gated.
- api.myaccount.microsoft.com SPA: clientId 8c59ead7-d703-4a27-9e55-c96a0054c8d2. Entra Verified ID: /api/issueVerifiedEmployeeCredential, /api/canVerifiedIdBeIssued. ToS: /api/termsofuse/{agreements,myacceptances,tenantbannerlogo,tenantdisplayname}. /api/shell/getshellinfo, /api/groups/settings, /api/instrument/logclient. Consumer tenant 9188040d-6c67-4c5b-b112-36a304b66dad.
- accounts.accesscontrol.windows.net: /metadata/json/1 (4 x509 keys), /tokens/OAuth/2, /tokens/delegation/1, /mgmt/delegation/1 (200 sign-in page). ESTS 2.1.24997.11.
- Graph agentic: /v1.0 and /beta me/agentSignInSessions alive (401 InvalidAuthenticationToken).
- controlplane.accledger.azure.com: Kestrel /health 200, structured 404 envelope, no swagger.
- GCP control-plane discovery APIs (identitytoolkit, iam, accesscontextmanager, beyondcorp, agentidentity, cloudbuild, artifactregistry, orgpolicy, binaryauthorization, assuredworkloads): all require consumer identity (API key/OAuth); no anonymous reads.
- google/adk-python: /run_sse endpoint exists in code; credential storage migrating to secret: scope (PR #5154 WIP).
- identitytoolkit.googleapis.com: 403 for unregistered callers on ALL methods incl. getProjects and v3 relyingparty.

## REUSABLE PATTERNS
- AAD authorization endpoint accepts any redirect_uri in the request → validation deferred to completion. Useful for detecting eager-validation bypasses on OTHER OAuth providers (same-class check: prompt=none + attacker redirect_uri + valid client_id).
- Firebase gating model: "unregistered callers" 403 = needs API key. Public Firebase API keys are project-bound; cross-project reads need the target project's key or OAuth+IAM.
- GCP $discovery docs expose full request schemas — use for schema-level trust-boundary review (fields marked "should only be specified by authenticated requests" = the trust boundary; hunt fields WITHOUT that annotation).
