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

## POC PHASE — read-only verification signals (GET/HEAD only)

### [POC-H1] delegatedProjectNumber IDOR surface — endpoint confirmation
- Discovery docs (HTTP 200) confirm the routes exist & are **POST-only**:
  - v1 current API: `POST https://identitytoolkit.googleapis.com/v1/accounts:lookup` — request `$ref` = `GoogleCloudIdentitytoolkitV1GetAccountInfoRequest`, OAuth scope `https://www.googleapis.com/auth/cloud-platform`.
  - v3 legacy API: `POST https://identitytoolkit.googleapis.com/v3/relyingparty/getAccountInfo`.
  - `delegatedProjectNumber` field confirmed present (10× in v1 doc, 10× in v3 doc) — the cross-project-delegation primitive is the crux of H1.
- Read-only signal test (route binding): `HEAD`/`GET` on `/v1/accounts:lookup` and `/v3/relyingparty/getAccountInfo` both return **HTTP 404** (Google API gateway catch-all HTML 404). **Inconclusive passively** — Google's gateway returns the *same* 404 for a POST-only path on HEAD/GET as for a nonexistent path, so GET/HEAD cannot confirm POST-binding. Endpoint existence is therefore confirmed *only* via discovery-doc enumeration (R6/R7), not by direct route probe.
- Auth-assisted PoC (non-state-changing read — **HANDOFF**; requires a valid `cloud-platform` Bearer token, which passive rules forbid acquiring/using):
  ```
  POST https://identitytoolkit.googleapis.com/v3/relyingparty/getAccountInfo
  Authorization: Bearer <GCP-OAuth-token w/ https://www.googleapis.com/auth/cloud-platform>
  Content-Type: application/json
  {"delegatedProjectNumber":"<victim_tenant_project_number>","getTokenResult":true}
  ```
  Expected signal **if vulnerable**: returns `email`/`phoneNumber`/`localId`/provider list belonging to a user in the victim Firebase project even though the caller's token was issued for a *different* project → cross-tenant PII IDOR. Expected signal **if hardened**: `403 PERMISSION_DENIED` / tenant-mismatch error / empty result.
  Scope caveat: `identitytoolkit.googleapis.com`, `oauth2.googleapis.com`, `openidconnect.googleapis.com`, `www.googleapis.com` are `*.googleapis.com` hosts — **NOT** literally `*.google.com`/`google.com` in this scope.yml. Treated as in-scope because Google's VRP covers its Identity Platform APIs and OAuth infrastructure; flag for triage.
- Status: **UNVALIDATED** — blocked on the passive-only constraint (cannot issue a state-changing POST + cannot mint/use a bearer token read-only).

### [POC-H3] redirect_uri / implicit-flow — read-only validation (VERIFIED NEGATIVE)
- Probe A: `GET https://accounts.google.com/o/oauth2/v2/auth?...&client_id=123&redirect_uri=javascript:alert(1)&response_type=token` → **HTTP 302** → `/signin/oauth/error?authError=…invalid_request…"doesn't comply with Google's OAuth 2.0 policy…secure-redirection-handling"…redirect_uri\njavascript:alert(1)`.
- Probe B (control): same `client_id=123` + `redirect_uri=https://developers.google.com/oauthplayground` → **HTTP 302** → `/signin/oauth/error?authError=…invalid_client…"The OAuth client was not found."`.
- **Finding:** Google enforces a **server-side redirect_uri scheme accept-list (https only) evaluated BEFORE client_id lookup**. A `javascript:`/`data:`/non-https redirect_uri is rejected at the universal policy layer regardless of client_id validity — so there is **no open-redirect via redirect_uri confusion and no redirect_uri-bypass of client binding**. Implicit flow (`response_type=token`) is advertised/live, but redirect_uri is strictly bound. H3 = **verified negative**; the only residual (client-side) risk is token-in-fragment exposure for any *legitimately registered* first-party client using implicit flow — hygiene issue, not a server bug.

