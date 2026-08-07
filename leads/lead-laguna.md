# Laguna leads — Google Bug Hunters (RECON→SURFACE→HYPOTHESIS)

## STATE
- scope.yml: `*.google.com`, `google.com`, GitHub org `google` (code review allowed). JS-rendered bughunters rules pages; sitemap (488 URLs) enumerated instead.
- crt.sh returned 502/404 throughout (transient/outage). Subdomain inventory deferred — used OIDC discovery + API discovery as the authoritative host inventory instead.
- Previous (stale) leads were Microsoft-scoped; reset for Google run.

## RECON (inventory)
[R1] Scope: `*.google.com` + `google.com`. Bug-hunt channel = bughunters.google.com (JS SPA, no SSR). `storage.googleapis.com` and `bughunters.google.com` (social OG image host) observed in page metadata.
[R2] Google GitHub org (`google`) — 99 non-fork repos (first 100 page). Top security/infra-relevant: `boringssl` (2246, C crypto), `mundane` (1084, Rust crypto), `oss-fuzz` (12496, fuzzing infra), `mantis` (722, "modular stack-agnostic toolkit of security review sk[...]"), `goonami-scanner` (20, "general purpose network security"), `secure-aggregation` (22), `prompt-encryption-sdk` (20), `safe-bindings` (19, Crubit C++↔Rust). Language mix: Python 31, Rust 13, Go 12, TypeScript 8, C++ 6, Kotlin 3, etc. Many repos updated within last 7 days (fresh attack surface).
[R3] `boringssl`, `mundane`, `oss-fuzz` were NOT in the first-100-by-stars list (API default sort = newest-first, capped at 100). Located via targeted `/search/repositories` (boringssl=2246, mundane=1084). boringssl is archived=False (active).

## SURFACE (auth + API)
[R4] OIDC discovery — `https://accounts.google.com/.well-known/openid-configuration` (HTTP 200, ACA-cors:*). AuthZ=`https://accounts.google.com/o/oauth2/v2/auth`; token=`https://oauth2.googleapis.com/token`; userinfo=`https://openidconnect.googleapis.com/v1/userinfo`; jwks=`https://www.googleapis.com/oauth2/v3/certs`; revoke=`https://oauth2.googleapis.com/revoke`. **response_types_supported include implicit flow: `token`, `id_token`, `code token`, `code id_token`, `token id_token`.** `code_challenge_methods_supported=[plain, S256]`. `token_endpoint_auth_methods_supported=[client_secret_post, client_secret_basic]`. **No `introspection_endpoint`** surfaced in discovery.
[R5] Auth-flow validation — GET to `/o/oauth2/v2/auth` with an unknown `client_id` (`0000…apps.googleusercontent.com`) returns HTTP 302 → `/signin/oauth/error?authError=…invalid_client…The OAuth client was not found.` → client_id IS validated pre-auth (unlike Microsoft ESTS). redirect_uri is NOT echoed back in the error body for unknown clients.
[R6] Identity Toolkit v3 — `https://identitytoolkit.googleapis.com/$discovery/rest?version=v3` (HTTP 200). basePath=`/identitytoolkit/v3/relyingparty/`, single resource `relyingparty` with 20 methods, all POST. Sensitive: `getAccountInfo`, `uploadAccount` (batch), `downloadAccount` (batch), `getOobConfirmationCode`, `verifyAssertion`, `verifyPassword`, `resetPassword`, `deleteAccount`, `signOutUser`, `signupNewUser`, `setAccountInfo`. `delegatedProjectNumber` token appears 10× in v3 discovery doc — the cross-project delegation field is present in this (legacy-but-still-served) API.
[R7] Identity Toolkit v1 (current API) — `https://identitytoolkit.googleapis.com/$discovery/rest?version=v1` (HTTP 200). Resources: `accounts` (16 methods), `v1` (4), `projects` (accounts + queryAccounts). **Current `accounts:lookup` (POST, path `v1/accounts:lookup`) — the supported replacement for v3 getAccountInfo — accepts request $ref `GoogleCloudIdentitytoolkitV1GetAccountInfoRequest`:** OAuth scope `https://www.googleapis.com/auth/cloud-platform`. **`delegatedProjectNumber` token appears 10× in v1 discovery doc** — confirmed present in the CURRENT API, not just legacy. `accounts.sendOobCode` (OOB email link / verify / recover-email / reset-password), `accounts.sendVerificationCode`, `accounts.verifyPhoneNumber` also current.
[R8] Public token introspection oracle — `https://oauth2.googleapis.com/tokeninfo`. No auth header required; accepts `?access_token=X` and `?id_token=X` as query params. Confirmed: missing token → 400 `{"error":"invalid_token","error_description":"Either access_token, id_token, or token_handle required"}`; bad token → 400 `{"error":"invalid_token","error_description":"Invalid Value"}`. Returns token metadata (aud, scope, expiry) when supplied a real token. (By-design OIDC, but a token-leak amplification oracle given token-in-query.)
[R9] userinfo error surface — `GET https://openidconnect.googleapis.com/v1/userinfo` w/o token → HTTP 404 (HEAD). `GET` with bad Bearer → HTTP 401 `{"error":"invalid_request","error_description":"Invalid Credentials"}` (proper OIDC 401, not 405/401-leak like MS Graph). `oauth2.googleapis.com/v1/token`, `securetoken.googleapis.com/v1/token`, `openidconnect…/v1/userinfo` all 404 on bare HEAD (routes exist for GET).
[R10] Google API Discovery — `https://www.googleapis.com/discovery/v1/apis` lists 527 public discovery docs. Identity-relevant present: Admin SDK (admin), Cloud Identity (cloudidentity v1), Identity Toolkit (identitytoolkit v1+v3). Many APIs → broad surface for next-slot enumeration.

