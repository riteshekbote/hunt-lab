# AGICAP KB (2026-08-16 session)

## Surface map (from public /assets/config.json on app.agicap.com — full microservice map, 60+ hosts)
Core API: app.agicap.com/api/* (ASP.NET Core, JSON ProblemDetails + traceId, JWT bearer). 31 endpoints mapped (cashflow, forecasting v1-v3, import, budget, customkpi, defaultkpi, entreprise, accessrights, bff-categories).
Identity: myaccount.agicap.com (IdentityServer4; issuer LEAK "http://my-account-web"), client_id agicap_spa_client (public client, PKCE).
MCP: mcp.agicap.com — live MCP gateway (401 {"message":"Missing or malformed auth token"}; bearer JWT parsed, server-side validation — forged alg:none/HS256-empty tokens all → "inactive auth token"; introspects server-side, NOT forgeable).
Feature flags: delorean.agicap.com/v2/feature-flags (public, no auth, full flag JSON — by design, frontend needs pre-auth).
Dev portal: api.agicap.com (AWS dev-portal SPA; "Developer Portal", usage-plans pattern).
Other API hosts (all in-scope *.agicap.com): payments, invoices-management, suppliers, pre-accounting, contract-management, expense-claims, spend-back, budget-controls, settings, subscription, custom-dashboards, exchangerates, invoice-financing, financial-investments, internal-financing, preaccounting, short-term-cash-management, cashflow-reconciliation, forecasting, cash-flow-planning, debt-management, financial-position, risk-management, account-manager-service, categorization-helper, ai-assistant, collect-api, reconciliation, permissions(NX), rebacca, refinement, pnl, business-definition, transformationmatrix, aggregator, di-business-core, di-business-file-import, di-business-einvoicing, di-banking-audit, di-banking-export, di-banking-import, di-banking-host-to-host, di-banking-fees, on-premise-connectors, expected-account-affectation-rules, prod-maintenance-bucket, openapi(404 root), telemetry, my-swan-account, cards-receipts-import, client, translations.
Cloud: LAB_API_HOST ai-dashboard-generator-378002083076.europe-west1.run.app (GCP Cloud Run, alive, all probed paths 404). Banking EDI: prod-di-banking-edi.agicap.cloud (prod). Umami: prod-umami.agicap.cloud. MFE: mfe.agicap.com (path-based MFE shells).

## Auth (IdentityServer4 at myaccount.agicap.com)
- .well-known/openid-configuration PUBLIC. issuer=http://my-account-web (INTERNAL HOSTNAME LEAK in discovery + JWTs; info-disclosure only, rejected class per policy).
- Grants advertised: authorization_code, client_credentials, refresh_token, implicit, password, device_code, CIBA, token-exchange. code_challenge_methods: plain + S256 (plain allowed).
- scopes_supported = FULL INTERNAL CATALOG (~200 scopes): mcp, mcp-gateway:user, mcp:read/write, mailer:send_email, user-account:create, myaccount:database, myaccount:platform, maintenance:write, webhooks:endpoints:secret, bus-messages:replay, di-business-mitm:read, aggregator:crashtest, di-business-agicapd:list/write:machines|proxies|servers, security:file_scan, crons:check, outbox-messages:process, two-factor-enforcement:write, sharepoint:write, sftp-api:user, file-import:user, org/user mgmt scopes, per-module access_management:* etc.
- Token endpoint: agicap_spa_client REJECTS client_credentials (unauthorized_client), REJECTS password grant (unauthorized_client), refresh_token processes without secret (invalid_grant on bad token). Public client locked down properly.
- Internal names leak: my-account-web, mcp gateway, di-business-mitm (MITM), agicapd machines/proxies/servers (on-prem connector infra), crons, outbox, bus-messages (event bus).

## Findings triage (all NON-reportable, reasons)
1. public S3/GCS buckets: prod-maintenance-bucket.agicap.com (common-maintenance-prod) + translations.agicap.com (agc-translations) — anonymous ListBucket via S3 API. Content = maintenance product JSON + i18n JSON. BY DESIGN (both must load pre-auth client-side). NOT findings.
2. OIDC issuer internal hostname http://my-account-web — info disclosure, no PoC chain → rejected class ("non-exploitable").
3. identity.agicap.com — Cloudflare 522 dead origin, CF-proxied, not claimable → DNS class, rejected.
4. delorean feature flags public — by design.
5. mcp.agicap.com JWT forge attempts (alg:none, HS256 empty key, claim variants) — all rejected server-side; introspection properly wired. NO vuln.
6. LAB Cloud Run — alive but no exposed paths; deferred.

## Next (needs account — HUMAN step, allowed by policy, no data mod)
- Register own account at app.agicap.com → auth'd testing: IDOR on 31 core endpoints (entity/workspace switching), MCP gateway tenant isolation (mcp:read/write on other tenants?), feature-flag read intel, short-term-cash-management BFF, forecasting v3, payments/beneficiaries (business logic), access rights endpoints (GrantAccessToCompany!), public-api (developer portal API keys), webhooks endpoints (secret read scope).
- NOTE: no_data_modification=true — NO PUT/POST/DELETE mutations during auth'd tests unless it's a benign create-own-object.
