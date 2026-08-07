# Microsoft surface inventory (seed 2026-08-07)

## ALIVE / CONFIRMED
- mysignins.microsoft.com SPA API map: clientId 19db86c3-b2b9-44cc-b339-36da233a3be2 (MSA); api.myaccount clientId 8c59ead7-d703-4a27-9e55-c96a0054c8d2; all endpoint probes → 401
- login.live vs AAD redirect asymmetry (known class, deferred validation AADSTS50011)
- ACS legacy (account.microsoft.com/auth/ACS): metadata alive, 4 keys (rotated from 5); endpoints /tokens/OAuth/2, /tokens/delegation, /mgmt/delegation/1
- Entra Verified ID endpoints (DID core, credential contracts, status API) — surface only
- accledger /health: informational only
- AAD authorize (clientId 19db86c3..., any redirect_uri incl. attacker + prompt=none): 200 generic "Redirecting" shell, no Location; validation ONLY at completion (AADSTS50011 exact-match)
- Graph agentSignInSessions endpoint: 401

## DEAD / REJECTED
- AAD deferred redirect validation: known class, not reportable (REJECTED)
- jarvisapi.account.microsoft.com: NXDOMAIN (internal URL leak dead)
- jcmsfd.account.microsoft.com/CPM: 400 MissingOrInvalidHeader (header-gated)
- /health metadata disclosures: informational, no-reward (REJECTED)
- Endpoint-map-only output: not a finding (REJECTED)
- Implicit-flow / email-enum surfaces: no-reward (REJECTED)

## NOTES
- ACS EOL; ESTS 2.1.24997.11 observed
- 401 endpoint maps: discovery only, need AUTH_HELPED/HUMAN tests to progress