## HYPOTHESES (one phase deeper — formal, with read-only PoC design)
[H1] **Cross-project Firebase user-data access (IDOR) via Identity Toolkit `delegatedProjectNumber`.** v1 `accounts:lookup` and v3 `relyingparty/getAccountInfo` both accept `delegatedProjectNumber` in their request schema (10× confirmed in each discovery doc). The field is intended for admin multi-resource contexts. If the resource layer binds the field only loosely to the caller's credential project (i.e., any OAuth token with `cloud-platform` scope can inject an arbitrary numeric `delegatedProjectNumber` and read `email`/`phoneNumber`/`localId`/`provider-issuers` from a *different* Firebase project), this is a cross-tenant PII IDOR. Impact: enumerate/resolve victim PII across Firebase tenant projects.
  - Read-only PoC design: `POST https://identitytoolkit.googleapis.com/v1/accounts:lookup` with `Authorization: Bearer <token>` and JSON `{"delegatedProjectNumber":"<victim project number>","localIds":["<uid>"]}` — observe whether records from the victim project are returned.
  - Constraint: requires a valid cloud-platform Bearer token (cannot be triggered with a naked GET/HEAD). Mitigation note: prior art is the 2021 "Firebase project compromise / project number collision" research class — Google's project-number namespace is global and numeric, a plausible confusion boundary.
  - Priority: HIGH. Hand off to next slot for auth-assisted POC (non-state-changing, GET-equivalent read) or source review of the `idtoken`/`user_id` verification path.
[H2] **Email/phone account-existence enumeration differential (regression surface).** IT v3 `getOobConfirmationCode` (RESET_PASSWORD) and v1 `accounts.sendOobCode` historically return distinct errors (`USER_NOT_FOUND` vs `INVALID_IDENTIFIER`) based on whether an identifier is registered. Google Bug Hunters' invalid-reports catalog lists "email enumeration" as a known/no-reward category — but the surface re-opens on every API-version migration (v3→v1) where error parity may drift. Lower confidence (likely already mitigated → invalid report), but the v1 current API is untested territory.
  - Read-only PoC design: POST `…/v1/accounts:sendOobCode` / `…v3/relyingparty/getOobConfirmationCode` with a probe email + a valid token; diff error codes for registered vs unregistered identifiers.
  - Constraint: POST + token. Document as a scoped regression check, not a standalone finding.
  - Priority: LOW.
[H3] **Implicit-flow (`response_type=token` / `id_token`) on first-party Google auth endpoint.** The OIDC discovery advertises implicit-flow response types. Implicit flow is deprecated due to token-in-fragment leakage (referrer / history / browser-extensions). For `accounts.google.com` itself this is first-party and gated by client allow-listing, but any first-party client_id that advertises an http(s) redirect_uri that is not strictly bound to a registered app host could leak tokens via referer. This is a client-registration hygiene surface, not a server bug.
  - Read-only PoC design: enumerate redirect_uris published for known Google client_ids via the `oauth2.googleapis.com/tokeninfo` aud claim (requires a real token) — cannot self-probe read-only.
  - Priority: INFO / non-actionable. Defer.

