
## 2026-08-07 08:55:53 UTC [microsoft] (model bigpickle)
- [UNVALIDATED] Program landscape**: MSRC Cloud programs cap at $100k (Identity), $60k (Azure), $30k (Copilot), $20k each (Azure DevOps / Dynamics+Power Platform / Defender); Open Source program caps at $15k for a fixed repo list; report portal `msrc.microsoft.com/report/vulnerability/new` (aka.ms/secure-at), Correlation ID required. Sources: https://www.microsoft.com/en-us/msrc/bounty-programs, https://www.microsoft.com/en-us/msrc/pentest-rules-of-engagement
- [UNVALIDATED] Identity bounty added 5 new hosts in Jul 2025 and account.microsoft.com in Nov 2025** — freshly-expanded scope is the best-hit-rate surface this run: `mysignins.microsoft.com`, `api.mysignins.microsoft.com`, `myaccount.microsoft.com`, `myaccess.microsoft.com`, `myapps.microsoft.com`, `microsoftazuread-sso.com`, `accounts.accesscontrol.windows.net`. Source: https://microsoft.com/msrc/bounty-microsoft-identity (Revision History).
- [UNVALIDATED] Identity out-of-scope traps**: subdomain takeover, pure URL redirects (unless chained), missing headers, cookie replay, DoS, MFA-bypass needing physical access — these will be triaged out, do not report.
- [UNVALIDATED] Rules of Engagement nuance**: retrieving/using credentials "regardless of how obtained, including leaked publicly" is prohibited — leaked-secret checks must stop at hashing + flagging for triage, never use.
- [UNVALIDATED] DNS**: in-scope hosts terminate on the AAD gateway cluster: `mysignins`/`api.mysignins` -> `prdf.aadg.msidentity.com`; `accounts.accesscontrol.windows.net` -> `prda.aadg.msidentity.com`; `adminwebservice.microsoftonline.com` -> `adminwebservice.mso.msidentity.com` (aadg). `microsoftazuread-sso.com` apex has NO A record (host-keyed domain). `provisioningapi.microsoftonline.com` had no A record at lookup time.
- [UNVALIDATED] OIDC v2.0 confirmed**: issuer templated `https://login.microsoftonline.com/{tenantid}/v2.0`, endpoints under `/common/oauth2/v2.0/` (authorize/token/devicecode/logout), `subject_types_supported: pairwise`, response types include `code id_token`. Metadata: https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
- [UNVALIDATED] crt.sh was down (404/502)** this run — CT-based inventory deferred, do not re-crawl aggressively.
- [UNVALIDATED] STATUS_PHASE: [RECON|SURFACE|HYPOTHESIS|POC]
- [UNVALIDATED] STATUS_STATE: [IN_PROGRESS|EXHAUSTED|HIGH_POTENTIAL]
- [UNVALIDATED] NEXT_STEP_1: ...
- [UNVALIDATED] NEXT_STEP_2: ...
- [UNVALIDATED] NEXT_STEP_3: ...
