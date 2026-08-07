
## RUN 2026-08-07 (big-pickle) — TARGET: microsoft — PHASE: RECON

### VDP / Disclosure notes
- Report via https://msrc.microsoft.com/report/vulnerability/new (portal: aka.ms/secure-at). Include Correlation ID per Identity program.
- Cloud programs: Identity up to $100k, Azure $60k, Copilot $30k, Azure DevOps $20k, Dynamics/PowerPlatform $20k, Defender $20k, M365 $19.5k, OpenSource $15k, .NET $15k.
- Identity bounty in-scope hosts (additions Jul 2025 + Nov 2025 are freshest):
  mysignins.microsoft.com, api.mysignins.microsoft.com, myaccount.microsoft.com, myaccess.microsoft.com, myapps.microsoft.com, microsoftazuread-sso.com, accounts.accesscontrol.windows.net, account.microsoft.com, provisioningapi.microsoftonline.com, adminwebservice.microsoftonline.com, login.windows.net, login.microsoftonline.com, login.live.com, account.live.com, signup.live.com, passwordreset.microsoftonline.com, account.windowsazure.com, credential.activedirectory.windowsazure.com + Azure AD B2C.
- Out-of-scope for Identity: subdomain takeover, pure URL redirect, missing headers, cookie replay, DoS, MFA bypass requiring physical device access, 3rd-party integration code.
- Open Source bounty (up to $15k): azure/azure-sdk* (8 repos), fluentui, PowerShell, typescript, vscode, monaco-editor, msquic, agent-framework. semantic-kernel & autogen EXCLUDED. GitHub Actions vulns $750-5k.
- Rules of Engagement: no DoS, no network-intense fuzzing, no accessing secrets even if publicly leaked (flag & report only), must not access customer data.

### Passive DNS (hackertarget hostsearch, ~50/microsoft.com + azure.com + bing.com)
- Notable: 3d-avatar-diffusion.microsoft.com (AI), 3papiprovider.microsoft.com, 3psecureppe.microsoft.com + api.3psecureppe.microsoft.com (PPE), publisher-aircapi.1pp.microsoft.com, 1esbot.microsoft.com / 1esbot-test.microsoft.com, studio.1s.microsoft.com, account-staging.azure.com (staging!), api.dev.accessmonitor.azure.com / api.prod.accessmonitor.azure.com, controlplane.accledger.azure.com + identity.accledger.azure.com, dxp.aad.azure.com + dxp-dev / dxp-ppe / tip.dxp (AAD gateway), support-ppe.iam.ad.azure.com, chat.aedeptooling-int.azure.com + mcp.aedeptooling-int.azure.com (int env), *.ms.a.microsoft.com (cache hosts), bing.com GPU gaming hosts (xCloud).
- DNS resolution checks (dig, passive):
  - accounts.accesscontrol.windows.net -> a.privatelink.msidentity.com -> prda.aadg.msidentity.com (AAD gateway)
  - mysignins.microsoft.com / api.mysignins.microsoft.com -> na.privatelink.msidentity.com -> prdf.aadg.msidentity.com
  - adminwebservice.microsoftonline.com -> adminwebservice.mso.msidentity.com -> prdv6e.aadg.msidentity.com
  - microsoftazuread-sso.com: apex has NO A/AAAA (interesting - host-only domain)
  - provisioningapi.microsoftonline.com: no A record returned at lookup time
- crt.sh returned 404/502 on this run (flaky); retry in a later run for CT-based inventory.

### OIDC surface (v2.0, login.microsoftonline.com/common/v2.0/.well-known/openid-configuration)
- issuer: https://login.microsoftonline.com/{tenantid}/v2.0
- authorize/token/devicecode/logout endpoints under /common/oauth2/v2.0/
- response_types: code, id_token, code id_token, id_token token
- subject_types_supported: pairwise
- jwks: /common/discovery/v2.0/keys

### GitHub org (microsoft, 100 by updated + 100 by stars)
- Recently active / new repos worth later look: amplifier-bundle-* (android-tester, computer-use, redaction, simulated-user-research), universal-artifact-sdk, upgrade-agent-actions, skill-recorder, Echoverse, CopilotScope, CoworkCost.
- security-sorted (stars): microsoft/Security-101 (6773★), microsoft/shell-intune-samples (1080★).
- No secrets flagged in reposcan (0 grep hits as of 08:40 UTC) — separate reposcan job covers this.

### Next phase direction (SURFACE)
- Map endpoints/auth on the Jul 2025 added identity hosts (mysignins, microsoftazuread-sso.com, myaccount, myaccess, myapps, accounts.accesscontrol.windows.net).
- Graph API identity workflow endpoints (v1.0 Directory Management / Governance / Identity & SignIn tabs).
- OAuth redirect_uri / response_mode surface on login.microsoftonline.com, login.live.com.
- Re-run crt.sh for CT inventory.