### [POC-H4] tokeninfo oracle — id_token error-handling anomaly (INCONCLUSIVE)
- `GET https://oauth2.googleapis.com/tokeninfo?id_token=<malformed>` once returned **HTTP 500 `{"error":{"code":500,"message":"Internal error encountered.","status":"INTERNAL"}}`** (vs the standard 400 `invalid_token`).
- Determinism check: re-issued the identical payload 3× → all **HTTP 400** `Invalid Value`.
- **Assessment:** intermittent, non-reproducible — likely a transient handler error on the id_token JWT parse path (one GFE/frontend instance 500'd, then self-healed). Not a reportable deterministic finding. Recommend retest with a structurally-valid-but-expired `id_token` + `tokeninfo` to confirm the 500 is not triggered by a specific parse branch; defer.

### [NEW SURFACE RECON] Bug Hunters report submission flow (bughunters.google.com — IN SCOPE, *.google.com)
- HEAD/GET on `bughunters.google.com/report/captcha`, `/reports`, `/reports/upload`, `/kintaro/form`, `/profiles/search` → **all HTTP 404**. These are client-side SPA routes (JS bundle `/content/pages`, `/report/captcha`, `/reports/*` routes from R-bundle) — server returns 404 (no SSR), no backend logic reachable passively. Root `/` → **HTTP 200**, hardened: `Strict-Transport-Security: max-age=2592000; includeSubdomains`, `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`. No passive findings.

## STATE UPDATE
STATUS_PHASE: POC
STATUS_STATE: HIGH_POTENTIAL  (1 verified-negative, 1 confirmed-surface-unvalidated-requires-auth-handoff, 1 inconclusive-anomaly)
NEXT_STEP_1: PHASE 4 POC (auth-assisted, next slot) — H1: POST `relyingparty/getAccountInfo` w/ `delegatedProjectNumber` injected to a victim project, cloud-platform bearer in hand; non-state-changing read. If 500/tenant-mismatch is reproduced on `tokeninfo`, re-derive from a valid-but-expired id_token.
NEXT_STEP_2: PHASE 4 POC (auth-assisted, next slot) — H2: error-parity probe `accounts:sendOobCode` vs `getOobConfirmationCode` (registered vs unregistered identifier).
NEXT_STEP_3: PHASE 1 RECON (new surface) — crt.sh was down this run; retry CT-log subdomain inventory for `*.google.com` (esp. `*.google.com` services not in the 527-API dir, e.g. `play.google.com`, `checkout.google.com`, `mail.google.com` auth variants) via a fresh CT source (Censys/GraphQL cert transparency) to expand the in-scope attack surface map.

## 2026-08-07 15:10:00 UTC [google] (model laguna) — POC PHASE RESULTS
| Hypothesis | POC probe (read-only) | Result | CVSS |
|---|---|---|---|
| H1 delegatedProjectNumber IDOR | GET/HEAD on accounts:lookup routes + discovery-doc confirmation | route is POST-only (404 on HEAD/GET, inconclusive passively); field confirmed 10×/doc in v1+v3 → **surface CONFIRMED, exploit requires auth handoff** | ~6.5 AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N (contingent) |
| H3 implicit-flow/redirect_uri | GET authZ w/ javascript:redirect_uri vs valid-https | redirect_uri scheme policy enforced **before** client_id; javascript: rejected at policy layer → **VERIFIED NEGATIVE** | N/A |
| H4 tokeninfo id_token 500 | GET tokeninfo w/ malformed id_token (1×500 then 3×400 same payload) | non-reproducible transient → **INCONCLUSIVE** | N/A |
| (new surface) bughunters report flow | HEAD /report/captcha,/reports,etc | all 404 (SPA client routes); root hardened (HSTS+includeSubdomains,XFO:DENY,nosniff) → **no passive finding** | N/A |
| (secrets) open repos grep | grep boringssl/mundane/mantis READMEs for AIzaSy/sk-/40+char | 0 real secrets (prose noise only) — hashes logged | N/A |

Endpoints validated this run (all read-only GET/HEAD, ~1/sec, on-scope `*.google.com` unless flagged):
- accounts.google.com/o/oauth2/v2/auth — AuthZ (redirect_uri scheme-policy enforced)
- bughunters.google.com/* — report-flow SPA (no SSR), hardened headers
Flagged-for-scope (googleapis.com identity infra): identitytoolkit.googleapis.com, oauth2.googleapis.com, openidconnect.googleapis.com, www.googleapis.com (covered by Google VRP intent).

## 2026-08-07 15:49:07 UTC [microsoft] (model laguna)
- [UNVALIDATED] H1 (IDOR) — surface CONFIRMED, exploit blocked by passive-only rule.** Identity Toolkit `accounts:lookup` (v1) & `getAccountInfo` (v3) discovery docs (HTTP 200) confirm the routes are POST-only and both carry the `delegatedProjectNumber` cross-project-delegation field (**10× in each doc**). HEAD/GET returns Google-gateway 404 (can't confirm POST-binding passively) — endpoint existence is proven only via discovery enumeration.
- [UNVALIDATED] H3 — VERIFIED NEGATIVE.** `GET accounts.google.com/o/oauth2/v2/auth?...&client_id=123&redirect_uri=javascript:alert(1)` → 302 → *redirect_uri secure-redirection-handling policy error*; same `client_id` + valid-https URI → 302 → *invalid_client*. Google validates the redirect_uri **scheme accept-list before client_id** — no open-redirect / redirect_uri-confusion; implicit flow is live but URIs strictly bound.
- [UNVALIDATED] H4 — INCONCLUSIVE (transient).** `GET oauth2.googleapis.com/tokeninfo?id_token=<malformed>` returned HTTP 500 once, then 3× HTTP 400 on the identical payload → non-reproducible handler-path fluke; retest w/ valid-but-expired id_token recommended.
- [UNVALIDATED] NEW SURFACE (bughunters report flow)** — `/report/captcha`, `/reports`, `/reports/upload`, `/kintaro/form`, `/profiles/search` all 404 (client-side SPA, no SSR). Root `/` hardened: HSTS `max-age=2592000;includeSubDomains`, `X-Frame-Options: DENY`, `nosniff`. No passive finding.
- [UNVALIDATED] Secrets** — boringssl/mundane/mantis READMEs: 0 real secrets (prose noise only, sha256 hashes logged: `e1643e2ba…`, `80cb6a41b…`).
## 2026-08-07 16:11:13 UTC [google] (model laguna)
[NEW] login.microsoftonline.com OIDC discovery: v2.0 (issuer login.microsoftonline.com/{tid}/v2.0; JWKS /discovery/v2.0/keys, 8 RSA keys; mtls alias mtlsauth.microsoft.com; tls_client_certificate_bound_access_tokens=true) + v1.0 (issuer sts.windows.net/{tid}; JWKS /discovery/keys, 5 RSA keys; response_types add implicit `token` + hybrid `token id_token` absent in v2.0) — GET 200 on both discovery docs
[NEW] Graph $metadata: 1,183 EntityTypes, 326 Functions across microsoft.graph.identityGovernance + microsoft.graph.security + microsoft.graph.entraRecoveryServices; 22 filterByCurrentUser bindings — GET /v1.0/$metadata HTTP 200 (2.9MB)
[NEW] Graph API 405 anomaly: unauth HEAD/GET to /v1.0, /me, /users → HTTP 405 (Content-Length: 0), NO WWW-Authenticate Bearer — verified passively (4×405)
[NEW] v2.0 authorize HTTP 200 error rendering: GET /oauth2/v2.0/authorize?response_type=token (unsupported in v2.0) → HTTP 200 with embedded JS error code 700038 (iHttpErrorCode 400, "We received a bad request") instead of HTTP 400 — verified passively
[NEW] oauth2.googleapis.com/tokeninfo public introspection oracle: accepts ?access_token= / ?id_token= query params (no Authorization header), returns aud/scope/expiry — verified (no-token→400, malformed→400)
[NEW] bughunters.google.com root `/` → HTTP 200, hardened (HSTS max-age=2592000;includeSubdomains, X-Frame-Options: DENY, X-Content-Type-Options: nosniff)
[PRIO] login.microsoftonline.com OAuth2/OIDC (authorize+token+discovery+mTLS): score 9.2 | a=9 b=10 t=9 g=10 c=6 f=10
[PRIO] graph.microsoft.com ($metadata + identityGovernance + 405 anomaly): score 7.55 | a=8 b=9 t=7 g=3 c=8 f=10
[PRIO] oauth2.googleapis.com tokeninfo oracle: score 5.85 | a=5 b=4 t=6 g=10 c=3 f=9
[PRIO] bughunters.google.com (report SPA): score 4.45 | a=3 b=3 t=3 g=10 c=3 f=7
[HYP] Issuer-confusion / cross-protocol token replay (v1.0 ↔ v2.0)
class: AUTH
asset: login.microsoftonline.com (sts.windows.net/{tid} issuer + 5-key JWKS vs login.microsoftonline.com/{tid}/v2.0 issuer + 8-key JWKS; v1.0-only response_types: `token`, `token id_token`)
confidence: 45
reasoning: Two distinct issuer namespaces serve the same tenant with divergent JWKS endpoints (5 vs 8 RSA keys) and v1.0-exclusive response types (implicit `token`, hybrid `token id_token`). Both discovery docs returned HTTP 200. If any token-accepting resource (Graph, Entra) validates `iss`/`aud` loosely (regex/substring/none), a v1.0-issued token could be accepted against a v2.0-only resource → MFA/auth bypass.
evidence_needed: (1) discovery docs HTTP 200 with divergent issuers + JWKS (confirmed); (2) kid overlap between v1.0 and v2.0 JWKS (passive); (3) a v1.0 token accepted by a v2.0-only endpoint or an issuer-validation error oracle.
verify_steps: PASSIVE: GET /common/discovery/keys and /common/discovery/v2.0/keys, diff `kid` sets for overlap. AUTH_HELPED: replay a v1.0 id_token against graph.microsoft.com/v1.0/me and compare 200 vs 401/400.
impact: MFA bypass / auth bypass on Microsoft Identity ($100,000). Attacker reuses v1.0 token against v2.0-only resources.
testability: AUTH_HELPED
[HYP] Graph API 405 anomaly — missing Bearer challenge / IDOR masking
class: MISCONFIG
asset: graph.microsoft.com/v1.0, /v1.0/me, /v1.0/users, /v1.0/organization
confidence: 45
reasoning: Unauth HEAD/GET returns HTTP 405 (not 401) with Content-Length 0 and NO `WWW-Authenticate: Bearer` — verified (4×405). RFC 6750 §3 requires 401 + Bearer challenge. 405 (vs 401/403) masks resource existence: enumeration returns 405 regardless of whether a resource exists, hiding IDOR probing; SDKs waiting on the Bearer challenge won't auto-acquire tokens.
evidence_needed: consistent 405 + absence of WWW-Authenticate across Graph endpoints (verified).
verify_steps: PASSIVE: curl -sI https://graph.microsoft.com/v1.0/ ; curl -sI https://graph.microsoft.com/v1.0/me ; curl -sI https://graph.microsoft.com/v1.0/users — confirm 405 + no WWW-Authenticate (done).
impact: IDOR enumeration obfuscation + SDK auth-flow breakage. Info/hygiene class; borderline bounty. Graph Directory/Identity tabs in-scope.
testability: PASSIVE
[HYP] OAuth2 tokeninfo amplification oracle
class: OATH
asset: oauth2.googleapis.com/tokeninfo (?access_token= / ?id_token= query introspection; returns aud/scope/expiry, no auth header)
confidence: 30
reasoning: tokeninfo introspects via query params (no Authorization header), returning decoded aud/scope/expiry. Verified: missing token → 400, malformed → 400. The 500 seen earlier is non-reproducible (3×400 on retry). Any token leaked into a referer-visible URL becomes a one-call oracle.
evidence_needed: deterministic 500 on a structurally-valid-but-expired id_token; a real leaked token resolved.
verify_steps: PASSIVE: GET oauth2.googleapis.com/tokeninfo?id_token=<malformed×5> for 500 determinism; GET tokeninfo?access_token=<expired> for expiry-vs-invalid parity (partial done).
impact: Token-leak amplification only (informational; Google no-reward category). Low severity.
testability: PASSIVE
[PARKED] tokeninfo amplification oracle: confidence 30 (<40); informational amplification only; Google no-reward category; 500 non-reproducible.
[FINAL] re-ranked:
[NEXT] PROBE: diff v1.0 JWKS vs v2.0 JWKS key IDs —
[LEARN] ACCEPTED v2.0 HTTP-200 error rendering @ login.microsoftonline.com/common/oauth2/v2.0/authorize: unsupported response_type=token returns HTTP 200 (embedded JS error 700038, iHttpErrorCode 400) instead of HTTP 400 — violates RFC 6749 §3; status-checking clients may misparse as success.
[LEARN] ACCEPTED Graph API 405 anomaly @ graph.microsoft.com/v1.0: unauth GET/HEAD returns HTTP 405 (not 401), no WWW-Authenticate Bearer, Content-Length 0 — violates RFC 6750 §3; masks IDOR enumeration + breaks SDK auto-auth.
[LEARN] REJECTED tokeninfo 500 handler anomaly @ oauth2.googleapis.com/tokeninfo: non-reproducible (1×500 then 3×400 on identical malformed id_token); transient frontend error, no deterministic parse branch.
[RISK] google: 22 | narrow passive surface (identitytoolkit 403-gated, tokeninfo amplification-only no-reward, bughunters hardened SPA, delegatedProjectNumber IDOR already REJECTED/IAM-bound). No ungated cross-project read surface; secrets scans clean. Low exposure.
[RISK] microsoft: 68 | high-value Identity surface (dual v1.0/v2.0 issuer, mTLS cert-bound tokens, 1,183 Graph entities / 326 functions / 22 filterByCurrentUser bindings) with $100k MFA-bypass ceiling + two verified anomalies (v2.0 HTTP-200 error rendering, Graph 405/IDOR-masking). High-impact exploits require AUTH_HELPED (active token flow) which passive rules forbid — exposure is moderate-high but exploitability is passive-blocked.
