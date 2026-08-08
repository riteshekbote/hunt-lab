
## 2026-08-07 08:57:28 UTC [microsoft] (model longcat)
- [UNVALIDATED] 1. **Google VRP scope**: `*.google.com`, `*.youtube.com`, `*.blogger.com`, `*.deepmind.com`, `*.waymo.com`, `*.wing.com` — rewards up to $101,010 for RCE on Tier-0.
- [UNVALIDATED] 2. **NEW AI VRP** launched Oct 2025 — unified abuse + security reward table for AI-specific issues.
- [UNVALIDATED] 3. **Google ADK (Agent Development Kit)** is a new high-value attack surface — Python + Kotlin SDKs for LLM agents with OAuth/OIDC tool auth.
- [UNVALIDATED] 4. **ADK issue #2128**: `/run_sse` endpoint broadcasts full OAuth `client_secret` + `client_id` to untrusted frontend clients — confirmed credential leakage pattern.
- [UNVALIDATED] 5. **ADK issue #5520**: Hardcoded Google OAuth token (`ya29.*`) found in git history at `tests/unittests/plugins/test_bigquery_agent_analytics_plugin.py`.
- [UNVALIDATED] 6. **ADK commit e3567a6**: Fixed credential leakage in Agent Registry (ADC headers passed to non-Google MCP toolsets).
- [UNVALIDATED] 7. **ADK commit 33012e6**: Fixed cross-user credential leaks from unstable `hash()` key generation.
- [UNVALIDATED] 8. **ADK PR #5154**: Migrating credential storage to `secret:` scope — OAuth tokens currently persist in session backends.
- [UNVALIDATED] 9. **A2A protocol** (Google Agent-to-Agent) — new attack surface: webhook SSRF, unsigned AgentCards, unauthenticated endpoints.
- [UNVALIDATED] 10. **CVE-2026-47391**: PraisonAI official A2A example — unauthenticated + `eval()` tool = RCE.
- [UNVALIDATED] 11. **GCP OAuth redirect_uri URL parsing confusion** (Benchikh, Apr 2025): IPv6 parser discrepancy → account takeover.
- [UNVALIDATED] 12. **Tenable TRA-2025-45**: SSRF in GCP Action Hub DataRobot action → IP allowlist bypass on Looker.
## 2026-08-07 15:49:41 UTC [google] (model longcat)
## 2026-08-07 15:59:22 UTC [google] (model longcat)
## 2026-08-07 16:34:08 UTC [google] (model longcat)
## 2026-08-07 17:30:21 UTC [google] (model longcat)
## 2026-08-07 18:26:44 UTC [google] (model longcat)
## 2026-08-07 18:46:46 UTC [google] (model longcat)
## 2026-08-07 19:15:31 UTC [google] (model longcat)
## 2026-08-07 19:31:18 UTC [google] (model longcat)
## 2026-08-07 20:19:26 UTC [google] (model longcat)
## 2026-08-07 21:04:44 UTC [google] (model longcat)
## 2026-08-07 21:52:42 UTC [google] (model longcat)
## 2026-08-07 22:35:52 UTC [google] (model longcat)
## 2026-08-07 23:16:12 UTC [google] (model longcat)
## 2026-08-07 23:51:26 UTC [google] (model longcat)
## 2026-08-08 01:39:55 UTC [google] (model longcat)
