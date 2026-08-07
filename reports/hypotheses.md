
## RANKED HYPOTHESES 2026-08-07 15:49:41 UTC

## RANKED HYPOTHESES 2026-08-07 16:11:28 UTC
- [75] `graph.microsoft.com/beta/copilot/agentRegistrations`: Agent Registration ownership boundary bypass via client-controlled `createdBy` + PATCH rewrite (from reports/hypotheses-nemotron3.txt)
- [55] POST: D2E conversation-ID resumption / cross-app hijack (from reports/hypotheses-bigpickle.txt)
- [45] login.microsoftonline.com: Issuer-confusion / cross-protocol token replay (v1.0 ↔ v2.0) (from reports/hypotheses-laguna.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET `https://graph.microsoft.com/beta/copilot/agentRegistrations` (with valid test-tenant Bearer token) — tests enumeration of registrations (doc says no
- NEXT(hypotheses-bigpickle.txt): RAG: In microsoft/Agents + @microsoft/agents-copilotstudio-client (both in-scope orgs), grep for CallerIdentityMismatch / CallerIdentityTypeMismatch / D2EAccess
- NEXT(hypotheses-laguna.txt): PROBE: diff v1.0 JWKS vs v2.0 JWKS key IDs —
- LEARN: ACCEPTED v2.0 HTTP-200 error rendering @ login.microsoftonline.com/common/oauth2/v2.0/authorize: unsupported response_type=token returns HTTP 200 (embedded JS e
- LEARN: ACCEPTED Graph API 405 anomaly @ graph.microsoft.com/v1.0: unauth GET/HEAD returns HTTP 405 (not 401), no WWW-Authenticate Bearer, Content-Length 0 — violates R
- LEARN: REJECTED tokeninfo 500 handler anomaly @ oauth2.googleapis.com/tokeninfo: non-reproducible (1×500 then 3×400 on identical malformed id_token); transient fronten