## SECRETS CHECK (public google/ repos, read-only grep — no scanners)
[H4] Scanned `google/boringssl`, `google/mundane`, `google/mantis` READMEs (+ first-100 org repo set) for `AIzaSy*` (GCP API key), `sk-[A-Za-z0-9]{20,}` (OpenAI key), and 40+ char base64 blobs. **Result: 0 real secrets.** All grep matches were README prose noise:
  - boringssl: sha256=`e1643e2bacc6dbeb…` — a 51-char substring of `https://www.chromium.org/Home/chromium-security/reporting-security-bugs/` (URL prose).
  - mantis: sha256=`80cb6a41bba70c3c…` — a 70-char run of markdown header underscores `____…____`.
  - Note: GitHub `search/code` API requires auth (HTTP 401) so could not be used; relied on direct-file greps. `google/mundane` README returned 404 (main vs master branch). Recommend next slot run authenticated secret-scan (gitleaks/trufflehog) over the full org if allowed by rules — excluded this run (no scanners per program rules).

## CVSS CANDIDATES
- H1 (delegatedProjectNumber IDOR): CVSS ~6.5 (AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N) — contingent on auth-assisted validation. Tentative only.
- H2 (enumeration): CVSS ~3.7 (AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N) — low; likely already mitigated.

## STATE UPDATE
STATUS_PHASE: HYPOTHESIS
STATUS_STATE: HIGH_POTENTIAL
NEXT_STEP_1: PHASE 4 POC (next slot) — auth-assisted H1 read probe (`accounts:lookup` with cross-project `delegatedProjectNumber`) using a non-state-changing GET-equivalent read; requires valid cloud-platform token. Also attempt H1 via OSS-Fuzz/oss-fuzz infra source review (the `src/` of relevant google repos) if a code path for `delegatedProjectNumber` resolution is open-source.
NEXT_STEP_2: PHASE 4 POC (next slot) — H2 regression probe on IT v1 `accounts:sendOobCode` error parity (registered vs unregistered) vs v3 `getOobConfirmationCode`; diff + report delta if drift exists.
NEXT_STEP_3: PHASE 2 SURFACE (next slot) — enumerate Identity Toolkit v3 relyingparty `getAccountInfo` `delegatedProjectNumber` + `returnUploadKey` schema in full detail from discovery doc; map the exact project-number binding semantics (request `projectNumber` vs delegated vs caller credential) to harden H1 scope vs dismiss.

## 2026-08-07 14:51:05 UTC [google] (model laguna)
- [UNVALIDATED] | Host / Endpoint | Purpose |
- [UNVALIDATED] |---|---|
- [UNVALIDATED] | `accounts.google.com/.well-known/openid-configuration` | OIDC discovery (implicit flow) |
- [UNVALIDATED] | `accounts.google.com/o/oauth2/v2/auth` | AuthZ w/ pre-auth client_id validation |
- [UNVALIDATED] | `oauth2.googleapis.com/token` · `/tokeninfo` · `/revoke` | Token issuance (public introspection oracle) |
- [UNVALIDATED] | `openidconnect.googleapis.com/v1/userinfo` | OIDC userinfo (401 JSON) |
- [UNVALIDATED] | `identitytoolkit.googleapis.com/$discovery/rest?version=v1` | **Current** Firebase Auth API (`accounts:lookup`, `sendOobCode`, `signInWith*`) |
- [UNVALIDATED] | `identitytoolkit.googleapis.com/$discovery/rest?version=v3` | Legacy `relyingparty/` (getAccountInfo, uploadAccount, downloadAccount…) |
- [UNVALIDATED] | `www.googleapis.com/discovery/v1/apis` | 527 API dir (admin, cloudidentity, identitytoolkit) |
- [UNVALIDATED] | `bughunters.google.com/sitemap.xml` | Scope/rules enumeration (JS-SPA bypass) |
- [UNVALIDATED] | `www.gstatic.com/bughunters/*/app_bundle_prod.js` | Client bundle w/ `/content/pages` + `/reports/*` routes |
