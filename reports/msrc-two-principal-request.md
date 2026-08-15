# MSRC — Request: Authorized two-principal test of Microsoft Graph Copilot agent registration API

To: MSRC (msrc.microsoft.com/bounty — contact/portal)
From: [researcher]
Date: 2026-08-15
Re: Request for authorization to validate a cross-principal ownership hypothesis on
    `graph.microsoft.com/beta/copilot/agentRegistrations*` with two app principals

## 1. What we are asking
Authorization to register **two application principals (App A, App B)** in test
tenants and to perform **PATCH/GET requests as App A against agent-registration
resources owned by App B** (and vice versa) for the sole purpose of proving or
disproving a cross-principal ownership-bypass hypothesis on the Microsoft Graph
Copilot Admin API family.

## 2. Observed preconditions (passive, verified live 2026-08-15)
- `OPTIONS graph.microsoft.com/beta/copilot/agentRegistrations{,/ {id},/admin/catalog/packages,/admin/policySettings}`
  → HTTP 200 with `Access-Control-Allow-Origin: *` and an `Access-Control-Allow-Methods`
  list that includes **PATCH** (re-confirmed at collection + item + agents + admin +
  policySettings levels; collection+item preflight 200 `ACAO:*`).
- `graph.microsoft.com/beta/$metadata` → `oAuth2PermissionGrant` EntityType block
  (458 bytes) with **0 OperationRestrictions** and 7 client-supplied properties —
  i.e., no owner/principal binding expressed in metadata for grant creation.
- `graph.microsoft.com` HEAD `/v1.0/me`, `/beta/copilot/*` → HTTP 405 with no
  `WWW-Authenticate: Bearer` challenge (RFC 6750 §3 deviation).

## 3. Hypothesis under test
**Cross-principal ownership bypass (CWE-284):** if `agentRegistrations` does not
enforce owner==principal on PATCH (ownership check at application level rather than
resource level), a principal that can reach the endpoint can modify/claim
agent-registration resources owned by another principal. The CORS `ACAO:*` + PATCH
allowlist makes the client-side portion of such a bypass browser-exploitable from
any origin.

## 4. Proposed test procedure (minimal footprint, no production data)
1. Register App A and App B (test tenants, no user data, no PII).
2. App A creates `agentRegistration R` (POST), App B attempts:
   - `GET /beta/copilot/agentRegistrations/{R}` (cross-principal read)
   - `PATCH /beta/copilot/agentRegistrations/{R}` (cross-principal modify, benign
     metadata-only change, immediately reverted)
   - `POST /beta/copilot/agentRegistrations` with an `owner` field set to App A's
     principal while authenticated as App B (ownership-spoof attempt)
3. Record status codes; if PATCH succeeds on foreign resources, we have a finding.

## 5. Commitments
- No production tenant data, no user accounts, no DoS, no bulk enumeration.
- All writes limited to the two test registrations; everything reverted after the test.
- Full disclosure of all findings to MSRC before any public discussion.
- We will treat any MSRC authorization as governing this specific test only.

## 6. Why now
This is the single bottleneck: the hypothesis is currently **AUTH_HELPED** — every
further step requires two real principals, which we will not create without MSRC
authorization. A one-line approval (or a pointer to the correct MSRC mechanism for
multi-tenant testing) unblocks it.
