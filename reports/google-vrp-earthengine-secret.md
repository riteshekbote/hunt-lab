# Google VRP — Hardcoded live OAuth client_secret in google/earthengine-api (python/ee/oauth.py:45)

## Summary
Google's own public repository `google/earthengine-api` ships a hardcoded, **live**
OAuth client_secret in `python/ee/oauth.py:45` (currently in `main` HEAD, file size
23,110 B, blob sha `97aa66f0…`, last modified by commit 22970614 "python: Fix
pyrefly checks", 2026-08-07). The secret authenticates successfully against
`oauth2.googleapis.com/token` (RFC 6749 §5.2 A/B oracle) as of 2026-08-15.

## Affected artifact
- Repo: https://github.com/google/earthengine-api
- File: `python/ee/oauth.py`, line 45
- `CLIENT_ID = '517222506229-vsmmajv00ul0bs7p89v5m89qs8eb9359.apps.googleusercontent.com'`
- `CLIENT_SECRET = 'RUP0RZ6e0pPhDzsqIJ7KlNd1'`
- Used at lines 98-99, 192-193, 413-415 (authorization flow default credentials),
  and written to `{credentials}-client-id.json` on disk (line 414-421).

## Impact
Anyone cloning the repository can use these credentials as the OAuth client for
Earth Engine API flows (installed-app OAuth). The client authenticates
(`invalid_grant` on a dummy refresh token vs `invalid_client` for a fake secret —
proving the secret is accepted by Google's token endpoint today, 2026-08-15
revoke-check). A live client_secret in a public repo:
- defeats the confidentiality of OAuth client registration (impersonation of the
  Earth Engine client),
- enables unauthorized token issuance if the client has consent/refresh paths
  (scope access then bounded by user consent, but the client identity is
  forgeable),
- is a token-hygiene defect: secrets in source should be rotated and moved to
  per-install client-ID files (the code already supports `--client-id-file`).

## Proof (A/B, 2026-08-15 06:4x UTC)
```
POST https://oauth2.googleapis.com/token
  client_id=517222506229-vsmmajv00ul0bs7p89v5m89qs8eb9359.apps.googleusercontent.com
  client_secret=<REAL from oauth.py:45>
  grant_type=refresh_token
  refresh_token=1//dummy-revoked
→ 400 {"error":"invalid_grant","error_description":"Bad Request"}
```
vs control with a fake secret → `401 {"error":"invalid_client",
"error_description":"The provided client secret is invalid."}`
Per RFC 6749 §5.2, `invalid_grant` (client authenticated, grant rejected) vs
`invalid_client` (client rejected) = the REAL secret is currently valid.

## Suggested fix
- Rotate/revoke this client and remove `CLIENT_SECRET` from the repo; default to
  the existing `--client-id-file` flow (no bundled secret).
- Add a secret-scan gate to earthengine-api CI.

## Disclosure notes
- Reported via Google VRP (bughunters.google.com) per the Earth Engine / open-source
  disclosure policy. No exploit was performed beyond the single token-endpoint
  A/B request above (no token obtained, no data accessed).
- The prior similar items on google/adk (issuetracker 504158909, /run_sse) were
  already public before this report; this earthengine-api secret is a separate
  artifact (verified present in main HEAD on the report date).
