## 2026-08-07 09:08:00 UTC [microsoft] (model bigpickle) — SURFACE

- [LEAD] mysignins.microsoft.com SPA clientId 19db86c3-b2b9-44cc-b339-36da233a3be2; backend api.mysignins.microsoft.com; full auth-gated API map (/api/session/*, /api/authenticationmethods/*, /api/signIns, /api/password/*, /api/captcha/*). Graph /me/agentSignInSessions. Region header x-ms-mysignins-region: westus2.
- [LEAD] api.myaccount.microsoft.com SPA clientId 8c59ead7-d703-4a27-9e55-c96a0054c8d2; config block leaks cpmBaseUrl jcmsfd.account.microsoft.com/CPM + cpmAudience https://jarvisapi.account.microsoft.com; Entra Verified ID issuance endpoints /api/issueVerifiedEmployeeCredential + /api/canVerifiedIdBeIssued; myAgentsUrl myaccount.microsoft.com/agents.
- [LEAD] accounts.accesscontrol.windows.net live legacy ACS: /metadata/json/1 returns 5 signing x509 keys; endpoints /tokens/OAuth/2, /tokens/delegation/1, /mgmt/delegation/1 alive (200 sign-in page). ESTS header 2.1.24997.11.
- [LEAD] login.live.com redirect_uri validation leak: 302 error string "client must be mobile or redirect must be absolute and registered" — fast-fail behavior; AAD v2 (login.microsoftonline.com) instead returns generic sign-in page for unregistered client+redirect (deferred validation) — asymmetry worth testing in HYPOTHESIS phase.
- [LEAD] controlplane.accledger.azure.com Kestrel /health 200 "Healthy" leaks pod name + x-ms-image-tag build ID; no swagger. identity.accledger.azure.com -> identity-accledger-prod-1.eastus.cloudapp.azure.com (no HTTPS).
- [LEAD] microsoftazuread-sso.com apex NXDOMAIN (NS on azure-dns), TXT google-site-verification only; no subdomain HTTP surface.
- [UNVALIDATED] api.dev/prod.accessmonitor.azure.com 403 AFD-gated; aedeptooling-int + api.3psecureppe no route from egress.

## 2026-08-07 09:08:17 UTC [google] (model bigpickle)
- [UNVALIDATED] mysignins.microsoft.com (My Sign-Ins SPA, v4.0.2233)** is a Microsoft Authentication Library (MSAL) app (clientId `19db86c3-b2b9-44cc-b339-36da233a3be2`) hitting a region-pinned gateway (`x-ms-mysignins-region: westus2`). Its 6MB bundle enumerates a full auth-gated API map: `/api/session/{authorize,currentuser,issessionvalid,extendsessionvalidity}`, `/api/authenticationmethods/*`, `/api/signIns`, `/api/signIns/{id}`, `/api/signInMap`, `/api/password/{change,reset}`, `/api/captcha/*`, `/api/signOutEverywhere`. Probing without a token returns `401 + WWW-Authenticate: Bearer`; `/api/session/currentuser` is POST-only (405).
- [UNVALIDATED] Agentic sign-in surface is new and material**: bundle references Graph `/me/agentSignInSessions` and My Sign-Ins `/recent-activity/agentic-sessions`; `graph.microsoft.com/v1.0/me/agentSignInSessions` + `/beta/me/agentSignInSessions` both resolve with `401 InvalidAuthenticationToken` (endpoints alive). This is the freshly-expanded "agent sessions" identity workflow area.
- [UNVALIDATED] api.myaccount.microsoft.com (My Account SPA, clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2`)** exposes Entra Verified ID endpoints (`/api/issueVerifiedEmployeeCredential`, `/api/canVerifiedIdBeIssued`), ToS agreement endpoints, `/api/shell/getshellinfo`, and config blocks leaking internal backends: `cpmBaseUrl https://jcmsfd.account.microsoft.com/CPM`, `cpmAudience https://jarvisapi.account.microsoft.com`, and `myAgentsUrl myaccount.microsoft.com/agents`. Consumer tenant `9188040d-6c67-4c5b-b112-36a304b66dad` is referenced for /v2.0.
- [UNVALIDATED] Legacy ACS host `accounts.accesscontrol.windows.net` is live** (served by AAD ESTS, `x-ms-ests-server: 2.1.24997.11`): `/metadata/json/1` returns 5 signing x509 certs (CN `accounts.accesscontrol.windows.net`); discovery advertises `/tokens/OAuth/2`, `/tokens/delegation/1` (DelegationIssuance1.0), `/mgmt/delegation/1` (DelegationManagement1.0) — GET returns a 200 sign-in page, so pathing is alive and auth-gated.
- [UNVALIDATED] OAuth validation asymmetry**: `login.live.com` fast-fails on bad redirect_uri with a precise 302 error ("client must be mobile or redirect must be absolute + registered"), while `login.microsoftonline.com/oauth2/v2.0/authorize` with an unregistered client+redirect returns a generic 200 sign-in page (validation deferred). Classic redirect_uri/response_mode research surface.
- [UNVALIDATED] `controlplane.accledger.azure.com`** is a live Kestrel API: `/health` → 200 `Healthy`, leaking `x-ms-machinename` (pod), `x-ms-image-tag 1.0.03467.455-73c40f8d7a2145ba44be22ca0c6029eca472d3d8`; structured 404 envelope `{"Status":"failed","Error":{...}}`; no swagger/ledger/tenants routes. `identity.accledger.azure.com` → `identity-accledger-prod-1.eastus.cloudapp.azure.com` (no HTTPS responder).
- [UNVALIDATED] `microsoftazuread-sso.com`: apex NXDOMAIN (NS on azure-dns), TXT `google-site-verification=...` only, no A/CNAME; subdomain probes (login/mysignins/sso) get no route — host-keyed SSO domain, low direct surface.
- [UNVALIDATED] api.mysignins.microsoft.com: /api/session/currentuser (405=POST), /api/authenticationmethods/availablemethods, /api/signIns, /api/tenants (401 baseline)
- [UNVALIDATED] api.myaccount.microsoft.com: /api/issueVerifiedEmployeeCredential, /api/canVerifiedIdBeIssued, /api/termsofuse/{agreements/,myacceptances,tenantbannerlogo,tenantdisplayname}, /api/shell/getshellinfo, /api/groups/settings, /api/instrument/logclient
- [UNVALIDATED] Backend leaks: jcmsfd.account.microsoft.com/CPM, jarvisapi.account.microsoft.com, myaccount.microsoft.com/agents, login.microsoftonline.com/9188040d-6c67-4c5b-b112-36a304b66dad/v2.0
- [UNVALIDATED] Graph: /v1.0 + /beta me/agentSignInSessions, /v1.0/me/authentication/methods
- [UNVALIDATED] accounts.accesscontrol.windows.net: /metadata/json/1 (JWKS), /tokens/OAuth/2, /tokens/delegation/1, /mgmt/delegation/1

## 2026-08-07 10:05:00 UTC [microsoft] (model bigpickle) — HYPOTHESIS

- [LEAD-HIGH] Source maps live on BOTH identity SPAs: mysignins main.caa6a456.js.map (no content, 4359 paths) and api.myaccount main.4e6e3dc6.js.map (sourcesContent for 4922 files). Verified ID + ToS request schemas recovered from extracted source. Hashes: myaccount.map sha256 72290126...d6541, mysignins.map sha256 2099f8a8...f3efbe.
- [LEAD-HIGH] api.myaccount /api/issueVerifiedEmployeeCredential: POST with Bearer token whose scope = the SPA's own clientId (8c59ead7-d703-4a27-9e55-c96a0054c8d2); backend gates on GuestIsNotAllowedToIssueVerifiedId / TenantIsNotInAllowedToIssueVerifiedId. Low-priv credential-minting hypothesis (test-tenant only).
- [LEAD-HIGH] /me/agentSignInSessions (v1.0+beta) alive but UNDOCUMENTED in public Graph docs; bundle modules agentSessionApis.ts / agentSessions.ts + /recent-activity/agentic-sessions. New-scope AgentIdentity.Read.All + ServiceIdentity SP type (useHasOwnedAgents → /users/{userId}/ownedObjects/...?$top=999).
- [LEAD-HIGH] accounts.accesscontrol.windows.net: allowedAudiences removed (tenant-enum patched Mar-2026); 5 live signing keys incl 2x CN=login.microsoftonline.us mixed into global JWKS; /tokens/OAuth/2, /tokens/delegation/1, /mgmt/delegation/1 pathing alive. Historical Sep-2025 CVSS-10.0 actor-token chain used this host (app 00000001-0000-0000-c000-000000000000) — known high-attention MSRC surface.
- [LEAD-MED] ToS unauthenticated consent flow embeds access_token + id_token_hint + claims (governance_control_state_per_device) in server-rendered data-requestbody DOM; policyfiles/{id}/data GET is an IDOR candidate across tenants (test-tenant only).
- [LEAD-MED] Cross-app URLs pass upn/tenant/login_hint/optIn to IAMUX + myapps + mygroups + myaccess; substrate search builder search/api/v1 (confused-deputy surface, test-tenant only).
- [LEAD-LOW] controlplane.accledger.azure.com /health leaks pod name + x-ms-image-tag; no swagger. No public repo/docs for Access Ledger found.

## 2026-08-07 09:38:54 UTC [microsoft] (model bigpickle)
- [UNVALIDATED] Source maps shipped live on both identity SPAs** — `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (no content, 4,359 paths) and `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35 MB, `sourcesContent` for 4,922 files). Recovered My Account request schemas below. Hashes (triage ref): myaccount.map `sha256 72290126…27d6541`, mysignins.map `sha256 2099f8a8…f3efbe`.
- [UNVALIDATED] Verified ID minting endpoint** `api.myaccount /api/issueVerifiedEmployeeCredential` = POST + `Bearer` token whose scope is the SPA's own `clientId` (8c59ead7-…-c8d2); backend gates solely on `GuestIsNotAllowedToIssueVerifiedId` / `TenantIsNotInAllowedToIssueVerifiedId` / feature-on — no visible admin-role check in the client contract.
- [UNVALIDATED] `/me/agentSignInSessions` (v1.0 + beta) is alive but undocumented** in public Graph docs (docs-contrib tree has no such API page); My Sign-Ins bundles `agentSessionApis.ts` / `agentSessions.ts` and `/recent-activity/agentic-sessions`; My Account uses new scope `AgentIdentity.Read.All` + `ServiceIdentity` SP type via `/users/{userId}/ownedObjects/Microsoft.graph.ServicePrincipal?$top=999`.
- [UNVALIDATED] `accounts.accesscontrol.windows.net` legacy-ACS surface**: `allowedAudiences` field is REMOVED from `/metadata/json/1` (tenant-enum patch confirmed per Sprocket 2026-05-19), but 5 live signing keys remain served — 3× self-signed `CN=accounts.accesscontrol.windows.net`, **2× `CN=login.microsoftonline.us` mixed into the global JWKS**; `/tokens/OAuth/2`, `/tokens/delegation/1`, `/mgmt/delegation/1` pathing alive (200 sign-in). This host was the vector for the Sep-2025 CVSS-10.0 ESTS actor-token impersonation chain (app `00000001-0000-0000-c000-000000000000`).
- [UNVALIDATED] ToS unauthenticated consent flow** embeds `access_token` + `id_token_hint` + `claims` (incl. `governance_control_state_per_device`) in the server-rendered `data-requestbody` DOM; `…/policyfiles/{policyFileId}/data` is a GET JSON (cross-tenant IDOR candidate).
- [UNVALIDATED] OAuth validation asymmetry** (SURFACE baseline): login.live.com fast-fails invalid `redirect_uri` (302, precise error); login.microsoftonline.com defers to a generic sign-in page. Documented AAD rules (reply-url doc): https-only (except localhost), case-sensitive exact match, `AADSTS50011`; RFC 6749 §3.1.2 string compare, RFC 9700 forbids wildcards.
- [UNVALIDATED] `controlplane.accledger.azure.com`: `/health` 200 leaks pod name + `x-ms-image-tag 1.0.03467.455-73c40f8d…`; no swagger, no public "Access Ledger" repo/docs found.
- [UNVALIDATED] api.myaccount: `/api/issueVerifiedEmployeeCredential` (POST, no body), `/api/canVerifiedIdBeIssued` (GET), `/api/termsofuse/agreements/{id}`, `/…/policyfiles/{id}`, `/…/policyfiles/{id}/data`, `/…/{id}/decline`, `/…/accept`, `/api/shell/navbardata`, `/api/dateTimeFormats`
- [UNVALIDATED] Graph beta: `/users/{id}/ownedObjects/Microsoft.graph.ServicePrincipal?$top=999`, `/users/{id}/sponsorOf/microsoft.graph.servicePrincipal?$top=999`, `/roleManagement/directory/roleAssignments?$filter=principalId eq '{id}'` (scope `AgentIdentity.Read.All`, SP type `ServiceIdentity`)
- [UNVALIDATED] IAMUX routing (adjacent, out-of-scope — NOT probed): `account-tip.activedirectory.windowsazure.com`, `account.activedirectory-ppe.windowsazure.com`, `account.activedirectory.windowsazure.us`, `account.aad.microsoft.scloud`, `account.aad.eaglex.ic.gov`
- [UNVALIDATED] ACS live token endpoints: `/tokens/OAuth/2`, `/tokens/delegation/1` (DelegationIssuance1.0), `/mgmt/delegation/1` (DelegationManagement1.0)

## 2026-08-07 10:45:00 UTC [microsoft] (model bigpickle) — POC

- [LIVE-VERIFIED] Graph $metadata crawl: agentSignInSession has 0 refs in BOTH v1 and beta $metadata (hashes 9d84e451..., ca304859...) — /me/agentSignInSessions is fully off-metadata. Agent ID entity model confirmed in metadata: agentIdentity (BaseType servicePrincipal, nav sponsors; beta adds inheritedAppRoleAssignments + inheritedOauth2PermissionGrants), agentIdentityBlueprint (application, nav inheritablePermissions ContainsTarget), agentIdentityBlueprintPrincipal, agentUser; user has sponsorOf+sponsors nav. Public docs (learn.microsoft.com/en-us/entra/agent-id + create-blueprint.md, 2026-05-01) + independent tests confirm sponsor-gated creation, blocked interactive flows/ROPC/client-secret-add, single-tenant agent identities, dir roles Agent ID Developer/Administrator, scopes AgentIdentityBlueprint.*/AgentIdentityBlueprintPrincipal.Create.
- [LIVE-VERIFIED] ACS /metadata/json/1 shape changed: keys now keyValue{type:x509Certificate,value,keyInfo{x5t,cloud_instance_name}}; allowedAudiences absent. 5 self-signed keys, active rotation (3x microsoftonline.com CN=accounts.accesscontrol.windows.net notBefore 06-08/06-17/07-04-2026; 2x microsoftonline.us CN=login.microsoftonline.us notBefore 07-01/07-26-2026), all notAfter 2031. DER sha256 05d17f72..., 34d09425..., 93a64915..., 1b499ff9..., a73ca1f9... x5t aFkmKVFc.../AahUf1bC.../fEtqrhKT... (com) + jvm_-Ttaq.../6hXLaIYN... (us).
- [LIVE-VERIFIED] Both .js.map still 200 text/plain with NO Cache-Control (immutable-hashed, deliberate-cacheable). myaccount CSP-Report-Only; mysignins real CSP + app-version 4.0.2233. main.shim.js = trivial loader + Azure Code Signing Authenticode block (ETag 1dd1b7e6aa49110).
- [POC-DESIGN] Agent-identity privilege inheritance: sponsor self-blueprint → agentIdentity inherits oauth2PermissionGrants/appRoleAssignments (beta nav) — test whether inherited grants can exceed the sponsor's own (test tenant). CVSS 7.0–9.0.
- [POC-DESIGN] agentSignInSessions cross-user: /users/{oid}/agentSignInSessions vs /me/ (test tenant). CVSS 6.5–8.8.
- [POC-DESIGN] Verified ID mint (no-role vs GlobalAdmin, both-204 signal). CVSS 7.1–9.1. policyfile IDOR cross-tenant GUID pairs. CVSS 5.3–7.5. redirect_uri matrix vs reply-url rules (documented variants, live only on in-scope login.live.com echo). CVSS 7.4–9.0.

## 2026-08-07 10:35:22 UTC [microsoft] (model bigpickle)
- [UNVALIDATED] `/me/agentSignInSessions` is fully off-metadata**: 0 references in both `v1.0/$metadata` (sha256 `9d84e451…`) and `beta/$metadata` (`ca304859…`) — the surface My Sign-Ins uses (`/recent-activity/agentic-sessions`) exists only as an internal route (baseline 401). Meanwhile the **Agent ID model IS public**: `agentIdentity` (BaseType `servicePrincipal`, nav `sponsors`; beta adds `inheritedAppRoleAssignments` + `inheritedOauth2PermissionGrants`), `agentIdentityBlueprint` (nav `inheritablePermissions` ContainsTarget), `agentIdentityBlueprintPrincipal`, `agentUser`; `user` exposes `sponsorOf`+`sponsors` nav (v1 metadata).
- [UNVALIDATED] ACS JWKS reconciled**: `allowedAudiences`/`Audience`/`Issuer` fields now absent from `/metadata/json/1` (tenant-enum patch reconfirmed). 5 self-signed signing keys with live rotation — 3× `CN=accounts.accesscontrol.windows.net` (notBefore 06-08/06-17/07-04-2026) tagged `cloud_instance_name=microsoftonline.com`, **2× `CN=login.microsoftonline.us`** (notBefore 07-01/07-26-2026) tagged `microsoftonline.us`. DER sha256: `05d17f72…`, `34d09425…`, `93a64915…`, `1b499ff9…`, `a73ca1f9…`.
- [UNVALIDATED] Public Agent ID docs + independent tests** (learn.microsoft.com/en-us/entra/agent-id; create-blueprint.md 2026-05-01; goodworkaround.com 2026-02-02; entrabot platform-docs) confirm hardening already in place: agent identities are single-tenant, interactive OAuth flows/ROPC blocked, `addPassword` on agentIdentity SP blocked, sponsor-required creation, dir roles `Agent ID Developer/Administrator`, scopes `AgentIdentityBlueprint.*`/`AgentIdentityBlueprintPrincipal.Create`. Feature is preview (Nov-2025) expanding to Dataverse (2026-08-06).
- [UNVALIDATED] Source maps still live** (api.myaccount 35.3 MB / mysignins 7 MB), both `200 text/plain`, **no Cache-Control** (immutable-hashed, deliberately cacheable — supporting asset only). myaccount shell: `CSP-Report-Only` + HSTS; mysignins shell: real CSP + `app-version 4.0.2233`.
- [UNVALIDATED] Graph (public metadata, read-only confirmed): `agentIdentity`, `agentIdentityBlueprint`, `agentIdentityBlueprintPrincipal`, `agentUser` entity types; `user.sponsorOf`/`user.sponsors`; beta `agentIdentity.inheritedOauth2PermissionGrants`/`inheritedAppRoleAssignments`.
- [UNVALIDATED] Creation endpoints (documented, test-tenant): `POST /v1.0/applications/microsoft.graph.agentIdentityBlueprint`, `/servicePrincipals/microsoft.graph.agentIdentityBlueprintPrincipal`, `/servicePrincipals/microsoft.graph.agentIdentity`, beta `/users` (agentUser).
- [UNVALIDATED] `api.myaccount /bundle/main.shim.js` (trivial loader + Azure Code Signing block, ETag `1dd1b7e6aa49110`).

## 2026-08-07 11:45:00 UTC [microsoft] (model bigpickle) — POC->RECON

- [LIVE-VERIFIED/EXHAUSTED] login.live.com redirect matrix: /oauth20_desktop.srf REMOVED (stub + ?removed=true for all inputs); /oauth20_authorize.srf returns generic sign-in 200 for all 8 redirect variants (host-case, path-case, trailing slash, extra path, http, relative, ?x=1, #frag), validation deferred post-auth, redirect_uri NOT echoed in HTML. No passive normalization signal. MSA client 00000000402B1722 used (docs).
- [RECON] agents.microsoft.com -> adoption.microsoft.com (Azure Front Door), cert CN=*.azureedge.net SAN mismatch => 404. Parked/adoption only, MS-controlled; no takeover. Agent Builder not at obvious hostname.
- [CODE-REVIEW] microsoft/entrabot docs/reference/token-flows.md + architecture/identity-and-token-flow.md: full three-hop Agent User chain (Hop1 client_credentials+cert+fmi_path; Hop2 FIC exchange; Hop3 grant_type=user_fic, user_id={oid} OR username={upn}, requested_token_use=on_behalf_of => idtyp=user delegated token).
- [LEAD-HIGH H-1] user_fic Hop-3 subject-validation gap hypothesis: does ESTS enforce identityParentId 1:1 link, or mint idtyp=user tokens for arbitrary user_id/UPN given valid blueprint+agent assertions? username={upn} alternate-key + requested_token_use=on_behalf_of = highest-risk knobs. CVSS 9.1-9.8 if real (test tenant only).
- [LEAD-HIGH H-2] Agent User delegated-consent escalation: can AgentIdUser.ReadWrite.IdentityParentedBy holder self-consent Files.ReadWrite.All/Mail.ReadWrite to own Agent User w/o admin approval? CVSS 7.5-8.8.
- [LEAD-MED H-3] managerApplications: up to 10 first-party apps manage Blueprints w/o AgentIdentityBlueprintPrincipal.ReadWrite.All — enumerate IDs, check for IDOR/confused deputy. 
- [CONFIRMED-HARDENING] blocked-permissions table public (agentid-platform-overview): 59 rows; app-perm agent blocks incl Application.ReadWrite.All, Directory.ReadWrite.All, RoleManagement.ReadWrite.*, Files/Sites/Group.*, User.ReadWrite.All; Directory.AccessAsUser.All delegated-blocked. Blueprint not a public client (publicClient/spa/isFallbackPublicClient excluded; web.redirectUris consent-recording only). No RFC7591 DCR. GA 2026-05-01; agentUser via POST /beta/users.

## 2026-08-07 11:34:29 UTC [google] (model bigpickle)
- [UNVALIDATED] login.live.com redirect_uri matrix EXHAUSTED (live, in-scope).** `/oauth20_desktop.srf` now serves a deprecation stub (`?removed=true`) for every input — the SURFACE-run fast-fail echo is gone because the endpoint was removed. `/oauth20_authorize.srf` returns a generic sign-in `200` for all 8 tested `redirect_uri` variants (host-case, path-case, trailing slash, extra path, `http://`, relative, `?x=1`, `#frag`) using documented MSA client `00000000402B1722`; validation is deferred post-auth and `redirect_uri` is **not** echoed in the HTML. No passive normalization signal exists → blocked behind authentication.
- [UNVALIDATED] New surface: Entra Agent ID went GA 2026-05-01**, and the `user_fic` Agent-User token chain is now fully documented + mirrored in `microsoft/entrabot` (in-scope org). Wire-level flow (learn.microsoft.com/en-us/entra/agent-id/agent-user-oauth-flow; github.com/microsoft/entrabot/blob/main/docs/reference/token-flows.md): Hop1 `client_credentials`+cert assertion+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` **or** `username={upn}` + `requested_token_use=on_behalf_of` → delegated token with `idtyp=user`, `oid={agent_user_oid}`. Agent User is a **real** Entra user (mailbox/Teams/license) with no own credentials, and its tokens "appear as a user to every Microsoft 365 API."
- [UNVALIDATED] Blocked-permissions table now public** (learn.microsoft.com/en-us/graph/api/resources/agentid-platform-overview): 59 rows. Agent identities cannot hold `Application.ReadWrite.*`, `Directory.ReadWrite.All`, `RoleManagement.ReadWrite.*`, `Sites/Files.ReadWrite.All`, `User.ReadWrite.All`, etc. Blueprints cannot be OAuth public clients (`publicClient`/`spa`/`isFallbackPublicClient` excluded; `web.redirectUris` consent-recording only). No RFC 7591 DCR; no `code_challenge_methods_supported` in OIDC metadata.
- [UNVALIDATED] `managerApplications`** on Blueprints: up to 10 first-party Microsoft apps can manage a Blueprint without `AgentIdentityBlueprintPrincipal.ReadWrite.All` — a supply-chain trust surface to enumerate.
- [UNVALIDATED] `agents.microsoft.com`** resolves into `adoption.microsoft.com` (Azure Front Door) and serves `*.azureedge.net` (SAN mismatch → 404). Parked/adoption only, MS-controlled; no takeover.
- [UNVALIDATED] Graph (v1.0, documented): `POST /v1.0/applications/microsoft.graph.agentIdentityBlueprint`, `/servicePrincipals/microsoft.graph.agentIdentityBlueprintPrincipal`, `/servicePrincipals/microsoft.graph.agentIdentity`; beta `POST /beta/users` (`@odata.type=microsoft.graph.agentUser`, prop `identityParentId`).
- [UNVALIDATED] Token endpoint usage (test-tenant): `POST /{tenant}/oauth2/v2.0/token` with `grant_type=user_fic`, `user_id`/`username`, `user_federated_identity_credential`, `requested_token_use=on_behalf_of`; consent via `POST /v1.0/oauth2PermissionGrants` (principalId=agent-user oid).
- [UNVALIDATED] Scopes/roles: `AgentIdUser.ReadWrite.IdentityParentedBy` / `AgentIdUser.ReadWrite.All`, dir roles `Agent ID Developer/Administrator`; blueprint scopes `AgentIdentityBlueprint.{Create,AddRemoveCreds.All,UpdateAuthProperties.All}`, `AgentIdentityBlueprintPrincipal.Create`.

## 2026-08-07 12:45:00 UTC [microsoft] (model bigpickle) — POC->RECON (Graph agent ecosystem + methodology fix)

- [METHODOLOGY-CORRECTION] Control test: nonexistent Graph paths AND documented POST-only actions all return 401 pre-routing (graph.microsoft.com authenticates before routing). All prior "401 = endpoint alive" claims for Graph (incl. /me/agentSignInSessions, /me/authentication/methods) are RETRACTED as null signals. Route existence must come from $metadata + docs. api.myaccount/api.mysignins 401/405 statuses remain valid.
- [NEW-SURFACE] Agent Registry (beta): /beta/agentRegistry singleton -> agentInstances/agentCardManifests/agentCollections (agentInstance links agentIdentityId+agentUserId+agentIdentityBlueprintId; url+additionalInterfaces; JWS agentCard signatures ES256 did:web agentcard+jws; ownerIds OR managedBy). Perms: AgentInstance.Read.All / ReadWrite.All / ReadWrite.ManagedBy; delegated requires Agent Registry Administrator role. Global cloud only. Deprecated May-2026 in favor of Agent 365.
- [NEW-SURFACE] Copilot agent management (beta): /beta/agents (nav copilotTools), /beta/copilot/agents, /beta/copilot/agentRegistrations, /beta/copilot/admin/catalog/packages (copilotPackageDetail; list/get/update/block/unblock/reassign; Agent 365 license + AI admin/Global admin), /beta/auditLogs/agents.
- [NEW-SURFACE] ID Protection agent risk (beta): /beta/identityProtection/riskyAgents (riskyAgentIdentity/riskyAgentIdentityBlueprintPrincipal/riskyAgentUser; confirmCompromised/confirmSafe/dismiss) + /beta/identityProtection/agentRiskDetections. Scope IdentityRiskyAgent.Read.All; roles Global Reader/Security Operator/Reader/Administrator.
- [NEW-SURFACE] Audit: signIn.agent = agentic.agentSignIn (agentServicePrincipalId, agentSubjectParentId, agentType/agentSubjectType, parentAppId); agentType enum notAgentic=0 agenticApp=2 agenticAppInstance=3 agentIdentityBlueprintPrincipal=4 agentIDuser=5.
- [LEAD-HIGH H-6] Agent Registry ownership boundary: managedBy enforced=caller appId? reassign/block role boundary? If not -> claim/overwrite any agent card manifest (instructions+endpoints). CVSS 7.5-9.0.
- [LEAD-HIGH H-7] agentCard JWS trust: is signature verified by M365 runtime or advisory? Unsigned/spoofed card = first-party impersonation. CVSS 6.5-9.0.
- [LEAD-MED H-8] riskyAgents write ops (confirmCompromised/Safe/dismiss) integrity vs agent-risk CA. CVSS 4-6. [LEAD-MED H-9] signIn.agent attribution as 2nd source for agentSignInSessions cross-user (PD-A). CVSS 6.5-8.8.
- [CODE-REVIEW] entrabot create_entra_agent_ids.py: consent grant POST /v1.0/oauth2PermissionGrants with CALLER-CHOSEN resourceId (Graph OR Azure Storage user_impersonation e406a681-...); one-time per-principal non-expiring; needs DelegatedPermissionGrant.ReadWrite.All. Application.Read.All NOT agent-blocked (role 9a5d68dd-...). Subtype navs: /servicePrincipals/{id}/microsoft.graph.agentIdentity/sponsors. Pooling anti-pattern note: object-id recycling residual-permission risk (design-acknowledged).

## 2026-08-07 12:32:14 UTC [google] (model bigpickle)
- [UNVALIDATED] Methodology correction (applies retroactively):** control-tested that unauthenticated GETs to *nonexistent* Graph paths and to *documented POST-only actions* all return `401 InvalidAuthenticationToken` — Graph authenticates **before** routing, so prior journal claims of "401 ⇒ endpoint alive" for Graph (incl. `/me/agentSignInSessions`, `/me/authentication/methods`) are **null signals and retracted**. Route existence now rests on public `$metadata` declarations + MS Learn docs. (api.myaccount / api.mysignins 401/405/404 remain valid signals — non-Graph hosts.)
- [UNVALIDATED] NEW surface — Agent Registry API** (`/beta/agentRegistry` singleton → `agentInstances`/`agentCardManifests`/`agentCollections`), declared in beta `$metadata` (sha256 `ca304859…`, unchanged from prior runs) + documented at learn.microsoft.com/graph/api/agentregistry-list-agentinstances. `agentInstance` binds `agentIdentityId`+`agentUserId`+`agentIdentityBlueprintId`, carries runtime endpoint `url`/`additionalInterfaces`, JWS-signed agent cards (`ES256`, `did:web` kid, `agentcard+jws`), and `ownerIds` **OR** `managedBy` ("either required"). Permissions: `AgentInstance.Read.All`/`.ReadWrite.All`/`.ReadWrite.ManagedBy`; delegated callers need **Agent Registry Administrator** role. Global cloud only; **deprecated May-2026** in favor of Agent 365.
- [UNVALIDATED] NEW surface — Copilot/agent admin** (beta): `/beta/agents` (nav `copilotTools`), `/beta/copilot/agents`, `/beta/copilot/agentRegistrations`, and `/beta/copilot/admin/catalog/packages` (copilotPackageDetail; list/get/update/**block/unblock/reassign**; Agent 365 license + AI admin/Global admin), plus `/beta/auditLogs/agents`.
- [UNVALIDATED] NEW surface — ID Protection agent risk** (beta): `/beta/identityProtection/riskyAgents` (subtypes riskyAgentIdentity / riskyAgentIdentityBlueprintPrincipal / riskyAgentUser; actions confirmCompromised/confirmSafe/dismiss) + `/beta/identityProtection/agentRiskDetections`. Scope `IdentityRiskyAgent.Read.All`; roles Global Reader/Security Operator/Reader/Administrator.
- [UNVALIDATED] NEW surface — audit attribution:** `signIn.agent` = `agentic.agentSignIn` (`agentServicePrincipalId`, `agentSubjectParentId`, `agentType`/`agentSubjectType`, `parentAppId`); enum `notAgentic=0, agenticApp=2, agenticAppInstance=3, agentIdentityBlueprintPrincipal=4, agentIDuser=5`.
- [UNVALIDATED] Code review (microsoft/entrabot):** consent-grant primitive is **resource-agnostic** — `POST /v1.0/oauth2PermissionGrants` with caller-chosen `resourceId` (Graph **or** Azure Storage `user_impersonation` `e406a681…`), one-time per-principal non-expiring. `Application.Read.All` is **not** in the agent blocked-permissions table (only `ReadWrite.*` are). entrabot design-docs acknowledge object-ID recycling/residual-permission risk ("pooling is an anti-pattern").
- [UNVALIDATED] Graph beta (all declared in `$metadata`, auth-gated): `/agentRegistry`, `/agentRegistry/agentInstances`, `/agentRegistry/agentCardManifests`, `/agentRegistry/agentCollections`, `/agents`, `/copilot/agents`, `/copilot/agentRegistrations`, `/copilot/admin/catalog/packages` (+`/{id}/block|unblock|reassign`), `/auditLogs/agents`, `/identityProtection/riskyAgents`, `/identityProtection/agentRiskDetections`.
- [UNVALIDATED] Graph beta entity model: `agent` (copilotTools), `agentRegistry`, `agentRegistration` (agentCard JSON manifest: iconUrl/provider/skills/security), `agentInstance` (signatures, url, interfaces), `agentCardManifest`, `agentCollection`, `copilotPackage`/`copilotPackageDetail` (allowedUsersAndGroups, sensitivityLabel), `riskyAgent*`, `agentic.agentSignIn` on `signIn`/`summarizedSignIn`.
- [UNVALIDATED] Graph API surface (entrabot): `POST /v1.0/oauth2PermissionGrants` (caller-chosen resourceId), `POST /v1.0/servicePrincipals/{oid}/appRoleAssignments` (Application.Read.All role `9a5d68dd…`), `GET /servicePrincipals/{oid}/microsoft.graph.agentIdentity/sponsors`, `GET /users?$filter=identityParentId eq '{oid}'`.

## 2026-08-07 13:55:00 UTC [microsoft] (model bigpickle) — POC->RECON (Agent 365 package API + entrabot wire review)

- [CODE-EXACT] Three-hop flow confirmed in code (entrabot src/entrabot/tools/teams.py:126-221): Hop1 client_credentials+fmi_path={agent_id}+assertion → T1; Hop2 FIC exchange (T1 as assertion) → T2; Hop3 grant_type=user_fic, client_assertion=T1, user_id={agent_user_oid}, user_federated_identity_credential=T2, requested_token_use=on_behalf_of, scope={resource}/.default → idtyp=user. H-1 knob = user_id/username subject selector at Hop 3.
- [LEAD-HIGH H-2 refinement] Consent escalation has TWO primitives: POST /v1.0/oauth2PermissionGrants (new grant, caller-chosen resourceId incl. Azure Storage e406a681-...) AND PATCH /v1.0/oauth2PermissionGrants/{id} {"scope": merged} to add scopes to existing grant (grant_consent.py). Needs DelegatedPermissionGrant.ReadWrite.All.
- [NEW-SURFACE] Copilot Package Management API documented at BOTH v1.0+beta (learn.microsoft.com/microsoft-365/copilot/extensibility/api/admin-settings/package/*): /copilot/admin/catalog/packages list/get/update/block/unblock/reassign. v1.0 $metadata DECLARES copilotRoot->copilotAdmin->copilotAdminCatalog.packages (verified). Scope CopilotPackages.Read.All delegated = NO admin consent required (merill.net). Requires Agent 365 license + AI admin/Global admin; was Frontier-program gated (michev.info 2026-04-07); writes non-functional as of Apr-2026; app-context GET = 424; $select unsupported; elementDetails (prompts/instructions) only on GET.
- [LEAD-MED H-10] Non-admin self-consent CopilotPackages.Read.All then GET catalog — is the backend admin-role check real or scope-only? 200 on non-admin => tenant-wide agent inventory + prompt disclosure. CVSS 5.3-7.5. Test-tenant.
- [LEAD-MED H-12] Consent primitive vs Work IQ MCP audiences: grant McpServers.Mail.All / McpServers.OneDriveSharepoint.All / Tools.ListInvoke.All (agent365.svc.cloud.microsoft, OUT OF SCOPE host) to Agent User — blocked-permissions table coverage unknown. Full-mailbox path via Agent User. CVSS 7.5-8.8.
- [LEAD-HIGH H-7 grounding] agentCardSignature = A2A v1.0 JWS shape (RFC 7515+RFC 8785); industry: signed cards mandatory for cross-org trust; unsigned = self-signed-TLS-tier. Precedent: agent-did "key purpose violation" (signature accepted from any DID-doc key). Test: M365 runtime verify+key-bind or accept unsigned.
- [RECON] crt.sh down 5th run; certspotter: no certs for agent365/agent.microsoft.com; Google CT endpoint 404. DNS: copilotstudio.microsoft.com resolves (IPv6, Copilot Studio portal); agent365/agentbuilder/copilotbuilder/agentregistry/agentcard*.microsoft.com NXDOMAIN.
- [SCOPE-FLAG] OfficeDev/CopilotPackageManager (Microsoft-owned, linked from MS Learn) — OfficeDev NOT in scope.yml github_orgs; clarify with MSRC before mining.

## 2026-08-07 13:57:09 UTC [google] (model bigpickle)
- [UNVALIDATED] Graph beta (all declared in `$metadata`, auth-gated): `/agentRegistry`, `/agentRegistry/agentInstances`, `/agentRegistry/agentCardManifests`, `/agentRegistry/agentCollections`, `/agents`, `/copilot/agents`, `/copilot/agentRegistrations`, `/copilot/admin/catalog/packages` (+`/{id}/block|unblock|reassign`), `/auditLogs/agents`, `/identityProtection/riskyAgents`, `/identityProtection/agentRiskDetections`.
- [UNVALIDATED] Graph beta entity model: `agent` (copilotTools), `agentRegistry`, `agentRegistration` (agentCard JSON manifest: iconUrl/provider/skills/security), `agentInstance` (signatures, url, interfaces), `agentCardManifest`, `agentCollection`, `copilotPackage`/`copilotPackageDetail` (allowedUsersAndGroups, sensitivityLabel), `riskyAgent*`, `agentic.agentSignIn` on `signIn`/`summarizedSignIn`.
- [UNVALIDATED] Graph API surface (entrabot): `POST /v1.0/oauth2PermissionGrants` (caller-chosen resourceId), `POST /v1.0/servicePrincipals/{oid}/appRoleAssignments` (Application.Read.All role `9a5d68dd…`), `GET /servicePrincipals/{oid}/microsoft.graph.agentIdentity/sponsors`, `GET /users?$filter=identityParentId eq '{oid}'`.

## 2026-08-07 14:45:00 UTC [microsoft] (model bigpickle) — POC->RECON (Agent 365 GA transition + new Agent Registration / Policy Settings surfaces)

- [TRANSITION] Agent Registry API RETIRED 2026-06-15 (MC1297981) -> replaced by Agent 365-powered Agent Registration API /beta/copilot/agentRegistrations (POST/GET/PATCH/DELETE; perm AgentRegistration.ReadWrite.All delegated 20f263bf-7d50-4e66-912c-16b4b4194fd4 / app 39fb8c64-7bd3-4107-8515-14d6e55ddda4; AdminConsentRequired=YES both; Global cloud only). Entity has NO signatures/JWS prop; agentCard=untyped graph.Json.
- [LEAD-HIGH H-13] Agent Registration ownership boundary: create requires CLIENT-SUPPLIED createdBy (+sourceCreated/LastModifiedDateTime); PATCH rewrites ownerIds/managedByAppId/agentIdentityId/agentCard of any registration; docs show NO ownership enforcement beyond scope. If PATCH/DELETE on foreign id accepted -> card rewrite = agent impersonation/supply-chain. CVSS 7.5-9.0. Test-tenant.
- [LEAD-MED-HIGH H-14] createdBy client-writable = forgeable audit attribution (breaks ownerless/reassign governance); GET by id cross-tenant/cross-user boundary untested; metadata is Collection so GET /copilot/agentRegistrations (no-list doc) may enumerate. CVSS 5.3-7.5.
- [NEW-SURFACE] Copilot Policy Settings API /beta/copilot/admin/policySettings/{id}: 5 settings (microsoft.copilot.{copilotchatpinning,blockaccesstoopenfiles,imagegeneration,allowwebsearch,allowinadmincenters}); no LIST; CopilotPolicySettings.Read/.ReadWrite delegated (app ctx 403); docs silent on admin role.
- [LEAD-MED H-15] policyId = Exchange storeId -> arbitration mailbox "Organization Partition_PolicyService_c2ada927-a9e2-4564-aae2-70775a2fa0af", settings under ApplicationDataRoot/a4900027-b443-4789-aade-1180a176b8d0, item class SDS.c2ada927-...Setting. Copilot governance lives in Exchange mailbox infra; any non-admin path to that mailbox/folder = policy tamper (blockaccesstoopenfiles/imagegeneration flip). CVSS 4-7.
- [STATUS] Package Mgmt API: GET/LIST GA'd to /v1.0/copilot/admin/catalog/packages, app perm CopilotPackages.Read.All works (michev 2026-07-25: 251 pkgs); writes /beta only, no app ctx; $select unsupported; Agent 365 SKU enforced server-side (403); reassign still broken (204-doc, fails live). Consent error leaks Agent 365 backend app id bdc49611-ba72-43b9-a868-652243121c10.
- [H-10 REVISED DOWN] license check server-side -> non-admin question narrows to role-vs-scope for pure read in licensed tenant (unverified, admin-only tests so far). H-11 app-context reads now work; cross-tenant package-id boundary untested.
- [CODE] entrabot a365 layer: 10 Work IQ MCP servers + scopes (McpServers.{Word,Mail,OneDriveSharepoint,User,Dataverse,Teams}.All / Tools.ListInvoke.All) at agent365.svc.cloud.microsoft/agents/servers/{name}; Work IQ token = same three-hop user_fic with resource_scope={audience}/.default (H-12 wire-exact); mcp_client 401=rejected/403=consent-missing; odsp tools getFileOrFolderMetadataByUrl/readSmallTextFile/readSmallBinaryFile XPIA-wrapped.
- [CODE] xpia.py prompt-injection envelope escapes only close tag (open tag not escaped; attribute-escape ok; ENTRABOT_XPIA_WRAP_ENABLE=false opt-out) - design-only, low. show_agent_status.py: consent filter shape clientId eq '{oid}' and principalId eq '{oid}'; AGENT_USER_WORK_IQ_LICENSE_SKU dedicated.
- [HOST] copilotstudio.microsoft.com = Copilot Studio/PowerVA maker portal: Island Gateway (x-ms-islandgateway), Service Fabric, 8408-byte shell for all paths, dynamic SPA at web.powerva.microsoft.com/v/0.0.20260729.1-26.07.26-prod/; connect-src api.powerplatform.com/apihub.azure.com/*.island.powerapps.com. Login-gated; low passive signal; future authenticated-flow surface.
- [META] beta+v1 $metadata hashes unchanged across GA (ca304859.../9d84e451...) - schema predates GA.

## 2026-08-07 14:49:14 UTC [google] (model bigpickle)
- [UNVALIDATED] Graph beta (documented): `POST/GET/PATCH/DELETE /copilot/agentRegistrations{,/{id}}`; `GET/PATCH /copilot/admin/policySettings/{id}`.
- [UNVALIDATED] Graph v1 (documented, GA): `GET /v1.0/copilot/admin/catalog/packages` (app-context read OK).
- [UNVALIDATED] Beta metadata entities: `agentRegistration` (no signatures), `copilotPolicySetting` (policyId+value), `copilotPackage` now carries `supportedHosts`/`isBlocked`/`ownerId`/`type`/`publisher`; `packageType` enum microsoft/external/shared/custom; `copilotAdmin.policySettings` nav.
- [UNVALIDATED] Public IDs: AgentRegistration scopes `20f263bf-…`/`39fb8c64-…`; Agent 365 backend app `bdc49611-…`; arbitration mailbox GUIDs `c2ada927-…`/`a4900027-…`.

## 2026-08-07 15:45:00 UTC [microsoft] (model bigpickle) — POC->RECON (NEW: Copilot Studio Direct-to-Engine S2S API, private preview)

- [NEW-SURFACE] Copilot Studio D2E (Direct-to-Engine) S2S API, private preview (microsoft/Agents docs/copilot-studio-client-s2s-doc.md + MS Learn 2026-08-03): programmatic access to published agents, AAD-JWT-only, Client->PPAPI Gateway->MCS. Perm **CopilotStudio.Copilots.Invoke** on Power Platform API (resourceAppId 8578e004-a5c6-46e7-913e-12f58912df43; app role 38c13204-7d79-4d83-bdbb-b770e28400df). ADMIN CONSENT REQUIRED (community confirmed). AuthZ = admin-consent + maker share-ACL (viewer) + same-tenant; modes True S2S (app-only, agent must be No-Auth) / OBO (Integrated auth).
- [LEAD-HIGH H-16] D2E conversation-ID NOT validated server-side (copilotStudioWebChat.ts:40-44 verbatim: server "does not validate conversation IDs. A non-existent GUID will silently create a new conversation under that ID"; .NET StartRequest.ConversationId -> x-ms-conversation-id on start). Documented turn-time guardrails (CallerIdentityMismatch / CallerIdentityTypeMismatch / D2EAccessDenied) unproven for: cross-app resumption, subscribe+Last-Event-Id replay, orchestrated path-param convId. If bypass -> conversation hijack / history disclosure / active-session prompt injection. CVSS 6.5-9.0. Test-tenant (POST-only, no passive PoC).
- [LEAD-MED-HIGH H-18] Orchestrated API /powervirtualagents/orchestrated/{cdsBotId}/conversations/{conversationId} (StartConversation/InvokeTool/HandleUserResponse/ConversationUpdate; 'internal use only' but public client, OrchestratedClient.cs). InvokeTool takes client-supplied toolSchemaName+inputs - is it validated against agent's registered tools / foreign conversation? Tool-confusion. CVSS 6.5-8.5.
- [LEAD-MED-LOW H-17] x-ms-d2e-experimental response header -> SDK promotes host to DirectConnectUrl and sends subsequent Bearer requests there (CopilotClient.cs:509-517). If header ever reflects attacker input -> client token exfiltration. CVSS 4-6.
- [LEAD-MED H-19] D2E envhost fully derivable from EnvironmentId GUID: {envid-last2hex}.{last2hex}.environment.{suffix}; endpoint /copilotstudio/dataverse-backed|prebuilt/authenticated/bots/{schemaName}/conversations?api-version=2022-03-01-preview. De-facto secrets = schemaName + share-ACL. PPAPI error-differentiation is the passive signal.
- [LEAD-LOW H-20] DirectLine channel secret requires NO admin consent vs D2E admin-consented Copilots.Invoke; same agent on both = lower-barrier path, no user identity propagation (attribution gap). design-note.
- [SCOPE-FLAG] D2E runtime hosts *.powerplatform.com / *.environment.api.powerplatform.com / api.gov/high.powerplatform.microsoft.us / api.appsplatform.us / api.powerplatform.microsoft.scloud / *.microsoftonline.cn are NOT in scope.yml - code-review only, NOT probed. Clarify with MSRC if D2E grows. *.island.powerapps.com (in scope) may host the experimental island URL.
- [SCOPE] google/a2a permanently moved to a2aproject/A2A (repo 954873280) - A2A spec repo no longer in declared google org; treat as doc-reference. microsoft/agents tree has 0 jws/did/agentcard hits - M365 Agent SDK does NOT implement card signature verification (strengthens H-7: card trust is server-side/arbitrary Json).
- [RECON] SDK cross-verified .NET/JS/Python (endpoint parity, api-version 2022-03-01-preview); package @microsoft/agents-copilotstudio-client v1.7.0-beta.5; PowerPlatformCloud enum leaks internal fleet: api.exp/prv/dev/preprod/test.powerplatform.com, api.appsplatform.us (DoD), api.powerplatform.eaglex.ic.gov, api.powerplatform.microsoft.scloud.

## 2026-08-07 15:46:38 UTC [microsoft] (model bigpickle)
- [UNVALIDATED] `POST /copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations` (start), `.../conversations/{id}` (turn), `.../conversations/{id}/subscribe` (SSE), `POST /powervirtualagents/orchestrated/{cdsBotId}/conversations/{id}` — all `https://{envhost}` `api-version=2022-03-01-preview`.
- [UNVALIDATED] Headers: `x-ms-conversation-id`, `x-ms-conversationid` (resp), `x-ms-d2e-experimental` (resp), `x-ms-client-request-id`, `x-ms-correlation-id`, `x-cci-agent-version`, `Last-Event-Id`.
- [UNVALIDATED] Perm/resource IDs: Power Platform API `8578e004-…`, `CopilotStudio.Copilots.Invoke` app role `38c13204-…`.
- [UNVALIDATED] Cloud fleet leaked by `PowerPlatformCloud` enum: `api.exp/prv/dev/preprod/test.powerplatform.com`, `api.appsplatform.us` (DoD), `api.powerplatform.eaglex.ic.gov`, `api.powerplatform.microsoft.scloud`.
## 2026-08-07 16:00:46 UTC [google] (model bigpickle)
[HYP] D2E conversation-ID resumption / cross-app hijack
class: IDOR
asset: POST https://{envhost}/copilotstudio/dataverse-backed|prebuilt/authenticated/bots/{schemaName}/conversations{,/{id}} — api-version=2022-03-01-preview
confidence: 55
reasoning: copilotStudioWebChat.ts:40-44 verbatim — server "does not validate conversation IDs; a non-existent GUID will silently create a new conversation under that ID." Documented turn-time guardrails (CallerIdentityMismatch/CallerIdentityTypeMismatch/D2EAccessDenied) are unproven for cross-app resumption and subscribe+Last-Event-Id replay. Envhost fully derivable from EnvironmentId GUID (H-19); de-facto secrets = schemaName + share-ACL.
evidence_needed: second principal resumes a conversation created by another app under a chosen GUID and reads/continues the turn stream (history disclosure / active-session injection).
verify_steps: AUTH_HELPED (test-tenant; prerequisites: MSRC clarify scope of *.powerplatform.com envhost, then admin-consented CopilotStudio.Copilots.Invoke on resourceAppId 8578e004-…): 1) app A POST start with x-ms-conversation-id=attacker-chosen GUID → 2) app B POST turn to …/conversations/{same GUID} with its own Bearer → 3) observe whether CallerIdentityMismatch fires or the conversation resumes. Passive equivalent: none (POST-only) — PPAPI error-differentiation on 401/403/424 is the only passive signal (recon only).
impact: conversation hijack / transcript disclosure / active-session prompt injection. CVSS 6.5–9.0.
testability: AUTH_HELPED
[HYP] Agent Registration ownership boundary (foreign PATCH/DELETE)
class: IDOR
asset: POST/GET/PATCH/DELETE /beta/copilot/agentRegistrations{,/{id}} (perm AgentRegistration.ReadWrite.All, Global cloud)
confidence: 50
reasoning: create requires CLIENT-SUPPLIED createdBy (+sourceCreated/LastModifiedDateTime); PATCH rewrites ownerIds/managedByAppId/agentIdentityId/agentCard of any registration id; docs show no ownership enforcement beyond scope (AgentIdentityId = Agent 365 backend bdc49611-…). Retired Agent Registry (2026-06-15) had same-shape managedBy/ownerIds fields.
evidence_needed: principal B PATCHes agentCard/ownerIds on a registration created by principal A in same tenant and succeeds (or 204 no-op rather than 403).
verify_steps: AUTH_HELPED (test-tenant, two principals): 1) A POSTs registration → 2) B GET /beta/copilot/agentRegistrations to test undocumented enumeration (metadata is Collection) → 3) B PATCH /beta/copilot/agentRegistrations/{A's id} {"agentCard":<rewrite>} → 4) observe status. Passive: none (auth-gated 401).
impact: agent card rewrite = agent impersonation / supply-chain; forgeable audit attribution. CVSS 7.5–9.0.
testability: AUTH_HELPED
[HYP] user_fic Hop-3 subject validation (arbitrary user_id/username)
class: AUTH
asset: POST https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token — grant_type=user_fic
confidence: 45
reasoning: three-hop flow code-exact (entrabot teams.py:126-221): Hop3 takes client_assertion=T1 + user_federated_identity_credential=T2 + user_id={oid} OR username={upn} + requested_token_use=on_behalf_of → idtyp=user. H-1 knob = alternate-key subject selector; identityParentId 1:1 linkage enforcement by ESTS is the unknown. Hardening noted: single-tenant agent identities, sponsor-gated creation, blocked interactive/ROPC — lowers prior but flow is high-value if broken.
evidence_needed: ESTS returns a token with idtyp=user and oid=victim for user_id of a user NOT the agent's parent — or rejects; username alternate-key accepted vs user_id.
verify_steps: AUTH_HELPED (test-tenant; needs blueprint+agent cert+client_assertion chain): 1) acquire T1/T2 per documented FIC flow → 2) POST token with user_id={unrelated second user's oid} → 3) decode idtyp/oid/identityParentId; repeat with username={other.upn}. Passive: none.
impact: delegated idtyp=user token for arbitrary subject = full-user impersonation to every M365 API. CVSS 9.1–9.8 if real.
testability: AUTH_HELPED
[NEXT] RAG: In microsoft/Agents + @microsoft/agents-copilotstudio-client (both in-scope orgs), grep for CallerIdentityMismatch / CallerIdentityTypeMismatch / D2EAccessDenied handlers and the subscribe path (Last-Event-Id, x-ms-conversation-id) to determine whether conversationId is bound to the caller's app identity/share-ACL server-side or trusted from the client — this decides whether H-A needs a full test-tenant run or is a dead end. No live requests (scope.yml passive_only).
## 2026-08-07 16:41:51 UTC [google] (model bigpickle)
[HYP] OAuth2 permission-grant escalation via caller-chosen resourceId
class: BUSLOGIC
asset: POST https://graph.microsoft.com/v1.0/oauth2PermissionGrants (and /beta)
confidence: 52
reasoning: consent primitive accepts caller-chosen `resourceId` spanning Graph or Azure Storage `user_impersonation` e406a681-…; `Application.Read.All` absent from the agent blocked-permissions list. If grant creation is scoped only by the caller's app-object write and not by an approved-permission policy, a low-privileged principal can mint unapproved resource grants.
evidence_needed: principal without delegated-admin/consent rights creates a grant with `resourceId`=Azure Storage and scope it never possessed; Graph returns 201/204 (vs 403 Forbidden).
verify_steps: AUTH_HELPED (test-tenant): 1) create app A (attacker) + app B; 2) principal owning A POSTs /v1.0/oauth2PermissionGrants {clientId:A, resourceId:<e406a681-…>, scope:"user_impersonation"} → 3) observe 201/204 vs 403; 4) if accepted, attempt AAD token for that resource under A's client_credentials. Passive: none (401).
impact: unapproved cross-resource consent grants → tenant storage/data exposure, permission creep. CVSS 6.5–8.5.
testability: AUTH_HELPED
[HYP] Verified ID employee credential minting without per-subject employee proof
class: AUTH
asset: POST https://api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential
confidence: 50
reasoning: own source-map probe proves a body-less Bearer-only POST whose full gate surface is 6 tenant-scoped checks + rate limit, and zero per-caller employee/HR verification; client carries no identity claims, so minting/claims are decided server-side from the token. Any non-guest member of an onboarded tenant is a candidate subject.
evidence_needed: in an onboarded test tenant, a synthetic member (non-employee, profile containing attacker-set jobTitle/department) mints successfully and the returned credential/QR embeds those directory-derived claims.
verify_steps: AUTH_HELPED: 1) confirm `/api/canVerifiedIdBeIssued` → 200 for the tenant; 2) acquire SPA-scope Bearer (clientId 8c59ead7-…) as the synthetic member; 3) POST `/api/issueVerifiedEmployeeCredential`; 4) decode returned credential/QR for claim provenance (server vs directory). Passive pre-stage DONE (gate map exhaustive client-side; residual risk = unmapped 500-class server checks).
impact: self-issued signed employee identity in the Entra Verified ID ecosystem (presentation to any relying party). CVSS 7.5–9.5 if real.
testability: AUTH_HELPED
[HYP] Blueprint managerApplications trust-boundary bypass
class: AUTH
asset: GET/PATCH AgentIdentityBlueprint.managerApplications (Graph beta, entrabot-documented)
confidence: 45
reasoning: ≤10 first-party manager apps can manage Blueprints WITHOUT `AgentIdentityBlueprintPrincipal.ReadWrite.All`; unknown whether the manager list is client-writable or whether manager tokens are validated to the exact app id — a client-controlled manager entry = blueprint read/write without the documented permission.
evidence_needed: non-admin principal enumerates or PATCHes itself into managerApplications and then GET/PATCHes a Blueprint (204/200 vs 403).
verify_steps: AUTH_HELPED (test-tenant): 1) GET managerApplications → observe whether enumeration demands the R/W scope; 2) PATCH managerApplications with attacker app id → observe 204 vs 403; 3) if writable, PATCH the Blueprint. Passive: none (401).
impact: agent identity blueprint tamper = agent supply-chain (inherited permissions). CVSS 6.5–8.5.
testability: AUTH_HELPED
[NEXT] HUMAN: In an onboarded Verified ID test tenant, run the authorized mint test for HYP-2 — synthetic non-employee member with attacker-set `jobTitle`/`department` → `POST /api/issueVerifiedEmployeeCredential` (SPA-scope Bearer, clientId 8c59ead7-…) → decode the returned credential/QR and trace every claim's provenance (server-derived vs directory-derived). Passive pre-stage already complete (35MB map grepped: 7-code gate surface, body-less POST, no per-subject proof). Do NOT file until a full mint with user-controlled claims is observed.
[RISK] google: 35 — remaining live surface (tokeninfo oracle, codelab keys) largely REJECTED/fixed/DUP; all GCP control-plane reads consumer-identity gated; no unauthenticated exploitable branch identified.
[RISK] microsoft: 78 — identity SPAs ship production source maps; Bearer-only Verified ID mint endpoint lacks per-subject proof; consent-grant primitive takes caller-chosen resourceId; off-metadata agentSession/agentRegistrations + D2E POST surface all in play behind auth.
## 2026-08-07 17:32:47 UTC [google] (model bigpickle)
[HYP] Agent Registration ownership boundary / undocumented collection enumeration
class: IDOR
asset: GET/POST/PATCH/DELETE https://graph.microsoft.com/beta/copilot/agentRegistrations{,/{id}} (scope AgentRegistration.ReadWrite.All, Global cloud)
confidence: 50
reasoning: create accepts CLIENT-SUPPLIED `createdBy`; PATCH rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agentCard` of any registration id; docs show no ownership enforcement beyond scope; metadata typed Collection so a bare GET may enumerate all tenant registrations. Convergence with nemotron3 [75] noted — differentiated sub-claim here is the **enumeration** probe, not the PATCH.
evidence_needed: principal B (no ownership) enumerates collection via GET and/or PATCHes A's registration → 200/204 list/rewrite vs 403.
verify_steps: AUTH_HELPED (test-tenant, two principals): 1) A POST creates registration → 2) B GET `/beta/copilot/agentRegistrations` (tests undocumented enumeration) → 3) B PATCH `/beta/copilot/agentRegistrations/{A's id}` `{"agentCard":<rewrite>}` → 4) observe 200/204 vs 403. Passive: none (401-gated).
impact: cross-app agent registration tamper → agent impersonation / supply-chain / forgeable audit attribution. CVSS 7.5–9.0.
testability: AUTH_HELPED
[HYP] user_fic Hop-3 subject validation (arbitrary user_id / username)
class: AUTH
asset: POST https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token — grant_type=user_fic
confidence: 45
reasoning: Hop3 takes client_assertion=T1 + user_federated_identity_credential=T2 + user_id={oid} OR username={upn} → requested_token_use=on_behalf_of → idtyp=user. Alternate-key selector is the H-1 knob; ESTS 1:1 linkage of subject-to-agent-parent is the unknown. Hardening (single-tenant agent identity, sponsor-gated creation, no interactive/ROPC) lowers prior.
evidence_needed: ESTS returns idtyp=user+oid for a user_id unrelated to the agent's parent, or accepts username alternate-key vs user_id — else rejects.
verify_steps: AUTH_HELPED (test-tenant; needs blueprint+agent cert+client_assertion chain): 1) acquire T1/T2 → 2) POST token `user_id={unrelated user oid}` → 3) decode idtyp/oid/identityParentId; repeat with `username={other.upn}`. Passive: none.
impact: delegated idtyp=user token for arbitrary subject = full-user impersonation to every M365 API. CVSS 9.1–9.8 if real.
testability: AUTH_HELPED
[HYP] D2E conversation-ID resumption / cross-app session hijack
class: IDOR
asset: POST https://{envhost}/copilotstudio/dataverse-backed/authenticated/bots/{schemaName}/conversations{,/{id}}?api-version=2022-03-01-preview (scope CopilotStudio.Copilots.Invoke)
confidence: 55
reasoning: RAG PROVES the official JS SDK threads a caller-chosen conversationId into the URL path for start (`StartRequest.conversationId`), execute (`executeStreaming`), and subscribe (`subscribeAsync` + `Last-Event-ID`), with zero client-side ownership checks and no client handling of CallerIdentityMismatch/D2EAccessDenied. Only the server-side envhost/Dataverse ACL remains as the guard.
evidence_needed: principal B executes a turn or subscribes (with Last-Event-ID) against a conversation A created under a caller-chosen GUID; either the turn stream resumes (bind failure) or a documented guard error fires (bind enforced).
verify_steps: AUTH_HELPED (test-tenant, two app principals; prereq MSRC confirm `*.powerplatform.com` envhost in scope): 1) app A POST start with `StartRequest.conversationId`=attacker-chosen GUID → 2) app B POST `…/conversations/{same GUID}` + GET subscribe with `Last-Event-ID` using its own Bearer → 3) observe whether conversation resumes or guard error returns. Passive: none (Bearer-gated, SSE only).
impact: cross-app conversation hijack / transcript disclosure / active-session prompt injection. CVSS 6.5–9.0.
testability: AUTH_HELPED
## 2026-08-07 18:28:48 UTC [google] (model bigpickle)
## 2026-08-07 18:48:39 UTC [google] (model bigpickle)
## 2026-08-07 19:17:28 UTC [google] (model bigpickle)
[HYP] Agent Registration ownership boundary / collection enumeration (carry-forward, differentiated sub-claim = enumeration)
class: IDOR
asset: GET/POST/PATCH/DELETE https://graph.microsoft.com/beta/copilot/agentRegistrations{,/{id}} (scope AgentRegistration.ReadWrite.All)
confidence: 50
reasoning: create accepts CLIENT-SUPPLIED createdBy; PATCH rewrites ownerIds/managedByAppId/agentIdentityId/agentCard; metadata types collection (contains-target) with no OperationRestrictions; converged with nemotron3 [75] — differentiated sub-claim is bare GET enumeration, not PATCH.
evidence_needed: principal B (no ownership) GETs collection (200 array) and/or PATCHes A's registration (200/204) vs 403.
verify_steps: AUTH_HELPED (test-tenant, two principals): 1) A POST creates registration; 2) B GET /beta/copilot/agentRegistrations (enumeration); 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":<rewrite>}; 4) observe 200/204 vs 403. Passive: none (401, re-confirmed).
impact: cross-app agent registration tamper → impersonation/supply-chain/forged audit attribution. CVSS 7.5–9.0.
testability: AUTH_HELPED
[HYP] OAuth2 permission-grant escalation via caller-chosen resourceId (carry-forward)
class: BUSLOGIC
asset: POST https://graph.microsoft.com/v1.0/oauth2PermissionGrants (and /beta)
confidence: 52
reasoning: consent primitive accepts caller-chosen resourceId spanning Graph or Azure Storage user_impersonation (e406a681-…); Application.Read.All absent from agent blocked list; passive 401 re-confirmed this cycle, no new gate.
evidence_needed: principal without delegated-admin/consent rights creates grant {clientId:A, resourceId:AzureStorage, scope:"user_impersonation"} → 201/204 vs 403.
verify_steps: AUTH_HELPED (test-tenant): 1) app A + app B; 2) owner of A POSTs grant as above; 3) observe 201/204 vs 403; 4) if accepted, AAD client_credentials for that resource.
impact: unapproved cross-resource consent grants → tenant storage/data exposure. CVSS 6.5–8.5.
testability: AUTH_HELPED
[HYP] D2E conversation-ID resumption / cross-app session hijack (carry-forward)
class: IDOR
asset: POST https://{envhost}/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations{,/{id}} (scope CopilotStudio.Copilots.Invoke)
confidence: 55
reasoning: RAG PROVES JS SDK threads caller-chosen conversationId into start/executeStreaming/subscribeAsync (+Last-Event-ID) with zero client ownership checks; only envhost/Dataverse ACL guards.
evidence_needed: principal B executes turn or subscribes against conversation A created under caller-chosen GUID → resumes vs documented guard error.
verify_steps: AUTH_HELPED (test-tenant, two app principals; prereq MSRC confirm envhost in scope): 1) app A POST start w/ chosen GUID; 2) app B POST /conversations/{same GUID} + GET subscribe w/ Last-Event-ID using own Bearer; 3) observe resume vs guard.
impact: cross-app conversation hijack / transcript disclosure / active-session prompt injection. CVSS 6.5–9.0.
testability: AUTH_HELPED
[PARKED] Verified ID employee-credential mint hypothesis (confidence 50, carry-forward) — retained but NOT re-filed this cycle: already flagged as aggregate NEXT (hypotheses-bigpickle.txt 18:48), business_value 9 highest, human test queued, no new passive evidence this cycle.
[FINAL] 1. agentRegistrations ownership/enumeration (IDOR, merged with nemotron3 [75]) — priority 6.55
[FINAL] 2. D2E conversation-ID resumption (IDOR) — priority 6.50
[FINAL] 3. oauth2PermissionGrants caller-chosen resourceId (BUSLOGIC) — priority 6.50
[NEXT] HUMAN: two-principal test-tenant probe (top-ranked, unexecuted): A POST /beta/copilot/agentRegistrations (createdBy/ownerIds client-set) → B GET /beta/copilot/agentRegistrations (collection enumeration, Bearer scope AgentRegistration.ReadWrite.All) → B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":...} → record 200/204 vs 403 for each hop.
[LEARN] REJECTED dual-JWKS rotation desync / alg-confusion @ login.microsoftonline.com/discovery/keys: verified live — v1 kid set (5) is a strict subset of v2.0 (8), all kty=RSA sig keys, no v1-exclusive kid, no x5c/EC key → no cross-endpoint rotation desync, no kid-collision confusion surface. (Answers laguna ov-kids probe; issuer-confusion sub-claim tracked by laguna [55].)
[RISK] google: 35 — tokeninfo oracle is the sole anonymous branch (info-only, no-token→400); all GCP control-plane reads consumer-identity gated; keys dual-endpoint verified clean; no exploitable unauth branch.
[RISK] microsoft: 78 — identity SPA source maps still live (main.caa6a456.js.map 7MB 200); off-metadata agentSignInSessions + agentRegistrations + consent-grant + D2E primitives all 401 but Bearer-gated behind authorized-token tests; exposure surface unchanged, exploit status pending test-tenant.
[LEARN] REJECTED dual-JWKS rotation desync / alg-confusion @ login.microsoftonline.com/discovery/keys: verified live — v1 kid set (5) is a strict subset of v2.0 (8), all kty=RSA sig keys, no v1-exclusive kid, no x5c/EC key → no cross-endpoint rotation desync, no kid-collision confusion surface. (Answers laguna ov-kids probe; issuer-confusion sub-claim tracked by laguna [55].)
[RISK] google: 35 — tokeninfo oracle is the sole anonymous branch (info-only, no-token→400); all GCP control-plane reads consumer-identity gated; keys dual-endpoint verified clean; no exploitable unauth branch.
[RISK] microsoft: 78 — identity SPA source maps still live (main.caa6a456.js.map 7MB 200); off-metadata agentSignInSessions + agentRegistrations + consent-grant + D2E primitives all 401 but Bearer-gated behind authorized-token tests; exposure surface unchanged, exploit status pending test-tenant.
[HYP] Agent Registration ownership boundary / collection enumeration (carry-forward; differentiated sub-claim = bare GET enumeration)
class: IDOR
asset: GET/POST/PATCH/DELETE https://graph.microsoft.com/beta/copilot/agentRegistrations{,/{id}} (scope AgentRegistration.ReadWrite.All)
confidence: 50
reasoning: create accepts client-supplied `createdBy`; PATCH rewrites `ownerIds`/`managedByAppId`/`agentIdentityId`/`agentCard`; metadata types it a contains-target collection with zero OperationRestrictions; converged with nemotron3 [75]. Re-probed: endpoint 401 `InvalidAuthenticationToken`, alive.
evidence_needed: principal B (no ownership) GETs collection (200 array) and/or PATCHes A's registration (200/204) vs 403.
verify_steps: AUTH_HELPED (test-tenant, two principals): 1) A POST creates registration; 2) B GET `/beta/copilot/agentRegistrations` (enumeration); 3) B PATCH `/beta/copilot/agentRegistrations/{A-id}` `{"agentCard":<rewrite>}`; 4) observe 200/204 vs 403. Passive: none (401, confirmed).
impact: cross-app agent registration tamper → agent impersonation / supply-chain / forged audit attribution. CVSS 7.5–9.0.
testability: AUTH_HELPED
[HYP] D2E conversation-ID resumption / cross-app session hijack (carry-forward)
class: IDOR
asset: POST https://{envhost}/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations{,/{id}} (scope CopilotStudio.Copilots.Invoke)
confidence: 55
reasoning: RAG proves JS SDK threads caller-chosen conversationId into start/executeStreaming/subscribeAsync (+Last-Event-ID) with zero client ownership checks; only envhost/Dataverse ACL remains.
evidence_needed: principal B executes a turn or subscribes against a conversation A created under a caller-chosen GUID → resumes vs documented guard error (CallerIdentityMismatch/D2EAccessDenied).
verify_steps: AUTH_HELPED (test-tenant, two app principals; prereq MSRC confirm envhost in scope): 1) app A POST start with chosen GUID; 2) app B POST `/conversations/{same GUID}` + GET subscribe with `Last-Event-ID` using its own Bearer; 3) observe resume vs guard.
impact: cross-app conversation hijack / transcript disclosure / active-session prompt injection. CVSS 6.5–9.0.
testability: AUTH_HELPED
[HYP] OAuth2 permission-grant escalation via caller-chosen resourceId (carry-forward)
class: BUSLOGIC
asset: POST https://graph.microsoft.com/v1.0/oauth2PermissionGrants (and /beta)
confidence: 52
reasoning: consent primitive accepts caller-chosen `resourceId` spanning Graph or Azure Storage `user_impersonation` (e406a681-…); `Application.Read.All` absent from agent blocked-permissions; endpoint re-probed 401, no new gate.
evidence_needed: principal without delegated-admin/consent rights creates grant `{clientId:A, resourceId:AzureStorage, scope:"user_impersonation"}` → 201/204 vs 403.
verify_steps: AUTH_HELPED (test-tenant): 1) app A + app B; 2) owner of A POSTs grant as above; 3) observe 201/204 vs 403; 4) if accepted, AAD client_credentials for that resource.
impact: unapproved cross-resource consent grants → tenant storage/data exposure, permission creep. CVSS 6.5–8.5.
testability: AUTH_HELPED
[NEXT] HUMAN: two-principal test-tenant probe of the top-ranked hypothesis (unexecuted): A POST `/beta/copilot/agentRegistrations` (client-set `createdBy`/`ownerIds`) → B GET `/beta/copilot/agentRegistrations` with its own Bearer (scope `AgentRegistration.ReadWrite.All`) to test collection enumeration → B PATCH `/beta/copilot/agentRegistrations/{A-id}` `{"agentCard":...}` → record 200/204 vs 403 at each hop.
[LEARN] REJECTED dual-JWKS rotation desync / alg-confusion @ login.microsoftonline.com/discovery/keys: verified live — v1 kid set (5) is a strict subset of v2.0 (8), all kty=RSA sig keys, no v1-exclusive kid, no EC/x5c divergence → no cross-endpoint rotation desync or kid-collision surface. (Answers laguna's ov-kids probe; issuer-confusion sub-claim stays with laguna [55].)
[RISK] google: 35 — tokeninfo oracle is the sole anonymous branch (info-only; no-token→400); all GCP control-plane reads consumer-identity gated; dual-JWKS verified clean; no exploitable unauth branch.
[RISK] microsoft: 78 — identity SPA source maps still live (main.caa6a456.js.map 7MB, HTTP 200); off-metadata agentSignInSessions + agentRegistrations + consent-grant + D2E primitives all re-confirmed 401/Bearer-gated; exposure surface unchanged, exploit status pending authorized test-tenant execution.
## 2026-08-07 19:31:53 UTC [google] (model bigpickle)
[HYP] Agent Registration ownership boundary bypass via client-controlled createdBy + PATCH rewrite
class: IDOR
asset: POST/GET/PATCH https://graph.microsoft.com/beta/copilot/agentRegistrations{,/{id}} (scope AgentRegistration.ReadWrite.All)
confidence: 55
reasoning: $metadata types this a contains-target collection with zero OperationRestrictions; create accepts client-supplied createdBy; PATCH rewrites ownerIds/managedByAppId/agentIdentityId/agentCard. Converged with laguna [82]/nemotron3 [75]. Re-probed 401 this cycle, alive.
evidence_needed: principal B GETs collection (200 array) and/or PATCHes A's registration (200/204) vs 403 with no ownership.
verify_steps: AUTH_HELPED (test-tenant, two principals): 1) A POST create w/ client-set createdBy/ownerIds; 2) B GET /beta/copilot/agentRegistrations (own Bearer) → enumeration; 3) B PATCH /beta/copilot/agentRegistrations/{A-id} {"agentCard":<rewrite>}; record 200/204 vs 403 at each hop.
impact: cross-app agent registration tamper → agent impersonation / supply-chain / forged audit attribution. CVSS 7.5–9.0.
testability: AUTH_HELPED
[HYP] D2E conversation-ID resumption / cross-app session hijack
class: IDOR
asset: POST https://{envhost}/copilotstudio/dataverse-backed/authenticated/bots/{schema}/conversations{,/{id}} (scope CopilotStudio.Copilots.Invoke)
confidence: 55
reasoning: JS SDK threads caller-chosen conversationId into start/executeStreaming/subscribeAsync (+Last-Event-ID) with zero client ownership checks; only envhost/Dataverse ACL guards remain. Envhost unresolvable anonymously (tenant-scoped) — prereq MSRC confirm in scope.
evidence_needed: principal B executes a turn or subscribes against a conversation A created under a caller-chosen GUID → resumes vs documented guard error.
verify_steps: AUTH_HELPED (test-tenant, two app principals): 1) A POST start w/ chosen GUID; 2) B POST /conversations/{same GUID} + GET subscribe w/ Last-Event-ID using own Bearer; observe resume vs guard.
impact: cross-app conversation hijack / transcript disclosure / active-session prompt injection. CVSS 6.5–9.0.
testability: AUTH_HELPED
[HYP] Verified ID employee-credential mint via caller-chosen claims
class: BUSLOGIC
asset: POST https://api.myaccount.microsoft.com/api/issueVerifiedEmployeeCredential
confidence: 50
reasoning: backend gates only on GuestIsNot/Tenant flags (per source-map analysis); claims/fields otherwise caller-supplied; Bearer scope = SPA clientId 8c59ead7-...; endpoint alive 401. Business value highest of program (verified-credential mint = identity trust root).
evidence_needed: test-tenant principal with a legit token mints a credential with attacker-modified claim fields → accepted vs claim-schema rejection.
verify_steps: AUTH_HELPED (test-tenant): 1) obtain Bearer via SPA clientId flow; 2) POST /api/issueVerifiedEmployeeCredential with modified employee claims; 3) observe issued-credential acceptance vs field-level rejection.
impact: forged Verified Employee credentials → downstream RP trust compromise / account takeover at relying parties. CVSS 7.5–9.5.
testability: AUTH_HELPED
