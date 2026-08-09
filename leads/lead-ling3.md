## 2026-08-07 15:59:11 UTC [google] (model ling3)
## 2026-08-07 16:34:12 UTC [google] (model ling3)
## 2026-08-07 17:30:12 UTC [google] (model ling3)
## 2026-08-07 18:26:33 UTC [google] (model ling3)
## 2026-08-07 18:46:40 UTC [google] (model ling3)
## 2026-08-07 19:15:31 UTC [google] (model ling3)
## 2026-08-07 19:31:15 UTC [google] (model ling3)
## 2026-08-07 20:19:25 UTC [google] (model ling3)
## 2026-08-07 21:04:45 UTC [google] (model ling3)
## 2026-08-07 21:52:25 UTC [google] (model ling3)
## 2026-08-07 22:35:49 UTC [google] (model ling3)
## 2026-08-07 23:16:04 UTC [google] (model ling3)
## 2026-08-07 23:51:31 UTC [google] (model ling3)
## 2026-08-08 01:39:58 UTC [google] (model ling3)
## 2026-08-08 03:13:21 UTC [google] (model ling3)
## 2026-08-08 04:31:31 UTC [google] (model ling3)
## 2026-08-08 05:23:13 UTC [google] (model ling3)
## 2026-08-08 06:06:09 UTC [google] (model ling3)
## 2026-08-08 07:11:47 UTC [google] (model ling3)
## 2026-08-08 08:03:12 UTC [google] (model ling3)
## 2026-08-08 08:56:30 UTC [google] (model ling3)
## 2026-08-08 09:41:17 UTC [google] (model ling3)
## 2026-08-08 10:15:35 UTC [google] (model ling3)
## 2026-08-08 10:54:39 UTC [google] (model ling3)
## 2026-08-08 11:29:02 UTC [google] (model ling3)
## 2026-08-08 11:57:01 UTC [google] (model ling3)
## 2026-08-08 12:57:44 UTC [google] (model ling3)
## 2026-08-08 13:50:37 UTC [google] (model ling3)
## 2026-08-08 14:32:04 UTC [google] (model ling3)
## 2026-08-08 15:01:44 UTC [google] (model ling3)
## 2026-08-08 15:42:54 UTC [google] (model ling3)
## 2026-08-08 16:37:31 UTC [google] (model ling3)
## 2026-08-08 17:02:04 UTC [google] (model ling3)
## 2026-08-08 17:41:17 UTC [google] (model ling3)
## 2026-08-08 18:04:45 UTC [google] (model ling3)
## 2026-08-08 18:54:42 UTC [google] (model ling3)
## 2026-08-08 19:29:21 UTC [google] (model ling3)
## 2026-08-08 19:57:03 UTC [google] (model ling3)
## 2026-08-08 20:37:06 UTC [google] (model ling3)
## 2026-08-08 21:04:42 UTC [google] (model ling3)
## 2026-08-08 21:42:58 UTC [google] (model ling3)
## 2026-08-08 22:07:11 UTC [google] (model ling3)
## 2026-08-08 22:45:12 UTC [google] (model ling3)
## 2026-08-08 23:12:33 UTC [google] (model ling3)
## 2026-08-08 23:47:16 UTC [google] (model ling3)
## 2026-08-09 00:37:14 UTC [google] (model ling3)
## 2026-08-09 02:50:34 UTC [google] (model ling3)
## 2026-08-09 04:09:07 UTC [google] (model ling3)
## 2026-08-09 05:18:36 UTC [google] (model ling3)
[PRIO] github.com/google/earthengine-api/python/ee/oauth.py:45, Hardcoded OAuth client_secret, 8.0
[PRIO] graph.microsoft.com/beta/copilot/agentRegistrations, Agent Registration ownership boundary bypass, 6.65
[PRIO] graph.microsoft.com/v1.0, Graph API 405 anomaly, 6.6
[HYP] Hardcoded OAuth client_secret in Google native-app SDK source
class: AUTH
asset: github.com/google/earthengine-api/python/ee/oauth.py:45
confidence: 90
reasoning: The hardcoded OAuth client_secret 'RUP0RZ6e0pPhDzsqIJ7KlNd1' is used as a default fallback at oauth.py:99, scopes include cloud-platform+drive+devstorage.full_control, OOB redirect URI deprecated. The secret is plaintext-readable from raw GitHub and sha256 `3f3f8d6f29db1b06cbfc212a718c181744db8f9bd25316c76ccebf8a1440d271` matches the KB.
evidence_needed: Verify the secret is used as default fallback at oauth.py:99 and check if any authorized client_credentials exchange uses this secret
verify_steps: PASSIVE: curl https://raw.githubusercontent.com/google/earthengine-api/master/python/ee/oauth.py to confirm plaintext secret and sha256 match KB
impact: Full GCP cloud-platform access for any authenticated entity using the hardcoded secret
testability: PASSIVE
[HYP] Agent Registration ownership boundary bypass via client-supplied createdBy + cross-principal PATCH
class: IDOR
asset: graph.microsoft.com/beta/copilot/agentRegistrations
confidence: 85
reasoning: The agentRegistration EntityType has zero OperationRestrictions/ReadRestrictions/UpdateRestrictions/InsertRestrictions/DeleteRestrictions, with client-supplied createdBy/ownerIds/agentCard/managedByAppId/agentIdentityId all Nullable=false. A cross-principal PATCH could bypass the ownership boundary.
evidence_needed: POST with two different createdBy principals to test IDOR
verify_steps: AUTH_HELPED: POST https://graph.microsoft.com/beta/copilot/agentRegistrations with two different createdBy principals to test IDOR
impact: Cross-principal agent registration manipulation
testability: AUTH_HELPED
[HYP] Graph API 405 anomaly (RFC 6750 §3 violation)
class: SSRF
asset: graph.microsoft.com/v1.0
confidence: 80
reasoning: Unauth GET/HEAD to /v1.0, /me, /users returns HTTP 405 (Content-Length: 0, NO WWW-Authenticate Bearer) instead of HTTP 401 with proper Bearer challenge. This violates RFC 6750 §3 and masks IDOR enumeration.
evidence_needed: Passive probes of /v1.0, /me, /users with unauthenticated HEAD and GET requests
verify_steps: PASSIVE: HEAD/GET /v1.0, /me, /users and verify HTTP 405 with no WWW-Authenticate Bearer
impact: IDOR enumeration + SDK auth breakage
testability: PASSIVE
[PARKED] Hardcoded OAuth client_secret at earthengine-api oauth.py:45: confidence is 90 but this is already a confirmed LIVE finding in the knowledge base — no new verification needed, it's already proven.
[PARKED] Graph API 405 anomaly at graph.microsoft.com/v1.0: confidence is 80 but this is already an ACCEPTED finding in the knowledge base — no new verification needed, it's already proven live.
[NEXT] AUTH_HELPED: POST https://graph.microsoft.com/beta/copilot/agentRegistrations with a cross-principal createdBy (two different UUIDs) to test whether the IDOR boundary is bypassed.
## 2026-08-09 06:04:07 UTC [google] (model ling3)
## 2026-08-09 07:11:54 UTC [google] (model ling3)
## 2026-08-09 08:03:13 UTC [google] (model ling3)
## 2026-08-09 08:57:20 UTC [google] (model ling3)
## 2026-08-09 09:47:32 UTC [google] (model ling3)
## 2026-08-09 10:18:26 UTC [google] (model ling3)
## 2026-08-09 10:57:25 UTC [google] (model ling3)
## 2026-08-09 11:36:20 UTC [google] (model ling3)
## 2026-08-09 12:01:24 UTC [google] (model ling3)
## 2026-08-09 13:10:41 UTC [google] (model ling3)
## 2026-08-09 13:59:26 UTC [google] (model ling3)
## 2026-08-09 14:44:22 UTC [google] (model ling3)
## 2026-08-09 15:15:09 UTC [google] (model ling3)
## 2026-08-09 15:51:26 UTC [google] (model ling3)
## 2026-08-09 16:22:44 UTC [google] (model ling3)
## 2026-08-09 17:03:52 UTC [google] (model ling3)
## 2026-08-09 17:44:05 UTC [google] (model ling3)
## 2026-08-09 18:16:32 UTC [google] (model ling3)
## 2026-08-09 19:06:58 UTC [google] (model ling3)
## 2026-08-09 19:47:15 UTC [google] (model ling3)
## 2026-08-09 20:14:51 UTC [google] (model ling3)