===== RUN 2026-08-07 08:55:53 UTC | model bigpickle | target microsoft =====
[0m
> build · big-pickle
[0m
[0m$ [0mls -la
total 64
drwxr-xr-x 9 runner runner 4096 Aug  7 08:52 .
drwxr-xr-x 3 runner runner 4096 Aug  7 08:52 ..
drwxr-xr-x 7 runner runner 4096 Aug  7 08:52 .git
drwxr-xr-x 3 runner runner 4096 Aug  7 08:52 .github
-rw-r--r-- 1 runner runner 1022 Aug  7 08:52 README.md
-rw-r--r-- 1 runner runner   15 Aug  7 08:52 ctx-leads.txt
-rw-r--r-- 1 runner runner   18 Aug  7 08:52 ctx-research.txt
-rw-r--r-- 1 runner runner   28 Aug  7 08:52 ctx-valid.txt
drwxr-xr-x 2 runner runner 4096 Aug  7 08:52 leads
-rw-r--r-- 1 runner runner   32 Aug  7 08:52 raw_bigpickle.txt
drwxr-xr-x 2 runner runner 4096 Aug  7 08:52 reports
drwxr-xr-x 2 runner runner 4096 Aug  7 08:52 reposcan-raw
drwxr-xr-x 2 runner runner 4096 Aug  7 08:52 research
-rw-r--r-- 1 runner runner 2858 Aug  7 08:52 research-prompt.txt
-rw-r--r-- 1 runner runner 1650 Aug  7 08:52 scope.yml
drwxr-xr-x 2 runner runner 4096 Aug  7 08:52 triage
[0m
[0m✱ [0mGlob "**/scope.yml"[90m 1 match[0m
[0m→ [0mRead scope.yml
[0m→ [0mRead README.md
[0m→ [0mRead research-prompt.txt
[0m→ [0mRead raw_bigpickle.txt
[0m→ [0mRead ctx-research.txt
[0m→ [0mRead ctx-leads.txt
[0m→ [0mRead ctx-valid.txt
[0m→ [0mRead reports
[0m→ [0mRead research
[0m→ [0mRead research/bigpickle.md
[0m→ [0mRead .github
[0m✱ [0mGlob ".github/workflows/*"[90m 0 matches[0m
[0m
[0m$ [0mls -la .github/workflows/ && cat .github/workflows/*.yml 2>/dev/null | head -100
total 36
drwxr-xr-x 2 runner runner  4096 Aug  7 08:52 .
drwxr-xr-x 3 runner runner  4096 Aug  7 08:52 ..
-rw-r--r-- 1 runner runner 10061 Aug  7 08:52 hunt.yml
-rw-r--r-- 1 runner runner  6859 Aug  7 08:52 reposcan.yml
-rw-r--r-- 1 runner runner  5037 Aug  7 08:52 triage.yml
name: Mega Hunt - 5 Models every 10 min

on:
  schedule:
    - cron: "*/10 * * * *"
  workflow_dispatch:

permissions:
  contents: write

jobs:
  hunter:
    runs-on: ubuntu-latest
    timeout-minutes: 25
    concurrency:
      group: hunt-${{ matrix.model.name }}
      cancel-in-progress: true
    strategy:
      fail-fast: false
      matrix:
        model:
          - { id: opencode/big-pickle, name: bigpickle }
          - { id: opencode/nemotron-3-ultra-free, name: nemotron3 }
          - { id: opencode/longcat-2.0-free, name: longcat }
          - { id: opencode/ling-3.0-flash-free, name: ling3 }
          - { id: opencode/laguna-s-2.1-free, name: laguna }

    env:
      MODEL_ID: ${{ matrix.model.id }}
      MODEL_NAME: ${{ matrix.model.name }}

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Install OpenCode
        run: |
          curl -fsSL https://opencode.ai/install | bash
          export PATH="$HOME/.opencode/bin:$HOME/.local/bin:$PATH"
          opencode --version

      - name: Decide Target (round-robin google/microsoft)
        run: |
          SLOT=$(( $(date +%s) / 600 % 2 ))
          if [ "$SLOT" = "0" ]; then
            echo "TARGET=google" >> $GITHUB_ENV
          else
            echo "TARGET=microsoft" >> $GITHUB_ENV
          fi
          echo "This run hunts: $TARGET"

      - name: Load State
        run: |
          STATE_FILE="state_${MODEL_NAME}.json"
          if [ -f "$STATE_FILE" ] && grep -q '"phase"' "$STATE_FILE" 2>/dev/null; then
            python3 << 'PYTHON_EOF'
          import json, os
          with open(f'state_{os.environ["MODEL_NAME"]}.json') as f:
              st = json.load(f)
          with open(os.environ['GITHUB_ENV'], 'a') as env:
              env.write(f"CURRENT_PHASE={st.get('phase','RECON')}\n")
              env.write(f"CURRENT_STATUS={st.get('status','IN_PROGRESS')}\n")
          PYTHON_EOF
          else
            echo "CURRENT_PHASE=RECON" >> $GITHUB_ENV
            echo "CURRENT_STATUS=FRESH" >> $GITHUB_ENV
          fi

      - name: Build Context (truncated, ARG_MAX-safe)
        run: |
          mkdir -p research leads triage reports
          touch research/${MODEL_NAME}.md leads/lead-${MODEL_NAME}.md
          if [ -s research/${MODEL_NAME}.md ]; then
            tail -n 1500 research/${MODEL_NAME}.md | awk '{ print (length($0) > 3000 ? substr($0,1,3000) "...[TRUNC]" : $0) }' | tail -c 90000 > ctx-research.txt
          else
            echo "# no research yet" > ctx-research.txt
          fi
          if [ -s leads/lead-${MODEL_NAME}.md ]; then
            tail -n 400 leads/lead-${MODEL_NAME}.md | tail -c 40000 > ctx-leads.txt
          else
            echo "# no leads yet" > ctx-leads.txt
          fi
          # Triage verdicts, if any (validated only)
          if ls triage/*VALID* >/dev/null 2>&1; then
            tail -c 30000 triage/*VALID* > ctx-valid.txt
          else
            echo "# no validated findings yet" > ctx-valid.txt
          fi
          # Running validated count
          if [ -f reports/valid-bugs.md ]; then grep -c '^| V' reports/valid-bugs.md || true; fi

      - name: Create Research Prompt
        run: |
          cat > research-prompt.txt << 'PROMPT_START'
          You are a security research analyst for bug bounty programs.
          Role: deep research, hypothesis generation, read-only PoC design.
          NOT: live exploitation, scanning, or invasive testing.
          TARGET PROGRAM THIS RUN: ${TARGET} (see scope.yml for exact domain list)
          RUNNING MODEL: ${MODEL_ID}
          PHASE: ${CURRENT_PHASE} (RECON -> SURFACE -> HYPOTHESIS -> POC, one step deeper per run)
[0m
[0m$ [0mcat .github/workflows/hunt.yml | sed -n '100,200p'
PHASE: ${CURRENT_PHASE} (RECON -> SURFACE -> HYPOTHESIS -> POC, one step deeper per run)

          ===== PROGRAM RULES (MANDATORY) =====
          1. PASSIVE ONLY. GET/HEAD requests, max 1 per second. No scanners, no
             fuzzers, no brute force, no account creation, no writes, no state changes.
          2. Attack surface = the ${TARGET} program domains from scope.yml ONLY.
             Never touch third-party hosts (CDNs, SaaS, analytics) beyond reading
             the program's own disclosure policy page.
          3. Public source code review IS allowed: google/ and microsoft/ GitHub
             org repos are in scope as code. Never commit raw secrets - write
             sha256 hashes instead and flag for triage.
          4. No pre-disclosure. Findings get reported via program channels only.

          ===== PREVIOUS CONTEXT =====
          PROMPT_START
          echo "MY RESEARCH JOURNAL:" >> research-prompt.txt
          cat ctx-research.txt >> research-prompt.txt
          echo "" >> research-prompt.txt
          echo "MY LEADS:" >> research-prompt.txt
          cat ctx-leads.txt >> research-prompt.txt
          echo "" >> research-prompt.txt
          echo "VALIDATED FINDINGS (do not re-report):" >> research-prompt.txt
          cat ctx-valid.txt >> research-prompt.txt

          cat >> research-prompt.txt << 'PROMPT_START'
          ===== YOUR TASK =====
          Continue deep research on the ${TARGET} program scope. Go ONE phase deeper
          than before. Build on previous context - never repeat it. When a phase is
          exhausted, start RECON on a NEW unexplored surface (new subdomain,
          new API version, new public repo, new feature).

          ===== PHASES =====
          PHASE 1 RECON: VDP/disclosure rules, DNS/subdomain inventory (crt.sh,
          certificate transparency, passive DNS), tech stack, public repos of the
          ${TARGET} GitHub org, API docs, changelogs.
          PHASE 2 SURFACE: endpoints, auth mechanisms (OAuth/OIDC/JWT/sessions),
          permission model, multi-tenant architecture, sensitive data flows.
          PHASE 3 HYPOTHESIS: for each - why it might exist, impact, read-only
          PoC design. Prioritize: auth bypass, IDOR, SSRF, OAuth redirect_uri,
          business logic, cache poisoning, cloud misconfig, leaked secrets in
          public repos, source maps, debug endpoints, GraphQL abuse.
          PHASE 4 POC: exact curl (GET/HEAD only), expected signal, CVSS estimate.

          ===== OUTPUT FORMAT =====
          # Program: ${TARGET}
          # Phase: [RECON/SURFACE/HYPOTHESIS/POC]
          ## Key Findings (one line each, specific)
          ## New Endpoints / Assets
          ## Hypotheses (numbered, with impact + read-only test)
          ## Secrets Check (public repos: what was found, hash only, where)
          ## CVSS Candidates

          ---
          ## STATE UPDATE (exact format for parsing)
          STATUS_PHASE: [RECON|SURFACE|HYPOTHESIS|POC]
          STATUS_STATE: [IN_PROGRESS|EXHAUSTED|HIGH_POTENTIAL]
          NEXT_STEP_1: ...
          NEXT_STEP_2: ...
          NEXT_STEP_3: ...

          ===== RESPONSE RULES =====
          ALWAYS: be specific, professional, cite sources (URLs).
          NEVER: claim you tested something live, invent vulnerabilities, run
          attacks, include raw secrets, reference out-of-scope hosts.
          Begin research now.
          PROMPT_START

      - name: Run Model
        run: |
          export PATH="$HOME/.opencode/bin:$HOME/.local/bin:$PATH"
          timeout 18m opencode run --model "$MODEL_ID" < research-prompt.txt > raw_${MODEL_NAME}.txt 2>&1 || true
          echo "=== Preview ==="; head -25 raw_${MODEL_NAME}.txt

      - name: Parse State
        run: |
          python3 << 'PARSE_PYTHON'
          import re, json, os
          name = os.environ['MODEL_NAME']
          with open(f'raw_{name}.txt', errors='ignore') as f:
              output = f.read()
          state = {'phase': 'RECON', 'status': 'IN_PROGRESS'}
          m = re.search(r'STATUS_PHASE:\s*(RECON|SURFACE|HYPOTHESIS|POC)', output, re.IGNORECASE)
          if m: state['phase'] = m.group(1).upper()
          m = re.search(r'STATUS_STATE:\s*(IN_PROGRESS|EXHAUSTED|HIGH_POTENTIAL)', output, re.IGNORECASE)
          if m: state['status'] = m.group(1).upper()
          with open(f'state_{name}.json', 'w') as f:
              json.dump(state, f)
          print(state)
          PARSE_PYTHON

      - name: Extract Leads
        run: |
          python3 << 'EXTRACT_PYTHON'
          import re, os
          from datetime import datetime, timezone
          name = os.environ['MODEL_NAME']
          tgt = os.environ['TARGET']
          with open(f'raw_{name}.txt', errors='ignore') as f:
              output = f.read()
          leads = []
          for pat in [r'## Key Findings\n(.*?)(?=\n##|\Z)', r'## New Endpoints / Assets\n(.*?)(?=\n##|\Z)']:
[0m
[0m$ [0mSLOT=$(( $(date +%s) / 600 % 2 )); if [ "$SLOT" = "0" ]; then echo "TARGET=google"; else echo "TARGET=microsoft"; fi
TARGET=microsoft
[0m
Target for this run is **microsoft** (round-robin slot), phase **RECON** (fresh state). Beginning passive recon now.
[0m% [0mWebFetch https://www.microsoft.com/en-us/msrc/bounty
[0m
[0m$ [0msleep 1
(no output)
[0m
[0m% [0mWebFetch https://www.microsoft.com/en-us/msrc/pentest-rules-of-engagement
Rules of Engagement captured. Now passive subdomain recon via certificate transparency.
[0m
[0m$ [0msleep 1
(no output)
[0m
[0m✗ [0mWebFetch https://crt.sh/?q=%25.microsoft.com&output=json failed
[91m[1mError: [0mStatusCode: non 2xx status code (404 GET https://crt.sh/?q=%25.microsoft.com&output=json)
[0m
[0m$ [0msleep 1
(no output)
[0m
[0m$ [0mcurl -s -o /tmp/opencode/crt.json -w "%{http_code} %{size_download}\n" "https://crt.sh/?q=%25.microsoft.com&output=json" --max-time 30
502 150
[0m
crt.sh is flaky (502). Retrying with proper headers.
[0m$ [0msleep 2 && curl -s -A "Mozilla/5.0 (research)" -o /tmp/opencode/crt.json -w "%{http_code} %{size_download}\n" "https://crt.sh/?q=microsoft.com&output=json" --max-time 45
502 150
[0m
[0m$ [0msleep 3 && curl -s -A "Mozilla/5.0 (research)" -o /tmp/opencode/crt2.json -w "%{http_code} %{size_download}\n" "https://crt.sh/?q=%25.microsoft.com&output=json&group=Y" --max-time 45
502 150
[0m
crt.sh is unstable right now. Pivoting to passive DNS lookup service for subdomain inventory.
[0m$ [0mcurl -s -o /tmp/opencode/msft-subdomains.txt -w "%{http_code} %{size_download}\n" "https://api.hackertarget.com/hostsearch/?q=microsoft.com" --max-time 30
200 2700
[0m
[0m$ [0mwc -l /tmp/opencode/msft-subdomains.txt && head -50 /tmp/opencode/msft-subdomains.txt
51 /tmp/opencode/msft-subdomains.txt
microsoft.com,150.171.109.118
064-smtp-in-2a.microsoft.com,157.54.41.37
1esbot.microsoft.com,40.118.129.241
1esbot-test.microsoft.com,172.184.103.81
apps.1js.microsoft.com,20.119.0.21
pr.apps.1js.microsoft.com,20.119.0.21
torus.apps.1js.microsoft.com,20.48.204.12
publisher-aircapi.1pp.microsoft.com,40.112.243.98
1s.microsoft.com,150.171.109.77
studio.1s.microsoft.com,20.36.155.75
3d-avatar-diffusion.microsoft.com,23.101.10.141
3p-dataconnectors.microsoft.com,20.118.40.7
3papiprovider.microsoft.com,20.119.16.30
3pc.microsoft.com,150.171.109.65
dev.3pc.microsoft.com,150.171.109.74
ppe.3pc.microsoft.com,150.171.110.194
3pcapi.microsoft.com,150.171.109.74
3psecureppe.microsoft.com,13.66.138.103
api.3psecureppe.microsoft.com,13.66.138.103
3rdpartycode.microsoft.com,20.40.202.15
3rdpartysource.microsoft.com,20.40.202.15
3sdash.microsoft.com,4.152.194.249
a00000000000000000000000000000000000000000000000000000000000001.ms.a.microsoft.com,23.219.36.143
a00000000000000000000000000000000000000000000000000000000000002.ms.a.microsoft.com,23.219.36.136
a00000000000000000000000000000000000000000000000000000000000003.ms.a.microsoft.com,23.219.36.134
a0000000000000000000000000000000000000000000000000000000000001.ms.a.microsoft.com,23.219.36.140
a00000000000000000000000000000000000000000000000000000000000010.ms.a.microsoft.com,23.219.36.132
a000000000000000000000000000000000000000000000000000000000001.ms.a.microsoft.com,23.219.36.140
a00000000000000000000000000000000000000000000000000000000001.ms.a.microsoft.com,23.219.36.140
a00000000000000000000000000000000000000000000000000000000001337.ms.a.microsoft.com,23.219.36.132
a0000000000000000000000000000000000000000000000000000000001.ms.a.microsoft.com,23.219.36.140
a000000000000000000000000000000000000000000000000000000001.ms.a.microsoft.com,23.219.36.143
a0000000000000000000000000000000000000001.ms.a.microsoft.com,23.219.36.143
a0000000000000000000000001.ms.a.microsoft.com,23.219.36.140
a0000000000000000000000002.ms.a.microsoft.com,23.219.36.136
a000000000000000000001.ms.a.microsoft.com,23.219.36.140
a000000000000000001.ms.a.microsoft.com,23.219.36.143
a000000000000000002.ms.a.microsoft.com,23.219.36.136
a000000000000000003.ms.a.microsoft.com,23.219.36.134
a00000000001.ms.a.microsoft.com,23.219.36.143
a0000000001.ms.a.microsoft.com,23.219.36.143
a000001.ms.a.microsoft.com,23.219.36.143
a00001.ms.a.microsoft.com,23.219.36.140
a0001.ms.a.microsoft.com,23.219.36.140
a001.ms.a.microsoft.com,23.219.36.143
a01.ms.a.microsoft.com,23.219.36.143
a1.ms.a.microsoft.com,23.219.36.143
a10.ms.a.microsoft.com,23.219.36.132
a100.ms.a.microsoft.com,23.219.36.144
a1000.ms.a.microsoft.com,23.219.36.140
[0m
[0m$ [0mtail -5 /tmp/opencode/msft-subdomains.txt && sleep 1 && curl -s -o /tmp/opencode/azure-subdomains.txt -w "azure: %{http_code} %{size_download}\n" "https://api.hackertarget.com/hostsearch/?q=azure.com" --max-time 30
a1.ms.a.microsoft.com,23.219.36.143
a10.ms.a.microsoft.com,23.219.36.132
a100.ms.a.microsoft.com,23.219.36.144
a1000.ms.a.microsoft.com,23.219.36.140
a101.ms.a.microsoft.com,23.219.36.133
azure: 200 2225
[0m
[0m$ [0msleep 1 && curl -s -o /tmp/opencode/bing-subdomains.txt -w "bing: %{http_code} %{size_download}\n" "https://api.hackertarget.com/hostsearch/?q=bing.com" --max-time 30
bing: 200 1572
[0m
[0m$ [0mcat /tmp/opencode/azure-subdomains.txt && echo "---BING---" && cat /tmp/opencode/bing-subdomains.txt
azure.com,20.76.201.171
a365.azure.com,150.171.109.74
ppe.a365.azure.com,150.171.109.74
dxp.aad.azure.com,20.190.152.144
dxp-dev.aad.azure.com,40.70.77.243
idsap.dxp-dev.aad.azure.com,40.70.77.243
dxp-ppe.aad.azure.com,20.190.151.0
idsap.dxp-ppe.aad.azure.com,70.37.96.167
virtualagent.dxp-ppe.aad.azure.com,20.190.151.0
idsap.dxp.aad.azure.com,13.66.34.127
tip.dxp.aad.azure.com,40.126.24.16
virtualagent.dxp.aad.azure.com,20.190.152.144
api.cbxp.ac.azure.com,40.77.56.174
api.dev.accessmonitor.azure.com,150.171.110.193
api.prod.accessmonitor.azure.com,150.171.110.193
controlplane.accledger.azure.com,52.146.82.120
identity.accledger.azure.com,20.81.9.214
account.azure.com,52.171.140.237
account-staging.azure.com,13.65.39.165
support.iam.ad.azure.com,20.190.152.23
support-ppe.iam.ad.azure.com,20.190.151.0
adf.azure.com,52.234.242.227
s1.adhybridhealth.azure.com,20.190.152.23
prod.australiaeast.rp.ads.azure.com,20.70.223.142
prod.canadacentral.rp.ads.azure.com,20.116.43.116
prod.eastus.rp.ads.azure.com,20.232.88.85
prod.northeurope.rp.ads.azure.com,20.223.64.61
prod.southcentralus.rp.ads.azure.com,20.118.79.33
prod.southeastasia.rp.ads.azure.com,20.195.87.122
prod.uksouth.rp.ads.azure.com,20.117.193.56
prod.westeurope.rp.ads.azure.com,20.105.215.225
prod.westus2.rp.ads.azure.com,20.125.5.242
rp-au.prod.ae2.azure.com,4.198.205.185
rp-global.prod.ae2.azure.com,48.221.128.51
rp-us.prod.ae2.azure.com,48.221.128.51
rp-au.test.ae2.azure.com,48.215.125.253
rp-global.test.ae2.azure.com,128.85.120.103
rp-us.test.ae2.azure.com,128.85.120.103
chat.aedeptooling-int.azure.com,150.171.109.71
mcp.aedeptooling-int.azure.com,150.171.109.73
test.aedeptooling-int.azure.com,13.107.253.40
watchdog.aedeptooling-int.azure.com,13.107.226.40
chat.aedeptooling.azure.com,150.171.109.77
mcp.aedeptooling.azure.com,150.171.109.77
prod.aedeptooling.azure.com,150.171.109.74
watchdog.aedeptooling.azure.com,13.107.226.40
0137efe824ce441e8dbfcf322ccb5d69.afc.azure.com,20.102.157.114
08b83bdfd418477595fe5755e3f71d7a.afc.azure.com,20.102.157.114
0bc1e738009f49a290f4d56d28ab72db.afc.azure.com,20.102.157.114
12bba0fd4eba4925b5604b5c4fffa1b3.afc.azure.com,20.102.157.114
13a2b7bcc8034aa5bcd1d9f2646ea9e1.afc.azure.com,20.102.157.114
---BING---
0esoq.bing.com,150.171.28.10
0jli4.bing.com,150.171.28.10
1001-110.bing.com,150.171.28.10
1001-173.bing.com,150.171.27.10
1001-235.bing.com,150.171.27.10
1001-247.bing.com,150.171.28.10
1050ti-2-160.bing.com,150.171.28.10
1050ti-2-187.bing.com,150.171.27.10
1050ti-2-247.bing.com,150.171.28.10
1050ti-2-76.bing.com,150.171.27.10
1050ti-2-78.bing.com,150.171.28.10
1050ti-2-92.bing.com,150.171.28.10
1070-8gb-116.bing.com,150.171.27.10
1070-8gb-133.bing.com,150.171.27.10
1070-8gb-217.bing.com,150.171.27.10
1070-8gb-73.bing.com,150.171.27.10
10bin.bing.com,150.171.28.10
10mxj.bing.com,150.171.27.10
110.bing.com,150.171.27.10
112.bing.com,150.171.27.10
114.bing.com,150.171.27.10
120.bing.com,150.171.27.10
123.bing.com,150.171.27.10
132.bing.com,150.171.28.10
133.bing.com,150.171.27.10
14-30-182.bing.com,150.171.27.10
14-30-203.bing.com,150.171.28.10
14-30-217.bing.com,150.171.28.10
14-30-99.bing.com,150.171.28.10
142.bing.com,150.171.28.10
14m2u-220.bing.com,150.171.27.10
14m2u-235.bing.com,150.171.27.10
14m2u-52.bing.com,150.171.28.10
14m2u-68.bing.com,150.171.28.10
14viw.bing.com,150.171.27.10
15.bing.com,150.171.28.10
15uq0.bing.com,150.171.28.10
16.bing.com,150.171.28.10
165hz-176.bing.com,150.171.28.10
165hz-58.bing.com,150.171.28.10
165hz-73.bing.com,150.171.29.10
165hz-79.bing.com,150.171.27.10
1660-122.bing.com,150.171.27.10
1660-225.bing.com,150.171.28.10
16gb-113.bing.com,150.171.28.10
16gb-131.bing.com,150.171.28.10
16gb-158.bing.com,150.171.28.10
16gb-218.bing.com,150.171.28.10
16gb-236.bing.com,150.171.28.10
16gb-243.bing.com,150.171.27.10
[0m
Solid passive DNS haul. Now surveying the `microsoft` GitHub org public repos (in-scope code).
[0m$ [0msleep 1 && curl -s -o /tmp/opencode/msft-org-repos.json -w "%{http_code} %{size_download}\n" "https://api.github.com/orgs/microsoft/repos?per_page=100&sort=updated&page=1" --max-time 30
200 660547
[0m
[0m$ [0mpython3 - <<'EOF'
import json
data = json.load(open('/tmp/opencode/msft-org-repos.json'))
print("total fetched:", len(data))
sec_kw = ['sec','bounty','vuln','risk','threat','hunt','pentest','offen','graphql','token','identity','oauth','entra','aad','adfs','federation','msal','intune','defender','sentinel']
for r in data:
    name = r['name']
    if any(k in name.lower() for k in sec_kw):
        print(f"{r['full_name']:60s} stars={r['stargazers_count']:6d} pushed={r['pushed_at'][:10]} arch={r['archived']} fork={r['fork']}")
EOF
total fetched: 100
microsoft/Security-101                                       stars=  6773 pushed=2025-12-21 arch=False fork=False
microsoft/shell-intune-samples                               stars=  1080 pushed=2026-08-06 arch=False fork=False
[0m
[0m$ [0mls -la /home/runner/work/hunt-lab/hunt-lab/reposcan-raw/ /home/runner/work/hunt-lab/hunt-lab/leads/ /home/runner/work/hunt-lab/hunt-lab/triage/ 2>/dev/null
/home/runner/work/hunt-lab/hunt-lab/leads/:
total 28
drwxr-xr-x 2 runner runner 4096 Aug  7 08:52 .
drwxr-xr-x 9 runner runner 4096 Aug  7 08:52 ..
-rw-r--r-- 1 runner runner    0 Aug  7 08:52 lead-bigpickle.md
-rw-r--r-- 1 runner runner  250 Aug  7 08:52 reposcan-2026-08-07-08-25.md
-rw-r--r-- 1 runner runner  355 Aug  7 08:52 reposcan-2026-08-07-08-29.md
-rw-r--r-- 1 runner runner  290 Aug  7 08:52 reposcan-2026-08-07-08-34.md
-rw-r--r-- 1 runner runner  301 Aug  7 08:52 reposcan-2026-08-07-08-40.md
-rw-r--r-- 1 runner runner  301 Aug  7 08:52 reposcan-latest.md

/home/runner/work/hunt-lab/hunt-lab/reposcan-raw/:
total 16
drwxr-xr-x 2 runner runner 4096 Aug  7 08:52 .
drwxr-xr-x 9 runner runner 4096 Aug  7 08:52 ..
-rw-r--r-- 1 runner runner  166 Aug  7 08:52 analysis.txt
-rw-r--r-- 1 runner runner   55 Aug  7 08:52 grep-hits.txt

/home/runner/work/hunt-lab/hunt-lab/triage/:
total 8
drwxr-xr-x 2 runner runner 4096 Aug  7 08:52 .
drwxr-xr-x 9 runner runner 4096 Aug  7 08:52 ..
[0m
[0m→ [0mRead leads/reposcan-latest.md
[0m→ [0mRead reposcan-raw/analysis.txt
[0m→ [0mRead reposcan-raw/grep-hits.txt
[0m
[0m$ [0msleep 1 && curl -s -o /tmp/opencode/msft-cloud-bounty.json -w "%{http_code}\n" "https://www.microsoft.com/en-us/api/msrc/bounty-programs" --max-time 30; echo "---"; curl -s -o /tmp/opencode/msrc-api.json -w "%{http_code}\n" "https://api.msrc.microsoft.com/api/programs" --max-time 30 2>/dev/null || true
404
---
404
[0m
[0m% [0mWebFetch https://www.microsoft.com/en-us/msrc/bounty-programs
[0m
[0m$ [0msleep 1
(no output)
[0m
[0m% [0mWebFetch https://microsoft.com/msrc/bounty-microsoft-identity
Strong RECON data. Now checking passive DNS on the newly in-scope identity endpoints and remaining GitHub org pages.
[0m
[0m$ [0mfor h in microsoftazuread-sso.com accounts.accesscontrol.windows.net mysignins.microsoft.com api.mysignins.microsoft.com provisioningapi.microsoftonline.com adminwebservice.microsoftonline.com; do echo "== $h"; dig +short "$h" | head -3; sleep 1; done
== microsoftazuread-sso.com
== accounts.accesscontrol.windows.net
a.privatelink.msidentity.com.
prda.aadg.msidentity.com.
www.tm.a.prd.aadg.akadns.net.
== mysignins.microsoft.com
na.privatelink.msidentity.com.
prdf.aadg.msidentity.com.
www.tm.f.prd.aadg.trafficmanager.net.
== api.mysignins.microsoft.com
na.privatelink.msidentity.com.
prdf.aadg.msidentity.com.
www.tm.f.prd.aadg.trafficmanager.net.
== provisioningapi.microsoftonline.com
== adminwebservice.microsoftonline.com
adminwebservice.mso.msidentity.com.
lrt.privatelink.msidentity.com.
prdv6e.aadg.msidentity.com.
[0m
[0m$ [0msleep 1
(no output)
[0m
[0m% [0mWebFetch https://www.microsoft.com/en-us/msrc/opensourcebountyprogram
[0m
[0m$ [0msleep 1 && curl -s -o /tmp/opencode/msft-org-stars.json -w "%{http_code}\n" "https://api.github.com/orgs/microsoft/repos?per_page=100&sort=stars&page=1" --max-time 30 && python3 - <<'EOF'
import json
data = json.load(open('/tmp/opencode/msft-org-stars.json'))
kw = ['sec','auth','identity','token','msal','entra','aad','graph','oauth','saml','openid','azure-pipelines','intune','defender','signed','codex','chakra']
for r in data:
    n = r['name'].lower()
    if any(k in n for k in kw):
        print(f"{r['full_name']:55s} stars={r['stargazers_count']:7d} lang={str(r['language']):12s} pushed={r['pushed_at'][:10]}")
EOF
200
microsoft/TWL400-Using-AI-to-Secure-the-Modern-SOC      stars=      2 lang=Python       pushed=2026-07-15
microsoft/TWL300-Entra-Suite-for-Frontier-Transformation stars=      0 lang=Ruby         pushed=2026-07-02
microsoft/TechLab-Module-19-Defender-for-Storage        stars=      0 lang=None         pushed=2026-06-25
microsoft/entra-sms-voice-usage-analyzer                stars=     12 lang=PowerShell   pushed=2026-06-24
microsoft/TechLab-Entra-Suite-for-Frontier-Transformation stars=      0 lang=None         pushed=2026-06-25
microsoft/TechLabModule17-DefenderCSPM                  stars=      0 lang=None         pushed=2026-07-10
microsoft/TechLab-Module-12-Defender-for-Databases      stars=      0 lang=None         pushed=2026-06-25
[0m
[0m$ [0mpython3 - <<'EOF'
import json
data = json.load(open('/tmp/opencode/msft-org-stars.json'))
print("TOP 40 BY STARS:")
for r in data[:40]:
    print(f"{r['full_name']:50s} stars={r['stargazers_count']:7d} pushed={r['pushed_at'][:10]} arch={r['archived']}")
EOF
TOP 40 BY STARS:
microsoft/GSA-Policy-Workbench                     stars=      0 pushed=2026-08-07 arch=False
microsoft/amplifier-bundle-android-tester          stars=      0 pushed=2026-08-07 arch=False
microsoft/agentic-journeys                         stars=      0 pushed=2026-08-05 arch=False
microsoft/azuresqlfoundations                      stars=      0 pushed=2026-08-05 arch=False
microsoft/llm-triage-experiment                    stars=      1 pushed=2026-08-04 arch=False
microsoft/agentic-loops                            stars=      1 pushed=2026-08-04 arch=False
microsoft/amplifier-bundle-computer-use            stars=      0 pushed=2026-08-07 arch=False
microsoft/skill-recorder                           stars=   2205 pushed=2026-08-06 arch=False
microsoft/postgres-mcp                             stars=      2 pushed=2026-08-06 arch=False
microsoft/fabric-clinical-supply-ontology          stars=      0 pushed=2026-07-28 arch=False
microsoft/dry4csharp                               stars=      3 pushed=2026-07-27 arch=False
microsoft/mutate4csharp                            stars=      3 pushed=2026-07-29 arch=False
microsoft/crap4csharp                              stars=      4 pushed=2026-08-06 arch=False
microsoft/amplifier-app-simulated-user-research    stars=      0 pushed=2026-08-05 arch=False
microsoft/Echoverse                                stars=     36 pushed=2026-08-04 arch=False
microsoft/VibeASR.cpp                              stars=     98 pushed=2026-08-03 arch=False
microsoft/medusa                                   stars=      3 pushed=2026-08-04 arch=False
microsoft/iqdeepdive                               stars=     17 pushed=2026-08-05 arch=False
microsoft/lvbs-build                               stars=      0 pushed=2026-07-23 arch=False
microsoft/lvbs-qemu                                stars=      0 pushed=2026-07-23 arch=False
microsoft/lvbs-linux                               stars=      0 pushed=2026-07-23 arch=False
microsoft/upgrade-agent-actions                    stars=      0 pushed=2026-07-21 arch=False
microsoft/CrewMeal                                 stars=      0 pushed=2026-08-05 arch=False
microsoft/cowork-investment-assessment             stars=      3 pushed=2026-07-20 arch=False
microsoft/github-copilot-java-mcp-demo             stars=      0 pushed=2026-08-03 arch=False
microsoft/IssueLens                                stars=      0 pushed=2026-08-07 arch=False
microsoft/MAI-Code                                 stars=      0 pushed=2026-07-17 arch=False
microsoft/BinaryShield                             stars=      3 pushed=2026-08-03 arch=False
microsoft/cobol-docs-pipeline-sample               stars=      1 pushed=2026-07-16 arch=False
microsoft/universal-artifact-sdk                   stars=      1 pushed=2026-08-05 arch=False
microsoft/foundrysamples                           stars=      2 pushed=2026-08-05 arch=False
microsoft/amplifier-bundle-redaction               stars=      0 pushed=2026-07-16 arch=False
microsoft/FSQ                                      stars=     10 pushed=2026-08-07 arch=False
microsoft/gopherconus-26-booth-exercises           stars=      0 pushed=2026-07-15 arch=False
microsoft/sqlaiinaction                            stars=      1 pushed=2026-07-30 arch=False
microsoft/new-copilot-studio-tech-guide-kr         stars=      0 pushed=2026-08-02 arch=False
microsoft/agent-framework-durable-extension        stars=      4 pushed=2026-08-05 arch=False
microsoft/images                                   stars=      1 pushed=2026-07-09 arch=False
microsoft/CoworkCost                               stars=      6 pushed=2026-07-08 arch=False
microsoft/CopilotScope                             stars=      5 pushed=2026-07-14 arch=False
[0m
[0m$ [0msleep 1 && curl -s -o /tmp/opencode/oidc.json -w "oidc: %{http_code}\n" "https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration" --max-time 20 && python3 -c "
import json
d=json.load(open('/tmp/opencode/oidc.json'))
print('issuer:',d.get('issuer'))
print('authorization_endpoint:',d.get('authorization_endpoint'))
print('token_endpoint:',d.get('token_endpoint'))
print('jwks_uri:',d.get('jwks_uri'))
print('device_authorization_endpoint:',d.get('device_authorization_endpoint'))
print('check_session_iframe:',d.get('check_session_iframe'))
print('end_session_endpoint:',d.get('end_session_endpoint'))
print('code_challenge_methods_supported:',d.get('code_challenge_methods_supported'))
print('response_types:',d.get('response_types_supported'))
print('subject_types_supported:',d.get('subject_types_supported'))
print('claims_supported (n=):',len(d.get('claims_supported',[])))
"
oidc: 200
issuer: https://login.microsoftonline.com/{tenantid}/v2.0
authorization_endpoint: https://login.microsoftonline.com/common/oauth2/v2.0/authorize
token_endpoint: https://login.microsoftonline.com/common/oauth2/v2.0/token
jwks_uri: https://login.microsoftonline.com/common/discovery/v2.0/keys
device_authorization_endpoint: https://login.microsoftonline.com/common/oauth2/v2.0/devicecode
check_session_iframe: None
end_session_endpoint: https://login.microsoftonline.com/common/oauth2/v2.0/logout
code_challenge_methods_supported: None
response_types: ['code', 'id_token', 'code id_token', 'id_token token']
subject_types_supported: ['pairwise']
claims_supported (n=): 19
[0m
[0m$ [0mcat >> /home/runner/work/hunt-lab/hunt-lab/research/bigpickle.md <<'EOF'

## RUN 2026-08-07 (big-pickle) — TARGET: microsoft — PHASE: RECON

### VDP / Disclosure notes
- Report via https://msrc.microsoft.com/report/vulnerability/new (portal: aka.ms/secure-at). Include Correlation ID per Identity program.
- Cloud programs: Identity up to $100k, Azure $60k, Copilot $30k, Azure DevOps $20k, Dynamics/PowerPlatform $20k, Defender $20k, M365 $19.5k, OpenSource $15k, .NET $15k.
- Identity bounty in-scope hosts (additions Jul 2025 + Nov 2025 are freshest):
  mysignins.microsoft.com, api.mysignins.microsoft.com, myaccount.microsoft.com, myaccess.microsoft.com, myapps.microsoft.com, microsoftazuread-sso.com, accounts.accesscontrol.windows.net, account.microsoft.com, provisioningapi.microsoftonline.com, adminwebservice.microsoftonline.com, login.windows.net, login.microsoftonline.com, login.live.com, account.live.com, signup.live.com, passwordreset.microsoftonline.com, account.windowsazure.com, credential.activedirectory.windowsazure.com + Azure AD B2C.
- Out-of-scope for Identity: subdomain takeover, pure URL redirect, missing headers, cookie replay, DoS, MFA bypass requiring physical device access, 3rd-party integration code.
- Open Source bounty (up to $15k): azure/azure-sdk* (8 repos), fluentui, PowerShell, typescript, vscode, monaco-editor, msquic, agent-framework. semantic-kernel & autogen EXCLUDED. GitHub Actions vulns $750-5k.
- Rules of Engagement: no DoS, no network-intense fuzzing, no accessing secrets even if publicly leaked (flag & report only), must not access customer data.

### Passive DNS (hackertarget hostsearch, ~50/microsoft.com + azure.com + bing.com)
- Notable: 3d-avatar-diffusion.microsoft.com (AI), 3papiprovider.microsoft.com, 3psecureppe.microsoft.com + api.3psecureppe.microsoft.com (PPE), publisher-aircapi.1pp.microsoft.com, 1esbot.microsoft.com / 1esbot-test.microsoft.com, studio.1s.microsoft.com, account-staging.azure.com (staging!), api.dev.accessmonitor.azure.com / api.prod.accessmonitor.azure.com, controlplane.accledger.azure.com + identity.accledger.azure.com, dxp.aad.azure.com + dxp-dev / dxp-ppe / tip.dxp (AAD gateway), support-ppe.iam.ad.azure.com, chat.aedeptooling-int.azure.com + mcp.aedeptooling-int.azure.com (int env), *.ms.a.microsoft.com (cache hosts), bing.com GPU gaming hosts (xCloud).
- DNS resolution checks (dig, passive):
  - accounts.accesscontrol.windows.net -> a.privatelink.msidentity.com -> prda.aadg.msidentity.com (AAD gateway)
  - mysignins.microsoft.com / api.mysignins.microsoft.com -> na.privatelink.msidentity.com -> prdf.aadg.msidentity.com
  - adminwebservice.microsoftonline.com -> adminwebservice.mso.msidentity.com -> prdv6e.aadg.msidentity.com
  - microsoftazuread-sso.com: apex has NO A/AAAA (interesting - host-only domain)
  - provisioningapi.microsoftonline.com: no A record returned at lookup time
- crt.sh returned 404/502 on this run (flaky); retry in a later run for CT-based inventory.

### OIDC surface (v2.0, login.microsoftonline.com/common/v2.0/.well-known/openid-configuration)
- issuer: https://login.microsoftonline.com/{tenantid}/v2.0
- authorize/token/devicecode/logout endpoints under /common/oauth2/v2.0/
- response_types: code, id_token, code id_token, id_token token
- subject_types_supported: pairwise
- jwks: /common/discovery/v2.0/keys

### GitHub org (microsoft, 100 by updated + 100 by stars)
- Recently active / new repos worth later look: amplifier-bundle-* (android-tester, computer-use, redaction, simulated-user-research), universal-artifact-sdk, upgrade-agent-actions, skill-recorder, Echoverse, CopilotScope, CoworkCost.
- security-sorted (stars): microsoft/Security-101 (6773★), microsoft/shell-intune-samples (1080★).
- No secrets flagged in reposcan (0 grep hits as of 08:40 UTC) — separate reposcan job covers this.

### Next phase direction (SURFACE)
- Map endpoints/auth on the Jul 2025 added identity hosts (mysignins, microsoftazuread-sso.com, myaccount, myaccess, myapps, accounts.accesscontrol.windows.net).
- Graph API identity workflow endpoints (v1.0 Directory Management / Governance / Identity & SignIn tabs).
- OAuth redirect_uri / response_mode surface on login.microsoftonline.com, login.live.com.
- Re-run crt.sh for CT inventory.
EOF
echo "journal written: $(wc -l < /home/runner/work/hunt-lab/hunt-lab/research/bigpickle.md) lines"
journal written: 39 lines
[0m
Solid RECON data gathered. Recording journal and updating state files.
# Program: microsoft
# Phase: RECON

## Key Findings
- **Program landscape**: MSRC Cloud programs cap at $100k (Identity), $60k (Azure), $30k (Copilot), $20k each (Azure DevOps / Dynamics+Power Platform / Defender); Open Source program caps at $15k for a fixed repo list; report portal `msrc.microsoft.com/report/vulnerability/new` (aka.ms/secure-at), Correlation ID required. Sources: https://www.microsoft.com/en-us/msrc/bounty-programs, https://www.microsoft.com/en-us/msrc/pentest-rules-of-engagement
- **Identity bounty added 5 new hosts in Jul 2025 and account.microsoft.com in Nov 2025** — freshly-expanded scope is the best-hit-rate surface this run: `mysignins.microsoft.com`, `api.mysignins.microsoft.com`, `myaccount.microsoft.com`, `myaccess.microsoft.com`, `myapps.microsoft.com`, `microsoftazuread-sso.com`, `accounts.accesscontrol.windows.net`. Source: https://microsoft.com/msrc/bounty-microsoft-identity (Revision History).
- **Identity out-of-scope traps**: subdomain takeover, pure URL redirects (unless chained), missing headers, cookie replay, DoS, MFA-bypass needing physical access — these will be triaged out, do not report.
- **Rules of Engagement nuance**: retrieving/using credentials "regardless of how obtained, including leaked publicly" is prohibited — leaked-secret checks must stop at hashing + flagging for triage, never use.
- **DNS**: in-scope hosts terminate on the AAD gateway cluster: `mysignins`/`api.mysignins` -> `prdf.aadg.msidentity.com`; `accounts.accesscontrol.windows.net` -> `prda.aadg.msidentity.com`; `adminwebservice.microsoftonline.com` -> `adminwebservice.mso.msidentity.com` (aadg). `microsoftazuread-sso.com` apex has NO A record (host-keyed domain). `provisioningapi.microsoftonline.com` had no A record at lookup time.
- **OIDC v2.0 confirmed**: issuer templated `https://login.microsoftonline.com/{tenantid}/v2.0`, endpoints under `/common/oauth2/v2.0/` (authorize/token/devicecode/logout), `subject_types_supported: pairwise`, response types include `code id_token`. Metadata: https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
- **crt.sh was down (404/502)** this run — CT-based inventory deferred, do not re-crawl aggressively.

## New Endpoints / Assets
- Identity in-scope: `microsoftazuread-sso.com`, `mysignins.microsoft.com`, `api.mysignins.microsoft.com`, `myaccount.microsoft.com`, `myaccess.microsoft.com`, `myapps.microsoft.com`, `accounts.accesscontrol.windows.net`, `account.microsoft.com`, `provisioningapi.microsoftonline.com`, `adminwebservice.microsoftonline.com`
- Passive-DNS surfaced (non-identity, in-scope *.azure.com / *.microsoft.com): `account-staging.azure.com`, `api.dev.accessmonitor.azure.com`, `api.prod.accessmonitor.azure.com`, `controlplane.accledger.azure.com`, `identity.accledger.azure.com`, `3psecureppe.microsoft.com` + `api.3psecureppe.microsoft.com`, `dxp.aad.azure.com` (+ `dxp-dev`, `dxp-ppe`, `tip.dxp`), `support-ppe.iam.ad.azure.com`, `chat.aedeptooling-int.azure.com`, `mcp.aedeptooling-int.azure.com`, `3d-avatar-diffusion.microsoft.com`
- OAuth endpoints: `/common/oauth2/v2.0/authorize`, `/token`, `/devicecode`, `/logout`; JWKS `/common/discovery/v2.0/keys`
- Open Source bounty repos: azure/azure-sdk* (8), fluentui, PowerShell, typescript, vscode, monaco-editor, msquic, agent-framework

## Hypotheses (for next phase, SURFACE)
1. **OAuth redirect_uri / response_mode confusion on freshly-added hosts** — new 2025 hosts (mysignins, microsoftazuread-sso.com) often still carry legacy SAML/WS-Fed or permissive redirect validation. Impact: account takeover (Identity: up to $100k). Test: read-only, fetch the hosts' authorization/discovery metadata and OIDC config; inspect documented redirect_uri validation in public docs before any live request.
2. **`microsoftazuread-sso.com` apex NXDOMAIN / host-keyed domain** — understand how the wildcard/custom SSO host is validated; misrouted `redirect_uri` or `whr` parameters are a classic AAD flaw class. Impact: token leakage/ATO. Read-only design: examine publicly documented SAML/SSO flows; verify registration semantics from docs only.
3. **`accounts.accesscontrol.windows.net` (legacy ACS)** — legacy endpoint family, historically weaker validation; check for alive legacy endpoints/trust configurations. Impact: token forgery/replay. Read-only: HEAD/GET of metadata documents (wsdl, OpenID config) only.
4. **Staging/PPE endpoints in scope** (`account-staging.azure.com`, `api.3psecureppe`, `dxp-ppe`, `support-ppe`, `aedeptooling-int`) — pre-prod often has debug/unauth routes. Impact: info disclosure/IDOR. Read-only: GET/HEAD on a bounded set of known paths; expect 401/404 baseline.
5. **Graph API Identity workflow endpoints** (Directory Management / Governance / Identity & Sign-In tabs) — IDOR via `beta` tenant-scoped reads. Impact: cross-tenant info disclosure. Read-only: only documented behavior and docs; live testing requires own tenant (rules allow test tenants, but not this run).
6. **Leaked secrets in newly-active microsoft org repos** — active `amplifier-bundle-*`, `universal-artifact-sdk`, `upgrade-agent-actions` etc. Impact: supply-chain. Read-only: scan for config/keys; hash+flag only. Reposcan job had 0 hits so far.

## Secrets Check (public repos: what was found, hash only, where)
- No secrets flagged this run. reposcan job (independent) reports 0 grep hits across scanned dirs as of 2026-08-07 08:40 UTC (`leads/reposcan-latest.md`). Per ROE, any future find will be hashed only (sha256) and flagged for triage, never used.

## CVSS Candidates
- (None yet — RECON only. For planning: OAuth redirect/ATO ~CVSS 8.1–9.3 if auth bypass+MFA bypass; IDOR on identity workflows ~CVSS 6.5–8.1; leaked-credential supply chain unrated until validated.)

---
## STATE UPDATE (exact format for parsing)
STATUS_PHASE: RECON
STATUS_STATE: IN_PROGRESS
NEXT_STEP_1: Retry crt.sh CT inventory for *.microsoft.com + newly-added identity hosts (one bounded query, ~1 rps).
NEXT_STEP_2: Advance to SURFACE on Jul-2025-added identity hosts: fetch their well-known/discovery + metadata docs (read-only) and map auth mechanisms.
NEXT_STEP_3: Enumerate live.com / outlook.com / xbox.com passive DNS + GitHub org pages 2-3 for new repos; log leads to leads/lead-bigpickle.md if any.
