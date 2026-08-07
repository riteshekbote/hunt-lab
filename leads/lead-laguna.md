# Laguna leads — Microsoft Bounty (RECON phase)

[L1] Microsoft Identity OAuth2/OIDC endpoints (login.microsoftonline.com) — need SURFACE of OIDC discovery + redirect_uri validation. High impact (MFA bypass up to $100k). RECON notes: in-scope per msrc/bounty-microsoft-identity.

[L2] graph.microsoft.com Directory/Governance/Identity+Sign-In API surface — permission/consent model under Global Admin; IDOR / cross-tenant data flow risk. RECON notes: in-scope; Graph v1.0.

[L3] Legacy token-issuance endpoints microsoftazuread-sso.com & accounts.accesscontrol.windows.net — possible token replay / issuer-confusion / redirect_uri quirks. RECON notes: in-scope.

[L4] MSRC Security Update Guide API (/updates, /updates/{key}, /cvrf/{id}, /csaf/{id}) — passive changelog to surface recently-patched vulns (regression hunting). RECON notes: public OData API, GET-safe.

[L5] Open-source native-code leads (OS bounty $750–15k): SymCrypt (C crypto impl), msquic (C QUIC parser), mimalloc (C allocator), DirectXShaderCompiler (C++). RECON notes: in-scope per msrc/opensourcebountyprogram.

[L6] Explicit OS-bounty GitHub repos to review for source code issues: azure/azure-sdk(-for-*), microsoft/fluentui, PowerShell/PowerShell, microsoft/typescript, microsoft/vscode, microsoft/agent-framework, microsoft/monaco-editor, microsoft/msquic. (semantic-kernel, autogen excluded.)

## 2026-08-07 09:01:01 UTC [microsoft] (model laguna)
- [UNVALIDATED] STATUS_PHASE: [RECON|SURFACE|HYPOTHESIS|POC]
- [UNVALIDATED] STATUS_STATE: [IN_PROGRESS|EXHAUSTED|HIGH_POTENTIAL]
- [UNVALIDATED] NEXT_STEP_1: ...
- [UNVALIDATED] NEXT_STEP_2: ...
- [UNVALIDATED] NEXT_STEP_3: ...
- [UNVALIDATED] ===== RESPONSE RULES =====
- [UNVALIDATED] ALWAYS: be specific, professional, cite sources (URLs).
- [UNVALIDATED] NEVER: claim you tested something live, invent vulnerabilities, run
- [UNVALIDATED] attacks, include raw secrets, reference out-of-scope hosts.
- [UNVALIDATED] Begin research now.
- [UNVALIDATED] PROMPT_START
- [UNVALIDATED] name: Run Model
