# Google surface inventory (seed 2026-08-07)

## ALIVE / CONFIRMED
- identitytoolkit.googleapis.com v1 discovery: getProjects/getProjectConfig → 403 "Method doesn't allow unregistered callers" (consumer-identity gate)
- firebaseauth v1: delegatedProjectNumber field DEPRECATED, caller-context only (Firebase V1 migration); authz via IAM firebaseauth.users.get on targetProjectId
- GCP $discovery docs: getAccountInfo has no documented IAM requirement (thread for authorized test)
- GCP service inventory: STS, WIP, Binary Authorization, Artifact Registry, Cloud Run, Cloud Functions, Secret Manager, Cloud Build, Storage, IAM — all auth-gated [UNVALIDATED]
- gstatic/githubusercontent codelab assets, API keys in public repos: adk-python /run_sse client_secret (DUP #2128), ya29 token (DUP #5520)

## DEAD / REJECTED
- Firebase unregistered-caller access: 403 gate (REJECTED)
- delegatedProjectNumber IDOR: deprecated + IAM-bound (REJECTED; resurrection = authorized two-project token test only)
- Firebase config reads: no-key 403 (REJECTED)

## NOTES
- Prior art: 2021 Firebase project-number collision class (patched)
- Model sessions have been mislabeled [microsoft] while probing googleapis endpoints — trust endpoint, not label
