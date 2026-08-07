# Laguna leads — Microsoft Bounty (RECON→SURFACE)

[L1] Microsoft Identity OAuth2/OIDC endpoints (login.microsoftonline.com) — SURFACE done. OIDC v2.0 discovery fully enumerated; dual v1.0 endpoint (issuer sts.windows.net) also found. redirect_uri NOT validated pre-auth (deferred to post-auth token issuance). Lead → HYPOTHESIS 1 (issuer confusion), 2 (mTLS cert-binding bypass), 3 (hybrid response_type quirks).

[L2] graph.microsoft.com Directory/Governance/Identity+Sign-In API surface — SURFACE done. Graph v1.0 $metadata: 1,183 EntityTypes, 326 Functions across microsoft.graph + microsoft.graph.identityGovernance + microsoft.graph.security namespaces. Service doc: 72 top-level collections. Lead → HYPOTHESIS 4 (Graph IDOR / consent scope escape in identityGovernance or security functions).

[L3] Legacy token-issuance endpoints microsoftazuread-sso.com & accounts.accesscontrol.windows.net — SURFACE partial. microsoftazuread-sso.com unreachable (HTTP 000) from test vantage; accounts.accesscontrol.windows.net not yet probed. v1.0 issuer sts.windows.net/{tenant} confirmed. Lead → HYPOTHESIS 1 (issuer confusion / token replay across protocol versions).

[L4] MSRC Security Update Guide API (/updates, /updates/{key}, /cvrf/{id}, /csaf/{id}) — RECON done. Passive changelog; regression-hunt source. Lead → HYPOTHESIS 5 (surf recently patched vulns for unfixed variants). Not yet surfaced.

[L5] Open-source native-code leads (OS bounty $750–15k): SymCrypt (C crypto impl), msquic (C QUIC parser), mimalloc (C allocator), DirectXShaderCompiler (C++). RECON done. Lead → HYPOTHESIS (memory-safety / parser bugs in msquic & SymCrypt). Not yet surfaced (source review deferred to next slot).

[L6] Explicit OS-bounty GitHub repos to review: azure/azure-sdk(-for-*), microsoft/fluentui, PowerShell/PowerShell, microsoft/typescript, microsoft/vscode, microsoft/agent-framework, microsoft/msquic, microsoft/monaco-editor. (semantic-kernel, autogen excluded.) RECON done. Lead → secrets-scan + source-review (deferred).

[L7] mtlsauth.microsoft.com (in-scope *.windows.net) — POC verified. Active ESTS host; token endpoint returns HTTP 200 (not 405) for GET. Shares backend with login.microsoftonline.com (same x-ms-ests-server version 2.1.24997.11). v2.0 discovery confirms `mtls_endpoint_aliases.token_endpoint=https://mtlsauth.microsoft.com/common/oauth2/v2.0/token` and `tls_client_certificate_bound_access_tokens=true`. Lead → HYPOTHESIS 3 (mTLS cert-binding bypass / cnfl claim validation gap at resource layer).

[L8] microsoft/msquic (GitHub, OS bounty C) — RECON+POC done. 851 src files in src/, integrated with Google OSS-Fuzz CIFuzz (300s fuzz/PR). `src/core/frame.c` reviewed: decode functions use SAL annotations + explicit bounds checks (`BufferLength < sizeof + *Offset`). Structural defensive patterns present — not obviously exploitable via passive review. Lead → next slot: deep-dive packet.c / VarInt decoding hot path.

[L9] SymCrypt source review deferred — API rate-limited (GitHub API returned JSON parse errors). Will retry in next slot.

## 2026-08-07 09:01:01 UTC [microsoft] (model laguna) — PHASE 2 SURFACE
STATUS_PHASE: SURFACE
STATUS_STATE: HIGH_POTENTIAL
NEXT_STEP_1: PHASE 3 HYPOTHESIS — formalize issuer-confusion (sts.windows.net vs login.microsoftonline.com) + mTLS cert-binding bypass + hybrid response_type quirks; design read-only PoCs (GET/HEAD only, no flow completion).
NEXT_STEP_2: PHASE 3 HYPOTHESIS — enumerate Graph identityGovernance/security functions (326 funcs) for IDOR/consent-scope-escape; map $metadata to in-scope tabs.
NEXT_STEP_3: PHASE 2 continued SURFACE — retry unreachable hosts (microsoftazuread-sso.com, api.mysignins.*, provisioningapi.*, accounts.accesscontrol.windows.net) via alternate paths; begin passive source review of msquic + SymCrypt.
