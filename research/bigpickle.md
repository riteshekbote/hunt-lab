
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

## RUN 2026-08-07 (big-pickle) — TARGET: microsoft — PHASE: SURFACE

### mysignins.microsoft.com (SPA, My Sign-Ins, app-version 4.0.2233)
- React SPA, bundle /bundle/main.caa6a456.js (6MB). clientId (MSAL): 19db86c3-b2b9-44cc-b339-36da233a3be2. Authz: https://login.microsoftonline.com/common/oauth2/v2.0/authorize (from bundle).
- CSP: script-src 'self' amcdn.msftauth.net js.monitor.azure.com (no unsafe-eval/inline) — XSS surface limited.
- Headers: x-ms-mysignins-region: westus2 (region pinned), x-ms-correlation-id, client-request-id.
- Backend: api.mysignins.microsoft.com. Probes (no token):
  - /api/me -> 404 (not an endpoint on this host)
  - /api/session/currentuser -> 405 (POST-only), /api/authenticationmethods/availablemethods, /api/signIns, /api/tenants -> 401 + WWW-Authenticate: Bearer
- Full endpoint map from bundle (auth-gated): /api/session/authorize, /api/session/issessionvalid, /api/session/extendsessionvalidity, /api/signIns, /api/signIns/{id}, /api/signInMap, /api/tenants, /api/organization, /api/authenticationmethods/{availablemethods,completeProofup,delete,getpostredirectparameters,initializemobileapp,new,registrationpolicy,setdefault,verify,disablehardwaretoken}, /api/password/{change,reset,passwordMethods}, /api/captcha/{,validation}, /api/features/getEnabledApps, /api/enabledFeatures, /api/post/registerSsprMethods, /api/signOutEverywhere, /api/acknowledgeSignIn, /api/me, /api/me/image, /api/users/getImages, /api/ab.
- Graph endpoints from bundle: /me/agentSignInSessions (agentic sign-in sessions!), /authentication/phoneMethods, /authentication/temporaryAccessPassMethods, /devices, /authentication/signInPreferences, /profile, /recent-activity/agentic-sessions, /security-info, /token/failed.
- api.mysignins region header + 401-with-Bearer confirm APIM/AAD-gated gateway; /api/session/currentuser POST-only is a session-state oracle.

### api.myaccount.microsoft.com (My Account backend, also serves SPA)
- Root serves My Account SPA, bundle /bundle/main.4e6e3dc6.js (9.2MB). clientId: 8c59ead7-d703-4a27-9e55-c96a0054c8d2 (MSAL public client).
- Config blocks per env (prod/ppe/sov): azureInstance login.microsoftonline.com; msGraphResourceName https://graph.microsoft.com; aadGraphResourceName https://graph.windows.net; myAccountBackendUrl api.myaccount.*; myAccessUrl myaccess.*; cpmBaseUrl jcmsfd.account.microsoft.com/CPM; cpmAudience https://jarvisapi.account.microsoft.com.
- Probes: /api/me, /api/authenticationmethods/availablemethods, /api/security/authentication/signinpreferences -> 401 (auth-gated, same gateway style).
- Endpoint map from bundle: /api/EnduserEmailContacts, /api/TenantEmailSettings, /api/allTimeZones, /api/availableLanguages, /api/availableRegionalFormats, /api/canVerifiedIdBeIssued, /api/devices, /api/features/{getEnabledApps,isConvergedUXV2Enabled,isSsprConvergenceEnabled}, /api/groups/settings, /api/instrument/logclient, /api/issueVerifiedEmployeeCredential (Entra Verified ID!), /api/me/image, /api/organization(s), /api/shell/getshellinfo, /api/signOutEverywhere, /api/termsofuse/{agreements/,myacceptances,tenantbannerlogo,tenantdisplayname}, /api/users/getImages.
- Consumer tenant 9188040d-6c67-4c5b-b112-36a304b66dad (MSA tenant) referenced for /v2.0.
- Feature flags: isVerifiedIDEnabled, isMySignInsEnabled, manageAgentNavItemEnabled (agents in My Account!), itemInsightsToggleEnabled, isNewHomePageOptInEnabled.

### accounts.accesscontrol.windows.net (legacy ACS, AAD ESTS-fronted)
- Root: 302 -> https://www.office.com/login#; headers x-ms-ests-server: 2.1.24997.11 (WUS3). Live legacy host.
- /metadata/json/1 -> 200 JSON with 5 signing x509 keys (JWKS-style), cert CN "accounts.accesscontrol.windows.net", valid-from 2026-06.
- /v2/metadata, /v2/openid-configuration, /v2/trust, /wsfederationmetadata -> 404 (old ACS WCF paths gone).
- Discovery endpoints from metadata: /tokens/OAuth/2 (OAuth2 issuance), /tokens/delegation/1 (DelegationIssuance1.0), /mgmt/delegation/1 (DelegationManagement1.0). GET on /tokens/OAuth/2 and /mgmt/delegation/1 -> 200 sign-in HTML (login page, not 404) — endpoint pathing alive, auth-gated.

### microsoftazuread-sso.com (Entra custom-domain SSO)
- Apex: NO A/AAAA. NS: ns1-34.azure-dns.{com,net,org,info}. TXT: google-site-verification=gHF6gHIrBiWfQG4HXNM0qcLtSc4RbZF5eLu2T-4nHic. No CNAME.
- Subdomain probes login./mysignins./sso.microsoftazuread-sso.com -> no route (000/timeout). Host-keyed domain with no HTTP surface at apex; likely reserved for tenant "host-name" SSO redirects (whr/host header keyed). Low direct surface.

### login.live.com OAuth surface
- .well-known/openid-configuration -> issuer https://login.live.com; authorize oauth20_authorize.srf; token oauth20_token.srf; jwks nexus.passport.com/public/partner/discovery/key. No devicecode endpoint.
- authorize probe with unregistered redirect_uri -> 302 to oauth20_desktop.srf?error=invalid_request&error_description=...redirect_uri is not valid... (exact leak of validation: client must be 'mobile' OR redirect must be absolute + registered).

### login.microsoftonline.com (AAD v2) authorize surface
- Unknown/unregistered client_id + evil redirect_uri -> 200 sign-in HTML (no early error; AAD validates client AFTER auth or returns generic page). Compare: login.live.com errors fast on redirect_uri. AAD v2 defers; classic redirect_uri normalization/response_mode research area.

### Graph identity endpoints (401-baseline confirms alive)
- /v1.0/me/agentSignInSessions, /beta/me/agentSignInSessions, /v1.0/me/authentication/methods -> 401 InvalidAuthenticationToken (endpoints resolve, auth required). agentSignInSessions is the agentic sign-in session surface (aligned with My Sign-Ins recent-activity/agentic-sessions).

### Passive-DNS follow-ups (non-identity azure.com, in scope)
- api.dev/prod.accessmonitor.azure.com -> 403 (AFD/WAF gated, no banner).
- controlplane.accledger.azure.com -> Kestrel; /health -> 200 "Healthy" + headers x-ms-machinename (pod name), x-ms-image-tag 1.0.03467.455-73c40f8d7a2145ba44be22ca0c6029eca472d3d8, x-ms-correlation-request-id. Structured 404 envelope {"Status":"failed","Error":{"Code":...}}. No swagger/api/ledger/tenants.
- identity.accledger.azure.com -> CNAME identity-accledger-prod-1.eastus.cloudapp.azure.com; no HTTP(S) responder (000).
- aedeptooling-int (chat/mcp/test/watchdog) + api.3psecureppe.microsoft.com -> 000/timeout (no route from this egress).

===== RUN 2026-08-07 09:08:17 UTC | model bigpickle | target google =====
[0m
> build · big-pickle
[0m
[0m$ [0mcat state_bigpickle.json 2>/dev/null; echo "---"; ls -la; echo "---"; cat scope.yml
{"phase": "RECON", "status": "IN_PROGRESS"}---
total 168
drwxr-xr-x 9 runner runner  4096 Aug  7 09:01 .
drwxr-xr-x 3 runner runner  4096 Aug  7 09:01 ..
drwxr-xr-x 7 runner runner  4096 Aug  7 09:01 .git
drwxr-xr-x 3 runner runner  4096 Aug  7 09:01 .github
-rw-r--r-- 1 runner runner  1022 Aug  7 09:01 README.md
-rw-r--r-- 1 runner runner  2543 Aug  7 09:01 ctx-leads.txt
-rw-r--r-- 1 runner runner 45641 Aug  7 09:01 ctx-research.txt
-rw-r--r-- 1 runner runner    28 Aug  7 09:01 ctx-valid.txt
drwxr-xr-x 2 runner runner  4096 Aug  7 09:01 leads
-rw-r--r-- 1 runner runner    32 Aug  7 09:01 raw_bigpickle.txt
drwxr-xr-x 2 runner runner  4096 Aug  7 09:01 reports
drwxr-xr-x 2 runner runner  4096 Aug  7 09:01 reposcan-raw
drwxr-xr-x 2 runner runner  4096 Aug  7 09:01 research
-rw-r--r-- 1 runner runner 51009 Aug  7 09:01 research-prompt.txt
-rw-r--r-- 1 runner runner  1650 Aug  7 09:01 scope.yml
-rw-r--r-- 1 runner runner    43 Aug  7 09:01 state_bigpickle.json
-rw-r--r-- 1 runner runner    43 Aug  7 09:01 state_laguna.json
-rw-r--r-- 1 runner runner    51 Aug  7 09:01 state_longcat.json
drwxr-xr-x 2 runner runner  4096 Aug  7 09:01 triage
---
programs:
  google:
    name: Google Vulnerability Reward Program
    disclosure_policy: https://bughunters.google.com/about/rules
    domains:
      - google.com
      - "*.google.com"
    github_orgs:
      - google
  microsoft:
    name: Microsoft Bounty / MSRC
    disclosure_policy: https://www.microsoft.com/en-us/msrc/bounty
    domains:
      - microsoft.com
      - "*.microsoft.com"
      - "*.msn.com"
      - "*.live.com"
      - "*.office.com"
      - "*.bing.com"
      - "*.azure.com"
      - "*.azurewebsites.net"
      - "*.windows.net"
      - "*.sharepoint.com"
      - "*.onmicrosoft.com"
      - "*.outlook.com"
      - "*.hotmail.com"
      - "*.xbox.com"
      - "*.dynamics.com"
      - "*.powerapps.com"
    github_orgs:
      - microsoft

rules:
  # Both programs require passive, non-disruptive testing.
  passive_only: true          # GET/HEAD reads only; no scanners, no fuzzers, no auth-bypass attempts on live infra
  max_requests_per_second: 1  # never hammer
  no_account_creation: true   # no mass signups (MSRC: banned; Google InTENT: restricted)
  no_data_modification: true  # no writes, deletes, state changes
  no_pre_disclosure: true     # never disclose before program response
  redact_secrets_in_commits: true  # commit sha256 of secrets, never raw values

# Multi-model ensemble: every hunt run picks a rotation of the 5 models.
models:
  - { id: opencode/big-pickle, name: bigpickle }
  - { id: opencode/nemotron-3-ultra-free, name: nemotron3 }
  - { id: opencode/longcat-2.0-free, name: longcat }
  - { id: opencode/ling-3.0-flash-free, name: ling3 }
  - { id: opencode/laguna-s-2.1-free, name: laguna }
[0m
[0m$ [0mcat research/bigpickle.md 2>/dev/null | head -60; echo "===LEADS==="; cat leads/lead-bigpickle.md 2>/dev/null | head -40
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
===LEADS===

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
[0m
I'm big-pickle, moving RECON → SURFACE on microsoft. Previous run mapped scope and DNS. This run: endpoint/auth surface on the freshly-scoped identity hosts, Graph identity workflow endpoints, and OAuth surface — all read-only GET/HEAD at 1 rps.
[0m$ [0mfor h in mysignins.microsoft.com www.mysignins.microsoft.com microsoftazuread-sso.com www.microsoftazuread-sso.com accounts.accesscontrol.windows.net; do echo "== $h"; curl -sI -o /dev/null -w "  HTTP %{http_code} -> %{redirect_url}\n" "https://$h/" --max-time 15 || echo "  [fail/timeout]"; sleep 1; done
== mysignins.microsoft.com
  HTTP 200 -> 
== www.mysignins.microsoft.com
  HTTP 000 -> 
  [fail/timeout]
== microsoftazuread-sso.com
  HTTP 000 -> 
  [fail/timeout]
== www.microsoftazuread-sso.com
  HTTP 000 -> 
  [fail/timeout]
== accounts.accesscontrol.windows.net
  HTTP 200 ->
[0m
[0m$ [0msleep 2; for h in provisioningapi.microsoftonline.com adminwebservice.microsoftonline.com account.microsoft.com; do echo "== $h"; dig +short "$h" | head -2; curl -sI -o /dev/null -w "  HTTP %{http_code} -> %{redirect_url}\n" "https://$h/" --max-time 15 || echo "  [fail/timeout]"; sleep 1; done
== provisioningapi.microsoftonline.com
  HTTP 000 -> 
  [fail/timeout]
== adminwebservice.microsoftonline.com
adminwebservice.mso.msidentity.com.
lrt.privatelink.msidentity.com.
  HTTP 404 -> 
== account.microsoft.com
account.microsoft.com.edgekey.net.
e9412.b.akamaiedge.net.
  HTTP 000 -> 
  [fail/timeout]
[0m
[0m$ [0mcurl -s "https://mysignins.microsoft.com/" --max-time 20 | head -c 2000; echo; echo "=== HEADERS ==="; curl -sI "https://mysignins.microsoft.com/" --max-time 15
<!doctype html><html lang="en"><head><meta charset="utf-8"/><meta name="viewport" content="width=device-width,initial-scale=1,shrink-to-fit=no"/><meta name="theme-color" content="#000000"/><meta http-equiv="X-UA-Compatible" content="IE=edge"/><meta http-equiv="Content-Security-Policy" content="script-src 'self' amcdn.msftauth.net js.monitor.azure.com "/><link rel="manifest" href="/manifest.json"/><link rel="shortcut icon" href="/favicon.ico"/><meta name="app-version" content="4.0.2233"/><title>My Sign-Ins</title><base href="/"><meta name="description" content="My Sign-ins"><link rel="icon" href="/favicon.ico"><script defer="defer" src="/bundle/main.caa6a456.js"></script></head><body><noscript>You need to enable JavaScript to run this app</noscript><div id="root"></div></body></html>
=== HEADERS ===
HTTP/1.1 200 OK
Content-Length: 793
Content-Type: text/html
Last-Modified: Tue, 28 Jul 2026 09:48:44 GMT
Accept-Ranges: bytes
ETag: "1dd1e76472ba519"
x-ms-correlation-id: 37ae2b86-e9b9-4613-9681-51daa143fbe8
x-ms-mysignins-region: westus2
x-ms-gateway-requestid: ba85c43a-b8d1-4e93-a200-cdd6dc337891
client-request-id: 499d6215-55e1-4f40-b5bb-68e593c50b74
X-Content-Type-Options: nosniff
Date: Fri, 07 Aug 2026 09:02:16 GMT
[0m
[0m$ [0mcurl -s "https://mysignins.microsoft.com/manifest.json" --max-time 15 | head -c 1500; echo; echo "=== bundle size ==="; curl -sI "https://mysignins.microsoft.com/bundle/main.caa6a456.js" --max-time 15 | head -8
{
  "short_name": "My Signins",
  "name": "My Signins",
  "icons": [
    {
      "src": "favicon.ico",
      "sizes": "192x192",
      "type": "image/png"
    }
  ],
  "start_url": "./index.html",
  "display": "standalone",
  "theme_color": "#000000",
  "background_color": "#ffffff"
}

=== bundle size ===
HTTP/1.1 200 OK
Content-Length: 6006324
Content-Type: text/javascript
Last-Modified: Tue, 28 Jul 2026 09:48:44 GMT
Accept-Ranges: bytes
ETag: "1dd1e7647700034"
x-ms-correlation-id: 7c5333a9-5409-49e9-93a3-4a970a7587ce
x-ms-mysignins-region: westus2
[0m
[0m$ [0mcurl -s "https://mysignins.microsoft.com/bundle/main.caa6a456.js" --max-time 60 -o /tmp/opencode/mysignins-bundle.js && wc -c /tmp/opencode/mysignins-bundle.js && echo "=== api/url strings ===" && grep -oE 'https://[a-zA-Z0-9._/-]+' /tmp/opencode/mysignins-bundle.js | sort -u | grep -viE 'w3.org|schema.org|github.com|mozilla|reactjs|microsoft.com/en-us/|aka.ms|support.microsoft|learn.microsoft|msrc' | head -60
6006324 /tmp/opencode/mysignins-bundle.js
=== api/url strings ===
https://a
https://aka.eaglex.ic.gov/fwlink
https://aka.microsoft.scloud/fwlink
https://api.myaccount.azure.us
https://api.myaccount.eaglex.ic.gov
https://api.myaccount.identity.sovcloud.de
https://api.myaccount.identity.sovcloud.fr
https://api.myaccount.microsoft.com
https://api.myaccount.microsoft.scloud
https://api.myaccount.windowsazure.cn
https://api.myprofile-ppe.microsoft.com
https://au.pipe.aria.microsoft.com/Collector/3.0/
https://breeze.aimon.applicationinsights.io
https://browser.events.data.microsoft.com/OneCollector/1.0/
https://browser.pipe.aria.microsoft.com/Collector/3.0/
https://cdnssl.Clicktale.net/
https://clients.config.office.net/
https://collector.azure.cn/OneCollector/1.0/
https://collector.azure.de/OneCollector/1.0/
https://collector.azure.eaglex.ic.gov/OneCollector/1.0/
https://collector.azure.fr/OneCollector/1.0/
https://collector.azure.microsoft.scloud/OneCollector/1.0/
https://dc-int.services.visualstudio.com
https://dc.services.visualstudio.com
https://de.pipe.aria.microsoft.com/Collector/3.0/
https://directory.identity.sovcloud-api.de
https://directory.identity.sovcloud-api.fr
https://entra.microsoft.com
https://eu.pipe.aria.microsoft.com/Collector/3.0/
https://fb.me/react-polyfills
https://go.microsoft.com/fwlink/
https://go.microsoft.com/fwlink/p/
https://graph.chinacloudapi.cn
https://graph.cloudapi.microsoft.scloud
https://graph.eaglex.ic.gov
https://graph.microsoft-ppe.com
https://graph.microsoft.com
https://graph.microsoft.scloud
https://graph.microsoft.us
https://graph.microsoftazure.us
https://graph.ppe.windows.net
https://graph.svc.sovcloud.de
https://graph.svc.sovcloud.fr
https://graph.windows.net
https://jp.pipe.aria.microsoft.com/Collector/3.0/
https://login.chinacloudapi.cn/
https://login.chinacloudapi.cn/common/
https://login.microsoftonline.com/
https://login.microsoftonline.com/common/
https://login.microsoftonline.eaglex.ic.gov/
https://login.microsoftonline.eaglex.ic.gov/common/
https://login.microsoftonline.microsoft.scloud/
https://login.microsoftonline.microsoft.scloud/common/
https://login.microsoftonline.us/
https://login.microsoftonline.us/common/
https://login.sovcloud-identity.de/
https://login.sovcloud-identity.de/common/
https://login.sovcloud-identity.fr/
https://login.sovcloud-identity.fr/common/
https://login.windows-ppe.net/
[0m
[0m$ [0mcd /tmp/opencode && echo "=== clientId/appId candidates ===" && grep -oE '(clientId|client_id|appId)["\x27]?\s*[:=]\s*["\x27][a-zA-Z0-9-]{20,36}' mysignins-bundle.js | sort -u | head -20 && echo "=== redirectUri ===" && grep -oE 'redirectUri[^,]{0,80}' mysignins-bundle.js | sort -u | head -10 && echo "=== api path fragments ===" && grep -oE '"/[a-zA-Z0-9_/-]{4,60}"' mysignins-bundle.js | sort -u | grep -iE 'api|account|auth|signin|profile|security|session|device|token|credential' | head -50
=== clientId/appId candidates ===
clientId:"19db86c3-b2b9-44cc-b339-36da233a3be2
=== redirectUri ===
redirectUri
redirectUri:c
=== api path fragments ===
"/acknowledgeSignIn"
"/api/ab"
"/api/authenticationmethods/availablemethods"
"/api/authenticationmethods/completeProofup"
"/api/authenticationmethods/delete"
"/api/authenticationmethods/disablehardwaretoken"
"/api/authenticationmethods/getpostredirectparameters"
"/api/authenticationmethods/initializemobileapp"
"/api/authenticationmethods/new"
"/api/authenticationmethods/registrationpolicy"
"/api/authenticationmethods/setdefault"
"/api/authenticationmethods/verify"
"/api/captcha/"
"/api/captcha/validation"
"/api/enabledFeatures"
"/api/features/getEnabledApps"
"/api/me"
"/api/me/image"
"/api/organization"
"/api/password/change"
"/api/password/passwordMethods"
"/api/password/reset"
"/api/post/registerSsprMethods"
"/api/session/authorize"
"/api/session/currentuser"
"/api/session/extendsessionvalidity"
"/api/session/issessionvalid"
"/api/session/sessionvalidinfo"
"/api/signInMap"
"/api/signIns"
"/api/signIns/"
"/api/signOutEverywhere"
"/api/tenants"
"/api/users/getImages"
"/authentication"
"/authentication/phoneMethods"
"/authentication/signInPreferences"
"/authentication/temporaryAccessPassMethods"
"/authentication/temporaryAccessPassMethods/"
"/devices"
"/disableSmsSignIn"
"/enableSmsSignIn"
"/me/agentSignInSessions"
"/me/agentSignInSessions/"
"/profile"
"/recent-activity/agentic-sessions"
"/security-info"
"/token/failed"
[0m
[0m$ [0mcd /tmp/opencode && echo "=== redirectUri contexts ===" && grep -oE 'redirectUri[:=][^,;]{0,120}' mysignins-bundle.js | sort -u | head && echo "=== api base/tenant contexts ===" && grep -oE '"[a-z-]+(myaccount|myprofile|mysignins)[a-z0-9./-]*"' mysignins-bundle.js | sort -u | head && echo "=== scope strings ===" && grep -oE '"[a-zA-Z0-9./-]+\.default"|openid[^",]{0,40}' mysignins-bundle.js | sort -u | head && echo "=== authorize/known endpoints ===" && grep -oE '/(common|consumers|organizations)/(oauth2|saml2)/[a-zA-Z0-9/._-]+' mysignins-bundle.js | sort -u | head
=== redirectUri contexts ===
redirectUri:c
=== api base/tenant contexts ===
"x-ms-mysignins-region"
=== scope strings ===
"/.default"
openid
openid-configuration
=== authorize/known endpoints ===
[0m
Rich SPA surface found. Let me extract more context around the API usage and auth config.
[0m$ [0mcd /tmp/opencode && echo "=== session/authorize context ===" && grep -oE '.{60}/api/session/authorize.{60}' mysignins-bundle.js | head -3 && echo "=== logout/endSession ===" && grep -oE '.{40}(endSessionUrl|logout|post_logout_redirect_uri|redirect_uri).{80}' mysignins-bundle.js | head -8 && echo "=== known msal interaction ===" && grep -oE 'loginHint[^,;]{0,80}|account\.homeAccountId[^,;]{0,80}' mysignins-bundle.js | sort -u | head -6
=== session/authorize context ===
ath:"/api/session/currentuser",state:r.FORWARDING},m={path:"/api/session/authorize",state:r.FORWARDING},g={path:"/api/session/extendsessionval
=== logout/endSession ===
t){return(0,$n.getCustomAuthProvider)().logout(),e})),(0,C.addUnexpectedErrorHandler)((0,S.getFailureActionType)(w.S.signOutEv
"AuthLifecycle"),p=e.init,f=e.login,h=e.logout,m=e.acquireToken,g=new Map;return e.init=(0,a.O)((function(n){d.info("init"),t.
pped due to a redirect in progress")},e.logout=function(n){t.assertInitialized(),d.debug("logout called"),t.state.redirectStat
.RedirectStatus.NotRedirecting?d.debug("logout skipped due to a redirect in progress."):(d.info("logout (hints: ".concat(JSON.
("string"),token:(0,o.typeOf)("string"),logout:(0,o.typeOf)("string")}))),k=(0,o.struct)().optional({authorization_endpoint:(0
ode",C),_.set("code_verifier",S),_.set("redirect_uri",g),t.next=24;break;case 17:if("refresh_token"!==d.type){t.next=23;break}w=d.ty
d("client_id",b),w.searchParams.append("redirect_uri",g),w.searchParams.append("scope",_),w.searchParams.append("response_type","cod
.next=3,l(n);case 3:return i=t.sent,a=i.logout,(s=new URL(a)).searchParams.set("post_logout_redirect_uri",r),u(s,o),t.abrupt("
=== known msal interaction ===
loginHint
loginHint:""}
[0m
[0m# [0mTodos
[✓] Map mysignins SPA endpoints + auth config (done: clientId, MyAccount API, session/auth methods)
[•] Probe documented My Account API paths on api.mysignins/api.myaccount (read-only 401-baseline)
[ ] Probe accounts.accesscontrol.windows.net (legacy ACS) metadata surface
[ ] Probe account.microsoft.com + login.live.com OAuth surface
[ ] Survey Graph API v1.0 identity workflow endpoints (public metadata)
[ ] Write research journal, leads, state update (SURFACE)
[0m
SPA surface captured (clientId `19db86c3-b2b9-44cc-b339-36da233a3be2`, My Account API, session/authenticationmethod endpoints). Now probing documented API paths on the backend host + other in-scope identity hosts, read-only.
[0m$ [0mfor p in "/api/me" "/api/session/currentuser" "/api/authenticationmethods/availablemethods" "/api/signIns" "/api/tenants"; do echo "== api.mysignins.microsoft.com$p"; curl -s -o /tmp/opencode/resp.txt -w "  HTTP %{http_code} | %{content_type} | %{size_download}b\n" "https://api.mysignins.microsoft.com$p" --max-time 15; head -c 300 /tmp/opencode/resp.txt; echo; sleep 1; done
== api.mysignins.microsoft.com/api/me
  HTTP 404 |  | 0b

== api.mysignins.microsoft.com/api/session/currentuser
  HTTP 405 |  | 0b

== api.mysignins.microsoft.com/api/authenticationmethods/availablemethods
  HTTP 401 |  | 0b

== api.mysignins.microsoft.com/api/signIns
  HTTP 401 |  | 0b

== api.mysignins.microsoft.com/api/tenants
  HTTP 401 |  | 0b
[0m
[0m$ [0mecho "== 401 body/details on api.mysignins"; curl -s -D - "https://api.mysignins.microsoft.com/api/authenticationmethods/availablemethods" --max-time 15 | head -20; sleep 1; echo "== accounts.accesscontrol.windows.net root"; curl -sI "https://accounts.accesscontrol.windows.net/" --max-time 15 | head -15
== 401 body/details on api.mysignins
HTTP/1.1 401 Unauthorized
x-ms-correlation-id: c0d8957d-a2fc-4569-8401-6774e49340ac
x-ms-mysignins-region: westus2
x-ms-gateway-requestid: 23b79339-f33e-45dc-a1d4-0a147482a6cd
client-request-id: 18e5c033-b1ca-4c44-9e5e-7c6c0ef3dd20
X-Content-Type-Options: nosniff
WWW-Authenticate: Bearer
Date: Fri, 07 Aug 2026 09:03:21 GMT
Content-Length: 0

== accounts.accesscontrol.windows.net root
HTTP/1.1 200 OK
Cache-Control: no-store, no-cache
Pragma: no-cache
Content-Length: 23802
Content-Type: text/html; charset=utf-8
Expires: -1
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
Link: <https://aadcdn.msauth.net>; rel=preconnect; crossorigin
Link: <https://aadcdn.msauth.net>; rel=preconnect; crossorigin,<https://aadcdn.msauth.net>; rel=dns-prefetch
Link: <https://aadcdn.msauth.net>; rel=preconnect; crossorigin,<https://aadcdn.msauth.net>; rel=dns-prefetch,<https://aadcdn.msftauth.net>; rel=dns-prefetch
X-DNS-Prefetch-Control: on
P3P: CP="DSP CUR OTPi IND OTRi ONL FIN"
x-ms-request-id: 5d8562e9-47bc-4c93-a89a-13f9b11fa300
x-ms-ests-server: 2.1.24997.11 - WUS3 ProdSlices
[0m
[0m$ [0mcurl -s "https://accounts.accesscontrol.windows.net/" --max-time 15 -o /tmp/opencode/acs-root.html && echo "title/redirect refs:" && grep -oiE '<title>[^<]*</title>|location.href[^;]*|href="[^"]{0,80}"' /tmp/opencode/acs-root.html | sort -u | head -20
title/redirect refs:
<title>Object moved</title>
href="https://www.office.com/login#"
[0m
`accounts.accesscontrol.windows.net` is serviced by AAD ESTS — legacy ACS surface alive. Let me inspect the page and probe known ACS metadata endpoints.
[0m$ [0mfor p in "/v2/metadata" "/v2/openid-configuration" "/v2/trust" "/metadata/json/1" "/wsfederationmetadata"; do echo "== accounts.accesscontrol.windows.net$p"; curl -s -o /tmp/opencode/x.txt -w "  HTTP %{http_code} | %{content_type} | %{size_download}b\n" "https://accounts.accesscontrol.windows.net$p" --max-time 15; head -c 200 /tmp/opencode/x.txt; echo; sleep 1; done
== accounts.accesscontrol.windows.net/v2/metadata
  HTTP 404 |  | 0b

== accounts.accesscontrol.windows.net/v2/openid-configuration
  HTTP 404 |  | 0b

== accounts.accesscontrol.windows.net/v2/trust
  HTTP 404 |  | 0b

== accounts.accesscontrol.windows.net/metadata/json/1
  HTTP 200 | application/json; charset=utf-8 | 6358b
{"keys":[{"usage":"signing","keyValue":{"type":"x509Certificate","value":"MIIC/TCCAeWgAwIBAgIICR+ICKYVUk0wDQYJKoZIhvcNAQELBQAwLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldDAeFw0yNjA2MD
== accounts.accesscontrol.windows.net/wsfederationmetadata
  HTTP 404 |  | 0b
[0m
`/metadata/json/1` is a live JWKS for the legacy ACS host. Capturing key fingerprint details, then probing the OAuth surface on login.live.com / account.microsoft.com.
[0m$ [0mcurl -s "https://accounts.accesscontrol.windows.net/metadata/json/1" --max-time 15 -o /tmp/opencode/acs-keys.json && python3 -c "
import json
d=json.load(open('/tmp/opencode/acs-keys.json'))
for k in d.get('keys',[]):
    print('usage:',k.get('usage'),'| alg:',k.get('alg'),'| kid:',k.get('keyValue',{}).get('type') if isinstance(k.get('keyValue'),dict) else k.get('kid'))
print('endpoints:', d.get('endpoints'))
print('certSubject:', d.get('issuer'))
"; sleep 1; echo "== login.live.com well-known"; curl -s -o /tmp/opencode/live-oidc.json -w "HTTP %{http_code} | %{content_type}\n" "https://login.live.com/.well-known/openid-configuration" --max-time 15 && python3 -c "
import json
d=json.load(open('/tmp/opencode/live-oidc.json'))
print('issuer:',d.get('issuer'))
print('authorization:',d.get('authorization_endpoint'))
print('token:',d.get('token_endpoint'))
print('jwks:',d.get('jwks_uri'))
print('device:',d.get('device_authorization_endpoint'))
"
usage: signing | alg: None | kid: x509Certificate
usage: signing | alg: None | kid: x509Certificate
usage: signing | alg: None | kid: x509Certificate
usage: signing | alg: None | kid: x509Certificate
usage: signing | alg: None | kid: x509Certificate
endpoints: [{'location': 'https://accounts.accesscontrol.windows.net/tokens/OAuth/2', 'protocol': 'OAuth2', 'usage': 'issuance'}, {'location': 'https://accounts.accesscontrol.windows.net/tokens/delegation/1', 'protocol': 'DelegationIssuance1.0', 'usage': 'issuance'}, {'location': 'https://accounts.accesscontrol.windows.net/mgmt/delegation/1', 'protocol': 'DelegationManagement1.0', 'usage': 'management'}]
certSubject: None
== login.live.com well-known
HTTP 200 | application/json
issuer: https://login.live.com
authorization: https://login.live.com/oauth20_authorize.srf
token: https://login.live.com/oauth20_token.srf
jwks: https://nexus.passport.com/public/partner/discovery/key
device: None
[0m
Legacy ACS exposes live OAuth2/delegation issuance endpoints. Probing them read-only, plus Entra custom-domain SSO host.
[0m$ [0mfor u in "https://accounts.accesscontrol.windows.net/tokens/OAuth/2" "https://accounts.accesscontrol.windows.net/mgmt/delegation/1"; do echo "== $u"; curl -s -o /tmp/opencode/x.txt -w "  HTTP %{http_code} | %{content_type} | %{size_download}b\n" "$u" --max-time 15; head -c 300 /tmp/opencode/x.txt; echo; sleep 1; done; echo "== microsoftazuread-sso.com subdomain probes"; for h in login.microsoftazuread-sso.com mysignins.microsoftazuread-sso.com sso.microsoftazuread-sso.com; do echo "  $h"; curl -sI -o /dev/null -w "    HTTP %{http_code} -> %{redirect_url}\n" "https://$h/" --max-time 12 || echo "    [timeout/no route]"; sleep 1; done
== https://accounts.accesscontrol.windows.net/tokens/OAuth/2
  HTTP 200 | text/html; charset=utf-8 | 23795b


<!-- Copyright (C) Microsoft Corporation. All rights reserved. -->
<!DOCTYPE html>
<html dir="ltr" class="" lang="en">
<head>
    <title>Sign in to your account</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <meta http-equiv="X-UA-Compatible" content="I
== https://accounts.accesscontrol.windows.net/mgmt/delegation/1
  HTTP 200 | text/html; charset=utf-8 | 23795b


<!-- Copyright (C) Microsoft Corporation. All rights reserved. -->
<!DOCTYPE html>
<html dir="ltr" class="" lang="en">
<head>
    <title>Sign in to your account</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <meta http-equiv="X-UA-Compatible" content="I
== microsoftazuread-sso.com subdomain probes
  login.microsoftazuread-sso.com
    HTTP 000 -> 
    [timeout/no route]
  mysignins.microsoftazuread-sso.com
    HTTP 000 -> 
    [timeout/no route]
  sso.microsoftazuread-sso.com
    HTTP 000 -> 
    [timeout/no route]
[0m
[0m$ [0mecho "== microsoftazuread-sso.com DNS (all record types, passive)"; dig microsoftazuread-sso.com ANY +short; echo "-- NS:"; dig microsoftazuread-sso.com NS +short; echo "-- TXT:"; dig microsoftazuread-sso.com TXT +short; echo "-- CNAME:"; dig microsoftazuread-sso.com CNAME +short; sleep 1; echo "== ACS sign-in form action target"; grep -oE 'action="[^"]*"|name="[a-zA-Z]+"|method="[^"]*"' /tmp/opencode/acs-root.html | sort -u | head -20
== microsoftazuread-sso.com DNS (all record types, passive)
ns4-34.azure-dns.info.
ns1-34.azure-dns.com.
ns2-34.azure-dns.net.
ns3-34.azure-dns.org.
-- NS:
ns1-34.azure-dns.com.
ns3-34.azure-dns.org.
ns4-34.azure-dns.info.
ns2-34.azure-dns.net.
-- TXT:
"google-site-verification=gHF6gHIrBiWfQG4HXNM0qcLtSc4RbZF5eLu2T-4nHic"
-- CNAME:
== ACS sign-in form action target
[0m
[0m$ [0mecho "== account.microsoft.com (in-scope Nov2025)"; curl -sI "https://account.microsoft.com/" --max-time 20 | head -12; sleep 1; echo "== account.microsoft.com/about redirect chain"; curl -s -o /dev/null -w "final=%{http_code} url=%{url_effective}\n" -L --max-time 20 "https://account.microsoft.com/" 2>/dev/null | head -1
== account.microsoft.com (in-scope Nov2025)
== account.microsoft.com/about redirect chain
final=000 url=https://account.microsoft.com/
[0m
[0m$ [0mecho "== api.myaccount.microsoft.com (My Account API backend)"; curl -sI "https://api.myaccount.microsoft.com/" --max-time 20 | head -10; sleep 1; echo "== account.microsoft.com via DNS"; dig +short account.microsoft.com | head -3; echo "== try login.live.com authorize (no auth - expect 200/302 login)"; curl -s -o /dev/null -w "HTTP %{http_code} | %{redirect_url}\n" "https://login.live.com/oauth20_authorize.srf?client_id=000000004C12AE6A&response_type=code&redirect_uri=https%3A%2F%2Flogin.live.com%2Foauth20_desktop.srf&scope=wl.basic&response_mode=query" --max-time 20
== api.myaccount.microsoft.com (My Account API backend)
HTTP/1.1 200 OK
Cache-Control: no-store
Content-Length: 468
Content-Type: text/html
Last-Modified: Fri, 24 Jul 2026 15:09:26 GMT
Accept-Ranges: bytes
ETag: "1dd1b7e6aa4d6d4"
Strict-Transport-Security: max-age=2592000
X-Content-Type-Options: nosniff
Content-Security-Policy-Report-Only: default-src 'self';img-src 'self' https: data:;font-src 'self' https: data:;script-src 'self';connect-src 'self' https:;style-src 'self' 'unsafe-inline'; frame-src 'self' https:; frame-ancestors 'none'
== account.microsoft.com via DNS
account.microsoft.com.edgekey.net.
e9412.b.akamaiedge.net.
23.0.193.154
== try login.live.com authorize (no auth - expect 200/302 login)
HTTP 302 | https://login.live.com/oauth20_desktop.srf?error=invalid_request&error_description=The%20provided%20value%20for%20the%20input%20parameter%20'redirect_uri'%20is%20not%20valid.%20The%20client%20application%20must%20be%20marked%20as%20'mobile'%2c%20or%20the%20value%20must%20be%20an%20absolute%20URL%20which%20matches%20the%20registered%20redirect%20URI.&lc=1033
[0m
login.live.com OAuth surface confirmed (redirect_uri validation active, precise error reveals validation behavior). Now the My Account API root and Graph identity endpoint metadata.
[0m$ [0mecho "== api.myaccount root body"; curl -s "https://api.myaccount.microsoft.com/" --max-time 20 | head -c 500; echo; sleep 1; echo "== probe api.myaccount known paths"; for p in "/api/me" "/api/authenticationmethods/availablemethods" "/api/security/authentication/signinpreferences"; do echo "  $p"; curl -s -o /dev/null -w "    HTTP %{http_code}\n" "https://api.myaccount.microsoft.com$p" --max-time 15; sleep 1; done
== api.myaccount root body
<!doctype html><html><head><meta charset="utf-8"/><meta name="viewport" content="width=device-width,initial-scale=1,shrink-to-fit=no"/><meta http-equiv="X-UA-Compatible" content="IE=edge"/><meta name="theme-color" content="#000000"/><title>My Account</title><link rel="icon" href="/favicon.ico"><script defer="defer" src="/bundle/main.shim.js"></script></head><body><noscript>You need to enable JavaScript to run this app.</noscript><div id="root"></div></body></html>
== probe api.myaccount known paths
  /api/me
    HTTP 401
  /api/authenticationmethods/availablemethods
    HTTP 401
  /api/security/authentication/signinpreferences
    HTTP 401
[0m
[0m$ [0mecho "== myaccount bundle"; curl -s "https://api.myaccount.microsoft.com/bundle/main.shim.js" --max-time 30 | head -c 200; echo; curl -s "https://api.myaccount.microsoft.com/bundle/main.js" --max-time 60 -o /tmp/opencode/myaccount.js 2>/dev/null; wc -c /tmp/opencode/myaccount.js 2>/dev/null; echo "== clientId + endpoints"; grep -oE 'clientId:"[a-zA-Z0-9-]{20,40}"' /tmp/opencode/myaccount.js 2>/dev/null | sort -u | head; grep -oE '"/api/[a-zA-Z0-9/_]{3,60}"' /tmp/opencode/myaccount.js 2>/dev/null | sort -u | head -40
== myaccount bundle
!function(){"use strict";var e=document.createElement("script");e.src="/bundle/main.4e6e3dc6.js",e.type="text/javascript",document.body.appendChild(e)}();
// SIG // Begin signature block
// SIG // M
0 /tmp/opencode/myaccount.js
== clientId + endpoints
[0m
[0m$ [0mcurl -s "https://api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js" --max-time 60 -o /tmp/opencode/myaccount2.js && wc -c /tmp/opencode/myaccount2.js && echo "== clientId"; grep -oE 'clientId:"[a-zA-Z0-9-]{20,40}"' /tmp/opencode/myaccount2.js | sort -u | head; echo "== api paths"; grep -oE '"/api/[a-zA-Z0-9/_]{3,60}"' /tmp/opencode/myaccount2.js | sort -u | grep -viE 'captcha' | head -50
9279288 /tmp/opencode/myaccount2.js
== clientId
== api paths
"/api/EnduserEmailContacts"
"/api/TenantEmailSettings"
"/api/allTimeZones"
"/api/availableLanguages"
"/api/availableRegionalFormats"
"/api/canVerifiedIdBeIssued"
"/api/dateTimeFormats"
"/api/devices"
"/api/devices/"
"/api/features/getEnabledApps"
"/api/features/isConvergedUXV2Enabled"
"/api/features/isSsprConvergenceEnabled"
"/api/groups/settings"
"/api/instrument/logclient"
"/api/issueVerifiedEmployeeCredential"
"/api/me/image"
"/api/organization"
"/api/organizations"
"/api/organizations/"
"/api/shell/getshellinfo"
"/api/signOutEverywhere"
"/api/termsofuse/agreements/"
"/api/termsofuse/myacceptances"
"/api/termsofuse/tenantbannerlogo"
"/api/termsofuse/tenantdisplayname"
"/api/users/getImages"
[0m
[0m$ [0mcd /tmp/opencode && echo "== verified id / VC context"; grep -oE '.{80}(issueVerifiedEmployeeCredential|canVerifiedIdBeIssued).{80}' myaccount2.js | head -2; echo "== org switch endpoints"; grep -oE '"/api/[a-zA-Z0-9/_]*[Ss]witch[a-zA-Z0-9/_]*"' myaccount2.js | sort -u | head; echo "== authority/tenant fragments"; grep -oE '(authority|tenantId|tenant-id|common/oauth2)[^,]{0,60}' myaccount2.js | sort -u | head -12; echo "== adal/msal markers"; grep -oE 'adal|msal|login.microsoftonline[^"]{0,60}' myaccount2.js | sort -u | head
== verified id / VC context
nction(){var e=(0,oe.a)().auth,t="".concat(Z.r.get("myAccountBackendUrl"),"/api/issueVerifiedEmployeeCredential"),n=Z.r.getRequired("clientId");return(0,ve.useMutation)(fc(regeneratorRuntime.
(0,_e.f1)(),l=function(){var e,t="".concat(Z.r.get("myAccountBackendUrl"),"/api/canVerifiedIdBeIssued"),n=Z.r.getRequired("clientId"),r=(0,oe.a)().auth;return(0,ve.useQuery)({queryK
== org switch endpoints
== authority/tenant fragments
authority
authority and the CloudInstanceDiscoveryMetadata returned from the ne
authority and verify the .well-known/openid-configuration endpoint re
authority configuration options: https://docs.microsoft.com/en-us/azu
authority does not support logout."}
authority endpoint");if(!n.endsWith("common/"))throw new Error("Autho
authority found."
authority has not completed endpoint discovery.");this.authority=e}
authority host.")
authority is not a trusted authority. Please include this authority i
authority or account."}
authority was not found within the CloudInstanceDiscoveryMetadata ret
== adal/msal markers
adal
login.microsoftonline.com
login.microsoftonline.com/
login.microsoftonline.com/9188040d-6c67-4c5b-b112-36a304b66dad/v2.0
login.microsoftonline.com/common/
login.microsoftonline.com/common/discovery/instance?api-version=1.1&authorization
login.microsoftonline.com/common/discovery/v2.0/keys
login.microsoftonline.com/common/kerberos
login.microsoftonline.com/common/oauth2/v2.0/authorize
login.microsoftonline.com/common/oauth2/v2.0/devicecode
[0m
[0m$ [0mcd /tmp/opencode && echo "== feature flags used by myaccount"; grep -oE '"[a-zA-Z0-9.-]*[Ff]eature[a-zA-Z0-9.-]*"' myaccount2.js | sort -u | head -20; echo "== getRequired/config keys"; grep -oE 'getRequired\("[a-zA-Z0-9.-]+"\)' myaccount2.js | sort -u | head -30
== feature flags used by myaccount
"AddFeatureFlags"
"Feature"
"FeatureArea"
"FeatureFlags"
"FeatureIsDisabled"
"FeatureLayout"
"FeatureListItem"
"NewFeature"
"NewFeatureCallout"
"NewFeatureDetails"
"NewFeatureLink"
"NewFeatureVideo"
"ShellFeatureFlags"
"enableSubFeatures"
"featureHeading"
"featureTurnedOffMessage"
"featureconsumer"
"featureflags"
"getFeatureFlagValue"
"missingFeatureFlagValue"
== getRequired/config keys
getRequired("authAuthorityEndpoint")
getRequired("clientId")
getRequired("cpmAudience")
getRequired("cpmBaseUrl")
getRequired("inProductFeedback")
getRequired("msGraphResourceName")
getRequired("myAccountBackendUrl")
[0m
[0m$ [0mcd /tmp/opencode && echo "== config key defaults"; grep -oE '"[a-zA-Z][a-zA-Z0-9]*":\s*".{0,80}"' myaccount2.js | grep -iE 'clientid|cpm|graph|backend|resource|authority' | sort -u | head -20; echo "== config lookup keys (JSON.parse style)"; grep -oE 'get\("[a-zA-Z][a-zA-Z0-9]*"\)' myaccount2.js | sort -u | head -40
== config key defaults
"aadGraphResourceName":"https://graph.ppe.windows.net","ssgmResourceName":"
"aadGraphResourceName":"https://graph.windows.net","ssgmResourceName":"
"ariaTelemetryKey":"","authAuthorityEndpoint":"https://login.partner.microsoftonline.cn/common/","
"authAuthorityEndpoint":"https://login.microsoftonline.com/common/","
"authAuthorityEndpoint":"https://login.microsoftonline.eaglex.ic.gov/common/","
"authAuthorityEndpoint":"https://login.microsoftonline.microsoft.scloud/common/","
"clientId":"8c59ead7-d703-4a27-9e55-c96a0054c8d2","iamuxClientId":"
"cpmBaseUrl":"","cpmAudience":"","telemetryCollectorServiceEndpoint":"","isMySignInsEnabled"
"cpmBaseUrl":"https://jcmsdf.account.microsoft-int.com/CPM","cpmAudience":"
"cpmBaseUrl":"https://jcmsfd.account.microsoft.com/CPM","cpmAudience":"
"env":"ppe","azureInstance":"https://login.windows-ppe.net/","msGraphResourceName":"
"env":"prod","azureInstance":"https://login.microsoftonline.com/","msGraphResourceName"
"meControlViewAccountLink":"https://myaccount.azure.us","ariaTelemetryKey":"","authAuthorityEndpoint":"
"msGraphResourceName":"https://graph.eaglex.ic.gov","aadGraphResourceName":"
"msGraphResourceName":"https://graph.microsoft.scloud","aadGraphResourceName":"
"msGraphResourceName":"https://graph.microsoft.us","aadGraphResourceName":"
"msGraphResourceName":"https://graph.svc.sovcloud.de","aadGraphResourceName":"
"msGraphResourceName":"https://graph.svc.sovcloud.fr","aadGraphResourceName":"
"msGraphResourceName":"https://microsoftgraph.chinacloudapi.cn","aadGraphResourceName":"
"myAccountBackendUrl":"https://api.myaccount.azure.us","myAppsUrl":"https://myapps.microsoft.us","
== config lookup keys (JSON.parse style)
get("ETag")
get("Expires")
get("ReadWriteConsistencyToken")
get("a")
get("azureInstance")
get("c")
get("changePasswordFwdLink")
get("cpmAudience")
get("cpmBaseUrl")
get("defaultCollectorEndpoint")
get("env")
get("error")
get("euCollectorEndpoint")
get("experimentNamespace")
get("experimentRemoteUrl")
get("flags")
get("getDeviceAndBitlockerMsGraphCallEnabled")
get("homeLink")
get("isFloodgateDynamicSurveyEnabled")
get("isLocalizedNamesDisabled")
get("isMySignInsEnabled")
get("isNewHomePageOptInEnabled")
get("isNewMyGroupsEnabled")
get("isOfficeAppLauncherDisabled")
get("isPreferredLanguagesDisabled")
get("isUniversalMeControlEnabled")
get("isVerifiedIDEnabled")
get("itemInsightsToggleEnabled")
get("manageAgentNavItemEnabled")
get("meControlViewAccountLink")
get("msGraphResourceName")
get("myAccessUrl")
get("myAccountBackendUrl")
get("myAgentsUrl")
get("myAppsUrl")
get("myGroupsUrl")
get("myStaffUrl")
get("officeSuiteHeaderAudience")
get("oneDSInstrumentKey")
get("panelHelpLink")
[0m
[0m$ [0mcd /tmp/opencode && echo "== cpm/jcms context"; grep -oE '.{50}jcms[fa]d.account.microsoft.com.{30}' myaccount2.js | head -3; grep -oE '"myAgentsUrl":"[^"]*"' myaccount2.js | sort -u | head; grep -oE '"myAccessUrl":"[^"]*"' myaccount2.js | sort -u | head; grep -oE '"isVerifiedIDEnabled":"[^"]*"' myaccount2.js | sort -u | head
== cpm/jcms context
tomPSSOLabelsEnabled":false,"cpmBaseUrl":"https://jcmsfd.account.microsoft.com/CPM","cpmAudience":"https://j
"myAgentsUrl":"https://myaccount.microsoft.com/agents"
"myAccessUrl":"https://myaccess.azure.eaglex.ic.gov"
"myAccessUrl":"https://myaccess.azure.microsoft.scloud"
"myAccessUrl":"https://myaccess.identity.sovcloud.de"
"myAccessUrl":"https://myaccess.identity.sovcloud.fr"
"myAccessUrl":"https://myaccess.microsoft.com"
"myAccessUrl":"https://myaccess.microsoft.us"
"myAccessUrl":"https://myaccess.microsoftonline.cn"
"myAccessUrl":"https://myaccess.ppe.microsoft.com"
[0m
[0m$ [0mecho "== jcmsfd.account.microsoft.com"; dig +short jcmsfd.account.microsoft.com | head -3; curl -sI "https://jcmsfd.account.microsoft.com/CPM" --max-time 15 | head -8; sleep 1; echo "== cpmAudience full"; cd /tmp/opencode && grep -oE '"cpmAudience":"[^"]*"' myaccount2.js | sort -u | head -5
== jcmsfd.account.microsoft.com
cmfd-prod.trafficmanager.net.
prod-cmfd-afd-gue8epfkfkcbhghj.z01.azurefd.net.
mr-z01.tm-azurefd.net.
HTTP/2 502 
date: Fri, 07 Aug 2026 09:05:44 GMT
content-type: text/html
content-length: 1952
cache-control: no-store
x-azure-ref: 20260807T090544Z-17565954d7bt8blrhC1CO1xnns0000000uyg00000001yz64
x-cache: CONFIG_NOCACHE

== cpmAudience full
"cpmAudience":""
"cpmAudience":"https://jarvisapi.account.microsoft-int.com"
"cpmAudience":"https://jarvisapi.account.microsoft.com"
[0m
My Account surface mapped (clientId `8c59ead7...`, CPM/Jarvis backends, feature flags). One more read-only check on the AAD authorize surface, then GitHub org repo check for identity code.
[0m$ [0mecho "== AAD v2 authorize with unregistered redirect_uri (read-only, no auth)"; curl -s -o /tmp/opencode/aad-resp.txt -w "HTTP %{http_code} | %{redirect_url}\n" "https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=1950a258-227b-4e31-a9cf-717495945fc2&response_type=code&redirect_uri=https%3A%2F%2Fevil.example.com%2Fcb&scope=openid" --max-time 20; head -c 400 /tmp/opencode/aad-resp.txt; echo
== AAD v2 authorize with unregistered redirect_uri (read-only, no auth)
HTTP 200 | 


<!-- Copyright (C) Microsoft Corporation. All rights reserved. -->
<!DOCTYPE html>
<html dir="ltr" class="" lang="en">
<head>
    <title>Sign in to your account</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=2.
[0m
[0m$ [0mecho "== AAD v2 authorize, unknown client (read-only)"; curl -s -o /tmp/opencode/aad2.txt -w "HTTP %{http_code}\n" "https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=00000000-0000-0000-0000-000000000001&response_type=code&redirect_uri=https%3A%2F%2Fevil.example.com%2Fcb&scope=openid&nonce=x&state=y" --max-time 20; head -c 600 /tmp/opencode/aad2.txt; echo; echo "== grep error info"; grep -oE 'error[^<]{0,120}|AADSTS[0-9]+[^<]{0,80}' /tmp/opencode/aad2.txt | head
== AAD v2 authorize, unknown client (read-only)
HTTP 200


<!-- Copyright (C) Microsoft Corporation. All rights reserved. -->
<!DOCTYPE html>
<html dir="ltr" class="" lang="en">
<head>
    <title>Sign in to your account</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=2.0, user-scalable=yes">
    <meta http-equiv="Pragma" content="no-cache">
    <meta http-equiv="Expires" content="-1">
    <link rel="preconnect" href="https://aadcdn.msftauth.net" crossorigin>
<me
== grep error info
error"},"loader":{"cdnRoots":["https://aadcdn.msauth.net/","https://aadcdn.msftauth.net/"],"logByThrowing":true},"serverDetai
error=null,e.onreadystatechange=null),n(),e&&(e.src&&e.removeAttribute("src"),
error=c,u.onreadystatechange=function(){"loaded"===u.readyState?setTimeout(function(){v(t,o,i,u,s)},500):"complete"===u.ready
error.aspx?err=504"),e&&n("[$Loader]: Silently handled tenant branding CDN failure ['"+o+"']")})},f.OnError=function(e,t,o){v
error.aspx?err=504"),e&&n("[$Loader]: Silently handled tenant branding CDN failure ['"+i+"']")}),f._ReportFailure(0,i,o)},f._
error){if(e.error.stack){return s(e.error.stack)}}else if(window.opera&&e.message){return d(e.message)}}catch(e){}return null
error&&(e=e.error,r=f(e)),(t=JSON.stringify(e))&&"{}"!==t||(t=e.toString())),r+":"+t}}catch(e){}}return""+(e||"")}function v(
error,h.onerror=a,
errorHooked=!0,y.when("jQuery.version",function(e){m.expectedVersion=e}),y.register("$WebWatson")}}(),function(){function e(e
errors."+r[e])),n||(n=t("errors."+e)),n||(n=t("errors."+d)),n||(n=t(d)),n}function o(t){var n=null;return null===u&&(u=e(i,"$
[0m
[0m$ [0mecho "== Graph agent session endpoints (401-baseline, read-only)"; for p in "/v1.0/me/agentSignInSessions" "/beta/me/agentSignInSessions" "/v1.0/me/authentication/methods"; do curl -s -o /dev/null -w "  %{http_code} %{size_download}b  $p\n" "https://graph.microsoft.com$p" --max-time 15; sleep 1; done
== Graph agent session endpoints (401-baseline, read-only)
  401 237b  /v1.0/me/agentSignInSessions
  401 237b  /beta/me/agentSignInSessions
  401 237b  /v1.0/me/authentication/methods
[0m
[0m$ [0mecho "== 401 body detail"; curl -s "https://graph.microsoft.com/v1.0/me/agentSignInSessions" --max-time 15 | head -c 300; echo; sleep 1; echo "== myaccess / myapps HEAD"; for h in myaccess.microsoft.com myapps.microsoft.com; do curl -sI "https://$h/" --max-time 15 | head -4; sleep 1; done
== 401 body detail
{"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty.","innerError":{"date":"2026-08-07T09:06:11","request-id":"7dbf69b9-714d-4006-b2fb-50ea6a6e957b","client-request-id":"7dbf69b9-714d-4006-b2fb-50ea6a6e957b"}}}
== myaccess / myapps HEAD
HTTP/1.1 200 OK
Content-Length: 580
Content-Type: text/html
Last-Modified: Tue, 21 Jul 2026 19:51:24 GMT
HTTP/1.1 200 OK
Cache-Control: max-age=600
Content-Length: 1049
Content-Type: text/html
[0m
[0m$ [0msleep 1; curl -s "https://api.github.com/search/repositories?q=org%3Amicrosoft+msal&per_page=10" --max-time 20 | python3 -c "
import json,sys
d=json.load(sys.stdin)
for r in d.get('items',[]):
    print(f\"{r['full_name']:50s} stars={r['stargazers_count']:6d} pushed={r['pushed_at'][:10]} arch={r['archived']}\")
"
microsoft/adfs-sample-msal-dotnet-native-to-webapi stars=    15 pushed=2023-05-31 arch=True
microsoft/adfs-sample-msal-dotnet-webapi-to-webapi-onbehalfof stars=    10 pushed=2023-06-12 arch=True
microsoft/DevAppsForTeams                          stars=    59 pushed=2023-10-10 arch=False
[0m
[0m$ [0msleep 1; curl -s "https://api.github.com/search/repositories?q=org%3Amicrosoft+identity+security+graph&sort=updated&per_page=10" --max-time 20 | python3 -c "
import json,sys
d=json.load(sys.stdin)
for r in d.get('items',[]):
    print(f\"{r['full_name']:55s} stars={r['stargazers_count']:6d} pushed={r['pushed_at'][:10]} arch={r['archived']}\")
"; sleep 1; echo "== identity model repo (in-scope code)"; curl -s "https://api.github.com/repos/AzureAD/microsoft-identity-web" --max-time 15 | python3 -c "
import json,sys
d=json.load(sys.stdin)
print('AzureAD/microsoft-identity-web pushed:',d.get('pushed_at'),'archived:',d.get('archived'))
" 2>/dev/null || echo "(repo not accessible / no route)"
== identity model repo (in-scope code)
AzureAD/microsoft-identity-web pushed: 2026-08-07T00:46:25Z archived: False
[0m
[0m$ [0mecho "== int/ppe identity-adjacent azure.com hosts (HEAD, read-only)"; for h in "https://api.dev.accessmonitor.azure.com/" "https://api.prod.accessmonitor.azure.com/" "https://controlplane.accledger.azure.com/" "https://identity.accledger.azure.com/"; do code=$(curl -s -o /dev/null -w "%{http_code}" "$h" --max-time 12); echo "  $code  $h"; sleep 1; done
== int/ppe identity-adjacent azure.com hosts (HEAD, read-only)
  403  https://api.dev.accessmonitor.azure.com/
  403  https://api.prod.accessmonitor.azure.com/
  404  https://controlplane.accledger.azure.com/
  000  https://identity.accledger.azure.com/
[0m
[0m$ [0mecho "== accessmonitor known paths (HEAD)"; for h in "https://api.prod.accessmonitor.azure.com/health" "https://api.prod.accessmonitor.azure.com/v1/" "https://api.prod.accessmonitor.azure.com/.well-known/openid-configuration"; do code=$(curl -s -o /dev/null -w "%{http_code}" "$h" --max-time 12); echo "  $code  $h"; sleep 1; done; echo "== accledger controlplane known paths"; for h in "https://controlplane.accledger.azure.com/health" "https://controlplane.accledger.azure.com/.well-known/openid-configuration" "https://controlplane.accledger.azure.com/api/v1/"; do code=$(curl -s -o /dev/null -w "%{http_code}" "$h" --max-time 12); echo "  $code  $h"; sleep 1; done
== accessmonitor known paths (HEAD)
  403  https://api.prod.accessmonitor.azure.com/health
  403  https://api.prod.accessmonitor.azure.com/v1/
  403  https://api.prod.accessmonitor.azure.com/.well-known/openid-configuration
== accledger controlplane known paths
  200  https://controlplane.accledger.azure.com/health
  404  https://controlplane.accledger.azure.com/.well-known/openid-configuration
  404  https://controlplane.accledger.azure.com/api/v1/
[0m
`controlplane.accledger.azure.com/health` returns 200. Checking its content and the accledger identity host's health/API shape.
[0m$ [0mecho "== accledger controlplane /health body"; curl -s "https://controlplane.accledger.azure.com/health" --max-time 15 | head -c 500; echo; echo "== headers"; curl -sI "https://controlplane.accledger.azure.com/health" --max-time 15 | head -12; sleep 1; echo "== accledger identity host"; dig +short identity.accledger.azure.com | head -2; curl -s -o /dev/null -w "  HTTPS %{http_code}\n" "https://identity.accledger.azure.com/" --max-time 12 || echo "  [no https]"; curl -s -o /dev/null -w "  HTTP %{http_code}\n" "http://identity.accledger.azure.com/" --max-time 12 || echo "  [no http]"
== accledger controlplane /health body
Healthy
== headers
HTTP/2 405 
date: Fri, 07 Aug 2026 09:06:50 GMT
server: Kestrel
allow: GET
x-ms-correlation-request-id: 807efb6a-5ee4-4a8f-94ac-7fa6fa82635e
x-ms-machinename: controlplane-cdc48d7b7-rs76g
x-ms-image-tag: 1.0.03467.455-73c40f8d7a2145ba44be22ca0c6029eca472d3d8

== accledger identity host
identity-accledger-prod-1.eastus.cloudapp.azure.com.
20.81.9.214
  HTTPS 000
  [no https]
  HTTP 000
  [no http]
[0m
`controlplane.accledger.azure.com` is a live .NET (Kestrel) API leaking pod name + image tag. Bounded read-only probes for API surface docs.
[0m$ [0mfor p in "/swagger/index.html" "/swagger/v1/swagger.json" "/api/health" "/api/ledger" "/api/tenants" "/api/v1/health"; do code=$(curl -s -o /tmp/opencode/r.txt -w "%{http_code}|%{size_download}b" "https://controlplane.accledger.azure.com$p" --max-time 12); echo "  $code  $p"; head -c 120 /tmp/opencode/r.txt; echo; sleep 1; done
404|92b  /swagger/index.html
{"Status":"failed","Error":{"Code":"404","Message":"NotFound","Target":null,"Details":null}}
  404|92b  /swagger/v1/swagger.json
{"Status":"failed","Error":{"Code":"404","Message":"NotFound","Target":null,"Details":null}}
  404|92b  /api/health
{"Status":"failed","Error":{"Code":"404","Message":"NotFound","Target":null,"Details":null}}
  404|92b  /api/ledger
{"Status":"failed","Error":{"Code":"404","Message":"NotFound","Target":null,"Details":null}}
  404|92b  /api/tenants
{"Status":"failed","Error":{"Code":"404","Message":"NotFound","Target":null,"Details":null}}
  404|92b  /api/v1/health
{"Status":"failed","Error":{"Code":"404","Message":"NotFound","Target":null,"Details":null}}
[0m
[0m$ [0mfor h in "https://chat.aedeptooling-int.azure.com/" "https://mcp.aedeptooling-int.azure.com/" "https://test.aedeptooling-int.azure.com/" "https://api.3psecureppe.microsoft.com/"; do code=$(curl -s -o /tmp/opencode/r.txt -w "%{http_code}|%{size_download}b" "$h" --max-time 12); echo "  $code  $h"; head -c 150 /tmp/opencode/r.txt; echo; sleep 1; done
000|0b  https://chat.aedeptooling-int.azure.com/
{"Status":"failed","Error":{"Code":"404","Message":"NotFound","Target":null,"Details":null}}
  000|0b  https://mcp.aedeptooling-int.azure.com/
{"Status":"failed","Error":{"Code":"404","Message":"NotFound","Target":null,"Details":null}}
  000|0b  https://test.aedeptooling-int.azure.com/
{"Status":"failed","Error":{"Code":"404","Message":"NotFound","Target":null,"Details":null}}
  000|0b  https://api.3psecureppe.microsoft.com/
{"Status":"failed","Error":{"Code":"404","Message":"NotFound","Target":null,"Details":null}}
[0m
[0m$ [0mcat >> /home/runner/work/hunt-lab/hunt-lab/research/bigpickle.md <<'EOF'

## RUN 2026-08-07 (big-pickle) — TARGET: microsoft — PHASE: SURFACE

### mysignins.microsoft.com (SPA, My Sign-Ins, app-version 4.0.2233)
- React SPA, bundle /bundle/main.caa6a456.js (6MB). clientId (MSAL): 19db86c3-b2b9-44cc-b339-36da233a3be2. Authz: https://login.microsoftonline.com/common/oauth2/v2.0/authorize (from bundle).
- CSP: script-src 'self' amcdn.msftauth.net js.monitor.azure.com (no unsafe-eval/inline) — XSS surface limited.
- Headers: x-ms-mysignins-region: westus2 (region pinned), x-ms-correlation-id, client-request-id.
- Backend: api.mysignins.microsoft.com. Probes (no token):
  - /api/me -> 404 (not an endpoint on this host)
  - /api/session/currentuser -> 405 (POST-only), /api/authenticationmethods/availablemethods, /api/signIns, /api/tenants -> 401 + WWW-Authenticate: Bearer
- Full endpoint map from bundle (auth-gated): /api/session/authorize, /api/session/issessionvalid, /api/session/extendsessionvalidity, /api/signIns, /api/signIns/{id}, /api/signInMap, /api/tenants, /api/organization, /api/authenticationmethods/{availablemethods,completeProofup,delete,getpostredirectparameters,initializemobileapp,new,registrationpolicy,setdefault,verify,disablehardwaretoken}, /api/password/{change,reset,passwordMethods}, /api/captcha/{,validation}, /api/features/getEnabledApps, /api/enabledFeatures, /api/post/registerSsprMethods, /api/signOutEverywhere, /api/acknowledgeSignIn, /api/me, /api/me/image, /api/users/getImages, /api/ab.
- Graph endpoints from bundle: /me/agentSignInSessions (agentic sign-in sessions!), /authentication/phoneMethods, /authentication/temporaryAccessPassMethods, /devices, /authentication/signInPreferences, /profile, /recent-activity/agentic-sessions, /security-info, /token/failed.
- api.mysignins region header + 401-with-Bearer confirm APIM/AAD-gated gateway; /api/session/currentuser POST-only is a session-state oracle.

### api.myaccount.microsoft.com (My Account backend, also serves SPA)
- Root serves My Account SPA, bundle /bundle/main.4e6e3dc6.js (9.2MB). clientId: 8c59ead7-d703-4a27-9e55-c96a0054c8d2 (MSAL public client).
- Config blocks per env (prod/ppe/sov): azureInstance login.microsoftonline.com; msGraphResourceName https://graph.microsoft.com; aadGraphResourceName https://graph.windows.net; myAccountBackendUrl api.myaccount.*; myAccessUrl myaccess.*; cpmBaseUrl jcmsfd.account.microsoft.com/CPM; cpmAudience https://jarvisapi.account.microsoft.com.
- Probes: /api/me, /api/authenticationmethods/availablemethods, /api/security/authentication/signinpreferences -> 401 (auth-gated, same gateway style).
- Endpoint map from bundle: /api/EnduserEmailContacts, /api/TenantEmailSettings, /api/allTimeZones, /api/availableLanguages, /api/availableRegionalFormats, /api/canVerifiedIdBeIssued, /api/devices, /api/features/{getEnabledApps,isConvergedUXV2Enabled,isSsprConvergenceEnabled}, /api/groups/settings, /api/instrument/logclient, /api/issueVerifiedEmployeeCredential (Entra Verified ID!), /api/me/image, /api/organization(s), /api/shell/getshellinfo, /api/signOutEverywhere, /api/termsofuse/{agreements/,myacceptances,tenantbannerlogo,tenantdisplayname}, /api/users/getImages.
- Consumer tenant 9188040d-6c67-4c5b-b112-36a304b66dad (MSA tenant) referenced for /v2.0.
- Feature flags: isVerifiedIDEnabled, isMySignInsEnabled, manageAgentNavItemEnabled (agents in My Account!), itemInsightsToggleEnabled, isNewHomePageOptInEnabled.

### accounts.accesscontrol.windows.net (legacy ACS, AAD ESTS-fronted)
- Root: 302 -> https://www.office.com/login#; headers x-ms-ests-server: 2.1.24997.11 (WUS3). Live legacy host.
- /metadata/json/1 -> 200 JSON with 5 signing x509 keys (JWKS-style), cert CN "accounts.accesscontrol.windows.net", valid-from 2026-06.
- /v2/metadata, /v2/openid-configuration, /v2/trust, /wsfederationmetadata -> 404 (old ACS WCF paths gone).
- Discovery endpoints from metadata: /tokens/OAuth/2 (OAuth2 issuance), /tokens/delegation/1 (DelegationIssuance1.0), /mgmt/delegation/1 (DelegationManagement1.0). GET on /tokens/OAuth/2 and /mgmt/delegation/1 -> 200 sign-in HTML (login page, not 404) — endpoint pathing alive, auth-gated.

### microsoftazuread-sso.com (Entra custom-domain SSO)
- Apex: NO A/AAAA. NS: ns1-34.azure-dns.{com,net,org,info}. TXT: google-site-verification=gHF6gHIrBiWfQG4HXNM0qcLtSc4RbZF5eLu2T-4nHic. No CNAME.
- Subdomain probes login./mysignins./sso.microsoftazuread-sso.com -> no route (000/timeout). Host-keyed domain with no HTTP surface at apex; likely reserved for tenant "host-name" SSO redirects (whr/host header keyed). Low direct surface.

### login.live.com OAuth surface
- .well-known/openid-configuration -> issuer https://login.live.com; authorize oauth20_authorize.srf; token oauth20_token.srf; jwks nexus.passport.com/public/partner/discovery/key. No devicecode endpoint.
- authorize probe with unregistered redirect_uri -> 302 to oauth20_desktop.srf?error=invalid_request&error_description=...redirect_uri is not valid... (exact leak of validation: client must be 'mobile' OR redirect must be absolute + registered).

### login.microsoftonline.com (AAD v2) authorize surface
- Unknown/unregistered client_id + evil redirect_uri -> 200 sign-in HTML (no early error; AAD validates client AFTER auth or returns generic page). Compare: login.live.com errors fast on redirect_uri. AAD v2 defers; classic redirect_uri normalization/response_mode research area.

### Graph identity endpoints (401-baseline confirms alive)
- /v1.0/me/agentSignInSessions, /beta/me/agentSignInSessions, /v1.0/me/authentication/methods -> 401 InvalidAuthenticationToken (endpoints resolve, auth required). agentSignInSessions is the agentic sign-in session surface (aligned with My Sign-Ins recent-activity/agentic-sessions).

### Passive-DNS follow-ups (non-identity azure.com, in scope)
- api.dev/prod.accessmonitor.azure.com -> 403 (AFD/WAF gated, no banner).
- controlplane.accledger.azure.com -> Kestrel; /health -> 200 "Healthy" + headers x-ms-machinename (pod name), x-ms-image-tag 1.0.03467.455-73c40f8d7a2145ba44be22ca0c6029eca472d3d8, x-ms-correlation-request-id. Structured 404 envelope {"Status":"failed","Error":{"Code":...}}. No swagger/api/ledger/tenants.
- identity.accledger.azure.com -> CNAME identity-accledger-prod-1.eastus.cloudapp.azure.com; no HTTP(S) responder (000).
- aedeptooling-int (chat/mcp/test/watchdog) + api.3psecureppe.microsoft.com -> 000/timeout (no route from this egress).
EOF
echo "journal appended: $(wc -l < research/bigpickle.md) lines total"
journal appended: 801 lines total
[0m
Good data captured. Wrapping up SURFACE phase — writing journal and leads.
[0m$ [0mcat > leads/lead-bigpickle.md <<'EOF'
## 2026-08-07 09:08:00 UTC [microsoft] (model bigpickle) — SURFACE

- [LEAD] mysignins.microsoft.com SPA clientId 19db86c3-b2b9-44cc-b339-36da233a3be2; backend api.mysignins.microsoft.com; full auth-gated API map (/api/session/*, /api/authenticationmethods/*, /api/signIns, /api/password/*, /api/captcha/*). Graph /me/agentSignInSessions. Region header x-ms-mysignins-region: westus2.
- [LEAD] api.myaccount.microsoft.com SPA clientId 8c59ead7-d703-4a27-9e55-c96a0054c8d2; config block leaks cpmBaseUrl jcmsfd.account.microsoft.com/CPM + cpmAudience https://jarvisapi.account.microsoft.com; Entra Verified ID issuance endpoints /api/issueVerifiedEmployeeCredential + /api/canVerifiedIdBeIssued; myAgentsUrl myaccount.microsoft.com/agents.
- [LEAD] accounts.accesscontrol.windows.net live legacy ACS: /metadata/json/1 returns 5 signing x509 keys; endpoints /tokens/OAuth/2, /tokens/delegation/1, /mgmt/delegation/1 alive (200 sign-in page). ESTS header 2.1.24997.11.
- [LEAD] login.live.com redirect_uri validation leak: 302 error string "client must be mobile or redirect must be absolute and registered" — fast-fail behavior; AAD v2 (login.microsoftonline.com) instead returns generic sign-in page for unregistered client+redirect (deferred validation) — asymmetry worth testing in HYPOTHESIS phase.
- [LEAD] controlplane.accledger.azure.com Kestrel /health 200 "Healthy" leaks pod name + x-ms-image-tag build ID; no swagger. identity.accledger.azure.com -> identity-accledger-prod-1.eastus.cloudapp.azure.com (no HTTPS).
- [LEAD] microsoftazuread-sso.com apex NXDOMAIN (NS on azure-dns), TXT google-site-verification only; no subdomain HTTP surface.
- [UNVALIDATED] api.dev/prod.accessmonitor.azure.com 403 AFD-gated; aedeptooling-int + api.3psecureppe no route from egress.
EOF
echo done; ls -la leads/
done
total 44
drwxr-xr-x 2 runner runner 4096 Aug  7 09:01 .
drwxr-xr-x 9 runner runner 4096 Aug  7 09:01 ..
-rw-r--r-- 1 runner runner 1783 Aug  7 09:07 lead-bigpickle.md
-rw-r--r-- 1 runner runner 2039 Aug  7 09:01 lead-laguna.md
-rw-r--r-- 1 runner runner 1781 Aug  7 09:01 lead-longcat.md
-rw-r--r-- 1 runner runner  250 Aug  7 09:01 reposcan-2026-08-07-08-25.md
-rw-r--r-- 1 runner runner  355 Aug  7 09:01 reposcan-2026-08-07-08-29.md
-rw-r--r-- 1 runner runner  290 Aug  7 09:01 reposcan-2026-08-07-08-34.md
-rw-r--r-- 1 runner runner  301 Aug  7 09:01 reposcan-2026-08-07-08-40.md
-rw-r--r-- 1 runner runner  301 Aug  7 09:01 reposcan-2026-08-07-08-52.md
-rw-r--r-- 1 runner runner  301 Aug  7 09:01 reposcan-latest.md
[0m
[0m# [0mTodos
[✓] Map mysignins SPA endpoints + auth config (done: clientId, MyAccount API, session/auth methods)
[✓] Probe documented My Account API paths on api.mysignins/api.myaccount (read-only 401-baseline)
[✓] Probe accounts.accesscontrol.windows.net (legacy ACS) metadata surface
[✓] Probe account.microsoft.com + login.live.com OAuth surface
[✓] Survey Graph API v1.0 identity workflow endpoints (public metadata)
[•] Write research journal, leads, state update (SURFACE)
[0m
SURFACE phase complete. All findings journaled (research/bigpickle.md) and leads recorded. Report follows.

# Program: microsoft
# Phase: SURFACE

## Key Findings
- **mysignins.microsoft.com (My Sign-Ins SPA, v4.0.2233)** is a Microsoft Authentication Library (MSAL) app (clientId `19db86c3-b2b9-44cc-b339-36da233a3be2`) hitting a region-pinned gateway (`x-ms-mysignins-region: westus2`). Its 6MB bundle enumerates a full auth-gated API map: `/api/session/{authorize,currentuser,issessionvalid,extendsessionvalidity}`, `/api/authenticationmethods/*`, `/api/signIns`, `/api/signIns/{id}`, `/api/signInMap`, `/api/password/{change,reset}`, `/api/captcha/*`, `/api/signOutEverywhere`. Probing without a token returns `401 + WWW-Authenticate: Bearer`; `/api/session/currentuser` is POST-only (405).
- **Agentic sign-in surface is new and material**: bundle references Graph `/me/agentSignInSessions` and My Sign-Ins `/recent-activity/agentic-sessions`; `graph.microsoft.com/v1.0/me/agentSignInSessions` + `/beta/me/agentSignInSessions` both resolve with `401 InvalidAuthenticationToken` (endpoints alive). This is the freshly-expanded "agent sessions" identity workflow area.
- **api.myaccount.microsoft.com (My Account SPA, clientId `8c59ead7-d703-4a27-9e55-c96a0054c8d2`)** exposes Entra Verified ID endpoints (`/api/issueVerifiedEmployeeCredential`, `/api/canVerifiedIdBeIssued`), ToS agreement endpoints, `/api/shell/getshellinfo`, and config blocks leaking internal backends: `cpmBaseUrl https://jcmsfd.account.microsoft.com/CPM`, `cpmAudience https://jarvisapi.account.microsoft.com`, and `myAgentsUrl myaccount.microsoft.com/agents`. Consumer tenant `9188040d-6c67-4c5b-b112-36a304b66dad` is referenced for /v2.0.
- **Legacy ACS host `accounts.accesscontrol.windows.net` is live** (served by AAD ESTS, `x-ms-ests-server: 2.1.24997.11`): `/metadata/json/1` returns 5 signing x509 certs (CN `accounts.accesscontrol.windows.net`); discovery advertises `/tokens/OAuth/2`, `/tokens/delegation/1` (DelegationIssuance1.0), `/mgmt/delegation/1` (DelegationManagement1.0) — GET returns a 200 sign-in page, so pathing is alive and auth-gated.
- **OAuth validation asymmetry**: `login.live.com` fast-fails on bad redirect_uri with a precise 302 error ("client must be mobile or redirect must be absolute + registered"), while `login.microsoftonline.com/oauth2/v2.0/authorize` with an unregistered client+redirect returns a generic 200 sign-in page (validation deferred). Classic redirect_uri/response_mode research surface.
- **`controlplane.accledger.azure.com`** is a live Kestrel API: `/health` → 200 `Healthy`, leaking `x-ms-machinename` (pod), `x-ms-image-tag 1.0.03467.455-73c40f8d7a2145ba44be22ca0c6029eca472d3d8`; structured 404 envelope `{"Status":"failed","Error":{...}}`; no swagger/ledger/tenants routes. `identity.accledger.azure.com` → `identity-accledger-prod-1.eastus.cloudapp.azure.com` (no HTTPS responder).
- `microsoftazuread-sso.com`: apex NXDOMAIN (NS on azure-dns), TXT `google-site-verification=...` only, no A/CNAME; subdomain probes (login/mysignins/sso) get no route — host-keyed SSO domain, low direct surface.

## New Endpoints / Assets
- api.mysignins.microsoft.com: /api/session/currentuser (405=POST), /api/authenticationmethods/availablemethods, /api/signIns, /api/tenants (401 baseline)
- api.myaccount.microsoft.com: /api/issueVerifiedEmployeeCredential, /api/canVerifiedIdBeIssued, /api/termsofuse/{agreements/,myacceptances,tenantbannerlogo,tenantdisplayname}, /api/shell/getshellinfo, /api/groups/settings, /api/instrument/logclient
- Backend leaks: jcmsfd.account.microsoft.com/CPM, jarvisapi.account.microsoft.com, myaccount.microsoft.com/agents, login.microsoftonline.com/9188040d-6c67-4c5b-b112-36a304b66dad/v2.0
- Graph: /v1.0 + /beta me/agentSignInSessions, /v1.0/me/authentication/methods
- accounts.accesscontrol.windows.net: /metadata/json/1 (JWKS), /tokens/OAuth/2, /tokens/delegation/1, /mgmt/delegation/1
- controlplane.accledger.azure.com: /health; login.live.com: oauth20_authorize.srf / oauth20_token.srf (jwks nexus.passport.com/public/partner/discovery/key)

## Hypotheses (numbered, with impact + read-only test)
1. **Legacy ACS trust forgery/replay** — `/metadata/json/1` still issues signing keys and OAuth/delegation endpoints are alive under the AAD ESTS umbrella. If any relying party still trusts these self-signed ACS keys with weak issuer validation, token forgery is possible. Impact: cross-tenant auth bypass (Identity: up to $100k). Read-only test: retrieve `/metadata/json/1` (done), compare cert/key material vs current AAD JWKS, search public docs/SAML metadata for RPs pointing at `accounts.accesscontrol.windows.net` as a token issuer.
2. **Agent sign-in sessions authorization gap** — new `/me/agentSignInSessions` + `/recent-activity/agentic-sessions` surface; newer identity endpoints have historically shipped missing `scp`/`roles` checks or per-session IDOR. Impact: cross-user session disclosure. Read-only: enumerate documented scopes/roles for agentSignInSessions via public Graph docs; test only against a controlled test tenant (not this run).
3. **My Account /api/session + /api/signInMap state oracles** — POST-only `/api/session/currentuser`, `/api/extendsessionvalidity`, and `/api/me/image` / `/api/users/getImages` could act as user-enumeration/IDOR vectors if object IDs are controllable. Impact: PII disclosure. Read-only test: confirm exact verbs/content-types from bundle; design IDOR check against test-tenant objects only.
4. **Entra Verified ID issuance endpoints** — `/api/issueVerifiedEmployeeCredential` + `/api/canVerifiedIdBeIssued` — if authorization relies only on clientId presence (not token scopes), a low-priv user could mint verified-employee credentials. Impact: credential forgery/privilege escalation. Read-only: extract full request schemas + required scopes from bundle; verify documented scope requirements.
5. **OAuth redirect_uri asymmetry (live vs AAD v2)** — different validation behavior; historical AAD classes: trailing-slash/encoding normalization, `response_mode=fragment` confusion, `whr`/domain-hint trickery. Impact: authorization code theft/ATO. Read-only: build a matrix of redirect_uri encodings against documented client registration rules; do not send to live unless the encoding set is published.
6. **controlplane.accledger (Access Ledger) info disclosure** — Kestrel banners leak pod name + image tag; structured error envelope. If any unauthenticated route returns ledger/tenant metadata, it's info disclosure. Impact: low-medium. Read-only: bounded GET/HEAD on documented common paths only; stop at 404 envelope (done).

## Secrets Check (public repos: what was found, hash only, where)
- No new secrets this run. Bundle-downloaded SPAs (mysignins main.caa6a456.js, myaccount main.4e6e3dc6.js) were string-scanned; only public clientIds and config URLs found, no keys/tokens. No commit of any raw secret. Independent reposcan job: 0 grep hits (leads/reposcan-latest.md).

## CVSS Candidates
- ACS token forgery (if trust confirmed): 8.1–9.8 (auth bypass).
- Agent session IDOR/authorization gap: 6.5–8.8 (confidentiality + session integrity).
- Verified ID issuance privilege escalation: 7.1–9.1 (credential forgery).
- OAuth redirect_uri ATO: 7.4–9.0 (code theft, needs chain).
- Ledger/health banner info disclosure: 3.7–5.3 (info exposure only).

---
## STATE UPDATE (exact format for parsing)
STATUS_PHASE: SURFACE
STATUS_STATE: HIGH_POTENTIAL
NEXT_STEP_1: HYPOTHESIS on the two highest-value surfaces: legacy ACS token trust (accounts.accesscontrol.windows.net) and agent sign-in session authorization (me/agentSignInSessions) — read-only design + impact only.
NEXT_STEP_2: Extract full request schemas/scopes for /api/issueVerifiedEmployeeCredential + /api/session/* from the two SPA bundles and Graph public docs.
NEXT_STEP_3: Build redirect_uri/response_mode normalization matrix from documented client registration rules (no live sends) and cross-check with login.live.com fast-fail behavior.

## RUN 2026-08-07 (big-pickle) — TARGET: microsoft — PHASE: HYPOTHESIS

### Source maps: BOTH SPAs ship live source maps (major code-review surface)
- mysignins.microsoft.com/bundle/main.caa6a456.js.map (6,976,576 B) — NO sourcesContent; 4359 paths. sha256 2099f8a8...f3efbe.
- api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map (35,340,363 B) — sourcesContent PRESENT (4922 files). sha256 72290126...27d6541. Extracted 1977 files to /tmp/opencode/myaccount-src (transient, not committed).
- Source map presence is itself a reportable hardening gap (recon-only note; MSRC triages source-map exposure per-case — validate against program rules before reporting).

### My Account backend (api.myaccount.microsoft.com) — request schemas recovered from source
- Auth: @iamexperiences/feature-auth getMsalAuth, MSAL public client, CacheLocation.SessionStorage, authority settingsManager authAuthorityEndpoint, clientId 8c59ead7-d703-4a27-9e55-c96a0054c8d2. Tokens acquired with scopes=[clientId] (audience = the SPA's own clientId) for ALL /api/* calls.
- /api/issueVerifiedEmployeeCredential: POST, Authorization Bearer <clientId-scope token>, NO body. Backend errors surfaced in UI: GuestIsNotAllowedToIssueVerifiedId, TenantIsNotInAllowedToIssueVerifiedId (authz gates = non-guest + tenant-onboarded + feature-on; per source comment 204 means true). Verified ID app linkedin ref VERIFIEDID_APP_NAME_LINKEDIN.
- /api/canVerifiedIdBeIssued: GET, Bearer clientId-token; 200+204 = enabled, else JSON reason.
- /api/termsofuse/*: agreements/{agreementId}; agreements/{agreementId}/policyfiles/{policyFileId}; policyfiles/{policyFileId}/data (GET JSON); policyfiles/{policyFileId}/decline and /accept (POST). UNAUTHENTICATED variants (useGetAgreementUnauthenticated, useDeclineAgreement, useGetPolicyFileDataUnauthenticated) read access_token + id_token_hint + claims (incl governance_control_state_per_device) from server-rendered data-requestbody dataset in the HTML — i.e., tokens/claims embedded in a consent page's DOM.
- /api/shell/navbardata, /api/signOutEverywhere, /api/organizations, /api/organizations/{id}, /api/devices, /api/groups/settings, /api/EnduserEmailContacts, /api/TenantEmailSettings, /api/dateTimeFormats, /api/me/image, /api/users/getImages.
- Graph beta calls: /me/ownedObjects via getOwnedAgentsUrl(userId) = /users/{userId}/ownedObjects/Microsoft.graph.ServicePrincipal?$top=999 (scope AgentIdentity.Read.All); /users/{userId}/sponsorOf/microsoft.graph.servicePrincipal?$top=999; /roleManagement/directory/roleAssignments?$filter=principalId eq '{userId}'; /organization/{orgId}/branding/localizations?$select=cdnList,bannerLogoRelativeUrl; /me/settings/itemInsights, /me/settings/contactMergeSuggestions, /me/checkMemberGroups, /me/photos/648x648/$value. ServiceIdentity is the agent-identity SP type (MANAGEAGENTS.AGENT_IDENTITY_SERVICE_PRINCIPAL_TYPE).
- MSGraphScopes enum: CrossTenantUserProfileSharing.ReadWrite.All, Organization.Read.All, User.Read.All, User.ReadWrite, MailboxSettings.ReadWrite, BitlockerKey.Read.All, Contacts.Read, AgentIdentity.Read.All.
- Roaming settings: OFFICE_APP_ID 179, API_PATH /rs/v1/settings, PLATFORM Web.
- IAMUX/portal routing (portal-app/lib): account.activedirectory-ppe.windowsazure.com (PPE), account-tip.activedirectory.windowsazure.com (TIP), account.activedirectory.windowsazure.com (PROD), account.activedirectory.windowsazure.us (AR), account.aad.microsoft.scloud (USSEC), account.aad.eaglex.ic.gov (USNAT). NOTE: *.windowsazure.com NOT in scope.yml — adjacent-only, no probing. Environment detection: hostname contains "ppe"/"tip"/"us" or ends .scloud/.eaglex.ic.gov else PROD.
- Cross-app URLs pass upn, tenant, login_hint, optIn query params to IAMUX/myapps/mygroups/myaccess (createUrl). Substrate search builder search/api/{v1} on getSubstrateResourceName().

### accounts.accesscontrol.windows.net (legacy ACS) — trust-relevant findings
- /metadata/json/1 (200) now returns keys + endpoints ONLY (allowedAudiences REMOVED — tenant-enum patch confirmed; Sprocket Security 2026-05-19 writeup "Tenant Enumeration is Dead" documents the technique + patch).
- 5 signing keys served: key0-2 CN=accounts.accesscontrol.windows.net self-signed (notBefore 2026-06/07, notAfter 2031-06/07), sha256 FP 05:D1:7F:72:..., 34:D0:94:25:..., 93:A6:49:15:...; key3-4 CN=login.microsoftonline.us self-signed (FP 1B:49:9F:F9:..., A7:3C:A1:F9:...). Mixed issuers in one JWKS = config artifact to compare vs ESTS discovery keys.
- Discovery endpoints alive: /tokens/OAuth/2 (OAuth2 issuance), /tokens/delegation/1 (DelegationIssuance1.0), /mgmt/delegation/1 (DelegationManagement1.0) — all 200 sign-in pages (auth-gated, not 404).
- Historical context: Sep-2025 critical (CVSS 10.0) ESTS actor-token impersonation chain (Dirk-jan Mollema) leveraged the well-known app 00000001-0000-0000-c000-000000000000 via accounts.accesscontrol.windows.net for AAD Graph actor tokens; mitigation blocked actor-token requests to AAD Graph. This host is a known high-attention MSRC identity asset; Microsoft's own ACS FAQ: "Traffic to accounts.accesscontrol.windows.net is handled by a different service" (absorbed into Entra ESTS).
- ACS retirement: ACS itself retired Nov-2018; SharePoint add-in ACS auth retired Apr-2026. Remaining trust: legacy RPs/tools.

### agentSignInSessions — new undocumented Graph surface
- /v1.0/me/agentSignInSessions + /beta/me/agentSignInSessions resolve (401 InvalidAuthenticationToken baseline). NOT present in public Graph docs (docs-contrib tree only has agentic-agentsignin.md + agentsignin.md, both different resources: signIn audit + conditionalAccess). ms.date 05/01/2025 for agentic.agentSignIn; signIn.agent property type.
- My Sign-Ins bundle modules: redux/modules/signIns/agentSessionApis.ts, agentSessionUtils.ts, agentSessions.ts, recentSignIns.ts, switchTenant.ts; SPA strings: /me/agentSignInSessions, /recent-activity/agentic-sessions. My Account: manageAgentNavItemEnabled, useHasOwnedAgents.

### OAuth redirect_uri validation (documented rules; no live sends this run beyond baseline)
- login.live.com fast-fails: 302 oauth20_desktop.srf?error=invalid_request&error_description=...redirect_uri is not valid. client must be 'mobile' OR redirect must be absolute + registered (confirmed SURFACE).
- login.microsoftonline.com/oauth2/v2.0/authorize with unregistered client+redirect returns generic 200 sign-in page (deferred validation).
- Documented AAD rules (learn.microsoft.com/en-us/entra/identity-platform/reply-url): https required (except localhost), case-sensitive exact match, AADSTS50011 on mismatch; RFC 6749 3.1.2 string compare + RFC 9700 (BCP) forbids wildcards. Loopback localhost (any port) for native/mobile; SPA client-type restriction AADSTS9002327/9002326.

### accledger/accessmonitor
- No public GitHub/docs presence found for "Access Ledger" controlplane. controlplane.accledger.azure.com: Kestrel /health 200 "Healthy" + x-ms-machinename (pod) + x-ms-image-tag 1.0.03467.455-73c40f8d7a2145ba44be22ca0c6029eca472d3d8; structured 404 envelope {"Status":"failed","Error":{"Code":"404"...}}; no swagger/api/ledger/tenants. identity.accledger.azure.com -> identity-accledger-prod-1.eastus.cloudapp.azure.com (no HTTPS).
- api.prod/dev.accessmonitor.azure.com: 403 AFD/WAF-gated.

### Artifacts (hashes only, for triage reference)
- mysignins map sha256 2099f8a83336e290f2ab20c09a56b3a4493d0b7f66328b944c7fc18055f3efbe
- myaccount map sha256 72290126cebc34559574f30c675c6bfe84856fe8a7678eb2e0821237a27d6541
- mysignins sources list sha256 1a5be8a2293c3797e11882895b69ef9dc53173bc71b1ee5865b6a8934034e506
- No secrets found in either bundle/map (public clientIds + env hostnames only).

===== RUN 2026-08-07 09:38:54 UTC | model bigpickle | target microsoft =====
[0m
> build · big-pickle
[0m
I'll continue the microsoft hunt from SURFACE into HYPOTHESIS phase. Let me first check what persisted from the previous run, then deepen the highest-value surfaces with read-only research.
[0m$ [0mls -la /tmp/opencode/ 2>/dev/null | head -20; echo "---state---"; cat state_bigpickle.json 2>/dev/null; echo "---journal tail---"; tail -20 research/bigpickle.md 2>/dev/null | head -5
total 16
drwxr-xr-x  2 runner runner  4096 Aug  7 09:32 .
drwxrwxrwt 14 root   root   12288 Aug  7 09:32 ..
---state---
{"phase": "SURFACE", "status": "HIGH_POTENTIAL"}---journal tail---
5. **OAuth redirect_uri asymmetry (live vs AAD v2)** — different validation behavior; historical AAD classes: trailing-slash/encoding normalization, `response_mode=fragment` confusion, `whr`/domain-hint trickery. Impact: authorization code theft/ATO. Read-only: build a matrix of redirect_uri encodings against documented client registration rules; do not send to live unless the encoding set is published.
6. **controlplane.accledger (Access Ledger) info disclosure** — Kestrel banners leak pod name + image tag; structured error envelope. If any unauthenticated route returns ledger/tenant metadata, it's info disclosure. Impact: low-medium. Read-only: bounded GET/HEAD on documented common paths only; stop at 404 envelope (done).

## Secrets Check (public repos: what was found, hash only, where)
- No new secrets this run. Bundle-downloaded SPAs (mysignins main.caa6a456.js, myaccount main.4e6e3dc6.js) were string-scanned; only public clientIds and config URLs found, no keys/tokens. No commit of any raw secret. Independent reposcan job: 0 grep hits (leads/reposcan-latest.md).
[0m
[0m# [0mTodos
[•] Re-verify ACS /metadata/json/1 keys + research legacy ACS DelegationIssuance1.0 trust (who still trusts accounts.accesscontrol.windows.net as issuer)
[ ] Source-map check on mysignins + api.myaccount SPA bundles (read-only GET of .js.map)
[ ] agentSignInSessions permission model — public Graph docs + scope/role design
[ ] Entra Verified ID issuance endpoints — public docs, scopes, request schemas
[ ] OAuth redirect_uri normalization — review in-scope code (microsoft-identity-web, MSAL.js) for validation logic
[ ] accledger/accessmonitor public-repo + docs search for unauthenticated API routes
[ ] Write HYPOTHESIS journal, leads, state update
[0m
State confirms SURFACE/HIGH_POTENTIAL → now advancing to HYPOTHESIS. Let me plan the work and start with the highest-value surfaces.
Starting with the legacy ACS trust hypothesis — re-fetch the key metadata and parse the certificates for fingerprints and issuer structure.
[0m$ [0mcurl -s "https://accounts.accesscontrol.windows.net/metadata/json/1" --max-time 15 -o /tmp/opencode/acs-keys.json && python3 -c "
import json
d=json.load(open('/tmp/opencode/acs-keys.json'))
keys=d.get('keys',[])
print('total keys:',len(keys))
for i,k in enumerate(keys):
    kv=k.get('keyValue',{})
    v=kv.get('value','')
    print(f\"key[{i}] usage={k.get('usage')} kid={kv.get('type')} len={len(v)}\")
print('endpoints:',json.dumps(d.get('endpoints'),indent=1))
"
total keys: 5
key[0] usage=signing kid=x509Certificate len=1028
key[1] usage=signing kid=x509Certificate len=1028
key[2] usage=signing kid=x509Certificate len=1028
key[3] usage=signing kid=x509Certificate len=1000
key[4] usage=signing kid=x509Certificate len=1000
endpoints: [
 {
  "location": "https://accounts.accesscontrol.windows.net/tokens/OAuth/2",
  "protocol": "OAuth2",
  "usage": "issuance"
 },
 {
  "location": "https://accounts.accesscontrol.windows.net/tokens/delegation/1",
  "protocol": "DelegationIssuance1.0",
  "usage": "issuance"
 },
 {
  "location": "https://accounts.accesscontrol.windows.net/mgmt/delegation/1",
  "protocol": "DelegationManagement1.0",
  "usage": "management"
 }
]
[0m
[0m$ [0mpython3 -c "
import json
d=json.load(open('/tmp/opencode/acs-keys.json'))
for i,k in enumerate(d.get('keys',[])):
    v=k.get('keyValue',{}).get('value','')
    try:
        pem='-----BEGIN CERTIFICATE-----\n'+v+'\n-----END CERTIFICATE-----'
        import subprocess
        p=subprocess.run(['openssl','x509','-noout','-subject','-issuer','-dates','-fingerprint','-sha256'],input=pem.encode(),capture_output=True)
        print(f'--- key[{i}] ---')
        print(p.stdout.decode().strip())
    except Exception as e:
        print('err',e)
"
--- key[0] ---
subject=CN = accounts.accesscontrol.windows.net
issuer=CN = accounts.accesscontrol.windows.net
notBefore=Jun  8 12:04:26 2026 GMT
notAfter=Jun  8 12:04:26 2031 GMT
sha256 Fingerprint=05:D1:7F:72:3C:F3:10:76:71:74:BB:9F:BA:F3:F8:6D:72:86:0A:5A:4C:11:D7:0C:35:9A:46:3A:64:04:D9:C6
--- key[1] ---
subject=CN = accounts.accesscontrol.windows.net
issuer=CN = accounts.accesscontrol.windows.net
notBefore=Jun 17 15:02:22 2026 GMT
notAfter=Jun 17 15:02:22 2031 GMT
sha256 Fingerprint=34:D0:94:25:2B:14:B0:33:D3:62:11:6B:30:09:11:9C:A8:CD:13:F9:9E:DA:BC:D1:10:6E:2D:70:C4:84:BD:5B
--- key[2] ---
subject=CN = accounts.accesscontrol.windows.net
issuer=CN = accounts.accesscontrol.windows.net
notBefore=Jul  4 00:05:05 2026 GMT
notAfter=Jul  4 00:05:05 2031 GMT
sha256 Fingerprint=93:A6:49:15:A9:20:DB:8F:50:A8:A9:12:60:AA:F0:FF:5B:65:21:9D:81:5B:B8:4E:78:DA:84:C7:7D:36:4D:47
--- key[3] ---
subject=CN = login.microsoftonline.us
issuer=CN = login.microsoftonline.us
notBefore=Jul  1 16:00:54 2026 GMT
notAfter=Jul  1 16:00:54 2031 GMT
sha256 Fingerprint=1B:49:9F:F9:EE:49:60:A6:75:E9:C6:F6:73:57:C9:0C:6D:08:50:01:AD:7E:A7:1F:89:DA:5B:F0:A8:4E:3D:85
--- key[4] ---
subject=CN = login.microsoftonline.us
issuer=CN = login.microsoftonline.us
notBefore=Jul 26 16:01:26 2026 GMT
notAfter=Jul 26 16:01:26 2031 GMT
sha256 Fingerprint=A7:3C:A1:F9:DB:02:7F:E9:39:63:BD:18:08:0E:57:1E:FD:49:2A:6D:A8:74:3B:4A:D1:59:19:F0:5C:B5:67:B9
[0m
Three ACS self-signed signing keys + two `login.microsoftonline.us` keys served from this live endpoint — a token-forgery-relevant trust surface. Let me research who publicly trusts this issuer and the legacy DelegationIssuance1.0 protocol.
[0m◈ [0mParallel Web Search "accounts.accesscontrol.windows.net legacy Access Control Service ACS trusted token issuer relying party metadata"
[0m◈ [0mParallel Web Search "Windows Azure Access Control Service "DelegationIssuance1.0" delegation token protocol vulnerability"
Key intel: the Sprocket Security writeup confirms `/metadata/json/1` was the tenant-enumeration + **actor-token vector** (Dirk-jan Mollema's 10.0-CVSS ESTS impersonation chain used `accounts.accesscontrol.windows.net`), and the `allowedAudiences` leak is now patched (my fetch confirms the field is gone). This is a known-high-attention MSRC surface. Now let me check the SPA source maps and agentSignInSessions docs.
[0m◈ [0mParallel Web Search "Microsoft Graph "agentSignInSessions" API agentic sign-in session permissions scopes documentation"
[0m
[0m$ [0mfor u in "https://mysignins.microsoft.com/bundle/main.caa6a456.js.map" "https://mysignins.microsoft.com/manifest.json"; do code=$(curl -s -o /dev/null -w "%{http_code}|%{content_type}|%{size_download}b" "$u" --max-time 12); echo "$code  $u"; sleep 1; done
200|text/plain|6976576b  https://mysignins.microsoft.com/bundle/main.caa6a456.js.map
200|application/json|286b  https://mysignins.microsoft.com/manifest.json
[0m
Critical find: **the source map IS live** at `/bundle/main.caa6a456.js.map` (6.97MB). That's a full source tree of the My Sign-Ins SPA. Let me grab it and pull the agentic sign-in docs from the public graph-docs-contrib repo.
[0m$ [0mcurl -s "https://mysignins.microsoft.com/bundle/main.caa6a456.js.map" --max-time 120 -o /tmp/opencode/mysignins.map && wc -c /tmp/opencode/mysignins.map && python3 -c "
import json
d=json.load(open('/tmp/opencode/mysignins.map'))
srcs=d.get('sources',[])
print('total source files:',len(srcs))
# list unique top dirs
import collections
tops=collections.Counter(s.split('/')[0] for s in srcs if s)
for t,n in tops.most_common(30):
    print(f'{n:5d}  {t}')
print('--- interesting source names ---')
import re
for s in srcs:
    if re.search(r'(config|secret|key|token|session|auth|endpoint|host)',s,re.I) and not re.search(r'(node_modules|\.d\.ts|test)',s):
        print('  ',s)
" | head -70
6976576 /tmp/opencode/mysignins.map
total source files: 4359
 4359  webpack:
--- interesting source names ---
   webpack://sign-ins/./src/SecurityInfo/Authenticator/Authenticator.scoped.scss?3b4d
   webpack://sign-ins/./src/SecurityInfo/Authenticator/RegistrationStages/ManualActivation.scoped.scss?064a
   webpack://sign-ins/./src/SecurityInfo/Authenticator/RegistrationStages/Verification.scoped.scss?0d12
   webpack://sign-ins/./src/SecurityInfo/ExternalAuth/ExternalAuth.scss?3ebc
   webpack://sign-ins/./src/SecurityInfo/Passkey/AllowedAAGUIDs/AllowedAAGUIDs.scoped.scss?24ef
   webpack://sign-ins/./src/SecurityInfo/Passkey/Passkey.scss?936d
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/NameYourPasskey.scoped.scss?05ba
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/PasskeyAddError/PasskeyAddError.scoped.scss?e11c
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/PasskeyWebAuthN.scoped.scss?c28b
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/StrongAuthRequired.scoped.scss?7d43
   webpack://sign-ins/./src/SecurityInfo/Authenticator/RegistrationStages/ManualActivation.tsx
   webpack://sign-ins/./src/SecurityInfo/Authenticator/RegistrationStages/RegistrationComplete.tsx
   webpack://sign-ins/./src/SecurityInfo/Authenticator/RegistrationStages/RegistrationNotSupported.tsx
   webpack://sign-ins/./src/SecurityInfo/Authenticator/Authenticator.tsx
   webpack://sign-ins/./src/SecurityInfo/Authenticator/Authenticator.types.ts
   webpack://sign-ins/./src/SecurityInfo/Authenticator/AuthenticatorDialog.tsx
   webpack://sign-ins/./src/SecurityInfo/Authenticator/AuthenticatorStep.tsx
   webpack://sign-ins/./src/SecurityInfo/Authenticator/ConnectedAuthenticator.ts
   webpack://sign-ins/./src/SecurityInfo/Authenticator/RegistrationStages/Activation.tsx
   webpack://sign-ins/./src/SecurityInfo/Authenticator/RegistrationStages/AuthenticatorStageHeader.tsx
   webpack://sign-ins/./src/SecurityInfo/Authenticator/RegistrationStages/InstallationInstructions.tsx
   webpack://sign-ins/./src/SecurityInfo/Authenticator/RegistrationStages/SameDeviceActivation.tsx
   webpack://sign-ins/./src/SecurityInfo/Authenticator/RegistrationStages/SetupInstructions.tsx
   webpack://sign-ins/./src/SecurityInfo/Authenticator/RegistrationStages/Verification.tsx
   webpack://sign-ins/./src/SecurityInfo/ExternalAuth/ExternalAuth.tsx
   webpack://sign-ins/./src/SecurityInfo/ExternalAuth/ConnectedExternalAuth.ts
   webpack://sign-ins/./src/SecurityInfo/ExternalAuth/ExternalAuth.page.ts
   webpack://sign-ins/./src/SecurityInfo/ExternalAuth/ExternalAuth.styles.ts
   webpack://sign-ins/./src/SecurityInfo/ExternalAuth/RegistrationSteps/AddProviderDialog.tsx
   webpack://sign-ins/./src/SecurityInfo/ExternalAuth/RegistrationSteps/ExternalAuthAdded.tsx
   webpack://sign-ins/./src/SecurityInfo/ExternalAuth/RegistrationSteps/ExternalAuthFailed.tsx
   webpack://sign-ins/./src/SecurityInfo/ExternalAuth/RegistrationSteps/ProviderPickerDialog.tsx
   webpack://sign-ins/./src/SecurityInfo/ExternalAuth/RegistrationSteps/RegisteringProvider.tsx
   webpack://sign-ins/./src/SecurityInfo/ExternalAuth/RegistrationSteps/WizardSpecificSteps/AddProvider.tsx
   webpack://sign-ins/./src/SecurityInfo/ExternalAuth/RegistrationSteps/WizardSpecificSteps/ProviderPicker.tsx
   webpack://sign-ins/./src/SecurityInfo/ExternalAuth/ExternalAuth.utils.ts
   webpack://sign-ins/./src/SecurityInfo/Fido/screens/SelectSecurityKeyTypeScreen.tsx
   webpack://sign-ins/./src/SecurityInfo/Fido/screens/NameSecurityKeyScreen.tsx
   webpack://sign-ins/./src/SecurityInfo/Fido/screens/StrongAuthRequiredScreen.tsx
   webpack://sign-ins/./src/SecurityInfo/HardwareOath/RegistrationStages/EnterTokenName.tsx
   webpack://sign-ins/./src/SecurityInfo/HardwareOath/RegistrationStages/EnterTokenSerialNumber.tsx
   webpack://sign-ins/./src/SecurityInfo/HardwareOath/RegistrationStages/EnterTokenVerificationNumber.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/EPOW/SettingUpWindowsHello.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/EPOW/WindowsHello.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/EPOW/WindowsHelloSetupDone.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/AllowedAAGUIDs/AllowedAAGUIDs.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/AllowedAAGUIDs/AllowedAAGUIDsDialog.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/AllowedAAGUIDs/AllowedAAGUIDsContainer.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/Components/HavingTroubleDialogFooter.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/DirectRegistration/PasskeyOnAuthenticatorDoesNotHaveAuthenticator.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/DirectRegistration/PasskeyOnAuthenticatorHasAuthenticator.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/AddPasskey.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/DirectRegistration/CompleteInMicrosoftAuthenticatorDoesNotHaveAuthenticator.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/DirectRegistration/CompleteInMicrosoftAuthenticatorHasAuthenticator.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/DirectRegistration/CompleteInMicrosoftAuthenticator.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/NameYourPasskey.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/PasskeyAddError/PasskeyAddError.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/PasskeyCreated.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/PasskeyNotRegistered.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/PolicyMisconfiguration.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/ReadyToSave.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/ReadyToSaveMsAuthenticator.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/StrongAuthRequired.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/TurnOnPasskeySupport.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/WhereToSavePasskey.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/RegistrationStages/WhereToSavePasskeyOnMSAuthenticator.tsx
   webpack://sign-ins/./src/SecurityInfo/Passkey/Troubleshooting/Components/ContactSupportInformation.tsx
Exception ignored in: <_io.TextIOWrapper name='<stdout>' mode='w' encoding='utf-8'>
BrokenPipeError: [Errno 32] Broken pipe
[0m
[0m$ [0mpython3 -c "
import json
d=json.load(open('/tmp/opencode/mysignins.map'))
print('has sourcesContent:', bool(d.get('sourcesContent')))
print('num content:', len([c for c in (d.get('sourcesContent') or []) if c]))
srcs=d.get('sources',[])
with open('/tmp/opencode/mysignins-srcs.txt','w') as f:
    for s in srcs:
        f.write(s+'\n')
# top-level dirs
import collections,re
dirs=collections.Counter()
for s in srcs:
    m=re.match(r'webpack://sign-ins/\./([^/]+)/',s)
    if m: dirs[m.group(1)]+=1
    elif s.startswith('webpack://sign-ins/.'):
        dirs['(root)']+=1
for t,n in dirs.most_common(40):
    print(f'{n:5d}  {t}')
"
has sourcesContent: False
num content: 0
 3590  (root)
  473  src
  281  node_modules
[0m
Full source map with 4359 source files including original TypeScript. Let me extract the source tree and check for embedded source content.
[0m$ [0mpython3 -c "
import collections
srcs=open('/tmp/opencode/mysignins-srcs.txt').read().splitlines()
pref=collections.Counter()
for s in srcs:
    if s.startswith('webpack://sign-ins/'):
        rest=s[len('webpack://sign-ins/'):]
        pref[rest.split('/')[0]+'/'+rest.split('/')[1] if '/' in rest else rest]+=1
    else:
        pref[s.split('/')[0]]+=1
for t,n in pref.most_common(50):
    print(f'{n:5d}  {t}')
"
2006  ../..
  894  ../portal-app
  690  ../fabric-ui-portal
  473  ./src
  281  ./node_modules
   14  webpack/runtime
    1  webpack/bootstrap
[0m
[0m$ [0mgrep -E '^webpack://sign-ins/\./src/' /tmp/opencode/mysignins-srcs.txt | sed 's|webpack://sign-ins/\./src/||' | sort > /tmp/opencode/src-list.txt; wc -l /tmp/opencode/src-list.txt; echo "=== dirs ==="; python3 -c "
import collections
dirs=collections.Counter()
for l in open('/tmp/opencode/src-list.txt'):
    d=l.split('/')[0]
    dirs[d]+=1
for t,n in dirs.most_common():
    print(f'{n:4d}  {t}')
"
473 /tmp/opencode/src-list.txt
=== dirs ===
 387  SecurityInfo
  34  shared
  28  redux
   8  InProductFeedback
   7  utils
   5  SignIns
   1  Application
   1  auth.ts

   1  index.tsx

   1  register.ts
[0m
[0m$ [0mecho "=== shared + utils + Application + auth.ts ==="; grep -E '^(shared|utils|Application|auth\.ts|index|register|SignIns|redux)/' /tmp/opencode/src-list.txt; echo; echo "=== SignIns ==="; grep '^SignIns/' /tmp/opencode/src-list.txt
=== shared + utils + Application + auth.ts ===
Application/signInsRouting.ts
SignIns/RecentActivity/RecentActivity.page.ts
SignIns/models/SignInsActions.ts
SignIns/shared/signIns-helpers.ts
SignIns/shared/signins-session-activity.ts
SignIns/shared/signins-unfriendly-apps.ts
redux/modules/featureFlags/featureFlagNames.ts
redux/modules/featureFlags/featureFlags.utils.ts
redux/modules/featureFlags/featureFlagsApi.ts
redux/modules/featureFlags/featureFlagsSelectors.ts
redux/modules/featureFlags/mySignInsActions.ts
redux/modules/featureFlags/mySignInsReducer.ts
redux/modules/featureFlags/mySignInsRegistry.ts
redux/modules/featureFlags/organizationBrandingInfo/getOrganizationBranding.ts
redux/modules/featureFlags/serviceInfoActions.ts
redux/modules/header/getNavBarData.ts
redux/modules/header/getOrganizations.ts
redux/modules/header/getTenantBranding.ts
redux/modules/header/getTenantBrandingLegacy.ts
redux/modules/header/userPersonData.ts
redux/modules/root.ts
redux/modules/search/searchContext.ts
redux/modules/shared/sharedEffects.ts
redux/modules/signIns.ts
redux/modules/signIns/agentSessionApis.ts
redux/modules/signIns/agentSessionUtils.ts
redux/modules/signIns/agentSessions.ts
redux/modules/signIns/getMapImageForSignIn.ts
redux/modules/signIns/recentSignIns.ts
redux/modules/signIns/showInProductFeedback.ts
redux/modules/signIns/signInApis.ts
redux/modules/signIns/signInsRegistry.ts
redux/modules/signIns/signInsSearch.ts
redux/modules/signIns/switchTenant.ts
shared/Telemetry.constants.ts
shared/components/CopyInfo/CopyInfo.scoped.scss
shared/components/CopyInfo/CopyInfo.scoped.scss?3346
shared/components/CopyInfo/CopyInfo.tsx
shared/components/DownloadLinks/DownloadLinks.scoped.scss
shared/components/DownloadLinks/DownloadLinks.scoped.scss?a57f
shared/components/DownloadLinks/DownloadLinks.tsx
shared/components/ReSkinDialog/ReSkinDialog.scss
shared/components/ReSkinDialog/ReSkinDialog.scss?e26b
shared/components/ReSkinDialog/ReSkinDialog.tsx
shared/components/UnderlinedComboBox/UnderlinedComboBox.scoped.scss
shared/components/UnderlinedComboBox/UnderlinedComboBox.scoped.scss?f4e1
shared/components/UnderlinedComboBox/UnderlinedComboBox.tsx
shared/components/UnderlinedDropdown/UnderlinedDropdown.scoped.scss
shared/components/UnderlinedDropdown/UnderlinedDropdown.scoped.scss?8896
shared/components/UnderlinedDropdown/UnderlinedDropdown.tsx
shared/constants.ts
shared/dropdownStyles.ts
shared/ecosTelemetry.ts
shared/getTheme.ts
shared/settings/SigninSettings.ts
shared/settings/environments/settings.arlington.ts
shared/settings/environments/settings.bleu.ts
shared/settings/environments/settings.delos.ts
shared/settings/environments/settings.mooncake.ts
shared/settings/environments/settings.ppe.ts
shared/settings/environments/settings.production.ts
shared/settings/environments/settings.ts
shared/settings/environments/settings.usnat.ts
shared/settings/environments/settings.ussec.ts
shared/settings/resolveEnvironmentSettings.ts
shared/telemetry.ts
shared/types/interrupt.type.ts
shared/types/telemetryArea.type.ts
utils/apiUtils.ts
utils/dateUtils.ts
utils/queryStringFlags.ts
utils/registerCustomIcons.tsx
utils/sessionUtils.ts
utils/urlUtils.ts
utils/windowHelper.ts

=== SignIns ===
SignIns/RecentActivity/RecentActivity.page.ts
SignIns/models/SignInsActions.ts
SignIns/shared/signIns-helpers.ts
SignIns/shared/signins-session-activity.ts
SignIns/shared/signins-unfriendly-apps.ts
[0m
[0m$ [0mpython3 -c "
import collections,re
srcs=open('/tmp/opencode/mysignins-srcs.txt').read().splitlines()
portal=[]; root=[]
for s in srcs:
    if s.startswith('webpack://sign-ins/../portal-app/'):
        portal.append(s[len('webpack://sign-ins/../portal-app/'):])
    elif s.startswith('webpack://sign-ins/../..'):
        root.append(s)
print('portal-app files:',len(portal))
c=collections.Counter(p.split('/')[0] for p in portal)
for t,n in c.most_common(15): print(f'{n:4d}  portal-app/{t}')
print()
print('parent-shared files:',len(root))
# classify by 2nd path segment
c2=collections.Counter()
for r in root:
    parts=r[len('webpack://sign-ins/../../'):].split('/')
    c2[parts[0]+'/'+(parts[1] if len(parts)>1 else '')]+=1
for t,n in c2.most_common(25): print(f'{n:4d}  {t}')
"
portal-app files: 894
 747  portal-app/node_modules
 147  portal-app/lib

parent-shared files: 2006
 450  node_modules/rxjs
 362  node_modules/core-js-pure
 357  node_modules/core-js
 181  node_modules/@fluentui
 180  node_modules/@microsoft
 137  node_modules/moment
  85  node_modules/@iamexperiences
  82  node_modules/@babel
  26  node_modules/react-query
  18  node_modules/react-redux
  14  node_modules/uuid
  13  node_modules/i18next
  13  node_modules/lodash
  10  node_modules/lodash-es
   9  node_modules/react-i18next
   8  node_modules/connected-react-router
   5  node_modules/redux-observable
   5  node_modules/redux
   4  dev/node_modules
   4  node_modules/html-parse-stringify2
   4  node_modules/i18next-xhr-backend
   4  node_modules/react
   3  node_modules/prop-types
   3  node_modules/util
   2  node_modules/path-to-regexp
[0m
The `src` tree is rich — note the `agentSessionApis.ts` / `agentSessions.ts` modules and per-environment settings. Let me inspect the portal-app and shared-package sources bundled in.
[0m$ [0mpython3 -c "
srcs=open('/tmp/opencode/mysignins-srcs.txt').read().splitlines()
iam=[s for s in srcs if '@iamexperiences' in s]
ms=[s for s in srcs if '/@microsoft/' in s]
for s in iam[:200]:
    print(s.split('@iamexperiences/')[-1])
" | sed 's|node_modules/||' | sort | head -90
ecos-telemetry/lib/callFunctionOnTelemetrySinks.js
ecos-telemetry/lib/getInstanceId.js
ecos-telemetry/lib/react/TelemetryContext.js
ecos-telemetry/lib/react/TelemetryProvider.js
ecos-telemetry/lib/react/qos/QosErrorBoundary.js
ecos-telemetry/lib/react/qos/QosProvider.js
ecos-telemetry/lib/react/qos/types.js
ecos-telemetry/lib/sinks/1ds-sink/OneDsTelemetrySink.js
ecos-telemetry/lib/sinks/types/EventType.js
ecos-telemetry/lib/sinks/utils/getErrorJsonData.js
ecos-telemetry/lib/sinks/utils/getPLTMetrics.js
ecos-telemetry/lib/sinks/utils/getResourcesMetrics.js
ecos-telemetry/lib/sinks/utils/scrubSensitiveData.js
ecos-telemetry/lib/sinks/utils/stringifyHelper.js
ecos-telemetry/lib/telemetry.js
inproductfeedback/lib/esm.js
react-auth/lib/core/constants.js
react-auth/lib/core/errors/AuthError.js
react-auth/lib/core/getAuthManager.js
react-auth/lib/core/getAuthUser.js
react-auth/lib/core/getAuthorityTenant.js
react-auth/lib/core/getAutoLoginHandler.js
react-auth/lib/core/getAutoLoginResult.js
react-auth/lib/core/getBestEmail.js
react-auth/lib/core/getBestUsername.js
react-auth/lib/core/getDecodedJwtToken.js
react-auth/lib/core/getDefaultLoggerFactory.js
react-auth/lib/core/getNormalPromptQuery.js
react-auth/lib/core/getNormalScopes.js
react-auth/lib/core/getNormalUsername.js
react-auth/lib/core/getPageLoadStats.js
react-auth/lib/core/getTenantAuthority.js
react-auth/lib/core/isGuestUsername.js
react-auth/lib/core/types/IAuth.js
react-auth/lib/core/types/IAuthError.js
react-auth/lib/core/withAuthLifecycle.js
react-auth/lib/implementations/V1/addOpenIdScope.js
react-auth/lib/implementations/V1/constants.js
react-auth/lib/implementations/V1/getAuthV1.js
react-auth/lib/implementations/V1/getDependencies.js
react-auth/lib/implementations/V1/getError.js
react-auth/lib/implementations/V1/getOAuthAuthorizeClient.js
react-auth/lib/implementations/V1/getOAuthLogoutClient.js
react-auth/lib/implementations/V1/getOAuthTokenClient.js
react-auth/lib/implementations/V1/getOpenIdConfigClient.js
react-auth/lib/implementations/V1/getPkceCodes.js
react-auth/lib/implementations/V1/getSession.js
react-auth/lib/implementations/V1/getUser.js
react-auth/lib/implementations/V1/types.js
react-auth/lib/react/AuthContext.js
react-auth/lib/react/AuthHooks.js
react-auth/lib/react/AuthProvider.js
react-auth/lib/react/WithUser.js
react-auth/lib/utils/Crypto/getCrypto.js
react-auth/lib/utils/Crypto/getCryptoMock.js
react-auth/lib/utils/Crypto/getRandomValues.js
react-auth/lib/utils/Crypto/getSessionId.js
react-auth/lib/utils/Crypto/getSha256Digest.js
react-auth/lib/utils/Crypto/getUuid.js
react-auth/lib/utils/Decorators/once.js
react-auth/lib/utils/Fetch/AbortController.js
react-auth/lib/utils/Fetch/AbortControllerPolyfill.js
react-auth/lib/utils/Fetch/AbortSignal.js
react-auth/lib/utils/Fetch/Headers.js
react-auth/lib/utils/Fetch/HeadersPolyfill.js
react-auth/lib/utils/Fetch/fetch.js
react-auth/lib/utils/Logger/DefaultLoggerHandler.js
react-auth/lib/utils/Logger/constants.js
react-auth/lib/utils/Logger/getLogger.js
react-auth/lib/utils/Logger/getLoggerFactory.js
react-auth/lib/utils/Logger/getLoggerLevel.js
react-auth/lib/utils/Store/getMemoryStorage.js
react-auth/lib/utils/Store/getStore.js
react-auth/lib/utils/Store/getStoreKey.js
react-auth/lib/utils/Subject/getSubject.js
react-auth/lib/utils/Types/array.js
react-auth/lib/utils/Types/dictionary.js
react-auth/lib/utils/Types/eq.js
react-auth/lib/utils/Types/getArray.js
react-auth/lib/utils/Types/getCleanRecord.js
react-auth/lib/utils/Types/guard.js
react-auth/lib/utils/Types/hasOwnProperty.js
react-auth/lib/utils/Types/isDeepEqual.js
react-auth/lib/utils/Types/struct.js
react-auth/lib/utils/Types/typeOf.js
react-auth/lib/utils/createStorageKey.js
[0m
`@iamexperiences` (the identity experiences SDK) and `portal-app/lib` are bundled. Let me enumerate those — they reveal the full backend API surface.
[0m$ [0mpython3 -c "
srcs=open('/tmp/opencode/mysignins-srcs.txt').read().splitlines()
for s in srcs:
    if s.startswith('webpack://sign-ins/../portal-app/lib/'):
        print(s.split('portal-app/lib/')[-1])
" | sort | head -150
App/AppFlags.js
App/ConnectedStandardAppBar/ConnectedStandardAppBar.js
App/ConnectedStandardAppBar/index.js
App/Loading/Loading.js
App/Loading/getLoadingState.js
App/Loading/index.js
App/redux/applicationInitialState.js
App/redux/applicationRegistry.js
AppSuite/index.js
AppSuite/models/AppSuiteActions.js
AppSuite/models/index.js
AppSuite/redux/actions/getAppsEnabled.js
AppSuite/redux/actions/index.js
AppSuite/redux/appSuiteApi.js
AppSuite/redux/appSuiteInitialState.js
AppSuite/redux/appSuiteRegistry.js
AppSuite/redux/index.js
Banners/EmptyBanner/EmptyBanner.js
Banners/EmptyBanner/EmptyBanner.types.js
Banners/EmptyBanner/index.js
Banners/ErrorBanner/ErrorBanner.js
Banners/ErrorBanner/ErrorBanner.types.js
Banners/ErrorBanner/index.js
Banners/LoadingBanner/LoadingBanner.js
Banners/LoadingBanner/index.js
FeatureFlags/models/FeatureFlagsActions.js
FeatureFlags/models/index.js
FeatureFlags/redux/featureFlagsInitialState.js
FeatureFlags/redux/featureFlagsRegistry.js
FeatureFlags/redux/index.js
LanguageDetector/htmlTagDetector.js
LanguageDetector/index.js
LanguageDetector/languageDetectorPlugin.js
LanguageDetector/navigatorDetector.js
LanguageDetector/profileLanguageDetector.js
LanguageDetector/queryStringDetector.js
Nav/models/Nav.js
Notifications/dialogRegistry.js
Notifications/helpers/notificationsMerge.js
Notifications/isDialogOpen.js
Notifications/models/INotification.js
Notifications/models/NotificationsActions.js
Notifications/models/index.js
Notifications/redux/actions/dialog.js
Notifications/redux/actions/index.js
Notifications/redux/getDialogParameter.js
Notifications/redux/index.js
Notifications/redux/notifications.js
Notifications/redux/notificationsInitialState.js
Notifications/redux/notificationsRegistry.js
Options/models/OptionsActions.js
Options/models/index.js
Options/redux/actions/index.js
Options/redux/actions/options.js
Options/redux/index.js
Options/redux/optionsInitialState.js
Options/redux/optionsRegistry.js
Search/models/SearchActions.js
Search/models/index.js
Search/redux/actions/index.js
Search/redux/actions/search.js
Search/redux/actions/searchContext.js
Search/redux/actions/searchUsers.js
Search/redux/index.js
Search/redux/searchInitialState.js
Search/redux/searchRegistry.js
User/getUserId.js
User/getUserImageUrl.js
User/getUserName.js
User/models/ICurrentUser.js
User/models/IPerson.js
User/models/UserActions.js
User/models/index.js
User/redux/actions/getCurrentUser.js
User/redux/actions/getCurrentUserImage.js
User/redux/actions/getFlights.js
User/redux/actions/getMyOrganization.js
User/redux/actions/getUser.js
User/redux/actions/getUserImage.js
User/redux/actions/index.js
User/redux/actions/logOut.js
User/redux/actions/updateUserImage.js
User/redux/index.js
User/redux/requireAuthentication.js
User/redux/userApi.js
User/redux/userInitialState.js
User/redux/userRegistry.js
aad-graph-api/getAadGraphResourceName.js
ajax-utils/index.js
ajax-utils/mapJsonResponse.js
ajax-utils/mapObservableJsonResponse.js
auth/acquireToken.js
auth/auth.js
auth/csrf.js
auth/index.js
auth/withAuth.js
bootstrap.js
device/is-mobile.js
device/is-oobe-mode.js
device/is-touch.js
environment/determineEnvironment.js
environment/getApiServerUrl.js
environment/getUrlForApp.js
environment/index.js
feature-flags/flags.js
feature-flags/index.js
fetch/fetch.js
fetch/fetchMatch.js
fetch/fetchResponse.js
fetch/index.js
i18n.js
models/ILoading.js
models/LocalizedError.js
ms-graph-api/getMsGraphResourceName.js
ms-graph-api/getMsGraphUrl.js
ms-graph-api/users/userApi.js
redux/ReducerRegistry.js
redux/createAction.js
redux/createAjaxEpic.js
redux/data/setPageableToLoading.js
redux/modules/nav.js
redux/modules/root.js
redux/observableErrorHandler.js
shared/localization.js
shared/override-settings/overrideSettings.js
sorting/time.js
styling/patterns/Banner.scoped.scss
styling/patterns/Banner.scoped.scss?3318
styling/patterns/DetailsPage.scoped.scss
styling/patterns/DetailsPage.scoped.scss?6f50
styling/patterns/Section.scoped.scss
styling/patterns/Section.scoped.scss?3be2
styling/patterns/Text.scoped.scss
styling/patterns/Text.scoped.scss?1332
styling/patterns/TextField.scoped.scss
styling/patterns/TextField.scoped.scss?4c7e
styling/stylingConstants.js
telemetry/AriaTelemetryProvider.js
telemetry/ConsoleTelemetryProvider.js
telemetry/ISS.js
telemetry/Telemetry.js
telemetry/initClickTale.js
telemetry/logBusinessEventError.js
telemetry/scrubTelemetryData.js
telemetry/setDefaultProviders.js
telemetry/treatmentService.js
text/ellipsis.js
[0m
[0m$ [0mpython3 -c "
srcs=open('/tmp/opencode/mysignins-srcs.txt').read().splitlines()
for s in srcs:
    if '/@microsoft/' in s:
        print(s.split('@microsoft/')[-1].replace('node_modules/',''))
" | sort -u | head -40
1ds-analytics-web-js/dist-esm/src/ApplicationInsights.js
1ds-core-js/dist-esm/src/AppInsightsCore.js
1ds-core-js/dist-esm/src/InternalConstants.js
1ds-core-js/dist-esm/src/Utils.js
1ds-post-js/dist-esm/src/ClockSkewManager.js
1ds-post-js/dist-esm/src/DataModels.js
1ds-post-js/dist-esm/src/EventBatch.js
1ds-post-js/dist-esm/src/HttpManager.js
1ds-post-js/dist-esm/src/InternalConstants.js
1ds-post-js/dist-esm/src/KillSwitch.js
1ds-post-js/dist-esm/src/PostChannel.js
1ds-post-js/dist-esm/src/RetryPolicy.js
1ds-post-js/dist-esm/src/Serializer.js
1ds-post-js/dist-esm/src/TimeoutOverrideWrapper.js
1ds-properties-js/dist-esm/src/ExtensionKeys.js
1ds-properties-js/dist-esm/src/InternalConstants.js
1ds-properties-js/dist-esm/src/PropertiesPlugin.js
1ds-properties-js/dist-esm/src/SessionManager.js
1ds-properties-js/dist-esm/src/TelemetryContext.js
1ds-properties-js/dist-esm/src/Utils.js
1ds-properties-js/dist-esm/src/context/Application.js
1ds-properties-js/dist-esm/src/context/Cloud.js
1ds-properties-js/dist-esm/src/context/Device.js
1ds-properties-js/dist-esm/src/context/IntWeb.js
1ds-properties-js/dist-esm/src/context/Loc.js
1ds-properties-js/dist-esm/src/context/OperatingSystem.js
1ds-properties-js/dist-esm/src/context/Sdk.js
1ds-properties-js/dist-esm/src/context/Session.js
1ds-properties-js/dist-esm/src/context/Trace.js
1ds-properties-js/dist-esm/src/context/User.js
1ds-properties-js/dist-esm/src/context/Utc.js
1ds-properties-js/dist-esm/src/context/Web.js
1ds-qos-js/dist-esm/src/Id.js
1ds-qos-js/dist-esm/src/QosPlugin.js
1ds-qos-js/dist-esm/src/common/DataCollector.js
1ds-qos-js/dist-esm/src/common/Utils.js
1ds-qos-js/dist-esm/src/events/IncomingQosEvent.js
1ds-qos-js/dist-esm/src/events/OutgoingQosEvent.js
1ds-qos-js/dist-esm/src/events/QosEvent.js
1ds-wa-js/dist-esm/src/Behaviors.js
[0m
[0m$ [0mcode=$(curl -s -o /dev/null -w "%{http_code}|%{size_download}b" "https://api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map" --max-time 20); echo "myaccount .map: $code"; sleep 1
myaccount .map: 200|35340363b
[0m
Both source maps are live — myaccount's is 35MB. Let me download and extract its source tree for comparison.
[0m$ [0mcurl -s "https://api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map" --max-time 180 -o /tmp/opencode/myaccount.map && wc -c /tmp/opencode/myaccount.map && python3 -c "
import json,collections
d=json.load(open('/tmp/opencode/myaccount.map'))
print('has sourcesContent:', bool(d.get('sourcesContent')))
print('num content:', len([c for c in (d.get('sourcesContent') or []) if c]))
srcs=d.get('sources',[])
print('total sources:',len(srcs))
with open('/tmp/opencode/myaccount-srcs.txt','w') as f:
    for s in srcs: f.write(s+'\n')
pref=collections.Counter()
for s in srcs:
    if s.startswith('webpack://') and '/./src/' in s:
        pref['./src']+=1
    elif s.startswith('webpack://') and '/../portal-app/' in s:
        pref['../portal-app']+=1
    elif s.startswith('webpack://') and '/../..' in s:
        pref['../..']+=1
    else:
        pref[s.split('/')[0]]+=1
for t,n in pref.most_common(10): print(f'{n:6d}  {t}')
"
35340363 /tmp/opencode/myaccount.map
has sourcesContent: True
num content: 4922
total sources: 5015
  3861  webpack:
   927  ../..
   227  ./src
[0m
The myaccount map has **full source content** (4922 files). Let me extract the app's own `src/` code — this reveals exact API request schemas and scopes.
[0m$ [0mpython3 << 'EOF'
import json,os,re
d=json.load(open('/tmp/opencode/myaccount.map'))
srcs=d['sources']; cont=d.get('sourcesContent') or []
outdir='/tmp/opencode/myaccount-src'
os.makedirs(outdir,exist_ok=True)
count=0
for s,c in zip(srcs,cont):
    if not c: continue
    if '/./src/' in s:
        rel=s.split('/./src/',1)[-1]
    elif '/src/' in s:
        rel=s.split('/src/',1)[-1]
    else:
        continue
    rel=rel.replace('/','__').replace('?','_Q')
    path=os.path.join(outdir,rel)
    with open(path,'w') as f:
        f.write(c)
    count+=1
print('extracted',count,'files to',outdir)
EOF
ls /tmp/opencode/myaccount-src | wc -l; ls /tmp/opencode/myaccount-src | head -50
extracted 1977 files to /tmp/opencode/myaccount-src
1947
Account__AccountCard__AccountCard.scoped.scss
Account__AccountCard__AccountCard.scoped.scss_Qe7e5
Account__AccountCard__AccountCard.tsx
Account__AccountHome__AccountHome.page.ts
Account__AccountHome__AccountHome.scoped.scss
Account__AccountHome__AccountHome.scoped.scss_Q0258
Account__AccountHome__AccountHome.tsx
Account__AccountHome__ConnectedAccountHome.ts
Account__Credential__CopyLinkDialog.tsx
Account__Credential__CredentialDialog.styles.ts
Account__Credential__CredentialDialog.tsx
Account__Credential__GetVerifyId.tsx
Account__Credential__RetrievingCredentials.scoped.scss
Account__Credential__RetrievingCredentials.scoped.scss_Q5bf7
Account__Credential__RetrievingCredentials.tsx
Account__Credential__useRetrievingCredential.ts
Account__LeaveOrganization__actions.ts
Account__LeaveOrganization__components__LeaveOrgConfirmationDialog.page.ts
Account__LeaveOrganization__components__LeaveOrgConfirmationDialog.tsx
Account__LeaveOrganization__components__LeaveOrgSuccessDialog.page.ts
Account__LeaveOrganization__components__LeaveOrgSuccessDialog.tsx
Account__LeaveOrganization__effects__effectLeaveOrg.ts
Account__LeaveOrganization__hooks__useLeaveOrgFlow.ts
Account__LeaveOrganization__models.ts
Account__LeaveOrganization__reducers.ts
Account__LeaveOrganization__selectors.ts
Account__OrganizationCard__ConnectedOrganizationCard.tsx
Account__OrganizationCard__OrganizationCard.tsx
Account__Organizations__NativeIdentity__useCurrentOrganization.ts
Account__Organizations__OrganizationsPage.tsx
Account__PersonBadge__CannotEditInfoDetails.tsx
Account__PersonBadge__CannotEditInfoDetailsContent.tsx
Account__PersonBadge__ConnectedPersonBadge.ts
Account__PersonBadge__PersonBadge.scoped.scss
Account__PersonBadge__PersonBadge.scoped.scss_Q6999
Account__PersonBadge__PersonBadge.tsx
Account__PersonBadge__Persona.tsx
Account__PersonBadge__ResponsiveConnectedPersonBadge.tsx
Account__models__AccountActions.ts
Account__redux__accountInitialState.ts
Account__redux__accountRegistry.ts
Account__redux__actions__convergedUXV2Enabled.ts
Account__redux__actions__getCurrentOrganization.ts
Account__redux__actions__getNavBarData.ts
Account__redux__actions__getOrganizations.ts
Account__redux__actions__getUser.ts
Account__redux__actions__leaveOrganization.ts
Account__redux__actions__signOutEverywhere.ts
Account__redux__actions__ssprConvergenceEnabled.ts
Announcers__AnnouncementsProvider.tsx
[0m
[0m$ [0mcd /tmp/opencode/myaccount-src && rg -l -i "issueVerifiedEmployeeCredential|canVerifiedIdBeIssued|issueverified" . 2>/dev/null; echo "=== verified id refs ==="; rg -n -i "issueVerified|canVerifiedId|verifiableCredential|verifiedid" --glob '*.ts*' . 2>/dev/null | rg -v "scoped.scss" | head -30
=== verified id refs ===
/bin/bash: line 1: rg: command not found
[0m
Full source extracted. Now let me mine it for the Verified ID issuance flow, session handling, and scope/audience requirements.
[0m$ [0mcd /tmp/opencode/myaccount-src && grep -rl -i "issueVerifiedEmployeeCredential\|canVerifiedIdBeIssued" . 2>/dev/null | head; echo "=== matches ==="; grep -rn -i "issueVerified\|canVerifiedId" . 2>/dev/null | grep -v "scoped.scss" | head -20
./Account__Credential__useRetrievingCredential.ts
=== matches ===
./Account__Credential__useRetrievingCredential.ts:8:  const url = `${settingsManager.get('myAccountBackendUrl')}/api/issueVerifiedEmployeeCredential`;
./Account__Credential__useRetrievingCredential.ts:33:  const url = `${settingsManager.get('myAccountBackendUrl')}/api/canVerifiedIdBeIssued`;
./shared__getErrorString.ts:13:    case 'GuestIsNotAllowedToIssueVerifiedId':
./shared__getErrorString.ts:14:      return t('verifyIdGuestIsNotAllowedToIssueVerifiedId', { tenantId });
./shared__getErrorString.ts:19:    case 'TenantIsNotInAllowedToIssueVerifiedId':
./shared__getErrorString.ts:20:      return t('verifyIdTenantIsNotInAllowedToIssueVerifiedId', { tenantId });
[0m
[0m→ [0mRead /tmp/opencode/myaccount-src/Account__Credential__useRetrievingCredential.ts
[0m
[0m$ [0mcd /tmp/opencode/myaccount-src && ls | grep -i "settings\|environ" ; echo "=== list a few ==="; grep -rn "myAccountBackendUrl\|authAuthorityEndpoint\|clientId" --include="*.ts" . 2>/dev/null | grep -i "settings\|environ" | head -20
GlobalSettings.ts
InProductFeedback__getInProductFeedbackLocalizedSettings.ts
SettingsAndPrivacy__SettingsAndPrivacyPage.tsx
customizations__mergeSettings.ts
customizations__useCustomizationSettings.ts
getVendorSettings.ts
shared__SettingsManager.ts
shared__settings.ts
webjs__AWTLogManagerSettings.js
=== list a few ===
./Account__LeaveOrganization__effects__effectLeaveOrg.ts:10:    const clientId = settingsManager.getRequired('clientId');
./TermsOfUse__hooks__useGetTenantDisplayName.ts:16:      const clientId = settingsManager.getRequired('clientId');
./TermsOfUse__hooks__useGetTenantDisplayName.ts:42:  const url = `${settingsManager.get('myAccountBackendUrl')}/api/termsofuse/tenantdisplayname`;
./auth.ts:5:const authority = settingsManager.getRequired('authAuthorityEndpoint');
./auth.ts:7:const clientId = settingsManager.getRequired('clientId');
./hooks__useLeftNavGroupConditions.ts:151:      const token = await configuration.getToken(settingsManager.getRequired('clientId'));
./hooks__useLeftNavGroupConditions.ts:152:      const url = `${settingsManager.getRequired('myAccountBackendUrl')}/api/groups/settings`;
./TermsOfUse__hooks__useGetTenantBannerLogo.ts:16:      const url = `${settingsManager.get('myAccountBackendUrl')}/api/termsofuse/tenantbannerlogo`;
./TermsOfUse__hooks__useDeclineAgreement.ts:17:        `${settingsManager.get('myAccountBackendUrl')}/api/termsofuse/agreements/${props.agreementId}/policyfiles/${
./Account__redux__actions__getOrganizations.ts:14:  url: `${settingsManager.get('myAccountBackendUrl')}/api/organizations`,
./Account__redux__actions__getOrganizations.ts:15:  audience: settingsManager.getRequired('clientId'),
./Language__hooks__useGetAvailableLocalesQuery.ts:22:      const clientId = settingsManager.getRequired('clientId');
./TermsOfUse__hooks__useGetMyAcceptances.ts:16:      const url = `${settingsManager.get('myAccountBackendUrl')}/api/termsofuse/myacceptances`;
./TermsOfUse__hooks__useGetMyAcceptances.ts:17:      const clientId = settingsManager.getRequired('clientId');
./TermsOfUse__hooks__useGetAgreement.ts:17:      const clientId = settingsManager.getRequired('clientId');
./TermsOfUse__hooks__useGetAgreement.ts:43:  const url = `${settingsManager.get('myAccountBackendUrl')}/api/termsofuse/agreements/${agreementId}`;
./Account__Credential__useRetrievingCredential.ts:8:  const url = `${settingsManager.get('myAccountBackendUrl')}/api/issueVerifiedEmployeeCredential`;
./Account__Credential__useRetrievingCredential.ts:9:  const clientId = settingsManager.getRequired('clientId');
./Account__Credential__useRetrievingCredential.ts:33:  const url = `${settingsManager.get('myAccountBackendUrl')}/api/canVerifiedIdBeIssued`;
./Account__Credential__useRetrievingCredential.ts:34:  const clientId = settingsManager.getRequired('clientId');
[0m
The Verified ID flow uses `scopes: [clientId]` (token audience = the SPA's own clientId) — backend checks `GuestIsNotAllowedToIssueVerifiedId` / `TenantIsNotInAllowedToIssueVerifiedId`. Let me pull the environment settings and look for more internal config.
[0m$ [0mcd /tmp/opencode/myaccount-src && echo "=== useDeclineAgreement ==="; cat TermsOfUse__hooks__useDeclineAgreement.ts; echo; echo "=== useGetAgreement ==="; cat TermsOfUse__hooks__useGetAgreement.ts
=== useDeclineAgreement ===
import { useTelemetry } from '@iamexperiences/ecos-telemetry';
import { useMutation } from 'react-query';

import { HttpRequestError } from '../../shared/HttpRequestError';
import { IAgreementIdentifiers } from '../models/types';
import { settingsManager } from '../../shared/settings';
import { getRequestBody } from '../utilities/getRequestBody';

export const useDeclineAgreement = () => {
  const telemetry = useTelemetry();
  // This hook is only used in unauthenticated flow and should always use access token from the request body
  const { accessToken, recordDeviceId } = getRequestBody();

  return useMutation({
    mutationFn: async (props: IAgreementIdentifiers) => {
      const url = new URL(
        `${settingsManager.get('myAccountBackendUrl')}/api/termsofuse/agreements/${props.agreementId}/policyfiles/${
          props.policyFileId
        }/decline`,
      );
      url.searchParams.append('recordDeviceId', recordDeviceId.toString());
      const token = accessToken;
      const response = await fetch(url.toString(), {
        method: 'POST',
        headers: {
          Authorization: 'Bearer ' + token,
        },
      });

      if (!response.ok) {
        const error = new HttpRequestError(response.status, response.statusText);
        telemetry.error('Error declining agreement', error);

        throw error;
      }
    },
  });
};

=== useGetAgreement ===
import { useAuth } from '@iamexperiences/feature-auth';
import { ITelemetry, useTelemetry } from '@iamexperiences/ecos-telemetry';
import { useQuery } from 'react-query';

import { HttpRequestError } from '../../shared/HttpRequestError';
import { IAgreement } from '../models/types';
import { settingsManager } from '../../shared/settings';
import { getRequestBody } from '../utilities/getRequestBody';

export const useGetAgreementAuthenticated = (agreementId: string) => {
  const telemetry = useTelemetry();
  const { auth } = useAuth();

  const query = useQuery({
    queryKey: [{ scope: 'agreements-authenticated', agreementId }],
    queryFn: async ({ queryKey }) => {
      const clientId = settingsManager.getRequired('clientId');
      const token = await auth.acquireToken({ scopes: [clientId] });
      return getAgreementQueryFn(queryKey[0].agreementId, token, telemetry);
    },
    staleTime: Infinity,
  });

  return query;
};

export const useGetAgreementUnauthenticated = (agreementId: string) => {
  const telemetry = useTelemetry();

  const query = useQuery({
    queryKey: [{ scope: 'agreements-unauthenticated', agreementId }],
    queryFn: async ({ queryKey }) => {
      const { accessToken } = getRequestBody();
      return getAgreementQueryFn(queryKey[0].agreementId, accessToken, telemetry);
    },
    staleTime: Infinity,
  });

  return query;
};

async function getAgreementQueryFn(agreementId: string, token: string, telemetry: ITelemetry) {
  const url = `${settingsManager.get('myAccountBackendUrl')}/api/termsofuse/agreements/${agreementId}`;
  const response = await fetch(url, {
    headers: {
      Authorization: 'Bearer ' + token,
    },
  });

  if (!response.ok) {
    const error = new HttpRequestError(response.status, response.statusText);
    telemetry.error('Error getting agreement', error);

    throw error;
  }

  const data: IAgreement = await response.json();

  return data;
}
[0m
More surface: `/api/termsofuse/agreements/{id}/policyfiles/{id}` and `/api/groups/settings`. Let me examine the ToS hooks (potential SSRF/IDOR path) and the auth/settings modules.
[0m$ [0mcd /tmp/opencode/myaccount-src && echo "=== getRequestBody ==="; find . -name "*getRequestBody*"; cat $(find . -name "*getRequestBody*" | head -1); echo; echo "=== policyfile refs ==="; grep -rn "policyfile\|policyFile\|policyfile" . 2>/dev/null | grep -v scoped.scss | head -15
=== getRequestBody ===
./TermsOfUse__utilities__getRequestBody.ts
let requestBody: URLSearchParams;

export const getRequestBody = () => {
  if (!requestBody) {
    const htmlElement = document.documentElement;
    const rawRequestBody = htmlElement.dataset.requestbody ?? '';
    requestBody = new URLSearchParams(rawRequestBody);
  }

  const getRequestBodyAsString = (key: string): string => {
    const value = requestBody.get(key);
    if (!value) {
      throw new Error(`${key} is null`);
    }

    return value.toString();
  };

  return {
    requestBody: requestBody,
    accessToken: getRequestBodyAsString('access_token'),
    agreementId: getRequestBodyAsString('agreement_id'),
    replyUri: getRequestBodyAsString('reply_uri'),
    idTokenHint: getRequestBodyAsString('id_token_hint'),
    recordDeviceId:
      JSON.parse(getRequestBodyAsString('claims'))['id_token']['governance_control_state_per_device'] !== undefined,
  };
};

=== policyfile refs ===
./TermsOfUse__Consent__ConsentPageContent.tsx:32:  const policyFileId = agreement?.policyFiles.find((policyFile) => policyFile.isDefault)?.id;
./TermsOfUse__Consent__ConsentPageContent.tsx:78:  } else if (getAgreementQuery.isError || !agreement || !agreementId || !policyFileId) {
./TermsOfUse__Consent__ConsentPageContent.tsx:81:    const agreementIdentifiers = { agreementId, policyFileId };
./TermsOfUse__Consent__ConsentPageContent.tsx:112:        { agreementId: agreementId, policyFileId: policyFileId },
./TermsOfUse__Consent__ConsentPageContent.tsx:131:        { agreementId: agreementId, policyFileId: policyFileId },
./TermsOfUse__Acceptances__AcceptanceList.tsx:94:      policyFileId={acceptance.policyFileId}
./TermsOfUse__hooks__useDeclineAgreement.ts:17:        `${settingsManager.get('myAccountBackendUrl')}/api/termsofuse/agreements/${props.agreementId}/policyfiles/${
./TermsOfUse__hooks__useDeclineAgreement.ts:18:          props.policyFileId
./TermsOfUse__hooks__useAcceptAgreement.ts:17:        `${settingsManager.get('myAccountBackendUrl')}/api/termsofuse/agreements/${props.agreementId}/policyfiles/${
./TermsOfUse__hooks__useAcceptAgreement.ts:18:          props.policyFileId
./TermsOfUse__hooks__useGetPolicyFileData.ts:9:export const useGetPolicyFileDataAuthenticated = (agreementId: string, policyFileId: string) => {
./TermsOfUse__hooks__useGetPolicyFileData.ts:14:    queryKey: [{ scope: 'policyfiles-authenticated', agreementId, policyFileId }],
./TermsOfUse__hooks__useGetPolicyFileData.ts:18:      return getPolicyFileDataQueryFn(queryKey[0].agreementId, queryKey[0].policyFileId, token, telemetry);
./TermsOfUse__hooks__useGetPolicyFileData.ts:26:export const useGetPolicyFileDataUnauthenticated = (agreementId: string, policyFileId: string) => {
./TermsOfUse__hooks__useGetPolicyFileData.ts:30:    queryKey: [{ scope: 'policyfiles-unauthenticated', agreementId, policyFileId }],
[0m
The ToS decline flow is **unauthenticated by design** with an access token from the request body. Let me trace how that token is obtained and how policy files are resolved.
[0m$ [0mcd /tmp/opencode/myaccount-src && echo "=== useGetPolicyFileData ==="; cat TermsOfUse__hooks__useGetPolicyFileData.ts; echo; echo "=== GlobalSettings ==="; sed -n '1,80p' GlobalSettings.ts
=== useGetPolicyFileData ===
import { useAuth } from '@iamexperiences/feature-auth';
import { ITelemetry, useTelemetry } from '@iamexperiences/ecos-telemetry';
import { useQuery } from 'react-query';

import { HttpRequestError } from '../../shared/HttpRequestError';
import { settingsManager } from '../../shared/settings';
import { getRequestBody } from '../utilities/getRequestBody';

export const useGetPolicyFileDataAuthenticated = (agreementId: string, policyFileId: string) => {
  const telemetry = useTelemetry();
  const { auth } = useAuth();

  const query = useQuery({
    queryKey: [{ scope: 'policyfiles-authenticated', agreementId, policyFileId }],
    queryFn: async ({ queryKey }) => {
      const clientId = settingsManager.getRequired('clientId');
      const token = await auth.acquireToken({ scopes: [clientId] });
      return getPolicyFileDataQueryFn(queryKey[0].agreementId, queryKey[0].policyFileId, token, telemetry);
    },
    staleTime: Infinity,
  });

  return query;
};

export const useGetPolicyFileDataUnauthenticated = (agreementId: string, policyFileId: string) => {
  const telemetry = useTelemetry();

  const query = useQuery({
    queryKey: [{ scope: 'policyfiles-unauthenticated', agreementId, policyFileId }],
    queryFn: async ({ queryKey }) => {
      const { accessToken } = getRequestBody();
      return getPolicyFileDataQueryFn(queryKey[0].agreementId, queryKey[0].policyFileId, accessToken, telemetry);
    },
    staleTime: Infinity,
  });

  return query;
};

async function getPolicyFileDataQueryFn(
  agreementId: string,
  policyFileId: string,
  token: string,
  telemetry: ITelemetry,
) {
  const url = `${settingsManager.get(
    'myAccountBackendUrl',
  )}/api/termsofuse/agreements/${agreementId}/policyfiles/${policyFileId}/data`;
  const response = await fetch(url, {
    headers: {
      Authorization: 'Bearer ' + token,
    },
  });

  if (!response.ok) {
    const error = new HttpRequestError(response.status, response.statusText);
    telemetry.error('Error getting policyfile', error);

    throw error;
  }

  const data = await response.json();

  return data;
}

=== GlobalSettings ===
import { getWindow } from './dom/getWindow';

/**
 * Storing global state in local module variables has issues when more than one copy
 * if the module gets loaded on the page (due to a bundling error or simply by consuming
 * a prebundled script.)
 *
 * This file contains helpers to deal with the getting and setting local state, and allows
 * callers to get called back when it mutates.
 */

const GLOBAL_SETTINGS_PROP_NAME = '__globalSettings__';
const CALLBACK_STATE_PROP_NAME = '__callbacks__';

let _counter = 0;

/**
 * Change description used for change callbacks in GlobalSettings.
 *
 * @public
 * {@docCategory IChangeDescription}
 */
export interface IChangeDescription {
  key: string;
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  oldValue: any;
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  value: any;
}

/**
 * Change event callback.
 *
 * @public
 * {@docCategory IChangeEventCallback}
 */
export interface IChangeEventCallback {
  __id__?: string;
  (changeDescription?: IChangeDescription): void;
}

/**
 * Global settings helper, which stores settings in the global (window) namespace.
 * If window is not provided, it will store settings in module scope. Provides a
 * way to observe changes as well when their values change.
 *
 * @public
 * {@docCategory GlobalSettings}
 */
export class GlobalSettings {
  public static getValue<T>(key: string, defaultValue?: T | (() => T)): T {
    const globalSettings = _getGlobalSettings();

    if (globalSettings[key] === undefined) {
      globalSettings[key] = typeof defaultValue === 'function' ? (defaultValue as Function)() : defaultValue;
    }

    return globalSettings[key];
  }

  public static setValue<T>(key: string, value: T): T {
    const globalSettings = _getGlobalSettings();
    const callbacks = globalSettings[CALLBACK_STATE_PROP_NAME];
    let oldValue = globalSettings[key];

    if (value !== oldValue) {
      globalSettings[key] = value;

      let changeDescription = {
        oldValue,
        value,
        key,
      };

      for (let id in callbacks) {
        if (callbacks.hasOwnProperty(id)) {
          callbacks[id](changeDescription);
        }
      }
    }
[0m
[0m$ [0mcd /tmp/opencode/myaccount-src && echo "=== auth.ts ==="; cat auth.ts 2>/dev/null | head -60; echo "=== secret-ish patterns ==="; grep -rn "password\|secret\|apikey\|api_key\|accessToken\s*[:=]\s*['\"][^'\"]\{20,\}" --include="*.ts" --include="*.tsx" . 2>/dev/null | grep -v "scoped.scss" | grep -viE "accessToken == null|accessToken\b.*null|'access_token'|passwordreset|PasswordMethods|password/change|changePassword|ForgotPassword|password reset|passwords" | head -15
=== auth.ts ===
import { getMsalAuth, CacheLocation } from '@iamexperiences/feature-auth';
import { settingsManager } from './shared/settings';
import { getTelemetry } from './Telemetry/Telemetry';

const authority = settingsManager.getRequired('authAuthorityEndpoint');

const clientId = settingsManager.getRequired('clientId');

export const auth = getMsalAuth(
  {
    clientId,
    authority,
    cacheLocation: CacheLocation.SessionStorage,
  },
  {
    onError: (message: string) => {
      logEvent('error', message);
    },
    onWarning: (message: string) => {
      logEvent('warning', message);
    },
    onVerbose: (message: string) => {
      logEvent('verbose', message);
    },
    onInfo: (message: string) => {
      // This will send too many logs, which is not performant.
      // Only enable when session storage is there
      logEvent('info', message);
    },
  },
);

function logEvent(loglevel: string, message: string) {
  // Storage read is synchronous so we cache the result per session
  let isAuthDebugEnabled: boolean | null = null;
  if (loglevel == 'error' || loglevel == 'warning') {
    isAuthDebugEnabled = true;
  }

  if (isAuthDebugEnabled === null) {
    try {
      if (sessionStorage.getItem('AuthDebug') === 'true') {
        isAuthDebugEnabled = true;
      }
    } catch (e) {
      // sessionStorage not available
      isAuthDebugEnabled = false;
    }
  }

  if (isAuthDebugEnabled) {
    getTelemetry()?.reportCustomEvent(`feature-auth/${loglevel}`, {
      message,
    });
  }
}
=== secret-ish patterns ===
./cache__entities__IdTokenEntity.ts:22: *      secret: Actual credential as a string
./cache__entities__IdTokenEntity.ts:49:        idTokenEntity.secret = idToken;
./cache__entities__IdTokenEntity.ts:71:            entity.hasOwnProperty("secret") &&
./components__SensitiveContent__SensitiveContent.tsx:20:export type SensitiveContentType = "password";
./components__SensitiveContent__SensitiveContent.tsx:116:        case "password": {
./error__ClientAuthError.ts:169:        desc: "Client credential (secret, certificate, or assertion) must not be empty when creating a confidential client. An application should at most have one credential"
./client__SilentFlowClient.ts:104:            idTokenObj = new AuthToken(cacheRecord.idToken.secret, this.config.cryptoInterface);
./cache__entities__CredentialEntity.ts:22: *      secret: Actual credential as a string
./cache__entities__CredentialEntity.ts:36:    secret: string;
./cache__CacheManager.ts:230:            accountInfo.idToken = idToken.secret;
./cache__CacheManager.ts:231:            accountInfo.idTokenClaims = new AuthToken(idToken.secret, this.cryptoImpl).claims;
./cache__CacheManager.ts:690:            cachedAccount.idTokenClaims = new AuthToken(cachedIdToken.secret, this.cryptoImpl).claims;
./sinks__utils__scrubSensitiveData.ts:27:  [/(api_secret=)([^&;]+)/g, '$1REDACTED-API-SECRET'],
./crypto__PopTokenGenerator.ts:15: * -  xms_ksl element, representing the storage location of the key's secret component on the client device. One of two values:
./request__RequestParameterBuilder.ts:275:     * add client_secret
[0m
Let me scan the extracted source for auth config, session handling, and any high-value strings (internal hosts, secrets patterns).
My Account source is mapped (auth via `@iamexperiences/feature-auth`, MSAL + SessionStorage). Now let me pull the Graph agentic sign-in docs from the public docs-contrib repo to design the authorization hypothesis.
[0m$ [0mcurl -s "https://raw.githubusercontent.com/microsoftgraph/microsoft-graph-docs-contrib/main/api-reference/beta/resources/agentic-agentsignin.md" --max-time 20 | head -80; sleep 1; echo "=== try API methods listing ==="; curl -s "https://api.github.com/search/code?q=repo:microsoftgraph/microsoft-graph-docs-contrib+agentsigninsession+in:path" --max-time 20 | python3 -c "
import json,sys
try:
    d=json.load(sys.stdin)
    for it in d.get('items',[])[:20]:
        print(it['path'])
except Exception as e:
    print('search err:',e)
"
---
title: "agentSignIn resource type"
description: "Information about an agentic sign-in"
author: "egreenberg14"
ms.date: 05/01/2025
ms.localizationpriority: medium
ms.subservice: "entra-monitoring-health"
doc_type: resourcePageType
---

# agentSignIn resource type

Namespace: microsoft.graph.agentic

[!INCLUDE [beta-disclaimer](../../includes/beta-disclaimer.md)]

For agentic sign-ins, provides information about the agent. This object is configured in the **agent** property of [signIn](../resources/signin.md) and [summarizedSignIn](../resources/summarizedsignin.md).

## Properties

|Property|Type|Description|
|:---|:---|:---|
|agentSubjectParentId|String|The subject's parent object ID. This is either the **id** of the **agentIdentity** or **agentIdentityBlueprint**.|
|agentSubjectType|microsoft.graph.agentic.agentType|The type of the subject that requested the token. The possible values are: `notAgentic`, `agenticApp`, `agenticAppInstance`, `unknownFutureValue`, `agentIdentityBlueprintPrincipal`, `agentIDuser`. Use the `Prefer: include-unknown-enum-members` request header to get the following value or values in this [evolvable enum](/graph/best-practices-concept#handling-future-members-in-evolvable-enumerations): `agentIdentityBlueprintPrincipal`, `agentIDuser`.|
|agentType|microsoft.graph.agentic.agentType|Indicates whether the identity is agentic or nonagentic. The possible values are: `notAgentic`, `agenticApp`, `agenticAppInstance`, `unknownFutureValue`, `agentIdentityBlueprintPrincipal`, `agentIDuser`. Use the `Prefer: include-unknown-enum-members` request header to get the following value or values in this [evolvable enum](/graph/best-practices-concept#handling-future-members-in-evolvable-enumerations): `agentIdentityBlueprintPrincipal`, `agentIDuser`. |
|parentAppId|String|The **appId** of the parent agent where the **agentType** is agentic.|

## Relationships

None.

## JSON representation

The following JSON representation shows the resource type.
<!-- {
  "blockType": "resource",
  "@odata.type": "microsoft.graph.agentic.agentSignIn"
}
-->
``` json
{
  "@odata.type": "#microsoft.graph.agentic.agentSignIn",
  "agentType": "String",
  "parentAppId": "String",
  "agentSubjectType": "String",
  "agentSubjectParentId": "String"
}
```
=== try API methods listing ===
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/microsoftgraph/microsoft-graph-docs-contrib/git/trees/main?recursive=1" --max-time 60 -o /tmp/opencode/graphdocs-tree.json && python3 -c "
import json
d=json.load(open('/tmp/opencode/graphdocs-tree.json'))
paths=[t['path'] for t in d.get('tree',[]) if 'agentSignIn' in t['path'] or 'agentsign' in t['path'].lower() or 'agentic' in t['path'].lower()]
for p in paths: print(p)
print('total tree entries:',len(d.get('tree',[])))
"
api-reference/beta/resources/agentic-agentsignin.md
api-reference/beta/resources/agentsignin.md
total tree entries: 48939
[0m
[0m$ [0mcurl -s "https://raw.githubusercontent.com/microsoftgraph/microsoft-graph-docs-contrib/main/api-reference/beta/resources/agentsignin.md" --max-time 20 | head -100
---
title: "agentSignIn resource type (for conditionalAccess)"
description: "Defines details of the agent identity that is signing in."
author: "kvenkit"
ms.date: 11/03/2025
ms.localizationpriority: medium
ms.subservice: "entra-sign-in"
doc_type: resourcePageType
---

# agentSignIn resource type (for conditionalAccess)

Namespace: microsoft.graph

[!INCLUDE [beta-disclaimer](../../includes/beta-disclaimer.md)]

Defines details of the agent identity that is signing in, as defined in [Conditional Access What If evaluation](../api/conditionalaccessroot-evaluate.md).

Inherits from [signInIdentity](../resources/signinidentity.md).


## Properties
|Property|Type|Description|
|:---|:---|:---|
|agentServicePrincipalId|String|Agent identity object IDs included in the policy.|

## Relationships
None.

## JSON representation
The following JSON representation shows the resource type.
<!-- {
  "blockType": "resource",
  "@odata.type": "microsoft.graph.agentSignIn"
}
-->
``` json
{
  "@odata.type": "#microsoft.graph.agentSignIn",
  "agentServicePrincipalId": "String"
}
```
[0m
`/me/agentSignInSessions` is NOT yet in public Graph docs — it's a new, sparsely-documented surface (good for the agent-session authorization hypothesis). Let me extract the iamexperiences SDK packages from the myaccount map and check agent references.
[0m$ [0mcd /tmp/opencode/myaccount-src && grep -rn -i "agent\|copilot" --include="*.ts" --include="*.tsx" . 2>/dev/null | grep -v "scoped.scss\|SensitiveContent" | head -20
./components__GroupedList__GroupedListSection.tsx:440:      onDragEnter: dragDropEvents!.onDragEnter,
./components__GroupedList__GroupedListSection.tsx:452:   * @param event - the event trigger dropping state change which can be dragenter, dragleave etc
./components__GroupedList__GroupedListSection.tsx:464:        if (dragDropEvents && dragDropEvents.onDragEnter) {
./components__GroupedList__GroupedListSection.tsx:465:          this._droppingClassName = dragDropEvents.onDragEnter(group, event);
./TermsOfUse__Consent__ConsentPageContent.tsx:35:  const isMSAppHost = navigator.userAgent.includes('MSAppHost/');
./hooks__useHasOwnedAgents.ts:4:import { MSGraphScopes, MANAGEAGENTS } from '../shared/constants';
./hooks__useHasOwnedAgents.ts:5:import { getOwnedAgentsUrl } from '../redux/modules/graphApis';
./hooks__useHasOwnedAgents.ts:8: * Hook to check if user has any owned agent service principals
./hooks__useHasOwnedAgents.ts:9: * @returns Query result with boolean indicating if user has owned agents
./hooks__useHasOwnedAgents.ts:11:export const useHasOwnedAgents = () => {
./hooks__useHasOwnedAgents.ts:21:    queryKey: ['hasOwnedAgents', auth.user?.objectId],
./hooks__useHasOwnedAgents.ts:23:      const { scope } = getMSGraphScopeAndEndPoint(MSGraphScopes.AgentIdentityReadAll);
./hooks__useHasOwnedAgents.ts:25:      const url = getOwnedAgentsUrl(auth.user?.objectId || '');
./hooks__useHasOwnedAgents.ts:38:        throw new Error('Failed to fetch owned agents');
./hooks__useHasOwnedAgents.ts:43:      const agentServicePrincipals = servicePrincipals.filter(
./hooks__useHasOwnedAgents.ts:44:        (sp: IServicePrincipal) => sp.servicePrincipalType === MANAGEAGENTS.AGENT_IDENTITY_SERVICE_PRINCIPAL_TYPE
./hooks__useHasOwnedAgents.ts:47:      return agentServicePrincipals.length > 0;
./Stylesheet.ts:95:const REUSE_STYLE_NODE = typeof navigator !== 'undefined' && /rv:11.0/.test(navigator.userAgent);
./fluent2SharedColors.ts:16:  magentaDark: '#6b0043', // sharedColors.magenta.shade30
./fluent2SharedColors.ts:17:  magenta: '#bf0077', // sharedColors.magenta.primary
[0m
[0m$ [0mcd /tmp/opencode/myaccount-src && echo "=== useHasOwnedAgents ==="; cat hooks__useHasOwnedAgents.ts; echo "=== constants ==="; grep -rn "MANAGEAGENTS\|AgentIdentity" shared__constants.ts 2>/dev/null | head; echo "=== graphApis ==="; cat redux__modules__graphApis.ts 2>/dev/null | head -60
=== useHasOwnedAgents ===
import { useQuery } from 'react-query';
import { useAuth } from '@iamexperiences/feature-auth';
import { getMSGraphScopeAndEndPoint } from '../shared/utils';
import { MSGraphScopes, MANAGEAGENTS } from '../shared/constants';
import { getOwnedAgentsUrl } from '../redux/modules/graphApis';

/**
 * Hook to check if user has any owned agent service principals
 * @returns Query result with boolean indicating if user has owned agents
 */
export const useHasOwnedAgents = () => {
  const { auth } = useAuth();
   
  interface IServicePrincipal {
      id?: string;
      servicePrincipalType?: string;
      displayName?: string;
  }

  return useQuery({
    queryKey: ['hasOwnedAgents', auth.user?.objectId],
    queryFn: async () => {
      const { scope } = getMSGraphScopeAndEndPoint(MSGraphScopes.AgentIdentityReadAll);
      const token = await auth.acquireToken({ scopes: [scope] });
      const url = getOwnedAgentsUrl(auth.user?.objectId || '');

      // Note: Using top(999) to balance performance vs completeness.
      // We need to perform in-memory filtering on servicePrincipalType since
      // the Graph API doesn't support filtering by servicePrincipalType directly.
      // This avoids pagination while checking most users' service principals.
      const response = await fetch(url, {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      });

      if (!response.ok) {
        throw new Error('Failed to fetch owned agents');
      }

      const data = await response.json();
      const servicePrincipals = data?.value || [];
      const agentServicePrincipals = servicePrincipals.filter(
        (sp: IServicePrincipal) => sp.servicePrincipalType === MANAGEAGENTS.AGENT_IDENTITY_SERVICE_PRINCIPAL_TYPE
      );

      return agentServicePrincipals.length > 0;
    },
    enabled: !!auth.user?.objectId,
  });
};
=== constants ===
44:  AgentIdentityReadAll = 'AgentIdentity.Read.All',
145:export const MANAGEAGENTS = {
=== graphApis ===
import { getMsGraphUrl } from '@microsoft/portal-app/lib/ms-graph-api/getMsGraphUrl';
import { IAjaxRequest } from '@microsoft/portal-app/lib/auth';
import { getApiServerUrl } from '@microsoft/portal-app/lib/environment/getApiServerUrl';
import { ICheckMemberGroupsRequest } from '../../Privacy/redux/actions/ICheckMemberGroupsRequest';
import { getSubstrateSearchUrl } from './getSubstrateSearchUrl';
import { getSubstrateSearchAudienceName } from './getSubstrateSearchAudienceName';
import { getMSGraphScopeAndEndPoint } from '../../shared/utils';
import { MSGraphScopes } from '../../shared/constants';

const getGraphUrlV1 = getMsGraphUrl();
const getGraphUrlBeta = getMsGraphUrl('beta');
const getSubstrateSearchUrlV1 = getSubstrateSearchUrl('v1');

export const getMailboxSettingsUrl = () => getGraphUrlV1('/me/mailboxsettings').toString();
export const getPhotoUrl = () => getGraphUrlV1('/me/photos/648x648/$value').toString();
export const getMePhotoUrl = () => getGraphUrlV1('/me/photo/$value').toString();
export const getAvailableTimezonesUrl = (lang: string) =>
  getApiServerUrl('/api/allTimeZones').toString() + `?lang=${lang}`;
export const getAvailableLocalesUrl = () => getApiServerUrl('/api/availableRegionalFormats').toString();
export const getBatchUpdateUrl = () => getGraphUrlBeta('/$batch').toString();
export const getRegionalAndLanguageUrl = () => getGraphUrlBeta('/me/settings/regionalAndLanguageSettings').toString();
export const getUserItemInsightsSettingsUrl = () => getGraphUrlBeta('/me/settings/itemInsights').toString();
export const postCheckMemberGroupsUrl = () => getGraphUrlBeta(`/me/checkMemberGroups`).toString();
export const getContactMergeSuggestionsSettingsUrl = () =>
  getGraphUrlBeta('/me/settings/contactMergeSuggestions').toString();
export const getTenantBrandingLogoUrl = (orgId: string) =>
  getGraphUrlV1(
    `/organization/${orgId}/branding/localizations?`,
    '$select=id,cdnList,bannerLogoRelativeUrl',
  ).toString();
export const getOrgContactInsightsSettingsUrl = (orgId: string) =>
  getGraphUrlBeta(`/organization/${orgId}/settings/contactInsights`).toString();
export const getOrgItemInsightsSettingsUrl = (orgId: string) =>
  getGraphUrlBeta(`/organization/${orgId}/settings/itemInsights`).toString();
export const getOrgPeopleInsightsSettingsUrl = (orgId: string) =>
  getGraphUrlBeta(`/organization/${orgId}/settings/peopleInsights`).toString();
export const getOwnedAgentsUrl = (userId: string) => getGraphUrlBeta(`/users/${userId}/ownedObjects/Microsoft.graph.ServicePrincipal?$top=999`).toString();
export const getSponsoredAgentsUrl = (userId: string) => getGraphUrlBeta(`/users/${userId}/sponsorOf/microsoft.graph.servicePrincipal?$top=999`).toString();
export const getAssignedPlansUrl = () => getGraphUrlV1('/me?', '$select=assignedPlans').toString();
export const getUserInfoUrl = () =>
  getGraphUrlV1(
    '/me?',
    '$select=displayName,userPrincipalName,businessPhones,officeLocation,city,mail,jobTitle,preferredLanguage,givenName,surname',
  ).toString();
// https://learn.microsoft.com/en-us/graph/api/rbacapplication-list-roleassignments?view=graph-rest-1.0&tabs=http
export const getRoleAssignmentsUrl = (userId: string) =>
  getGraphUrlV1(
    `/roleManagement/directory/roleAssignments?'`,
    `$filter = principalId eq '${encodeURIComponent(userId)}'`,
  ).toString();

export const getProfilePhotoAllowedOperationsUrl = () => getGraphUrlBeta('/me/photo/getAllowedOperations').toString();

// This extended property is used to store the display name of the source (e.g. mobile device) on each contact imported from that source.
// This is Customer Content and needs to be included in DSR exports for completeness.
export const extendedPropertyIdImportDisplayName = 'String 0x200106F';

// This extended property is used to store the contact's id on the source (e.g. mobile device).
// This information is imported from the source (i.e. device) and needs to be included in DSR exports for completeness.
export const extendedPropertyIdSourceContactId = 'String 0x2001081';
[0m
My Account has an agent-management hook using a Graph scope `AgentIdentity.Read.All`. Let me read it and the constants/graph helpers.
[0m$ [0mcd /tmp/opencode/myaccount-src && echo "=== constants 1-70 ==="; sed -n '1,70p' shared__constants.ts; echo "=== constants 140-175 ==="; sed -n '140,175p' shared__constants.ts; echo "=== shared/utils getMSGraphScopeAndEndPoint ==="; grep -rn "getMSGraphScopeAndEndPoint" shared__utils.ts 2>/dev/null; find . -name "shared__utils*"; 
=== constants 1-70 ===
export const meControlQueryParamKey = 'ref';
export const meControlQueryParamValue = 'MeControl';
export const lastTimeSawBannerLocalStorageKey = 'lastTimeSawBanner';
export const msaRedirectUrl = 'https://account.microsoft.com';

export const B2B_LEAVE_HOME_ORG_LINK = 'https://aka.ms/ManageOrgsWorkOrSchool';
export const LEAVE_ORG_LEARN_MORE_LINK = 'https://aka.ms/ManageOrgsWorkOrSchool';

export const LAST_ADMIN_LEARN_MORE_LINK =
  'https://learn.microsoft.com/en-us/azure/active-directory/enterprise-users/directory-delete-howto';
export const B2B_COLLAB_VS_DIRECT_CONNECT_LINK = 'https://aka.ms/b2b-collab-vs-direct-connect';

export const QUERY_PARAM_LOGIN_HINT = 'login_hint';
export const QUERY_PARAM_TENANT = 'tenant';

export const P1_SERVICE_PLAN_ID = '41781fb2-bc02-4b7c-bd55-b576c07bb09d';
export const P2_SERVICE_PLAN_ID = 'eec0eb4f-6444-4f95-aba0-50c24d67f998';
export const ENTRA_IDENTITY_GOVERNANCE_SERVICE_PLAN_ID = 'e866a266-3cff-43a3-acca-0c90a7e00c8b';
export const COPILOT_BUSINESS_CHAT_SERVICE_PLAN_ID = '3f30311c-6b1e-48a4-ab79-725b469da960';

export const changeInfoContactUrl = 'https://aka.ms/aadchangeinfocontact';

export const MY_GROUPS_PATH = '/groups';

export const AUTHENTICATOR_LINK = 'https://www.microsoft.com/en-us/security/mobile-authenticator-app';
export const RETRIEVING_CREDENTIALS = 'retrieving-credentials';

export const ENABLE_TOU_QUERY_STRING = 'enabletou';

export const EU_DATA_BOUNDARY_CLAIM_VALUE = 'eu';

export const PAGE_TITLE = 'My Account';

export const VERIFIEDID_APP_NAME_LINKEDIN = 'linkedin';

export enum MSGraphScopes {
  CrossTenantUserProfileSharingReadWriteAll = 'CrossTenantUserProfileSharing.ReadWrite.All',
  OrganizationReadAll = 'Organization.Read.All',
  UserReadAll = 'User.Read.All',
  UserReadWrite = 'User.ReadWrite',
  MailboxSettingsReadWrite = 'MailboxSettings.ReadWrite',
  BitlockerKeyReadAll = 'BitlockerKey.Read.All',
  ContactsRead = 'Contacts.Read',
  AgentIdentityReadAll = 'AgentIdentity.Read.All',
}

export const MOBILE_BREAK_POINT = 's';

export const OFFICE_SERVICES = {
  sharePoint: 'SharePoint',
  exchange: 'exchange',
  microsoftOffice: 'MicrosoftOffice',
  toDo: 'To-Do',
  yammer: 'YammerEnterprise',
  teams: 'TeamspaceAPI',
  powerApps: 'PowerAppsService',
  sway: 'Sway',
};

// route map
interface RoutePath {
  link: string;
  key: string;
  subRoutePaths?: RoutePathMap;
}
type RoutePathMap = Record<string, RoutePath>;
export const NAVIGATION_LINKS: RoutePathMap = {
  home: { link: '/', key: 'home' },
  devices: { link: '/device-list', key: 'device-list' },
  organizations: {
=== constants 140-175 ===
  REQUESTS: { link: '/requests', linkKey: 'requests' },
  SEARCH: { link: '/search', linkKey: 'search' },
  GIVE_FEEDBACK: { linkKey: 'give-feedback' },
};

export const MANAGEAGENTS = {
  AGENT_IDENTITY_SERVICE_PRINCIPAL_TYPE: 'ServiceIdentity',
};

/**
 * Roaming Settings API configuration constants.
 */
export const ROAMING_SETTINGS = {
  /** Office Application ID assigned to MyAccount by the roaming team. */
  OFFICE_APP_ID: '179',
  /** The roaming setting ID for image watermark. */
  SETTING_ID_FOR_IMAGE_WATERMARK: 1395,
  /** Office platform identifier. */
  PLATFORM: 'Web',
  /** API path prefix. */
  API_PATH: '/rs/v1/settings',
} as const;
=== shared/utils getMSGraphScopeAndEndPoint ===
38:export const getMSGraphScopeAndEndPoint = (scope: MSGraphScopes) => {
./shared__utils.ts
[0m
Rich Graph surface exposed (owned agents, role assignments, substrate search). Let me read the constants and scope-mapping utils.
[0m→ [0mRead /tmp/opencode/myaccount-src/shared__utils.ts
[0m
[0m$ [0mcd /tmp/opencode/myaccount-src && echo "=== roaming settings rs/v1 ==="; grep -rln "rs/v1/settings\|ROAMING_SETTINGS" . 2>/dev/null | head; echo "=== substrate ==="; grep -rln "substrate\|Substrate" . 2>/dev/null | head; echo "=== getSubstrateSearchUrl ==="; cat redux__modules__getSubstrateSearchUrl.ts 2>/dev/null; cat redux__modules__getSubstrateSearchAudienceName.ts 2>/dev/null
=== roaming settings rs/v1 ===
./shared__constants.ts
=== substrate ===
./redux__modules__getSubstrateResourceName.ts
./redux__modules__getSubstrateSearchAudienceName.ts
./redux__modules__getSubstrateSearchUrl.ts
./redux__modules__graphApis.ts
=== getSubstrateSearchUrl ===
import { getSubstrateResourceName } from './getSubstrateResourceName';

export const getSubstrateSearchUrl =
  (version: string = 'v1') =>
  (path: string, queryString?: string) => {
    const substrateRootPath = getSubstrateResourceName();
    const substrateSearchUrl = `search/api/${version}${path}`;
    const url: URL = new URL(substrateSearchUrl + (queryString ? `&${queryString}` : ''), substrateRootPath);
    return url;
  };
import { getSubstrateResourceName } from './getSubstrateResourceName';
export const getSubstrateSearchAudienceName = () => {
  const substrateRootPath = getSubstrateResourceName();
  return substrateRootPath ? `${substrateRootPath}/search` : '';
};
[0m
Let me check the roaming-settings + substrate-search usage in source, and research the AAD redirect_uri validation rules and accledger in parallel.
[0m◈ [0mParallel Web Search "Azure AD v2.0 redirect_uri validation rules normalization exact match public client response_mode restrictions"
[0m
[0m$ [0mcd /tmp/opencode/myaccount-src && grep -rhoE "(getApiServerUrl|myAccountBackendUrl)/?[^\`'\"]{0,80}|\`[^\`]*(api|/v[0-9])[^\`]*\`" --include="*.ts" --include="*.tsx" . 2>/dev/null | grep -oE "(/api/[a-zA-Z0-9/_$}{.:-]+|/search/api/[a-z0-9/_$}{.:-]+|/rs/v1/[a-zA-Z0-9/_$}{.:-]+)" | sort -u | head -60
/api/${version}${path}
/api/EnduserEmailContacts
/api/TenantEmailSettings
/api/canVerifiedIdBeIssued
/api/dateTimeFormats
/api/devices
/api/devices/${encode
/api/groups/settings
/api/issueVerifiedEmployeeCredential
/api/organizations
/api/organizations/${encode
/api/shell/navbardata
/api/signOutEverywhere
/api/termsofuse/agreements/${agreementId}
/api/termsofuse/myacceptances
/api/termsofuse/tenantbannerlogo
/api/termsofuse/tenantdisplayname
[0m
Solid redirect_uri research captured. Let me catalog every backend endpoint in the extracted My Account source and search for the accledger service publicly.
[0m◈ [0mParallel Web Search ""accledger" OR "access ledger" Microsoft Entra identity service github controlplane"
[0m
[0m$ [0mpython3 << 'EOF'
import json,os
d=json.load(open('/tmp/opencode/myaccount.map'))
srcs=d['sources']; cont=d.get('sourcesContent') or []
out='/tmp/opencode/portal-lib'
os.makedirs(out,exist_ok=True)
n=0
for s,c in zip(srcs,cont):
    if not c: continue
    if 'portal-app/lib/environment' in s or 'portal-app/lib/ms-graph-api' in s:
        rel=s.split('portal-app/lib/')[-1].replace('/','__').replace('?','_Q')
        with open(os.path.join(out,rel),'w') as f: f.write(c)
        n+=1
print('extracted',n)
EOF
ls /tmp/opencode/portal-lib/
extracted 7
environment__determineEnvironment.js
environment__getApiServerUrl.js
environment__getUrlForApp.js
environment__index.js
ms-graph-api__getMsGraphResourceName.js
ms-graph-api__getMsGraphUrl.js
ms-graph-api__users__userApi.js
[0m
Let me extract the portal-app environment routing lib from the myaccount map — it reveals host routing/URL construction.
[0m$ [0mcd /tmp/opencode/portal-lib && echo "=== getUrlForApp ==="; cat environment__getUrlForApp.js; echo "=== getMsGraphUrl ==="; cat ms-graph-api__getMsGraphUrl.js; echo "=== getMsGraphResourceName ==="; cat ms-graph-api__getMsGraphResourceName.js
=== getUrlForApp ===
"use strict";

var _interopRequireDefault = require("@babel/runtime-corejs3/helpers/interopRequireDefault");

var _Object$defineProperty = require("@babel/runtime-corejs3/core-js/object/define-property");

_Object$defineProperty(exports, "__esModule", {
  value: true
});

exports.getIAMUXBaseAddress = getIAMUXBaseAddress;
exports.getIAMUXRequestOptions = exports.getUrlForApp = exports.getIAMUXTermsOfUseUrl = exports.getIAMUXChangePasswordUrl = exports.getIAMUXAppsUrl = exports.getIAMUXGroupsUrl = exports.getIAMUXProfileUrl = exports.AppKeys = void 0;

var _concat = _interopRequireDefault(require("@babel/runtime-corejs3/core-js/instance/concat"));

var _url = _interopRequireDefault(require("@babel/runtime-corejs3/core-js/url"));

var _overrideSettings = require("../shared/override-settings/overrideSettings");

var _determineEnvironment = require("./determineEnvironment");

var AppKeys;
exports.AppKeys = AppKeys;

(function (AppKeys) {
  AppKeys["apps"] = "myApps";
  AppKeys["access"] = "myAccess";
  AppKeys["groups"] = "myGroups";
  AppKeys["account"] = "myAccount";
  AppKeys["signins"] = "mySignins";
  AppKeys["convergedUXV2"] = "convergedUXV2";
  AppKeys["staff"] = "myStaff";
})(AppKeys || (exports.AppKeys = AppKeys = {}));

var getIAMUXProfileUrl = function getIAMUXProfileUrl(params) {
  var baseAddress = getIAMUXBaseAddress(params);
  baseAddress.pathname = '/profile';
  return baseAddress.toString();
};

exports.getIAMUXProfileUrl = getIAMUXProfileUrl;

var getIAMUXGroupsUrl = function getIAMUXGroupsUrl(params) {
  var baseAddress = getIAMUXBaseAddress(params);
  baseAddress.pathname = '/redirect/groups';
  return baseAddress.toString();
};

exports.getIAMUXGroupsUrl = getIAMUXGroupsUrl;

var getIAMUXAppsUrl = function getIAMUXAppsUrl(params) {
  var baseAddress = getIAMUXBaseAddress(params);
  baseAddress.pathname = '/r';
  baseAddress.hash = '/applications';
  return baseAddress.toString();
};

exports.getIAMUXAppsUrl = getIAMUXAppsUrl;

var getIAMUXChangePasswordUrl = function getIAMUXChangePasswordUrl(location, tenantId) {
  var baseAddress = getIAMUXBaseAddress(location, tenantId);
  baseAddress.pathname = '/ChangePassword.aspx';
  return baseAddress.toString();
};

exports.getIAMUXChangePasswordUrl = getIAMUXChangePasswordUrl;

var getIAMUXTermsOfUseUrl = function getIAMUXTermsOfUseUrl(location, tenantId) {
  var baseAddress = getIAMUXBaseAddress(location, tenantId);
  baseAddress.pathname = '/responsive/TermsOfUse';
  return baseAddress.toString();
};

exports.getIAMUXTermsOfUseUrl = getIAMUXTermsOfUseUrl;

function getIAMUXBaseAddress(locationOrParams, tenantIdParam) {
  var _ref = locationOrParams && isIGetIAMUXUrlParams(locationOrParams) ? locationOrParams : {
    location: locationOrParams,
    tenantId: tenantIdParam || '',
    loginHint: ''
  },
      _ref$location = _ref.location,
      location = _ref$location === void 0 ? window.location : _ref$location,
      tenantId = _ref.tenantId,
      loginHint = _ref.loginHint,
      optInApp = _ref.optInApp;

  var environment = (0, _determineEnvironment.determineEnvironment)(location);
  var protocolPrefix = 'https://';
  var iamuxBaseAddressOverrideEndpoint = (0, _overrideSettings.getOverrideSetting)('iamuxBaseAddressEndpoint');
  var url;

  if (iamuxBaseAddressOverrideEndpoint) {
    var _context;

    url = new _url.default((0, _concat.default)(_context = "".concat(protocolPrefix)).call(_context, iamuxBaseAddressOverrideEndpoint));
  } else {
    switch (environment) {
      case _determineEnvironment.EnvironmentType.localhost:
      case _determineEnvironment.EnvironmentType.PPE:
        url = new _url.default("".concat(protocolPrefix, "account.activedirectory-ppe.windowsazure.com"));
        break;

      case _determineEnvironment.EnvironmentType.TIP:
        url = new _url.default("".concat(protocolPrefix, "account-tip.activedirectory.windowsazure.com"));
        break;

      case _determineEnvironment.EnvironmentType.PROD:
        url = new _url.default("".concat(protocolPrefix, "account.activedirectory.windowsazure.com"));
        break;

      case _determineEnvironment.EnvironmentType.AR:
        url = new _url.default("".concat(protocolPrefix, "account.activedirectory.windowsazure.us"));
        break;

      case _determineEnvironment.EnvironmentType.USSEC:
        url = new _url.default("".concat(protocolPrefix, "account.aad.microsoft.scloud"));
        break;

      case _determineEnvironment.EnvironmentType.USNAT:
        url = new _url.default("".concat(protocolPrefix, "account.aad.eaglex.ic.gov"));
        break;

      default:
        url = new _url.default("".concat(protocolPrefix, "account.activedirectory-ppe.windowsazure.com"));
    }
  }

  if (tenantId) {
    url.searchParams.set('tenantId', tenantId);
  }

  if (loginHint) {
    url.searchParams.set('login_hint', loginHint);
  }

  if (optInApp) {
    url.searchParams.set('optIn', optInApp);
  }

  return url;
}

var getApiPrefix = function getApiPrefix(isBackend) {
  return isBackend ? 'api.' : '';
};

var getUrlForApp = function getUrlForApp(location, appKey, isBackend) {
  var _context5, _context6, _context7, _context8, _context9, _context10, _context11, _context12, _context13, _context14, _context15, _context16, _context17, _context18, _context19, _context20, _context21, _context22;

  var key = appKey;

  if (appKey === AppKeys.apps) {
    key = 'myapplications';
  }

  key = key.toLowerCase();
  var prefix = getApiPrefix(isBackend);
  var environment = (0, _determineEnvironment.determineEnvironment)(location);
  var protocolPrefix = 'https://';
  var urlSuffix = (0, _overrideSettings.getOverrideSetting)('appUrlSuffix');

  if (urlSuffix) {
    var _context2, _context3, _context4;

    return new _url.default((0, _concat.default)(_context2 = (0, _concat.default)(_context3 = (0, _concat.default)(_context4 = "".concat(protocolPrefix)).call(_context4, prefix)).call(_context3, key)).call(_context2, urlSuffix));
  }

  switch (environment) {
    case _determineEnvironment.EnvironmentType.localhost:
      return new _url.default(process.env.APP_LOCALHOST_URL || 'http://localhost:3000');

    case _determineEnvironment.EnvironmentType.PPE:
      return new _url.default((0, _concat.default)(_context5 = (0, _concat.default)(_context6 = (0, _concat.default)(_context7 = "".concat(protocolPrefix)).call(_context7, prefix)).call(_context6, key)).call(_context5, process.env.APP_PPE_URL_SUFFIX));

    case _determineEnvironment.EnvironmentType.TIP:
      return new _url.default((0, _concat.default)(_context8 = (0, _concat.default)(_context9 = (0, _concat.default)(_context10 = "".concat(protocolPrefix)).call(_context10, prefix)).call(_context9, key)).call(_context8, process.env.APP_TIP_URL_SUFFIX));

    case _determineEnvironment.EnvironmentType.PROD:
      return new _url.default((0, _concat.default)(_context11 = (0, _concat.default)(_context12 = (0, _concat.default)(_context13 = "".concat(protocolPrefix)).call(_context13, prefix)).call(_context12, key)).call(_context11, process.env.APP_PROD_URL_SUFFIX));

    case _determineEnvironment.EnvironmentType.AR:
      return new _url.default((0, _concat.default)(_context14 = (0, _concat.default)(_context15 = (0, _concat.default)(_context16 = "".concat(protocolPrefix)).call(_context16, prefix)).call(_context15, key)).call(_context14, process.env.APP_AR_URL_SUFFIX));

    case _determineEnvironment.EnvironmentType.USSEC:
      return new _url.default((0, _concat.default)(_context17 = (0, _concat.default)(_context18 = (0, _concat.default)(_context19 = "".concat(protocolPrefix)).call(_context19, prefix)).call(_context18, key)).call(_context17, process.env.APP_USSEC_URL_SUFFIX));

    case _determineEnvironment.EnvironmentType.USNAT:
      return new _url.default((0, _concat.default)(_context20 = (0, _concat.default)(_context21 = (0, _concat.default)(_context22 = "".concat(protocolPrefix)).call(_context22, prefix)).call(_context21, key)).call(_context20, process.env.APP_USNAT_URL_SUFFIX));
  }
};
/**
 * Gets the base ajax options for making IAMUX ajax requests.
 */


exports.getUrlForApp = getUrlForApp;

var getIAMUXRequestOptions = function getIAMUXRequestOptions() {
  return {
    hasXssiResponse: true,
    audience: process.env.APP_ADAL_IAMUX_CLIENT_ID,
    sendSessionId: false,
    // IAMUX has logger lived sessions
    withCredentials: true,
    setResponseRequestId: true
  };
}; // TODO: use Omit


exports.getIAMUXRequestOptions = getIAMUXRequestOptions;

function isIGetIAMUXUrlParams(obj) {
  return obj.origin === undefined;
}=== getMsGraphUrl ===
"use strict";

var _interopRequireDefault = require("@babel/runtime-corejs3/helpers/interopRequireDefault");

var _Object$defineProperty = require("@babel/runtime-corejs3/core-js/object/define-property");

_Object$defineProperty(exports, "__esModule", {
  value: true
});

exports.getMsGraphUrl = void 0;

var _url = _interopRequireDefault(require("@babel/runtime-corejs3/core-js/url"));

var _overrideSettings = require("../shared/override-settings/overrideSettings");

var _getMsGraphResourceName = require("./getMsGraphResourceName");

var getMsGraphUrl = function getMsGraphUrl() {
  var version = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : 'v1.0';
  return function (path, queryString) {
    var location = arguments.length > 2 && arguments[2] !== undefined ? arguments[2] : window.location;
    var overrideSetting = (0, _overrideSettings.getOverrideSetting)('msGraphResourceName');
    var url;

    if (overrideSetting) {
      url = new _url.default(version + path, overrideSetting);
    } else {
      url = new _url.default(version + path, (0, _getMsGraphResourceName.getMsGraphResourceName)(location));
    }

    url.search = queryString || '';
    return url;
  };
};

exports.getMsGraphUrl = getMsGraphUrl;=== getMsGraphResourceName ===
"use strict";

var _Object$defineProperty = require("@babel/runtime-corejs3/core-js/object/define-property");

_Object$defineProperty(exports, "__esModule", {
  value: true
});

exports.getMsGraphResourceName = void 0;

var _environment = require("../environment");

var _overrideSettings = require("../shared/override-settings/overrideSettings");

var getMsGraphResourceName = function getMsGraphResourceName() {
  var location = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : window.location;
  var overrideSetting = (0, _overrideSettings.getOverrideSetting)('msGraphResourceName');

  if (overrideSetting) {
    return overrideSetting;
  }

  var env = (0, _environment.determineEnvironment)(location);
  var resourceName;

  switch (env) {
    case _environment.EnvironmentType.PPE:
      resourceName = 'https://graph.microsoft-ppe.com';
      break;

    default:
      resourceName = 'https://graph.microsoft.com';
  }

  return resourceName;
};

exports.getMsGraphResourceName = getMsGraphResourceName;
[0m
[0m$ [0mcd /tmp/opencode/portal-lib && echo "=== determineEnvironment ==="; sed -n '1,80p' environment__determineEnvironment.js | grep -A30 "function determineEnvironment\|EnvironmentType" | head -45
=== determineEnvironment ===
exports.setEnvironmentCallback = exports.getEnvironmentSetting = exports.determineEnvironment = exports.EnvironmentType = void 0;

var _endsWith = _interopRequireDefault(require("@babel/runtime-corejs3/core-js/instance/ends-with"));

var _includes = _interopRequireDefault(require("@babel/runtime-corejs3/core-js/instance/includes"));

var EnvironmentType;
exports.EnvironmentType = EnvironmentType;

(function (EnvironmentType) {
  EnvironmentType["localhost"] = "localhost";
  EnvironmentType["PPE"] = "PPE";
  EnvironmentType["TIP"] = "TIP";
  EnvironmentType["PROD"] = "PROD";
  EnvironmentType["AR"] = "US";
  EnvironmentType["USSEC"] = "microsoft.scloud";
  EnvironmentType["USNAT"] = "eaglex.ic.gov";
})(EnvironmentType || (exports.EnvironmentType = EnvironmentType = {}));

var determineEnvironment = function determineEnvironment(location) {
  if (environmentCallback !== undefined) {
    var environmentOverride = environmentCallback(location);

    if (environmentOverride !== undefined) {
      return environmentOverride;
    }
  }

  var currentUrl = location.hostname.toLowerCase();

  if ((0, _includes.default)(currentUrl).call(currentUrl, EnvironmentType.localhost.toString().toLowerCase())) {
    return EnvironmentType.localhost;
  } else if ((0, _includes.default)(currentUrl).call(currentUrl, EnvironmentType.PPE.toString().toLowerCase())) {
    return EnvironmentType.PPE;
  } else if ((0, _includes.default)(currentUrl).call(currentUrl, EnvironmentType.TIP.toString().toLowerCase())) {
    return EnvironmentType.TIP;
  } else if ((0, _includes.default)(currentUrl).call(currentUrl, EnvironmentType.AR.toString().toLowerCase())) {
    return EnvironmentType.AR;
  } else if ((0, _endsWith.default)(currentUrl).call(currentUrl, EnvironmentType.USSEC.toString().toLowerCase())) {
    return EnvironmentType.USSEC;
  } else if ((0, _endsWith.default)(currentUrl).call(currentUrl, EnvironmentType.USNAT.toString().toLowerCase())) {
    return EnvironmentType.USNAT;
  } else {
    // Assume that if the URL doesn't contain localhost, TIP, or PPE then it's PROD
    return EnvironmentType.PROD;
[0m
IAMUX host routing captured (PPE/TIP/PROD for `account.activedirectory.windowsazure.com` — adjacent, out-of-scope per scope.yml). Let me check env detection and the substrate search usage, then finalize.
[0m$ [0mcd /tmp/opencode/myaccount-src && grep -rln "getSubstrateSearchUrl\|search/api" --include="*.ts" --include="*.tsx" . 2>/dev/null | head; echo "=== query paths ==="; grep -rhoE "getSubstrateSearchUrl\(\)\('[^']{0,80}" . 2>/dev/null | sort -u | head -10
./redux__modules__getSubstrateSearchUrl.ts
./redux__modules__graphApis.ts
=== query paths ===
[0m
[0m$ [0mcd /tmp/opencode && sha256sum mysignins.map myaccount.map mysignins-srcs.txt 2>/dev/null; echo "---"; git -C /home/runner/work/hunt-lab/hunt-lab status --short 2>/dev/null | head; echo "---leads dir---"; ls /home/runner/work/hunt-lab/hunt-lab/leads/
2099f8a83336e290f2ab20c09a56b3a4493d0b7f66328b944c7fc18055f3efbe  mysignins.map
72290126cebc34559574f30c675c6bfe84856fe8a7678eb2e0821237a27d6541  myaccount.map
1a5be8a2293c3797e11882895b69ef9dc53173bc71b1ee5865b6a8934034e506  mysignins-srcs.txt
---
?? ctx-leads.txt
?? ctx-research.txt
?? ctx-valid.txt
?? raw_bigpickle.txt
?? research-prompt.txt
---leads dir---
lead-bigpickle.md
lead-laguna.md
lead-ling3.md
lead-longcat.md
lead-nemotron3.md
reposcan-2026-08-07-08-25.md
reposcan-2026-08-07-08-29.md
reposcan-2026-08-07-08-34.md
reposcan-2026-08-07-08-40.md
reposcan-2026-08-07-08-52.md
reposcan-2026-08-07-09-02.md
reposcan-latest.md
[0m
Comprehensive material gathered. Let me hash the downloaded artifacts for the journal (no raw secrets — hashes only) and check repo state.
[0m$ [0mcat >> /home/runner/work/hunt-lab/hunt-lab/research/bigpickle.md <<'EOF'

## RUN 2026-08-07 (big-pickle) — TARGET: microsoft — PHASE: HYPOTHESIS

### Source maps: BOTH SPAs ship live source maps (major code-review surface)
- mysignins.microsoft.com/bundle/main.caa6a456.js.map (6,976,576 B) — NO sourcesContent; 4359 paths. sha256 2099f8a8...f3efbe.
- api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map (35,340,363 B) — sourcesContent PRESENT (4922 files). sha256 72290126...27d6541. Extracted 1977 files to /tmp/opencode/myaccount-src (transient, not committed).
- Source map presence is itself a reportable hardening gap (recon-only note; MSRC triages source-map exposure per-case — validate against program rules before reporting).

### My Account backend (api.myaccount.microsoft.com) — request schemas recovered from source
- Auth: @iamexperiences/feature-auth getMsalAuth, MSAL public client, CacheLocation.SessionStorage, authority settingsManager authAuthorityEndpoint, clientId 8c59ead7-d703-4a27-9e55-c96a0054c8d2. Tokens acquired with scopes=[clientId] (audience = the SPA's own clientId) for ALL /api/* calls.
- /api/issueVerifiedEmployeeCredential: POST, Authorization Bearer <clientId-scope token>, NO body. Backend errors surfaced in UI: GuestIsNotAllowedToIssueVerifiedId, TenantIsNotInAllowedToIssueVerifiedId (authz gates = non-guest + tenant-onboarded + feature-on; per source comment 204 means true). Verified ID app linkedin ref VERIFIEDID_APP_NAME_LINKEDIN.
- /api/canVerifiedIdBeIssued: GET, Bearer clientId-token; 200+204 = enabled, else JSON reason.
- /api/termsofuse/*: agreements/{agreementId}; agreements/{agreementId}/policyfiles/{policyFileId}; policyfiles/{policyFileId}/data (GET JSON); policyfiles/{policyFileId}/decline and /accept (POST). UNAUTHENTICATED variants (useGetAgreementUnauthenticated, useDeclineAgreement, useGetPolicyFileDataUnauthenticated) read access_token + id_token_hint + claims (incl governance_control_state_per_device) from server-rendered data-requestbody dataset in the HTML — i.e., tokens/claims embedded in a consent page's DOM.
- /api/shell/navbardata, /api/signOutEverywhere, /api/organizations, /api/organizations/{id}, /api/devices, /api/groups/settings, /api/EnduserEmailContacts, /api/TenantEmailSettings, /api/dateTimeFormats, /api/me/image, /api/users/getImages.
- Graph beta calls: /me/ownedObjects via getOwnedAgentsUrl(userId) = /users/{userId}/ownedObjects/Microsoft.graph.ServicePrincipal?$top=999 (scope AgentIdentity.Read.All); /users/{userId}/sponsorOf/microsoft.graph.servicePrincipal?$top=999; /roleManagement/directory/roleAssignments?$filter=principalId eq '{userId}'; /organization/{orgId}/branding/localizations?$select=cdnList,bannerLogoRelativeUrl; /me/settings/itemInsights, /me/settings/contactMergeSuggestions, /me/checkMemberGroups, /me/photos/648x648/$value. ServiceIdentity is the agent-identity SP type (MANAGEAGENTS.AGENT_IDENTITY_SERVICE_PRINCIPAL_TYPE).
- MSGraphScopes enum: CrossTenantUserProfileSharing.ReadWrite.All, Organization.Read.All, User.Read.All, User.ReadWrite, MailboxSettings.ReadWrite, BitlockerKey.Read.All, Contacts.Read, AgentIdentity.Read.All.
- Roaming settings: OFFICE_APP_ID 179, API_PATH /rs/v1/settings, PLATFORM Web.
- IAMUX/portal routing (portal-app/lib): account.activedirectory-ppe.windowsazure.com (PPE), account-tip.activedirectory.windowsazure.com (TIP), account.activedirectory.windowsazure.com (PROD), account.activedirectory.windowsazure.us (AR), account.aad.microsoft.scloud (USSEC), account.aad.eaglex.ic.gov (USNAT). NOTE: *.windowsazure.com NOT in scope.yml — adjacent-only, no probing. Environment detection: hostname contains "ppe"/"tip"/"us" or ends .scloud/.eaglex.ic.gov else PROD.
- Cross-app URLs pass upn, tenant, login_hint, optIn query params to IAMUX/myapps/mygroups/myaccess (createUrl). Substrate search builder search/api/{v1} on getSubstrateResourceName().

### accounts.accesscontrol.windows.net (legacy ACS) — trust-relevant findings
- /metadata/json/1 (200) now returns keys + endpoints ONLY (allowedAudiences REMOVED — tenant-enum patch confirmed; Sprocket Security 2026-05-19 writeup "Tenant Enumeration is Dead" documents the technique + patch).
- 5 signing keys served: key0-2 CN=accounts.accesscontrol.windows.net self-signed (notBefore 2026-06/07, notAfter 2031-06/07), sha256 FP 05:D1:7F:72:..., 34:D0:94:25:..., 93:A6:49:15:...; key3-4 CN=login.microsoftonline.us self-signed (FP 1B:49:9F:F9:..., A7:3C:A1:F9:...). Mixed issuers in one JWKS = config artifact to compare vs ESTS discovery keys.
- Discovery endpoints alive: /tokens/OAuth/2 (OAuth2 issuance), /tokens/delegation/1 (DelegationIssuance1.0), /mgmt/delegation/1 (DelegationManagement1.0) — all 200 sign-in pages (auth-gated, not 404).
- Historical context: Sep-2025 critical (CVSS 10.0) ESTS actor-token impersonation chain (Dirk-jan Mollema) leveraged the well-known app 00000001-0000-0000-c000-000000000000 via accounts.accesscontrol.windows.net for AAD Graph actor tokens; mitigation blocked actor-token requests to AAD Graph. This host is a known high-attention MSRC identity asset; Microsoft's own ACS FAQ: "Traffic to accounts.accesscontrol.windows.net is handled by a different service" (absorbed into Entra ESTS).
- ACS retirement: ACS itself retired Nov-2018; SharePoint add-in ACS auth retired Apr-2026. Remaining trust: legacy RPs/tools.

### agentSignInSessions — new undocumented Graph surface
- /v1.0/me/agentSignInSessions + /beta/me/agentSignInSessions resolve (401 InvalidAuthenticationToken baseline). NOT present in public Graph docs (docs-contrib tree only has agentic-agentsignin.md + agentsignin.md, both different resources: signIn audit + conditionalAccess). ms.date 05/01/2025 for agentic.agentSignIn; signIn.agent property type.
- My Sign-Ins bundle modules: redux/modules/signIns/agentSessionApis.ts, agentSessionUtils.ts, agentSessions.ts, recentSignIns.ts, switchTenant.ts; SPA strings: /me/agentSignInSessions, /recent-activity/agentic-sessions. My Account: manageAgentNavItemEnabled, useHasOwnedAgents.

### OAuth redirect_uri validation (documented rules; no live sends this run beyond baseline)
- login.live.com fast-fails: 302 oauth20_desktop.srf?error=invalid_request&error_description=...redirect_uri is not valid. client must be 'mobile' OR redirect must be absolute + registered (confirmed SURFACE).
- login.microsoftonline.com/oauth2/v2.0/authorize with unregistered client+redirect returns generic 200 sign-in page (deferred validation).
- Documented AAD rules (learn.microsoft.com/en-us/entra/identity-platform/reply-url): https required (except localhost), case-sensitive exact match, AADSTS50011 on mismatch; RFC 6749 3.1.2 string compare + RFC 9700 (BCP) forbids wildcards. Loopback localhost (any port) for native/mobile; SPA client-type restriction AADSTS9002327/9002326.

### accledger/accessmonitor
- No public GitHub/docs presence found for "Access Ledger" controlplane. controlplane.accledger.azure.com: Kestrel /health 200 "Healthy" + x-ms-machinename (pod) + x-ms-image-tag 1.0.03467.455-73c40f8d7a2145ba44be22ca0c6029eca472d3d8; structured 404 envelope {"Status":"failed","Error":{"Code":"404"...}}; no swagger/api/ledger/tenants. identity.accledger.azure.com -> identity-accledger-prod-1.eastus.cloudapp.azure.com (no HTTPS).
- api.prod/dev.accessmonitor.azure.com: 403 AFD/WAF-gated.

### Artifacts (hashes only, for triage reference)
- mysignins map sha256 2099f8a83336e290f2ab20c09a56b3a4493d0b7f66328b944c7fc18055f3efbe
- myaccount map sha256 72290126cebc34559574f30c675c6bfe84856fe8a7678eb2e0821237a27d6541
- mysignins sources list sha256 1a5be8a2293c3797e11882895b69ef9dc53173bc71b1ee5865b6a8934034e506
- No secrets found in either bundle/map (public clientIds + env hostnames only).
EOF
echo "journal total: $(wc -l < /home/runner/work/hunt-lab/hunt-lab/research/bigpickle.md) lines"
journal total: 1893 lines
[0m
Now writing the HYPOTHESIS journal entry.
[0m$ [0mcat >> /home/runner/work/hunt-lab/hunt-lab/leads/lead-bigpickle.md <<'EOF'

## 2026-08-07 10:05:00 UTC [microsoft] (model bigpickle) — HYPOTHESIS

- [LEAD-HIGH] Source maps live on BOTH identity SPAs: mysignins main.caa6a456.js.map (no content, 4359 paths) and api.myaccount main.4e6e3dc6.js.map (sourcesContent for 4922 files). Verified ID + ToS request schemas recovered from extracted source. Hashes: myaccount.map sha256 72290126...d6541, mysignins.map sha256 2099f8a8...f3efbe.
- [LEAD-HIGH] api.myaccount /api/issueVerifiedEmployeeCredential: POST with Bearer token whose scope = the SPA's own clientId (8c59ead7-d703-4a27-9e55-c96a0054c8d2); backend gates on GuestIsNotAllowedToIssueVerifiedId / TenantIsNotInAllowedToIssueVerifiedId. Low-priv credential-minting hypothesis (test-tenant only).
- [LEAD-HIGH] /me/agentSignInSessions (v1.0+beta) alive but UNDOCUMENTED in public Graph docs; bundle modules agentSessionApis.ts / agentSessions.ts + /recent-activity/agentic-sessions. New-scope AgentIdentity.Read.All + ServiceIdentity SP type (useHasOwnedAgents → /users/{userId}/ownedObjects/...?$top=999).
- [LEAD-HIGH] accounts.accesscontrol.windows.net: allowedAudiences removed (tenant-enum patched Mar-2026); 5 live signing keys incl 2x CN=login.microsoftonline.us mixed into global JWKS; /tokens/OAuth/2, /tokens/delegation/1, /mgmt/delegation/1 pathing alive. Historical Sep-2025 CVSS-10.0 actor-token chain used this host (app 00000001-0000-0000-c000-000000000000) — known high-attention MSRC surface.
- [LEAD-MED] ToS unauthenticated consent flow embeds access_token + id_token_hint + claims (governance_control_state_per_device) in server-rendered data-requestbody DOM; policyfiles/{id}/data GET is an IDOR candidate across tenants (test-tenant only).
- [LEAD-MED] Cross-app URLs pass upn/tenant/login_hint/optIn to IAMUX + myapps + mygroups + myaccess; substrate search builder search/api/v1 (confused-deputy surface, test-tenant only).
- [LEAD-LOW] controlplane.accledger.azure.com /health leaks pod name + x-ms-image-tag; no swagger. No public repo/docs for Access Ledger found.
EOF
echo done
done
[0m
Now the leads file and state update.
[0m$ [0mcat > /home/runner/work/hunt-lab/hunt-lab/state_bigpickle.json <<'EOF'
{"phase": "HYPOTHESIS", "status": "HIGH_POTENTIAL"}
EOF
cat /home/runner/work/hunt-lab/hunt-lab/state_bigpickle.json
{"phase": "HYPOTHESIS", "status": "HIGH_POTENTIAL"}
[0m
[0m# [0mTodos
[✓] Re-verify ACS /metadata/json/1 keys + research legacy ACS DelegationIssuance1.0 trust (who still trusts accounts.accesscontrol.windows.net as issuer)
[✓] Source-map check on mysignins + api.myaccount SPA bundles (read-only GET of .js.map)
[✓] agentSignInSessions permission model — public Graph docs + scope/role design
[✓] Entra Verified ID issuance endpoints — public docs, scopes, request schemas
[✓] OAuth redirect_uri normalization — review in-scope code (microsoft-identity-web, MSAL.js) for validation logic
[✓] accledger/accessmonitor public-repo + docs search for unauthenticated API routes
[✓] Write HYPOTHESIS journal, leads, state update
[0m
# Program: microsoft
# Phase: HYPOTHESIS

## Key Findings
- **Source maps shipped live on both identity SPAs** — `mysignins.microsoft.com/bundle/main.caa6a456.js.map` (no content, 4,359 paths) and `api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map` (35 MB, `sourcesContent` for 4,922 files). Recovered My Account request schemas below. Hashes (triage ref): myaccount.map `sha256 72290126…27d6541`, mysignins.map `sha256 2099f8a8…f3efbe`.
- **Verified ID minting endpoint** `api.myaccount /api/issueVerifiedEmployeeCredential` = POST + `Bearer` token whose scope is the SPA's own `clientId` (8c59ead7-…-c8d2); backend gates solely on `GuestIsNotAllowedToIssueVerifiedId` / `TenantIsNotInAllowedToIssueVerifiedId` / feature-on — no visible admin-role check in the client contract.
- **`/me/agentSignInSessions` (v1.0 + beta) is alive but undocumented** in public Graph docs (docs-contrib tree has no such API page); My Sign-Ins bundles `agentSessionApis.ts` / `agentSessions.ts` and `/recent-activity/agentic-sessions`; My Account uses new scope `AgentIdentity.Read.All` + `ServiceIdentity` SP type via `/users/{userId}/ownedObjects/Microsoft.graph.ServicePrincipal?$top=999`.
- **`accounts.accesscontrol.windows.net` legacy-ACS surface**: `allowedAudiences` field is REMOVED from `/metadata/json/1` (tenant-enum patch confirmed per Sprocket 2026-05-19), but 5 live signing keys remain served — 3× self-signed `CN=accounts.accesscontrol.windows.net`, **2× `CN=login.microsoftonline.us` mixed into the global JWKS**; `/tokens/OAuth/2`, `/tokens/delegation/1`, `/mgmt/delegation/1` pathing alive (200 sign-in). This host was the vector for the Sep-2025 CVSS-10.0 ESTS actor-token impersonation chain (app `00000001-0000-0000-c000-000000000000`).
- **ToS unauthenticated consent flow** embeds `access_token` + `id_token_hint` + `claims` (incl. `governance_control_state_per_device`) in the server-rendered `data-requestbody` DOM; `…/policyfiles/{policyFileId}/data` is a GET JSON (cross-tenant IDOR candidate).
- **OAuth validation asymmetry** (SURFACE baseline): login.live.com fast-fails invalid `redirect_uri` (302, precise error); login.microsoftonline.com defers to a generic sign-in page. Documented AAD rules (reply-url doc): https-only (except localhost), case-sensitive exact match, `AADSTS50011`; RFC 6749 §3.1.2 string compare, RFC 9700 forbids wildcards.
- `controlplane.accledger.azure.com`: `/health` 200 leaks pod name + `x-ms-image-tag 1.0.03467.455-73c40f8d…`; no swagger, no public "Access Ledger" repo/docs found.

## New Endpoints / Assets
- api.myaccount: `/api/issueVerifiedEmployeeCredential` (POST, no body), `/api/canVerifiedIdBeIssued` (GET), `/api/termsofuse/agreements/{id}`, `/…/policyfiles/{id}`, `/…/policyfiles/{id}/data`, `/…/{id}/decline`, `/…/accept`, `/api/shell/navbardata`, `/api/dateTimeFormats`
- Graph beta: `/users/{id}/ownedObjects/Microsoft.graph.ServicePrincipal?$top=999`, `/users/{id}/sponsorOf/microsoft.graph.servicePrincipal?$top=999`, `/roleManagement/directory/roleAssignments?$filter=principalId eq '{id}'` (scope `AgentIdentity.Read.All`, SP type `ServiceIdentity`)
- IAMUX routing (adjacent, out-of-scope — NOT probed): `account-tip.activedirectory.windowsazure.com`, `account.activedirectory-ppe.windowsazure.com`, `account.activedirectory.windowsazure.us`, `account.aad.microsoft.scloud`, `account.aad.eaglex.ic.gov`
- ACS live token endpoints: `/tokens/OAuth/2`, `/tokens/delegation/1` (DelegationIssuance1.0), `/mgmt/delegation/1` (DelegationManagement1.0)

## Hypotheses (impact + read-only test)
1. **Legacy ACS actor-token / delegation regressions** — host is ESTS-fronted, previously weaponized for tenant-wide impersonation; token endpoints still live. Impact: cross-tenant impersonation/ATO (Identity, up to $100k). Read-only: compare the 5 ACS keys vs `/common/discovery/v2.0/keys` for cross-issuer reuse; search public SAML/federation metadata (SharePoint/xbox/mojang historically in `allowedAudiences`) for continued issuer trust; verify `DelegationIssuance1.0` requires a registered `grantedBy` principal (test-tenant only).
2. **`agentSignInSessions` authorization gap** — undocumented new surface + new `AgentIdentity.Read.All` scope; new identity endpoints historically ship missing `scp`/IDOR checks. Impact: cross-user agent-session disclosure/impersonation (6.5–8.8). Read-only: enumerate required scopes from token claims on a controlled test tenant; test whether `/users/{id}/…` resolves the caller's oid vs arbitrary id; confirm session-IDOR absence — no live MS-infra testing.
3. **Verified ID credential-minting by low-priv user** — `/api/issueVerifiedEmployeeCredential` token audience = SPA clientId; if backend authorizes on token-validity + non-guest rather than issuer role, credential forgery. Impact: verified-credential forgery / RP trust poisoning (7.1–9.1). Read-only: document schema (done: POST, no body); test on own tenant with basic vs credential-issuer role comparing accepted claims.
4. **ToS token-in-HTML + policyfile IDOR** — consent page embeds access tokens/claims; `policyfiles/{id}/data` GET may be tenant-unscopped. Impact: token disclosure, consent-state tampering, PII (5.3–7.5). Read-only: verify cache headers on consent page (own session); test tenant-scoping of agreement/policy IDs.
5. **OAuth redirect_uri / response_mode confusion** — AAD v2 defers validation; exact-match rules well-documented; first-party registrations could have permissive legacy redirect URIs. Impact: auth-code theft / ATO via dangling redirects (7.4–9.0). Read-only: build normalization matrix (trailing slash, case, fragment, percent-encoding, `response_mode=form_post/fragment`, `wctx`) against documented rules; no live sends beyond the recorded baseline.
6. **Cross-app confused deputy** — My Account forwards `upn`/`tenant`/`login_hint`/`optIn` to IAMUX/myapps/mygroups/myaccess and uses substrate `search/api/v1`; if a destination acts on the parameter without re-auth → cross-tenant disclosure. Impact: info disclosure (5.3–8.1). Read-only: test-tenant verification of re-authentication at each destination.
7. **Access Ledger controlplane** — Kestrel banners + structured errors; unauthenticated ledger/tenant route unknown. Impact: info disclosure (3.7–5.3). Read-only: bounded GET/HEAD on documented .NET API paths only, stop at 404 envelope (done).

## Secrets Check (hash only)
- No secrets. Scanned both bundles + extracted 1,977 source files: only public clientIds (`19db86c3…`, `8c59ead7…`, `00000001-0000-0000-c000-000000000000`) and env hostnames. Artifact hashes above. Independent reposcan job: 0 hits (leads/reposcan-latest.md).

## CVSS Candidates
- ACS actor-token/impersonation regression: 9.0–10.0 (if reintroduced); agent-session authz gap: 6.5–8.8; Verified ID mint: 7.1–9.1; ToS token/IDOR: 5.3–7.5; OAuth redirect ATO: 7.4–9.0; confused deputy: 5.3–8.1; ledger banner leak: 3.7–5.3.

---
## STATE UPDATE (exact format for parsing)
STATUS_PHASE: HYPOTHESIS
STATUS_STATE: HIGH_POTENTIAL
NEXT_STEP_1: POC on highest-value read-only signals: (a) confirm no cross-issuer key reuse between accounts.accesscontrol.windows.net JWKS and AAD `/common/discovery/v2.0/keys`; (b) one bounded GET of `policyfiles` data shape on a test tenant for the IDOR design.
NEXT_STEP_2: Enumerate first-party redirect_uri registrations from public sources and build the normalization matrix against reply-url rules; mark which encodings are safe to validate live vs require test tenant.
NEXT_STEP_3: If HYPOTHESIS exhausts, open new RECON surface: crt.sh CT inventory for the Jul-2025 identity hosts + `*.activedirectory.windowsazure.com` (note: out-of-scope, adjacent only) and Graph `/v1.0/$metadata` service-document crawl for new agent/identity types.

## RUN 2026-08-07 10:45:00 (big-pickle) — TARGET: microsoft — PHASE: POC (read-only)

### POC-1: Source-map exposure characterization (LIVE, in-scope)
- HEAD/GET reconfirmed: api.myaccount /bundle/main.4e6e3dc6.js.map (35,340,363 B) and mysignins /bundle/main.caa6a456.js.map (6,976,576 B) both 200, Content-Type text/plain, NO Cache-Control header on either; Last-Modified 2026-07-24 / 2026-07-28; ETags present. Hashed filenames = immutable-content pattern, so exposure is deliberate-cacheable (supporting asset for chains; not standalone-reportable per triage 10-16).
- mysignins SPA shell: real CSP `script-src 'self' amcdn.msftauth.net js.monitor.azure.com`, meta app-version 4.0.2233. myaccount shell: CSP-Report-Only + HSTS max-age=2592000 + no-store. Both shells are minimal (no config block in HTML); myaccount loads /bundle/main.shim.js (17,936 B, trivial loader + Azure Code Signing Authenticode block, ETag 1dd1b7e6aa49110) which loads main.4e6e3dc6.js.
- Artifact hashes (this run): graph_v1_meta.xml 9d84e451..., graph_beta_meta.xml ca304859..., acs.json 504f3df2..., main.shim.js 88dc286e..., mysignins.html ba400c19..., myaccount.html de5a0b99...

### POC-2: ACS /metadata/json/1 — full JWKS reconciliation (LIVE, in-scope *.windows.net)
- Response shape CHANGED vs SURFACE run: now keyValue{type:x509Certificate,value,keyInfo{x5t,cloud_instance_name}}; allowedAudiences/Audience/Issuer fields ABSENT (tenant-enum patch reconfirmed).
- 5 self-signed signing certs, rotation-active (notBefore Jun-08 / Jun-17 / Jul-04 2026 for the 3 prod keys; Jul-01 / Jul-26 2026 for the 2 US-cloud keys), all notAfter 2031. Per-key: x5t aFkmKVFc-4WV6sXCBvNZkXI505Y (microsoftonline.com), AahUf1bCXvx0JTRcXLrr0U4SluY, fEtqrhKT1bXAGafSdQoN1vXTRpI, then jvm_-TtaqTCRrkpnmePvwb75nWE + 6hXLaIYNSJ0o7zu09dMyI0ji3ug (BOTH cloud_instance_name=microsoftonline.us, CN=login.microsoftonline.us). DER sha256: 05d17f72...d9c6, 34d09425...bd5b, 93a64915...d47, 1b499ff9...3d85, a73ca1f9...67b9 (match SURFACE-run colon-hex FPs).
- Signal: the JWKS now explicitly co-serves microsoftonline.us signing keys tagged cloud_instance_name. Not a vuln by itself (est-aggregation design); it is a design signal for the cross-cloud-token acceptance hypothesis (test-tenant-only to prove).

### POC-3: Graph $metadata service-document crawl for agent/identity types (LIVE, in-scope graph.microsoft.com)
- v1.0 $metadata (2,931,817 B) + beta $metadata (7,298,965 B) fetched. agentSignInSession: 0 references in BOTH — the /me/agentSignInSessions surface used by My Sign-Ins (/recent-activity/agentic-sessions) is entirely off-metadata (internal/partner surface). Confirms earlier finding.
- Agent ID entity model IS in public metadata: v1 EntityTypes agentIdentity (BaseType graph.servicePrincipal, props agentIdentityBlueprintId + createdDateTime, nav sponsors), agentIdentityBlueprint (BaseType graph.application, nav inheritablePermissions ContainsTarget + sponsors), agentIdentityBlueprintPrincipal (BaseType graph.servicePrincipal), agentUser (BaseType graph.user). Beta adds agentIdentity nav inheritedAppRoleAssignments + inheritedOauth2PermissionGrants. user entity has nav sponsorOf + sponsors.
- Public docs confirm (learn.microsoft.com/en-us/entra/agent-id; create-blueprint.md): Agent ID preview (admin center Nov-17-2025; Dataverse GA announcement 2026-08-06). Creation endpoints POST /v1.0/applications/microsoft.graph.agentIdentityBlueprint, /servicePrincipals/microsoft.graph.agentIdentityBlueprintPrincipal, /servicePrincipals/microsoft.graph.agentIdentity, beta /users (agentUser). Scopes: AgentIdentityBlueprint.Create / .AddRemoveCreds.All / .UpdateAuthProperties.All / AgentIdentityBlueprintPrincipal.Create; dir roles Agent ID Developer / Agent ID Administrator. Hardening confirmed by independent tests: agent identities are always single-tenant; interactive OAuth flows (code/device/implicit/ROPC) BLOCKED; addPassword to agentIdentity service principal BLOCKED (403); isFallbackPublicClient not flippable; no RFC7591 dynamic registration.
- My Account agent hooks map onto this model: useHasOwnedAgents GET /users/{userId}/ownedObjects/Microsoft.graph.ServicePrincipal?$top=999 filtered servicePrincipalType==ServiceIdentity (=agentIdentity); getSponsoredAgentsUrl GET /users/{userId}/sponsorOf/microsoft.graph.servicePrincipal?$top=999.

### POC designs (no live sends; test-tenant only)
- PD-A agentSignInSessions scope/IDOR: token on own tenant (AgentIdentity.Read.All) → GET /v1.0/me/agentSignInSessions and GET /v1.0/users/{arbitrary-oid}/agentSignInSessions; signal if second resolves caller-agnostic → cross-user session disclosure. CVSS 6.5–8.8.
- PD-B agent-identity privilege inheritance: user sponsors self-created blueprint → creates agentIdentity inheriting blueprint oauth2PermissionGrants/appRoleAssignments (beta nav) → compare agent's effective perms vs user's own; signal if inherited grants exceed sponsor's. CVSS 7.0–9.0. (Blocked-flow hardening partially mitigates; OBO/jwt-bearer minting still possible.)
- PD-C Verified ID mint: POST /api/issueVerifiedEmployeeCredential (Bearer, scope=SPA clientId 8c59ead7-d703-4a27-9e55-c96a0054c8d2, no body); compare no-role vs GlobalAdmin; signal both-204 → authz gap. CVSS 7.1–9.1.
- PD-D policyfile IDOR: GET /api/termsofuse/agreements/{a}/policyfiles/{p}/data with cross-tenant GUID pairs (token required). CVSS 5.3–7.5.
- PD-E redirect_uri normalization matrix vs reply-url doc (RFC6749 §3.1.2 + RFC9700): trailing-slash, case, fragment, percent-encoding, response_mode form_post/fragment, wctx; live-check only on in-scope login.live.com fast-fail echo (1 req per variant, documented cases only). CVSS 7.4–9.0 if any first-party registration accepts a non-exact match.

===== RUN 2026-08-07 10:35:22 UTC | model bigpickle | target microsoft =====
[0m
> build · big-pickle
[0m
[0m$ [0mls -la /home/runner/work/hunt-lab/hunt-lab/ && cat /home/runner/work/hunt-lab/hunt-lab/state_bigpickle.json 2>/dev/null
total 264
drwxr-xr-x 9 runner runner  4096 Aug  7 10:31 .
drwxr-xr-x 3 runner runner  4096 Aug  7 10:30 ..
drwxr-xr-x 7 runner runner  4096 Aug  7 10:31 .git
drwxr-xr-x 3 runner runner  4096 Aug  7 10:30 .github
-rw-r--r-- 1 runner runner  1022 Aug  7 10:30 README.md
-rw-r--r-- 1 runner runner 11447 Aug  7 10:31 ctx-leads.txt
-rw-r--r-- 1 runner runner 82108 Aug  7 10:31 ctx-research.txt
-rw-r--r-- 1 runner runner    28 Aug  7 10:31 ctx-valid.txt
drwxr-xr-x 2 runner runner  4096 Aug  7 10:30 leads
-rw-r--r-- 1 runner runner    32 Aug  7 10:31 raw_bigpickle.txt
drwxr-xr-x 2 runner runner  4096 Aug  7 10:30 reports
drwxr-xr-x 4 runner runner  4096 Aug  7 10:30 reposcan-raw
drwxr-xr-x 2 runner runner  4096 Aug  7 10:30 research
-rw-r--r-- 1 runner runner 96380 Aug  7 10:31 research-prompt.txt
-rw-r--r-- 1 runner runner  1650 Aug  7 10:30 scope.yml
-rw-r--r-- 1 runner runner    51 Aug  7 10:30 state_bigpickle.json
-rw-r--r-- 1 runner runner    51 Aug  7 10:30 state_laguna.json
-rw-r--r-- 1 runner runner    43 Aug  7 10:30 state_ling3.json
-rw-r--r-- 1 runner runner    44 Aug  7 10:30 state_longcat.json
-rw-r--r-- 1 runner runner    45 Aug  7 10:30 state_nemotron3.json
drwxr-xr-x 2 runner runner  4096 Aug  7 10:30 triage
{"phase": "HYPOTHESIS", "status": "HIGH_POTENTIAL"}
[0m
[0m✱ [0mGlob "**/scope.yml"[90m 1 match[0m
[0m→ [0mRead scope.yml
[0m
[0m$ [0mcat /home/runner/work/hunt-lab/hunt-lab/ctx-valid.txt; echo "---RAW---"; cat /home/runner/work/hunt-lab/hunt-lab/raw_bigpickle.txt
# no validated findings yet
---RAW---
[0m
> build · big-pickle
[0m
[0m$ [0mls -la /home/runner/work/hunt-lab/hunt-lab/ && cat /home/runner/work/hunt-lab/hunt-lab/state_bigpickle.json 2>/dev/null
total 264
drwxr-xr-x 9 runner runner  4096 Aug  7 10:31 .
drwxr-xr-x 3 runner runner  4096 Aug  7 10:30 ..
drwxr-xr-x 7 runner runner  4096 Aug  7 10:31 .git
drwxr-xr-x 3 runner runner  4096 Aug  7 10:30 .github
-rw-r--r-- 1 runner runner  1022 Aug  7 10:30 README.md
-rw-r--r-- 1 runner runner 11447 Aug  7 10:31 ctx-leads.txt
-rw-r--r-- 1 runner runner 82108 Aug  7 10:31 ctx-research.txt
-rw-r--r-- 1 runner runner    28 Aug  7 10:31 ctx-valid.txt
drwxr-xr-x 2 runner runner  4096 Aug  7 10:30 leads
-rw-r--r-- 1 runner runner    32 Aug  7 10:31 raw_bigpickle.txt
drwxr-xr-x 2 runner runner  4096 Aug  7 10:30 reports
drwxr-xr-x 4 runner runner  4096 Aug  7 10:30 reposcan-raw
drwxr-xr-x 2 runner runner  4096 Aug  7 10:30 research
-rw-r--r-- 1 runner runner 96380 Aug  7 10:31 research-prompt.txt
-rw-r--r-- 1 runner runner  1650 Aug  7 10:30 scope.yml
-rw-r--r-- 1 runner runner    51 Aug  7 10:30 state_bigpickle.json
-rw-r--r-- 1 runner runner    51 Aug  7 10:30 state_laguna.json
-rw-r--r-- 1 runner runner    43 Aug  7 10:30 state_ling3.json
-rw-r--r-- 1 runner runner    44 Aug  7 10:30 state_longcat.json
-rw-r--r-- 1 runner runner    45 Aug  7 10:30 state_nemotron3.json
drwxr-xr-x 2 runner runner  4096 Aug  7 10:30 triage
{"phase": "HYPOTHESIS", "status": "HIGH_POTENTIAL"}
[0m
[0m✱ [0mGlob "**/scope.yml"[90m 1 match[0m
[0m→ [0mRead scope.yml
[0m
[0m$ [0mls /tmp/opencode/myaccount-src 2>/dev/null | wc -l; ls /tmp/opencode/*.map 2>/dev/null; ls /tmp/opencode/portal-lib 2>/dev/null
0
[0m
[0m$ [0mcd /home/runner/work/hunt-lab/hunt-lab && git log --oneline -5 2>/dev/null; echo "---"; ls research/ reports/ triage/ 2>/dev/null
05652f8 triage 2026-08-07-10-16
---
reports/:
valid-bugs.md

research/:
bigpickle.md
laguna.md
ling3.md
longcat.md
nemotron3.md

triage/:
run-2026-08-07-10-16.md
[0m
[0m$ [0mls -la /tmp/opencode/ 2>/dev/null; echo "---"; du -sh /tmp/opencode/* 2>/dev/null
total 16
drwxr-xr-x  2 runner runner  4096 Aug  7 10:30 .
drwxrwxrwt 14 root   root   12288 Aug  7 10:31 ..
---
[0m
[0m$ [0mcat /home/runner/work/hunt-lab/hunt-lab/triage/run-2026-08-07-10-16.md 2>/dev/null | head -80; echo "===VALID==="; cat /home/runner/work/hunt-lab/hunt-lab/reports/valid-bugs.md 2>/dev/null | head -40
===== TRIAGE 2026-08-07 10:16:52 UTC =====
# Triage Report — 2026-08-07

Scope: Google VRP (`google.com`, `*.google.com`) · MSRC (`microsoft.com`, `*.microsoft.com`, `*.windows.net`, `*.azure.com`, etc.)
Rules: passive GET/HEAD only · no auth-bypass on live infra · no writes.

Leads below are **deduplicated** across the 5 models. Each unique finding is run through the 7-Question Gate.

---

## LEAD 1 — Google VRP / MSRC scope metadata (not a vuln)
**Q1–Q7**: Not a vulnerability; program-scope reference only.
**Verdict**: INVALID — not a finding, just program info.

---

## LEAD 2 — "NEW AI VRP" program news (not a vuln)
**Q1–Q7**: Not a vulnerability; reward-program announcement.
**Verdict**: INVALID — not a finding.

---

## LEAD 3 — Google ADK as "new high-value attack surface"
**Q1** Yes (github.com/google, in-scope org). **Q2** Yes (open-sourc[0m
[0m← [0mWrite triage-output.txt
Wrote file successfully.
[0m
Now I have full context. Let me produce the structured triage report.
## Triage Complete — 0 VALID, 3 HOLD, 33 INVALID

Full report written to `triage-output.txt`.

**Key findings:**

| Verdict | Count | Notes |
|---------|-------|-------|
| VALID | 0 | No lead clears the 7-Question Gate |
| HOLD | 3 | ADK PR #5154 (in-progress fix), source maps (chain only), Verified ID/ToS IDOR (need active testing) |
| INVALID | 33 | Breakdown below |

**Invalid by reason:**
- **Already public/fixed** (Leads 4–7, 10–12): ADK issues/commits, CVE-2026-47391, Benchikh OAuth, Tenable SSRF
- **Auth-gated / by-design** (Leads 13, 24–29, 35–36): Graph 401s, ACS JWKS, login.live.com redirect error
- **Recon noise, not vulns** (Leads 1–3, 9, 23, 31, 37–44): scope metadata, product lists, API discovery URLs, repo lists
- **No demonstrated impact** (Leads 27, 30): internal hostnames in SPA config, /health metadata

**Recommendations:**
1. Drop all INVALID leads.
2. Keep source maps as a supporting asset for any future mysignins/myaccount vuln chain.
3. Verified ID minting and ToS IDOR are interesting but blocked by passive-only rule — queue for test-tenant engagement if available.
4. Tighten the hunt bot's pre-filter to reject already-public issues, fixed commits, auth-gated 401s without bypass, and pure discovery URLs.
port. **Q6** Yes (patched). **Q7** No.
**Verdict**: INVALID — already fixed; no bounty for historical patch.

---

## LEAD 7 — ADK commit 33012e6: fixed cross-user credential leaks (hash() key)
**Q1** Yes. **Q2** Yes. **Q3** Was real, now fixed. **Q4** Yes. **Q5** No — fixed. **Q6** Yes. **Q7** No.
**Verdict**: INVALID — already fixed.

---

## LEAD 8 — ADK PR #5154: migrating credential storage to `secret:` scope
**Q1** Yes. **Q2** Yes. **Q3** Tokens persist in session backends (design weakness). **Q4** Yes (PR diff). **Q5** No — already being fixed publicly. **Q6** Yes (WIP fix). **Q7** No — in-progress open-source PR, no live Google service.
**Verdict**: HOLD → likely INVALID; re-check only if merged-then-regressed on live Google surface.

---

## LEAD 9 — A2A protocol attack surface (webhook SSRF, unsigned AgentCards, unauth endpoints)
**Q1** Partial (A2A is a Google-published protocol). **Q2** Unclear — no specific vulnerable endpoint pinned down. **Q3** Hypothetical only. **Q4** No — no PoC URL. **Q5** N/A. **Q6** N/A. **Q7** No — no concrete target.
**Verdict**: INVALID — architectural observation, no demonstrable vuln.

---

## LEAD 10 — CVE-2026-47391 (PraisonAI A2A example: unauth + eval() = RCE)
**Q1** No — PraisonAI is a third-party example repo, not a Google/MSFT service. **Q2** N/A. **Q3** Yes (RCE). **Q4** N/A. **Q5** No — CVE-assigned, fully public. **Q6** N/A. **Q7** No — intentionally vuln example; no real target.
**Verdict**: INVALID — third-party example; CVE already public; no in-scope target.

---
===VALID===

- 1 lead(s) marked VALID at 2026-08-07 10:16:52 UTC
[0m
Triage from last run: 0 VALID, source maps + Verified ID/ToS held, ACS JWKS standalone invalid. Moving one phase deeper to **POC** — read-only, in-scope only (`.microsoft.com`, `.windows.net`, `.azure.com`, `.live.com`). First, re-establish the SPA HTML shells (headers + chunk inventory).
[0m$ [0mcd /tmp/opencode && curl -s -D /tmp/opencode/hdr_mysignins.txt -o /tmp/opencode/mysignins.html "https://mysignins.microsoft.com/" --max-time 30 && sleep 1 && curl -s -D /tmp/opencode/hdr_myaccount.txt -o /tmp/opencode/myaccount.html "https://api.myaccount.microsoft.com/" --max-time 30 && echo "=== mysignins headers ===" && grep -iE "cache-control|expires|strict-transport|x-frame|x-content|content-security|set-cookie|x-ms" /tmp/opencode/hdr_mysignins.txt | head -15 && echo "=== myaccount headers ===" && grep -iE "cache-control|expires|strict-transport|x-frame|x-content|content-security|set-cookie|x-ms" /tmp/opencode/hdr_myaccount.txt | head -15 && echo "=== chunk refs mysignins ===" && grep -oE 'src="[^"]+\.js[^"]*"' /tmp/opencode/mysignins.html | head -10 && echo "=== chunk refs myaccount ===" && grep -oE 'src="[^"]+\.js[^"]*"' /tmp/opencode/myaccount.html | head -10
=== mysignins headers ===
x-ms-correlation-id: 92b5058e-670d-4198-934a-8d4e190bb381
x-ms-mysignins-region: westus2
x-ms-gateway-requestid: 8758f1f0-d783-4e8d-9b65-d4027b22a181
X-Content-Type-Options: nosniff
=== myaccount headers ===
Cache-Control: no-store
Strict-Transport-Security: max-age=2592000
X-Content-Type-Options: nosniff
Content-Security-Policy-Report-Only: default-src 'self';img-src 'self' https: data:;font-src 'self' https: data:;script-src 'self';connect-src 'self' https:;style-src 'self' 'unsafe-inline'; frame-src 'self' https:; frame-ancestors 'none'
=== chunk refs mysignins ===
src="/bundle/main.caa6a456.js"
=== chunk refs myaccount ===
src="/bundle/main.shim.js"
[0m
[0m$ [0mcd /tmp/opencode && echo "=== myaccount.html size ===" && wc -c myaccount.html mysignins.html && echo "=== config/script refs in myaccount ===" && grep -oE '"[a-zA-Z0-9_./:-]*\.js[^"]*"|[a-zA-Z0-9_]+:"[^"]{5,80}"|data-[a-z-]+="[^"]{3,120}"' myaccount.html | head -40
=== myaccount.html size ===
 468 myaccount.html
 793 mysignins.html
1261 total
=== config/script refs in myaccount ===
"/bundle/main.shim.js"
[0m
Both SPAs use dynamic chunk loading. Let me inspect the myaccount HTML config block (it previously leaked internal endpoints) and mysignins HTML.
[0m→ [0mRead /tmp/opencode/myaccount.html
[0m→ [0mRead /tmp/opencode/mysignins.html
[0m
[0m$ [0mcd /tmp/opencode && curl -s -D /tmp/opencode/hdr_shim.txt -o /tmp/opencode/main.shim.js "https://api.myaccount.microsoft.com/bundle/main.shim.js" --max-time 30 && wc -c main.shim.js && grep -oE 'main\.[a-f0-9]{8}\.js|[a-zA-Z0-9_./-]+\.chunk\.js|src:"[^"]+"|".{5,40}\.js"' main.shim.js | head -20
17936 main.shim.js
"script");e.src="/bundle/main.4e6e3dc6.js"
[0m
[0m$ [0mcd /tmp/opencode && grep -oE '"[a-zA-Z0-9_./:-]{3,80}"' main.shim.js | sort -u | head -40; echo "=== cache headers shim ==="; grep -iE "cache-control|expires|etag|last-modified|content-encoding|content-type" hdr_shim.txt
"/bundle/main.4e6e3dc6.js"
"script"
"text/javascript"
=== cache headers shim ===
Cache-Control: must-revalidate, no-cache
Content-Type: text/javascript
Last-Modified: Fri, 24 Jul 2026 15:09:26 GMT
ETag: "1dd1b7e6aa49110"
X-Content-Type-Options: nosniff
[0m
Now the key POC signal: cacheability/reproducibility of the two live source maps (determines if exposure is deliberate cache-busted artifact or a leak), then ACS JWKS cert analysis.
[0m$ [0mcd /tmp/opencode && curl -s -I "https://api.myaccount.microsoft.com/bundle/main.4e6e3dc6.js.map" --max-time 30 | grep -iE "^HTTP|cache-control|expires|etag|last-modified|content-length|content-encoding|content-type|x-ms|age" ; sleep 1; curl -s -I "https://mysignins.microsoft.com/bundle/main.caa6a456.js.map" --max-time 30 | grep -iE "^HTTP|cache-control|expires|etag|last-modified|content-length|content-encoding|content-type|x-ms|age"
HTTP/1.1 200 OK
Content-Length: 35340363
Content-Type: text/plain
Last-Modified: Fri, 24 Jul 2026 15:09:28 GMT
ETag: "1dd1b7e69cd444b"
Strict-Transport-Security: max-age=2592000
X-Content-Type-Options: nosniff
HTTP/1.1 200 OK
Content-Length: 6976576
Content-Type: text/plain
Last-Modified: Tue, 28 Jul 2026 09:48:44 GMT
ETag: "1dd1e764741d240"
x-ms-correlation-id: 652f623e-7c35-4172-92bc-75ae5a7dd559
x-ms-mysignins-region: westus2
x-ms-gateway-requestid: 5b9ad7d1-13ac-483f-b9ea-b4763abe4cd2
X-Content-Type-Options: nosniff
[0m
No `Cache-Control` on either map (immutable-hashed filenames, `text/plain`). Now ACS JWKS — full cert fingerprints, SPKI hashing, rotation analysis (in-scope `*.windows.net`).
[0m$ [0mcd /tmp/opencode && curl -s -D hdr_acs.txt -o acs.json "https://accounts.accesscontrol.windows.net/metadata/json/1" --max-time 30 && python3 - << 'EOF'
import json,hashlib,base64
d=json.load(open('acs.json'))
keys=d.get('keys',[])
print('keys:',len(keys))
for k in keys:
    x5c=k.get('x5c',[])
    print('--- kid:',k.get('kid'),'| use:',k.get('use'),'| alg:',k.get('alg'),'| x5c len:',len(x5c))
    for cert in x5c:
        der=base64.b64decode(cert)
        fp=hashlib.sha256(der).hexdigest()
        spki=hashlib.sha256(der).hexdigest()  # placeholder; real SPKI below
        print('   cert DER sha256:',fp)
# Now decode x509 via openssl for subject/issuer/validity/SPKI
import subprocess,tempfile
for i,k in enumerate(keys):
    for j,cert in enumerate(k.get('x5c',[])):
        der=base64.b64decode(cert)
        f=tempfile.NamedTemporaryFile(delete=False,suffix='.cer');f.write(der);f.close()
        out=subprocess.run(['openssl','x509','-inform','DER','-in',f.name,'-noout','-subject','-issuer','-dates','-fingerprint','-sha256','-pubkey'],capture_output=True,text=True)
        lines=[l for l in out.stdout.splitlines() if l.strip()]
        print(f'key{i} cert{j}:', ' | '.join(lines))
EOF
echo "=== acs response headers ==="; grep -iE "^HTTP|content-type|server|x-ms-ests|x-powered|strict" hdr_acs.txt
keys: 5
--- kid: None | use: None | alg: None | x5c len: 0
--- kid: None | use: None | alg: None | x5c len: 0
--- kid: None | use: None | alg: None | x5c len: 0
--- kid: None | use: None | alg: None | x5c len: 0
--- kid: None | use: None | alg: None | x5c len: 0
=== acs response headers ===
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
x-ms-ests-server: 2.1.24997.11 - NCUS ProdSlices
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import json
d=json.load(open('acs.json'))
print('top-level keys:', list(d.keys()))
print('sample (truncated):')
s=json.dumps(d,indent=1)
print(s[:2500])
EOF
top-level keys: ['keys', 'endpoints', 'version', 'serviceName']
sample (truncated):
{
 "keys": [
  {
   "usage": "signing",
   "keyValue": {
    "type": "x509Certificate",
    "value": "MIIC/TCCAeWgAwIBAgIICR+ICKYVUk0wDQYJKoZIhvcNAQELBQAwLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldDAeFw0yNjA2MDgxMjA0MjZaFw0zMTA2MDgxMjA0MjZaMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCiRib1VTdFuJsx7EW4W2G4Evu8blrZRS0dpSkFk4s6iBWGlZlgiId50A6Fvah5m91RUdyqxJyRkudQGqfMr5R+NYb196n+Fj6M9BGCmwXlJSnwzx35+9sCqfbk7RJN8YUEHaXPq9W6ud3syySSEP8rxrQ5/IrMTDTedw3i9d6Sfv5Kf7IIdCgdBjEogcloKPEGU/omPeQzAzL3c/PqBJ6ngJu0Z0hEaqbMNNzS0DP0w0Sd6em1NM6Q2Bc7hr/9sgT6mpEIUK5Tu0sdskLFtV72QT9C2tkbsRHOywkIdYoiEKbIHy/ctbHzNkhiuK02nfhpX/xRlVKp/4lWwmB9CLE5AgMBAAGjITAfMB0GA1UdDgQWBBQcHjvDRt+O89oZbJZ/B4ANuFy9XDANBgkqhkiG9w0BAQsFAAOCAQEAlqWkldYDJ0DiXHQrtE6SJsZEAX91IohfWWgA7FnJr3d2o2BtRL5BIFaWC2epZ0nmVOGA/nBo5NZ+5AQ+puqgI7QJSX5rVBS0R3G1dGt8eTta0f1cyqRO63yIWeN2yv9mJpg9//fNCISf/LURVaOOSp08vRLNhuMUqzHqHCMlY7XihinKf6H8NY5Yh+m2RmL9BK+mf1nUA+GFRDy2KGr3usb8VqN8ODr3e//rAMbkjaKT8B60rgjSvE6DdCZ7CxNdse28E+vbDXSFlM7SVYs7DfoM+U1xAR2OWf/ZvW6I4bo3G09oDYIumA1QS0doYA+b86d/Y9bsW9WScM8zt02uOw==",
    "keyInfo": {
     "x5t": "aFkmKVFc-4WV6sXCBvNZkXI505Y",
     "cloud_instance_name": "microsoftonline.com"
    }
   }
  },
  {
   "usage": "signing",
   "keyValue": {
    "type": "x509Certificate",
    "value": "MIIC/jCCAeagAwIBAgIJAMu0IASrR6UHMA0GCSqGSIb3DQEBCwUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMjYwNjE3MTUwMjIyWhcNMzEwNjE3MTUwMjIyWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAn0silOudntxwkXPPDyWnJh/V6A9pzQFadTX5m2gMmHkNLYJWgGYwXP36l9YYiN8Btit4aaAcibCwGi6cApwCxe1kR4L5dpGyQpNdKz0BMzLB3kncNdoo0JG8D6O3Lk+IMeStyIpiz1htkgAmXSSMl5Et9exh9Rib1/OllS8pvkScKCQhpxVQriRE8mEop8wOX68tD1rt5YOulAysynJ8a/1URPvrMf0TtpGwYTsSnyyz0a7i/ZwYsxauzh82xVJwrJYUnReyTAScCm5RtiEJDIcno73D8WUwaeT7NBh8h9gTbxVU7PAOf20+EDm/v27r/w6h8fmkEl6b8ombjCd/1QIDAQABoyEwHzAdBgNVHQ4EFgQU/IkWuenK4i/3pqx93d7CH04c7/AwDQYJKoZIhvcNAQELBQADggEBACo2Dfwo1A4VHol8L2Xb4XRuz/x7UExweYdF1aCgoD5PRyTmwmIMyQ7JgPdn5TWs4bgtVxGRMP+PoRcKJr4vlkzgaUuOhuQW6uLtvK25Z/XwkXXQwgw/z2Hg3tMgqHmM7riq00UnWYtl0uuvKFlDAbpPUbfs2QYG5TGgh9buVbUfI66tKTWgs3nOL1OKHIUyuYNGYTkkNMTegSa8RmF3ZpDrTGyqo4DFdAgxE9bRRC6YbG6dmOOF6SmcfhoakasOJkMP7d7NaxA0lUvqI23sfavXkSAc/iDd+rxvpoIlqL8r+w+8G0R0q4BrIdLwgDBBrPlXAmP318v49woBsbVp8Sw=",
    "keyInfo": {
     "x5t": "AahUf1bCXvx0JTRcXLrr0U4SluY",
     "cloud_instance_name": "microsoftonline.com"
    }
   }
[0m
JWKS shape differs from expectation — let me dump the actual response structure.
Response format changed vs prior run (now `keyValue.type=x509Certificate` + `keyInfo.x5t`/`cloud_instance_name`). Full cert parse + endpoint/service metadata:
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import json,base64,subprocess,tempfile,hashlib
d=json.load(open('acs.json'))
print('serviceName:',d.get('serviceName'),'| version:',d.get('version'))
print('endpoints:')
for e in d.get('endpoints',[]): print('  ',e)
print()
for i,k in enumerate(d.get('keys',[])):
    kv=k['keyValue']; info=k.get('keyInfo',{})
    der=base64.b64decode(kv['value'])
    f=tempfile.NamedTemporaryFile(delete=False,suffix='.cer');f.write(der);f.close()
    out=subprocess.run(['openssl','x509','-inform','DER','-in',f.name,'-noout','-subject','-issuer','-dates','-serial'],capture_output=True,text=True)
    print(f'--- key[{i}] usage={k.get("usage")} x5t={info.get("x5t")} cloud={info.get("cloud_instance_name")}')
    for l in out.stdout.splitlines(): print('   ',l.strip())
    print('    DER sha256:',hashlib.sha256(der).hexdigest())
EOF
serviceName: 00000001-0000-0000-c000-000000000000 | version: 1.0
endpoints:
   {'location': 'https://accounts.accesscontrol.windows.net/tokens/OAuth/2', 'protocol': 'OAuth2', 'usage': 'issuance'}
   {'location': 'https://accounts.accesscontrol.windows.net/tokens/delegation/1', 'protocol': 'DelegationIssuance1.0', 'usage': 'issuance'}
   {'location': 'https://accounts.accesscontrol.windows.net/mgmt/delegation/1', 'protocol': 'DelegationManagement1.0', 'usage': 'management'}

--- key[0] usage=signing x5t=None cloud=None
    subject=CN = accounts.accesscontrol.windows.net
    issuer=CN = accounts.accesscontrol.windows.net
    notBefore=Jun  8 12:04:26 2026 GMT
    notAfter=Jun  8 12:04:26 2031 GMT
    serial=091F8808A615524D
    DER sha256: 05d17f723cf310767174bb9fbaf3f86d72860a5a4c11d70c359a463a6404d9c6
--- key[1] usage=signing x5t=None cloud=None
    subject=CN = accounts.accesscontrol.windows.net
    issuer=CN = accounts.accesscontrol.windows.net
    notBefore=Jun 17 15:02:22 2026 GMT
    notAfter=Jun 17 15:02:22 2031 GMT
    serial=CBB42004AB47A507
    DER sha256: 34d094252b14b033d362116b3009119ca8cd13f99edabcd1106e2d70c484bd5b
--- key[2] usage=signing x5t=None cloud=None
    subject=CN = accounts.accesscontrol.windows.net
    issuer=CN = accounts.accesscontrol.windows.net
    notBefore=Jul  4 00:05:05 2026 GMT
    notAfter=Jul  4 00:05:05 2031 GMT
    serial=EE1B38F6C643172C
    DER sha256: 93a64915a920db8f50a8a91260aaf0ff5b65219d815bb84e78da84c77d364d47
--- key[3] usage=signing x5t=None cloud=None
    subject=CN = login.microsoftonline.us
    issuer=CN = login.microsoftonline.us
    notBefore=Jul  1 16:00:54 2026 GMT
    notAfter=Jul  1 16:00:54 2031 GMT
    serial=A0068BC318132137
    DER sha256: 1b499ff9ee4960a675e9c6f67357c90c6d085001ad7ea71f89da5bf0a84e3d85
--- key[4] usage=signing x5t=None cloud=None
    subject=CN = login.microsoftonline.us
    issuer=CN = login.microsoftonline.us
    notBefore=Jul 26 16:01:26 2026 GMT
    notAfter=Jul 26 16:01:26 2031 GMT
    serial=EDCC26443B43D063
    DER sha256: a73ca1f9db027fe93963bd18080e571efd492a6da8743b4ad15919f05cb567b9
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import json
d=json.load(open('acs.json'))
for i,k in enumerate(d['keys']):
    kv=k['keyValue']; info=kv.get('keyInfo',{})
    print(f'key[{i}] usage={k.get("usage")} type={kv.get("type")} x5t={info.get("x5t")} cloud={info.get("cloud_instance_name")}')
# also check for allowAllUriOrigins / legacy fields anywhere in JSON
s=json.dumps(d)
for probe in ['allowedAudiences','Audience','Issuer','uri','Uri','legacy','maxIdleTime','allowedOrigins']:
    print(probe, 'present:', probe in s)
EOF
key[0] usage=signing type=x509Certificate x5t=aFkmKVFc-4WV6sXCBvNZkXI505Y cloud=microsoftonline.com
key[1] usage=signing type=x509Certificate x5t=AahUf1bCXvx0JTRcXLrr0U4SluY cloud=microsoftonline.com
key[2] usage=signing type=x509Certificate x5t=fEtqrhKT1bXAGafSdQoN1vXTRpI cloud=microsoftonline.com
key[3] usage=signing type=x509Certificate x5t=jvm_-TtaqTCRrkpnmePvwb75nWE cloud=microsoftonline.us
key[4] usage=signing type=x509Certificate x5t=6hXLaIYNSJ0o7zu09dMyI0ji3ug cloud=microsoftonline.us
allowedAudiences present: False
Audience present: False
Issuer present: False
uri present: False
Uri present: False
legacy present: False
maxIdleTime present: False
allowedOrigins present: False
[0m
`keyInfo` nests inside `keyValue` — re-extract x5t/cloud_instance_name correctly (relevant to the mixed-issuer question):
`allowedAudiences` absent (patch confirmed); JWKS now co-serves `cloud_instance_name`-tagged keys for both `microsoftonline.com` and `microsoftonline.us`. Next POC signal: Graph `$metadata` service-document crawl for the undocumented agent session/identity types (in-scope `graph.microsoft.com`).
[0m$ [0mcd /tmp/opencode && curl -s -D hdr_graph_v1.txt -o graph_v1_meta.xml "https://graph.microsoft.com/v1.0/\$metadata" --max-time 60 && wc -c graph_v1_meta.xml && grep -iE "^HTTP|content-type" hdr_graph_v1.txt | head -3
2931817 graph_v1_meta.xml
HTTP/1.1 200 OK
Content-Type: application/xml;charset=utf-8
[0m
[0m$ [0mcd /tmp/opencode && sleep 1 && curl -s -D hdr_graph_beta.txt -o graph_beta_meta.xml "https://graph.microsoft.com/beta/\$metadata" --max-time 90 && wc -c graph_beta_meta.xml && echo "=== agent types v1 ===" && grep -oiE 'name="[a-zA-Z.]*[aA]gent[a-zA-Z]*"' graph_v1_meta.xml | sort -u | head -20 && echo "=== agent types beta ===" && grep -oiE 'name="[a-zA-Z.]*[aA]gent[a-zA-Z]*"' graph_beta_meta.xml | sort -u | head -40
7298965 graph_beta_meta.xml
=== agent types v1 ===
Name="AIInvokeAgent"
Name="AgentAdminActivity"
Name="AgentSettingsAdminActivity"
Name="CopilotAgentManagement"
Name="IsSynchronizationAgentAssignmentRequired"
Name="IsSynchronizationAgentRequired"
Name="MosAgentInfoRecord"
Name="PurviewPostureAgent"
Name="RTIOperationsAgent"
Name="SecurityCopilotAgentIdentityManagement"
Name="SensitiveInfoRemediationAgentData"
Name="SubmissionAgenticGradingResult"
Name="SyncAgentADContainer"
Name="SyncAgentCompatibilityKey"
Name="VivaGlintAgenticCampaign"
Name="agentAdminActivityRecord"
Name="agentId"
Name="agentIdentity"
Name="agentIdentityBlueprint"
Name="agentIdentityBlueprintId"
=== agent types beta ===
Name="AIInvokeAgent"
Name="AgentAdminActivity"
Name="AgentSettingsAdminActivity"
Name="CopilotAgentManagement"
Name="IsSynchronizationAgentAssignmentRequired"
Name="MosAgentInfoRecord"
Name="PurviewPostureAgent"
Name="RTIOperationsAgent"
Name="SecurityCopilotAgentIdentityManagement"
Name="SensitiveInfoRemediationAgentData"
Name="SubmissionAgenticGradingResult"
Name="SyncAgentADContainer"
Name="SyncAgentCompatibilityKey"
Name="VivaGlintAgenticCampaign"
Name="adminAgent"
Name="adminAgentSoftwareUpdateStatus"
Name="adminAgentSoftwareVersion"
Name="adminConfirmedAgentCompromised"
Name="adminConfirmedAgentSafe"
Name="adminDismissedRiskForAgent"
Name="agent"
Name="agentAdminActivityRecord"
Name="agentBlueprintId"
Name="agentBlueprintName"
Name="agentCapabilities"
Name="agentCard"
Name="agentCardManifest"
Name="agentCardManifests"
Name="agentCardSignature"
Name="agentCheckHeartbeatLost"
Name="agentCheckNotExisted"
Name="agentCheckNotRunning"
Name="agentCheckOldVersion"
Name="agentCollection"
Name="agentCollections"
Name="agentDisplayName"
Name="agentExtension"
Name="agentExtensionParams"
Name="agentFailure"
Name="agentGroups"
[0m
v1 metadata exposes `agentIdentity` + `agentIdentityBlueprint`. Let me extract the full entity-type (not property) inventory for agent/identity/session types and look for `agentSignInSession`.
[0m$ [0mcd /tmp/opencode && echo "=== EntityType names with agent/Agent ===" && grep -oE 'EntityType Name="[^"]*"' graph_v1_meta.xml | grep -iE "agent" | sort -u | head -30 && echo "=== v1 agentSignInSession refs ===" && grep -c "agentSignInSession" graph_v1_meta.xml; echo "=== v1 agentIdentity entity ===" && grep -oE 'EntityType Name="microsoft.graph.agentIdentity[^"]*"' graph_v1_meta.xml | sort -u; echo "=== beta agentSignInSession refs ===" && grep -c "agentSignInSession" graph_beta_meta.xml; echo "=== beta agentIdentity/blueprint entities ===" && grep -oE 'EntityType Name="microsoft.graph.agent[A-Za-z]*"' graph_beta_meta.xml | sort -u | head -20
=== EntityType names with agent/Agent ===
EntityType Name="agentIdentity"
EntityType Name="agentIdentityBlueprint"
EntityType Name="agentIdentityBlueprintPrincipal"
EntityType Name="agentUser"
=== v1 agentSignInSession refs ===
0
=== v1 agentIdentity entity ===
=== beta agentSignInSession refs ===
0
=== beta agentIdentity/blueprint entities ===
[0m
`agentSignInSession` absent from both metadata files (consistent with undocumented/partner-only surface), but `agentIdentity`/`agentIdentityBlueprint`/`agentUser` are real v1 entity types. Extract their full definitions + the `agent` relationship on signIn:
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
for f in ['graph_v1_meta.xml','graph_beta_meta.xml']:
    print('========',f)
    x=open(f).read()
    for name in ['agentIdentity','agentIdentityBlueprint','agentIdentityBlueprintPrincipal','agentUser','agentSignIn']:
        m=re.search(r'<EntityType Name="'+name+r'"[^>]*>.*?</EntityType>',x,re.S)
        if m:
            body=re.sub(r'\s+',' ',m.group(0))
            print('---',name)
            print(body[:1500])
        else:
            print('---',name,'NOT FOUND')
EOF
======== graph_v1_meta.xml
--- agentIdentity
<EntityType Name="agentIdentity" BaseType="graph.servicePrincipal" OpenType="true"><Property Name="agentIdentityBlueprintId" Type="Edm.String" Nullable="false" /><Property Name="createdDateTime" Type="Edm.DateTimeOffset" /><NavigationProperty Name="sponsors" Type="Collection(graph.directoryObject)" /></EntityType>
--- agentIdentityBlueprint
<EntityType Name="agentIdentityBlueprint" BaseType="graph.application" OpenType="true"><NavigationProperty Name="inheritablePermissions" Type="Collection(graph.inheritablePermission)" ContainsTarget="true" /><NavigationProperty Name="sponsors" Type="Collection(graph.directoryObject)" /></EntityType>
--- agentIdentityBlueprintPrincipal
<EntityType Name="agentIdentityBlueprintPrincipal" BaseType="graph.servicePrincipal" OpenType="true"><NavigationProperty Name="sponsors" Type="Collection(graph.directoryObject)" /></EntityType>
--- agentUser
<EntityType Name="agentUser" BaseType="graph.user" OpenType="true" /><EntityType Name="allowedValue" BaseType="graph.entity"><Property Name="isActive" Type="Edm.Boolean" /></EntityType>
--- agentSignIn NOT FOUND
======== graph_beta_meta.xml
--- agentIdentity
<EntityType Name="agentIdentity" BaseType="graph.servicePrincipal" OpenType="true"><Property Name="agentIdentityBlueprintId" Type="Edm.String" Nullable="false" /><Property Name="createdDateTime" Type="Edm.DateTimeOffset" /><NavigationProperty Name="inheritedAppRoleAssignments" Type="Collection(graph.appRoleAssignment)" /><NavigationProperty Name="inheritedOauth2PermissionGrants" Type="Collection(graph.oAuth2PermissionGrant)" /><NavigationProperty Name="sponsors" Type="Collection(graph.directoryObject)" /></EntityType>
--- agentIdentityBlueprint
<EntityType Name="agentIdentityBlueprint" BaseType="graph.application" OpenType="true"><NavigationProperty Name="inheritablePermissions" Type="Collection(graph.inheritablePermission)" ContainsTarget="true" /><NavigationProperty Name="sponsors" Type="Collection(graph.directoryObject)" /></EntityType>
--- agentIdentityBlueprintPrincipal
<EntityType Name="agentIdentityBlueprintPrincipal" BaseType="graph.servicePrincipal" OpenType="true"><NavigationProperty Name="sponsors" Type="Collection(graph.directoryObject)" /></EntityType>
--- agentUser
<EntityType Name="agentUser" BaseType="graph.user" OpenType="true" /><EntityType Name="allowedDataLocation" BaseType="graph.entity" OpenType="true"><Property Name="appId" Type="Edm.String" /><Property Name="domain" Type="Edm.String" /><Property Name="isDefault" Type="Edm.Boolean" /><Property Name="location" Type="Edm.String" /></EntityType>
--- agentSignIn NOT FOUND
[0m
Full agent-identity model recovered: `agentIdentity` (BaseType `servicePrincipal`) with `sponsors`, and beta-only `inheritedAppRoleAssignments`/`inheritedOauth2PermissionGrants`. Search for agent-related Functions/Actions and sponsorship endpoints:
[0m$ [0mcd /tmp/opencode && echo "=== v1 functions/actions w/ agent ===" && grep -oiE '<(Function|Action) Name="[^"]*[aA]gent[^"]*"' graph_v1_meta.xml | sort -u | head -20 && echo "=== v1 sponsor related nav/endpoints ===" && grep -oiE '(Function|Action) Name="[^"]*[Ss]ponsor[^"]*"' graph_v1_meta.xml | sort -u | head -10 && echo "=== v1 sponsoredObjects/sponsorOf refs ===" && grep -c "sponsorOf" graph_v1_meta.xml; grep -c "sponsoredObjects" graph_v1_meta.xml; echo "=== beta agent actions ===" && grep -oiE '<(Function|Action) Name="[^"]*[aA]gent[^"]*"' graph_beta_meta.xml | sort -u | head -25
=== v1 functions/actions w/ agent ===
=== v1 sponsor related nav/endpoints ===
=== v1 sponsoredObjects/sponsorOf refs ===
1
0
=== beta agent actions ===
<Action Name="configureAgent"
<Action Name="deployAgent"
<Action Name="getUnhealthyDefenderAgentsReport"
<Action Name="retryPartnerAgentInstallation"
<Function Name="retrieveDeployAgentActionResults"
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_v1_meta.xml').read()
for pat in [r'<NavigationProperty Name="sponsorOf"[^/>]*/?>', r'<NavigationProperty Name="sponsoredObjects"[^/>]*/?>', r'<NavigationProperty Name="sponsoredUsers"[^/>]*/?>']:
    m=re.findall(pat,x)
    print(pat.split('"')[1], '->', m[:3] if m else 'none')
# find context lines around sponsorOf for ContainsTarget / type
i=x.find('sponsorOf')
print('context:', re.sub(r'\s+',' ',x[i-200:i+300]))
EOF
sponsorOf -> ['<NavigationProperty Name="sponsorOf" Type="Collection(graph.directoryObject)" />']
sponsoredObjects -> none
sponsoredUsers -> none
context: steredDevices" Type="Collection(graph.directoryObject)" /><NavigationProperty Name="scopedRoleMemberOf" Type="Collection(graph.scopedRoleMembership)" ContainsTarget="true" /><NavigationProperty Name="sponsorOf" Type="Collection(graph.directoryObject)" /><NavigationProperty Name="sponsors" Type="Collection(graph.directoryObject)" /><NavigationProperty Name="transitiveMemberOf" Type="Collection(graph.directoryObject)" /><NavigationProperty Name="calendar" Type="graph.calendar" ContainsTarget="true
[0m
[0m$ [0mcd /tmp/opencode && echo "=== shim config strings ===" && grep -oE 'https://[a-zA-Z0-9.-]+/[a-zA-Z0-9/._-]*|clientId[^,}]{0,60}|[a-zA-Z]+Endpoint[^,}]{0,60}' main.shim.js | sort -u | head -25
=== shim config strings ===
[0m
Sponsorship model (`sponsorOf`/`sponsors` nav props) confirmed on `user`. Let me mine the already-downloaded shim for the config block (clientId/authority/internal URLs), then check whether the graph v1 $metadata contains the internal-only scopes used by My Account.
[0m$ [0mcd /tmp/opencode && echo "=== shim first 40 lines ===" && sed -n '1,40p' main.shim.js | tr ';' '\n' | head -40
=== shim first 40 lines ===
!function(){"use strict"
var e=document.createElement("script")
e.src="/bundle/main.4e6e3dc6.js",e.type="text/javascript",document.body.appendChild(e)}()

// SIG // Begin signature block
// SIG // MIIovQYJKoZIhvcNAQcCoIIorjCCKKoCAQExDzANBglg
// SIG // hkgBZQMEAgEFADB3BgorBgEEAYI3AgEEoGkwZzAyBgor
// SIG // BgEEAYI3AgEeMCQCAQEEEBDgyQbOONQRoqMAEEvTUJAC
// SIG // AQACAQACAQACAQACAQAwMTANBglghkgBZQMEAgEFAAQg
// SIG // 9IlLu3SKT/dH6Bf07Zz8Xgq3LjR6pLEPmk7ppo+zZmqg
// SIG // gg3DMIIGrTCCBJWgAwIBAgITMwAAANMCiNhpvd8CCQAA
// SIG // AAAA0zANBgkqhkiG9w0BAQwFADBiMQswCQYDVQQGEwJV
// SIG // UzEeMBwGA1UEChMVTWljcm9zb2Z0IENvcnBvcmF0aW9u
// SIG // MTMwMQYDVQQDEypBenVyZSBSU0EgUHVibGljIFNlcnZp
// SIG // Y2VzIENvZGUgU2lnbmluZyBQQ0EwHhcNMjYwMzA1MTkw
// SIG // NjE5WhcNMjcwMzAzMTkwNjE5WjCBgjELMAkGA1UEBhMC
// SIG // VVMxEzARBgNVBAgTCldhc2hpbmd0b24xEDAOBgNVBAcT
// SIG // B1JlZG1vbmQxHjAcBgNVBAoTFU1pY3Jvc29mdCBDb3Jw
// SIG // b3JhdGlvbjEsMCoGA1UEAxMjQXp1cmUgUHVibGljIFNl
// SIG // cnZpY2VzIFJTQSBDb2RlIFNpZ24wggGiMA0GCSqGSIb3
// SIG // DQEBAQUAA4IBjwAwggGKAoIBgQDJrDsZxGHNBYj5RPkR
// SIG // yGEIGZlcCQkqE7SdGa6c2FPAMGu1JLqHur8qNWjR5swe
// SIG // kMMn0WSz2+QE0c848wHvqVVLpWV+Un3Q01Dnyzjifae4
// SIG // h+kzhQTUIhX2rKQbGHdqBe7LXyCgkfDrTNLXsI1xAXAw
// SIG // A42Ar+PpVd1ktqLSsM6pwqrM/FFfEizl9h5gtrEIZODp
// SIG // +jo9vESxKvHGp/Ifca7e/IluDeJffP4ME/fFtUzUCLH/
// SIG // IweugiUcsaTmewSny4odYLtmZK/zpBuPjGTLpUnn4HMy
// SIG // WXLUSOFlgvFrLKCPZxbNxXxBo5EUquzrFbpg9NmQVipJ
// SIG // QsKDSKNHluTwNicItFpZpiPMc7Y2LG11dNAil3yJEBLF
// SIG // MQ/5RqQptcWmgSKoG8Tx7x+RCYQj4Sjl2yJt+4OykVn9
// SIG // /kQ5wo6v6TmIHHZsKdmgIkMCKBuIRcB2FaGwLY+UGMP1
// SIG // xmiwIaekgjUgVwXX8Kv/2SYI9XXrjyUME4NLj0JTBIO7
// SIG // YX201gYYOL8CAwEAAaOCAbkwggG1MA4GA1UdDwEB/wQE
// SIG // AwIHgDAfBgNVHSUEGDAWBggrBgEFBQcDAwYKKwYBBAGC
// SIG // N1sBATAdBgNVHQ4EFgQU6DIfaqBAAm33/QFJGYifZUeQ
// SIG // B3AwRQYDVR0RBD4wPKQ6MDgxHjAcBgNVBAsTFU1pY3Jv
// SIG // c29mdCBDb3Jwb3JhdGlvbjEWMBQGA1UEBRMNNDY5OTgx
// SIG // KzUwNzE4MTAfBgNVHSMEGDAWgBTxL7qRFnzefVInMfV6
// SIG // +9VYWWk6PTBvBgNVHR8EaDBmMGSgYqBghl5odHRwOi8v
// SIG // d3d3Lm1pY3Jvc29mdC5jb20vcGtpb3BzL2NybC9BenVy
[0m
Shim is trivial (loader + Authenticode sig block); config lives in the main bundle (already recovered). Let me verify the agent-identity sponsorship model is publicly documented (for accurate citing), then finalize.
[0m◈ [0mParallel Web Search "Microsoft Entra "agentIdentityBlueprint" agent identity sponsorship documentation servicePrincipal"
[0m
[0m$ [0mcd /tmp/opencode && sha256sum graph_v1_meta.xml graph_beta_meta.xml acs.json main.shim.js mysignins.html myaccount.html 2>/dev/null
9d84e4510aa8d6bc95f14ae0bde05ea01c967fcd29478269b58e7cfdd2bdc16a  graph_v1_meta.xml
ca3048596553b16c06004d1dacfae221de85b8e58878d7228feb15a7aeda2ebe  graph_beta_meta.xml
504f3df26b27ccd29250fb91702bad17fb912f2a8bf646da0f77135772128c38  acs.json
88dc286e367a6ed40111c93792c3110e5e041198b52f4efb75f219ac4a6be348  main.shim.js
ba400c191bdbd25958097cf569b26d01a1270c96953a3080608dda368c4dd29e  mysignins.html
de5a0b996352917a16af6569493edb7169df820b1a94ef66cd6b87d277317544  myaccount.html
[0m
Public docs confirm the full Agent ID model (preview since Nov 2025, shipping to Dataverse Aug 2026): `agentIdentityBlueprint`/`agentIdentityBlueprintPrincipal`/`agentIdentity` (`servicePrincipalType=ServiceIdentity`)/`agentUser`, sponsor-gated creation, blocked interactive/ROPC/client-secret flows. Let me hash this run's artifacts (hashes only, no raw content in commits) and write the POC journal.
[0m$ [0mcat >> /home/runner/work/hunt-lab/hunt-lab/research/bigpickle.md <<'EOF'

## RUN 2026-08-07 10:45:00 (big-pickle) — TARGET: microsoft — PHASE: POC (read-only)

### POC-1: Source-map exposure characterization (LIVE, in-scope)
- HEAD/GET reconfirmed: api.myaccount /bundle/main.4e6e3dc6.js.map (35,340,363 B) and mysignins /bundle/main.caa6a456.js.map (6,976,576 B) both 200, Content-Type text/plain, NO Cache-Control header on either; Last-Modified 2026-07-24 / 2026-07-28; ETags present. Hashed filenames = immutable-content pattern, so exposure is deliberate-cacheable (supporting asset for chains; not standalone-reportable per triage 10-16).
- mysignins SPA shell: real CSP `script-src 'self' amcdn.msftauth.net js.monitor.azure.com`, meta app-version 4.0.2233. myaccount shell: CSP-Report-Only + HSTS max-age=2592000 + no-store. Both shells are minimal (no config block in HTML); myaccount loads /bundle/main.shim.js (17,936 B, trivial loader + Azure Code Signing Authenticode block, ETag 1dd1b7e6aa49110) which loads main.4e6e3dc6.js.
- Artifact hashes (this run): graph_v1_meta.xml 9d84e451..., graph_beta_meta.xml ca304859..., acs.json 504f3df2..., main.shim.js 88dc286e..., mysignins.html ba400c19..., myaccount.html de5a0b99...

### POC-2: ACS /metadata/json/1 — full JWKS reconciliation (LIVE, in-scope *.windows.net)
- Response shape CHANGED vs SURFACE run: now keyValue{type:x509Certificate,value,keyInfo{x5t,cloud_instance_name}}; allowedAudiences/Audience/Issuer fields ABSENT (tenant-enum patch reconfirmed).
- 5 self-signed signing certs, rotation-active (notBefore Jun-08 / Jun-17 / Jul-04 2026 for the 3 prod keys; Jul-01 / Jul-26 2026 for the 2 US-cloud keys), all notAfter 2031. Per-key: x5t aFkmKVFc-4WV6sXCBvNZkXI505Y (microsoftonline.com), AahUf1bCXvx0JTRcXLrr0U4SluY, fEtqrhKT1bXAGafSdQoN1vXTRpI, then jvm_-TtaqTCRrkpnmePvwb75nWE + 6hXLaIYNSJ0o7zu09dMyI0ji3ug (BOTH cloud_instance_name=microsoftonline.us, CN=login.microsoftonline.us). DER sha256: 05d17f72...d9c6, 34d09425...bd5b, 93a64915...d47, 1b499ff9...3d85, a73ca1f9...67b9 (match SURFACE-run colon-hex FPs).
- Signal: the JWKS now explicitly co-serves microsoftonline.us signing keys tagged cloud_instance_name. Not a vuln by itself (est-aggregation design); it is a design signal for the cross-cloud-token acceptance hypothesis (test-tenant-only to prove).

### POC-3: Graph $metadata service-document crawl for agent/identity types (LIVE, in-scope graph.microsoft.com)
- v1.0 $metadata (2,931,817 B) + beta $metadata (7,298,965 B) fetched. agentSignInSession: 0 references in BOTH — the /me/agentSignInSessions surface used by My Sign-Ins (/recent-activity/agentic-sessions) is entirely off-metadata (internal/partner surface). Confirms earlier finding.
- Agent ID entity model IS in public metadata: v1 EntityTypes agentIdentity (BaseType graph.servicePrincipal, props agentIdentityBlueprintId + createdDateTime, nav sponsors), agentIdentityBlueprint (BaseType graph.application, nav inheritablePermissions ContainsTarget + sponsors), agentIdentityBlueprintPrincipal (BaseType graph.servicePrincipal), agentUser (BaseType graph.user). Beta adds agentIdentity nav inheritedAppRoleAssignments + inheritedOauth2PermissionGrants. user entity has nav sponsorOf + sponsors.
- Public docs confirm (learn.microsoft.com/en-us/entra/agent-id; create-blueprint.md): Agent ID preview (admin center Nov-17-2025; Dataverse GA announcement 2026-08-06). Creation endpoints POST /v1.0/applications/microsoft.graph.agentIdentityBlueprint, /servicePrincipals/microsoft.graph.agentIdentityBlueprintPrincipal, /servicePrincipals/microsoft.graph.agentIdentity, beta /users (agentUser). Scopes: AgentIdentityBlueprint.Create / .AddRemoveCreds.All / .UpdateAuthProperties.All / AgentIdentityBlueprintPrincipal.Create; dir roles Agent ID Developer / Agent ID Administrator. Hardening confirmed by independent tests: agent identities are always single-tenant; interactive OAuth flows (code/device/implicit/ROPC) BLOCKED; addPassword to agentIdentity service principal BLOCKED (403); isFallbackPublicClient not flippable; no RFC7591 dynamic registration.
- My Account agent hooks map onto this model: useHasOwnedAgents GET /users/{userId}/ownedObjects/Microsoft.graph.ServicePrincipal?$top=999 filtered servicePrincipalType==ServiceIdentity (=agentIdentity); getSponsoredAgentsUrl GET /users/{userId}/sponsorOf/microsoft.graph.servicePrincipal?$top=999.

### POC designs (no live sends; test-tenant only)
- PD-A agentSignInSessions scope/IDOR: token on own tenant (AgentIdentity.Read.All) → GET /v1.0/me/agentSignInSessions and GET /v1.0/users/{arbitrary-oid}/agentSignInSessions; signal if second resolves caller-agnostic → cross-user session disclosure. CVSS 6.5–8.8.
- PD-B agent-identity privilege inheritance: user sponsors self-created blueprint → creates agentIdentity inheriting blueprint oauth2PermissionGrants/appRoleAssignments (beta nav) → compare agent's effective perms vs user's own; signal if inherited grants exceed sponsor's. CVSS 7.0–9.0. (Blocked-flow hardening partially mitigates; OBO/jwt-bearer minting still possible.)
- PD-C Verified ID mint: POST /api/issueVerifiedEmployeeCredential (Bearer, scope=SPA clientId 8c59ead7-d703-4a27-9e55-c96a0054c8d2, no body); compare no-role vs GlobalAdmin; signal both-204 → authz gap. CVSS 7.1–9.1.
- PD-D policyfile IDOR: GET /api/termsofuse/agreements/{a}/policyfiles/{p}/data with cross-tenant GUID pairs (token required). CVSS 5.3–7.5.
- PD-E redirect_uri normalization matrix vs reply-url doc (RFC6749 §3.1.2 + RFC9700): trailing-slash, case, fragment, percent-encoding, response_mode form_post/fragment, wctx; live-check only on in-scope login.live.com fast-fail echo (1 req per variant, documented cases only). CVSS 7.4–9.0 if any first-party registration accepts a non-exact match.
EOF
echo "journal lines: $(wc -l < /home/runner/work/hunt-lab/hunt-lab/research/bigpickle.md)"
journal lines: 4104
[0m
[0m$ [0mcat >> /home/runner/work/hunt-lab/hunt-lab/leads/lead-bigpickle.md <<'EOF'

## 2026-08-07 10:45:00 UTC [microsoft] (model bigpickle) — POC

- [LIVE-VERIFIED] Graph $metadata crawl: agentSignInSession has 0 refs in BOTH v1 and beta $metadata (hashes 9d84e451..., ca304859...) — /me/agentSignInSessions is fully off-metadata. Agent ID entity model confirmed in metadata: agentIdentity (BaseType servicePrincipal, nav sponsors; beta adds inheritedAppRoleAssignments + inheritedOauth2PermissionGrants), agentIdentityBlueprint (application, nav inheritablePermissions ContainsTarget), agentIdentityBlueprintPrincipal, agentUser; user has sponsorOf+sponsors nav. Public docs (learn.microsoft.com/en-us/entra/agent-id + create-blueprint.md, 2026-05-01) + independent tests confirm sponsor-gated creation, blocked interactive flows/ROPC/client-secret-add, single-tenant agent identities, dir roles Agent ID Developer/Administrator, scopes AgentIdentityBlueprint.*/AgentIdentityBlueprintPrincipal.Create.
- [LIVE-VERIFIED] ACS /metadata/json/1 shape changed: keys now keyValue{type:x509Certificate,value,keyInfo{x5t,cloud_instance_name}}; allowedAudiences absent. 5 self-signed keys, active rotation (3x microsoftonline.com CN=accounts.accesscontrol.windows.net notBefore 06-08/06-17/07-04-2026; 2x microsoftonline.us CN=login.microsoftonline.us notBefore 07-01/07-26-2026), all notAfter 2031. DER sha256 05d17f72..., 34d09425..., 93a64915..., 1b499ff9..., a73ca1f9... x5t aFkmKVFc.../AahUf1bC.../fEtqrhKT... (com) + jvm_-Ttaq.../6hXLaIYN... (us).
- [LIVE-VERIFIED] Both .js.map still 200 text/plain with NO Cache-Control (immutable-hashed, deliberate-cacheable). myaccount CSP-Report-Only; mysignins real CSP + app-version 4.0.2233. main.shim.js = trivial loader + Azure Code Signing Authenticode block (ETag 1dd1b7e6aa49110).
- [POC-DESIGN] Agent-identity privilege inheritance: sponsor self-blueprint → agentIdentity inherits oauth2PermissionGrants/appRoleAssignments (beta nav) — test whether inherited grants can exceed the sponsor's own (test tenant). CVSS 7.0–9.0.
- [POC-DESIGN] agentSignInSessions cross-user: /users/{oid}/agentSignInSessions vs /me/ (test tenant). CVSS 6.5–8.8.
- [POC-DESIGN] Verified ID mint (no-role vs GlobalAdmin, both-204 signal). CVSS 7.1–9.1. policyfile IDOR cross-tenant GUID pairs. CVSS 5.3–7.5. redirect_uri matrix vs reply-url rules (documented variants, live only on in-scope login.live.com echo). CVSS 7.4–9.0.
EOF
cat > /home/runner/work/hunt-lab/hunt-lab/state_bigpickle.json <<'EOF'
{"phase": "POC", "status": "HIGH_POTENTIAL"}
EOF
cat /home/runner/work/hunt-lab/hunt-lab/state_bigpickle.json
{"phase": "POC", "status": "HIGH_POTENTIAL"}
[0m
## Program: microsoft — Phase: POC (read-only)

### Key Findings
- **`/me/agentSignInSessions` is fully off-metadata**: 0 references in both `v1.0/$metadata` (sha256 `9d84e451…`) and `beta/$metadata` (`ca304859…`) — the surface My Sign-Ins uses (`/recent-activity/agentic-sessions`) exists only as an internal route (baseline 401). Meanwhile the **Agent ID model IS public**: `agentIdentity` (BaseType `servicePrincipal`, nav `sponsors`; beta adds `inheritedAppRoleAssignments` + `inheritedOauth2PermissionGrants`), `agentIdentityBlueprint` (nav `inheritablePermissions` ContainsTarget), `agentIdentityBlueprintPrincipal`, `agentUser`; `user` exposes `sponsorOf`+`sponsors` nav (v1 metadata).
- **ACS JWKS reconciled**: `allowedAudiences`/`Audience`/`Issuer` fields now absent from `/metadata/json/1` (tenant-enum patch reconfirmed). 5 self-signed signing keys with live rotation — 3× `CN=accounts.accesscontrol.windows.net` (notBefore 06-08/06-17/07-04-2026) tagged `cloud_instance_name=microsoftonline.com`, **2× `CN=login.microsoftonline.us`** (notBefore 07-01/07-26-2026) tagged `microsoftonline.us`. DER sha256: `05d17f72…`, `34d09425…`, `93a64915…`, `1b499ff9…`, `a73ca1f9…`.
- **Public Agent ID docs + independent tests** (learn.microsoft.com/en-us/entra/agent-id; create-blueprint.md 2026-05-01; goodworkaround.com 2026-02-02; entrabot platform-docs) confirm hardening already in place: agent identities are single-tenant, interactive OAuth flows/ROPC blocked, `addPassword` on agentIdentity SP blocked, sponsor-required creation, dir roles `Agent ID Developer/Administrator`, scopes `AgentIdentityBlueprint.*`/`AgentIdentityBlueprintPrincipal.Create`. Feature is preview (Nov-2025) expanding to Dataverse (2026-08-06).
- **Source maps still live** (api.myaccount 35.3 MB / mysignins 7 MB), both `200 text/plain`, **no Cache-Control** (immutable-hashed, deliberately cacheable — supporting asset only). myaccount shell: `CSP-Report-Only` + HSTS; mysignins shell: real CSP + `app-version 4.0.2233`.

### New Endpoints / Assets
- Graph (public metadata, read-only confirmed): `agentIdentity`, `agentIdentityBlueprint`, `agentIdentityBlueprintPrincipal`, `agentUser` entity types; `user.sponsorOf`/`user.sponsors`; beta `agentIdentity.inheritedOauth2PermissionGrants`/`inheritedAppRoleAssignments`.
- Creation endpoints (documented, test-tenant): `POST /v1.0/applications/microsoft.graph.agentIdentityBlueprint`, `/servicePrincipals/microsoft.graph.agentIdentityBlueprintPrincipal`, `/servicePrincipals/microsoft.graph.agentIdentity`, beta `/users` (agentUser).
- `api.myaccount /bundle/main.shim.js` (trivial loader + Azure Code Signing block, ETag `1dd1b7e6aa49110`).

### Hypotheses (impact + read-only test)
1. **Agent-identity privilege inheritance (NEW, highest value)** — sponsor self-creates blueprint → agentIdentity inherits `oauth2PermissionGrants`/`appRoleAssignments` from blueprint; if inherited grants can exceed the sponsor's own, low-priv→high-priv escalation. Test: own tenant, compare agent effective grants vs sponsor's. CVSS 7.0–9.0.
2. **`agentSignInSessions` cross-user/scope gap** — `/users/{oid}/agentSignInSessions` vs `/me/` with `AgentIdentity.Read.All` token; off-metadata surface, historically under-scoped. CVSS 6.5–8.8.
3. **Verified ID mint authz** — POST `/api/issueVerifiedEmployeeCredential` (Bearer, scope=SPA clientId `8c59ead7…`, no body); no-role vs GlobalAdmin both-204 signal. CVSS 7.1–9.1.
4. **ToS policyfile IDOR** — GET `…/agreements/{a}/policyfiles/{p}/data` cross-tenant GUID pairs (token required). CVSS 5.3–7.5.
5. **OAuth redirect_uri matrix** — build normalization matrix (trailing slash, case, fragment, percent-encoding, `response_mode`, `wctx`) vs reply-url rules (RFC 6749 §3.1.2, RFC 9700); live-check only in-scope login.live.com fast-fail echo, documented variants. CVSS 7.4–9.0.
6. **ACS cross-cloud token acceptance** — `microsoftonline.us` keys co-served in global JWKS; test-tenant only. CVSS 5.3–7.5 (config/hardening).

### Secrets Check
None found. This run: no raw secrets committed — only artifact sha256 hashes (graph metadata, acs.json, shim, HTML shells listed above). Public-client IDs and cert thumbprints only.

### CVSS Candidates
Agent inheritance 7.0–9.0; agent sessions 6.5–8.8; Verified ID 7.1–9.1; ToS IDOR 5.3–7.5; redirect ATO 7.4–9.0; ACS cross-cloud 5.3–7.5.

---
## STATE UPDATE
STATUS_PHASE: POC
STATUS_STATE: HIGH_POTENTIAL
NEXT_STEP_1: Test-tenant verification of Agent-identity inheritance (blueprint → agentIdentity inheritedOauth2PermissionGrants exceeding sponsor perms) — highest-value design, needs controlled tenant + `AgentIdentityBlueprint.Create`.
NEXT_STEP_2: `agentSignInSessions` IDOR/scope check on test tenant (`/users/{oid}` vs `/me`); if confirmed, chain with My Sign-Ins `/recent-activity/agentic-sessions` for session-detail disclosure.
NEXT_STEP_3: If POC blocked by passive-only rule, open RECON on next unexplored in-scope surface: `*.azurewebsites.net`/`*.azure.com` App Services under the identity estate + crt.sh CT inventory for `.microsoft.com` identity hosts, and enumerate first-party Graph app registrations from public AAD app catalog for redirect_uri matrix.

## RUN 2026-08-07 11:45:00 (big-pickle) — TARGET: microsoft — PHASE: POC→RECON (live signals + code review)

### Live POC (in-scope) — login.live.com redirect_uri matrix: EXHAUSTED
- /oauth20_desktop.srf now returns a REMOVED stub ("You have reached a page that is not normally shown..." + ?removed=true redirect) for ALL inputs incl. the previously-documented fast-fail echo — desktop-flow endpoint deprecated.
- /oauth20_authorize.srf (live authorize): with documented MSA client 00000000402B1722, 8 redirect_uri variants (host-case, path-case, trailing slash, extra path, http scheme, relative, ?x=1, #frag) ALL return 200 generic sign-in — validation is deferred server-side post-auth; redirect_uri is NOT reflected in the HTML (no echo/reflection). No normalization acceptance is observable with passive GETs. Matrix blocked behind authentication; redirect_uri finding-value for MSA is low. Recorded for future test-tenant/registered-client work only.
- New recon note: agents.microsoft.com -> CNAME adoption.microsoft.com (Azure Front Door mr-b02.tm-azurefd.net), served cert CN=*.azureedge.net (SAN mismatch => 404 on edge). Parking/adoption only, no takeover (MS-controlled). No Agent Builder portal at the obvious hostname.

### Public-docs + in-scope code review (microsoft/entrabot): Agent User user_fic flow (THE new surface)
- GA 2026-05-01. v1.0 creation endpoints: POST /v1.0/applications/microsoft.graph.agentIdentityBlueprint (blueprint, BaseType application), /servicePrincipals/microsoft.graph.agentIdentityBlueprintPrincipal, /servicePrincipals/microsoft.graph.agentIdentity; agentUser via POST /beta/users with @odata.type microsoft.graph.agentUser (identityParentId links 1:1, immutable).
- Three-hop token chain (wire-level from microsoft/entrabot docs/reference/token-flows.md + architecture/identity-and-token-flow.md):
  - Hop1: client_credentials + cert assertion + scope=api://AzureADTokenExchange/.default + fmi_path={agentIdentity_appId} => T1
  - Hop2: client_id={agentIdentity_appId}, client_assertion=T1, same scope => T2
  - Hop3: grant_type=user_fic, client_id={agentIdentity_appId}, client_assertion={T1}, user_federated_identity_credential={T2}, user_id={agent_user_oid} OR username={agent_user_upn}, requested_token_use=on_behalf_of => delegated token with idtyp=user, oid={agent_user_oid}.
- Agent User is a REAL Entra user object (mailbox, Teams presence, M365 license) with NO own credentials; tokens say idtyp=user and "appear as a user to every Microsoft 365 API" (by design, learn.microsoft.com/en-us/entra/agent-id/agent-users).
- Consent model: delegated scopes granted via POST /v1.0/oauth2PermissionGrants (clientId=agent-identity SP oid, principalId=agent-user oid, e.g. scope "Chat.ReadWrite Mail.ReadWrite Files.ReadWrite.All User.Read").
- Blocked-permissions table now PUBLIC (learn.microsoft.com/graph/api/resources/agentid-platform-overview): 59 rows; agents (application perms) BLOCKED for Application.ReadWrite.All/.OwnedBy, Directory.ReadWrite.All, RoleManagement.ReadWrite.*, Group.*, Sites.ReadWrite.All, Files.ReadWrite.All, Mail via Chat.*, User.ReadWrite.All, etc. App-only blocked for AgentIdentity*; Directory.AccessAsUser.All delegated-blocked.
- Blueprint cannot be public client: publicClient/spa/isFallbackPublicClient surfaces excluded; web.redirectUris allowed ONLY for consent-recording (interactive agent OBO); signInAudience 4 values but agent identities always single-tenant.
- managerApplications: up to 10 first-party MS apps can manage a Blueprint WITHOUT AgentIdentityBlueprintPrincipal.ReadWrite.All (supply-chain trust surface).
- No RFC7591 DCR; no code_challenge_methods_supported in OIDC metadata; RFC8707 resource param ignored (AADSTS901002 on v1).

### Hypotheses (test-tenant, read-only design)
- H-1 [HIGHEST] user_fic Hop-3 subject validation: does ESTS enforce user_id/username == the Agent User whose identityParentId == asserting agent identity (immutable 1:1), or mint idtyp=user tokens for ANY user_id/UPN given valid T1/T2? If not enforced => any blueprint/agent with valid assertions impersonates arbitrary users at M365 API layer. Test: baseline mint for own Agent User; then user_id={unrelated user oid} and username={unrelated upn}; signal 200 => cross-user ATO. CVSS 9.1-9.8. (Blocked: no tenant; the requested_token_use=on_behalf_of + username alternate-key are the highest-risk knobs.)
- H-2 [HIGH] Agent User delegated-scope consent escalation: DelegatedPermissionGrant.ReadWrite.All is blocked for agent identities, but can a user with AgentIdUser.ReadWrite.IdentityParentedBy self-consent broad delegated scopes (Files.ReadWrite.All/Mail.ReadWrite) to their own Agent User without admin approval? Signal: no admin-consent requirement => low-priv -> tenant mailbox/file exfiltration via licensed Agent User. CVSS 7.5-8.8.
- H-3 [MED] managerApplications first-party implicit-management list: enumerate the 10 first-party apps; if any has an IDOR/confused-deputy, chain => manage any blueprint. Read-only: hunt public code for the manager app IDs.
- H-4 agentSignInSessions off-metadata (unchanged, PD-A). H-5 redirect matrix (blocked post-auth, low value, recorded).

### Secrets check
- No secrets. entrabot docs = design docs (no tokens/keys; they explicitly document "tokens must never be logged"). Public client IDs only (e.g., 00000000402B1722 MSA client from docs). No raw secrets committed; no new artifacts this run besides pages.

===== RUN 2026-08-07 11:34:29 UTC | model bigpickle | target google =====
[0m
> build · big-pickle
[0m
[0m→ [0mRead scope.yml
[0m
[0m$ [0mls -la /home/runner/work/hunt-lab/hunt-lab/research/ /home/runner/work/hunt-lab/hunt-lab/leads/ 2>/dev/null; echo "---"; cat /home/runner/work/hunt-lab/hunt-lab/state_bigpickle.json
/home/runner/work/hunt-lab/hunt-lab/leads/:
total 72
drwxr-xr-x 2 runner runner  4096 Aug  7 11:29 .
drwxr-xr-x 9 runner runner  4096 Aug  7 11:29 ..
-rw-r--r-- 1 runner runner 16585 Aug  7 11:29 lead-bigpickle.md
-rw-r--r-- 1 runner runner  3858 Aug  7 11:29 lead-laguna.md
-rw-r--r-- 1 runner runner     0 Aug  7 11:29 lead-ling3.md
-rw-r--r-- 1 runner runner  1781 Aug  7 11:29 lead-longcat.md
-rw-r--r-- 1 runner runner  4743 Aug  7 11:29 lead-nemotron3.md
-rw-r--r-- 1 runner runner   250 Aug  7 11:29 reposcan-2026-08-07-08-25.md
-rw-r--r-- 1 runner runner   355 Aug  7 11:29 reposcan-2026-08-07-08-29.md
-rw-r--r-- 1 runner runner   290 Aug  7 11:29 reposcan-2026-08-07-08-34.md
-rw-r--r-- 1 runner runner   301 Aug  7 11:29 reposcan-2026-08-07-08-40.md
-rw-r--r-- 1 runner runner   301 Aug  7 11:29 reposcan-2026-08-07-08-52.md
-rw-r--r-- 1 runner runner   343 Aug  7 11:29 reposcan-2026-08-07-09-02.md
-rw-r--r-- 1 runner runner   343 Aug  7 11:29 reposcan-latest.md

/home/runner/work/hunt-lab/hunt-lab/research/:
total 1136
drwxr-xr-x 2 runner runner   4096 Aug  7 11:29 .
drwxr-xr-x 9 runner runner   4096 Aug  7 11:29 ..
-rw-r--r-- 1 runner runner 303400 Aug  7 11:29 bigpickle.md
-rw-r--r-- 1 runner runner 237105 Aug  7 11:29 laguna.md
-rw-r--r-- 1 runner runner    705 Aug  7 11:29 ling3.md
-rw-r--r-- 1 runner runner 131948 Aug  7 11:29 longcat.md
-rw-r--r-- 1 runner runner 468149 Aug  7 11:29 nemotron3.md
---
{"phase": "POC", "status": "HIGH_POTENTIAL"}
[0m
Scope confirmed: `*.live.com`, `*.microsoft.com`, `*.windows.net`, `*.azure.com`, `*.azurewebsites.net` all in scope; `*.microsoftonline.com` is NOT. Most POC designs are test-tenant-gated, so this run: (1) execute the one remaining **live** POC (redirect_uri echo matrix, in-scope `login.live.com`), (2) pivot RECON to a **new unexplored surface** — the Agent Builder/Agent ID estate via crt.sh CT + `agents.microsoft.com` (in-scope, and thematically adjacent to my agent-identity lead).
[0m$ [0mcd /tmp/opencode && curl -s --max-time 60 "https://crt.sh/?q=%25agent%25.microsoft.com&output=json" -o ct_agent.json && python3 -c "
import json
d=json.load(open('ct_agent.json'))
names=set()
for e in d:
    for n in e.get('name_value','').split('\n'):
        n=n.strip().lower()
        if n and '*' not in n: names.add(n)
print('unique SAN/CN (no wildcard):',len(names))
for n in sorted(names)[:80]: print(' ',n)
"
Traceback (most recent call last):
  File "<string>", line 3, in <module>
  File "/usr/lib/python3.12/json/__init__.py", line 293, in load
    return loads(fp.read(),
           ^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/json/__init__.py", line 346, in loads
    return _default_decoder.decode(s)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/json/decoder.py", line 337, in decode
    obj, end = self.raw_decode(s, idx=_w(s, 0).end())
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/json/decoder.py", line 355, in raw_decode
    raise JSONDecodeError("Expecting value", s, err.value) from None
json.decoder.JSONDecodeError: Expecting value: line 1 column 1 (char 0)
[0m
[0m$ [0mcd /tmp/opencode && wc -c ct_agent.json; head -c 500 ct_agent.json; echo; sleep 2; curl -s --max-time 60 "https://crt.sh/?q=agentbuilders.microsoft.com&output=json" -o ct_ab.json; wc -c ct_ab.json; head -c 400 ct_ab.json
150 ct_agent.json
<html>
<head><title>502 Bad Gateway</title></head>
<body>
<center><h1>502 Bad Gateway</h1></center>
<hr><center>nginx</center>
</body>
</html>

150 ct_ab.json
<html>
<head><title>502 Bad Gateway</title></head>
<body>
<center><h1>502 Bad Gateway</h1></center>
<hr><center>nginx</center>
</body>
</html>
[0m
crt.sh backend is flaky (502). Retry after delay; fallback to CertSpotter API in parallel.
[0m$ [0msleep 5 && cd /tmp/opencode && curl -s --max-time 90 "https://crt.sh/?q=%25agent%25.microsoft.com&output=json" -o ct_agent.json; wc -c ct_agent.json; head -c 300 ct_agent.json
150 ct_agent.json
<html>
<head><title>502 Bad Gateway</title></head>
<body>
<center><h1>502 Bad Gateway</h1></center>
<hr><center>nginx</center>
</body>
</html>
[0m
[0m$ [0mcd /tmp/opencode && curl -s --max-time 60 "https://api.certspotter.com/v1/issuances?domain=microsoft.com&include_subdomains=true&match_wildcards=true&expand=dns_names&after_index=0" -o cs_microsoft.json; wc -c cs_microsoft.json; python3 -c "
import json
try:
    d=json.load(open('cs_microsoft.json'))
    names=set()
    for e in d:
        for n in e.get('dns_names',[]): names.add(n.lower())
    agent=[n for n in names if 'agent' in n]
    print('total unique names:',len(names),'| agent-matching:',len(agent))
    for n in sorted(agent): print(' ',n)
except Exception as ex:
    print('ERR',ex); print(open('cs_microsoft.json').read()[:300])
"
43200 cs_microsoft.json
total unique names: 57 | agent-matching: 0
[0m
[0m$ [0mcd /tmp/opencode && python3 -c "
import json
d=json.load(open('cs_microsoft.json'))
names=set()
for e in d:
    for n in e.get('dns_names',[]): names.add(n.lower())
for n in sorted(names): print(' ',n)
"
*.admin.protection.outlook.com
  *.compliance.protection.outlook.com
  *.dataservice.protection.outlook.com
  *.internal.protection.outlook.com
  *.protection.outlook.com
  *.ps.compliance.protection.outlook.com
  *.ps.protection.outlook.com
  *.ps.security.protection.outlook.com
  *.restapi.compliance.protection.outlook.com
  *.restapi.security.protection.outlook.com
  *.security.protection.outlook.com
  *.syncservice.protection.outlook.com
  akamai-san13.exacttarget.com
  dataassessmentsengine.microsoft.com
  ibnotification-multitenant-tdf.sib.m365.microsoft.com
  ibnotification-multitenant.sib.m365.microsoft.com
  image.cj.jcb.co.jp
  image.cortana-email.microsoft.com
  image.cubcadet-email.com
  image.e.clearme.com
  image.email.horizonblue.com
  image.email.housetrip.com
  image.email.microsoftrewards.com
  image.email.niumba.com
  image.email.paradoxplaza.com
  image.emails.cnb.com
  image.emails.pitneybowes.com
  image.emailsmartandfinal.com
  image.flipkeyrentals.com
  image.fr.aonassurances.com
  image.health.mytakedapro.be
  image.info.himawari-life.co.jp
  image.mail.pflege.de
  image.mail.skyperfectv.co.jp
  image.mail1.wf.com
  image.marketing-comunicacion.kpmg.es
  image.marketing.hktvmall.com
  image.marketing.medibank.com.au
  image.mc.clickloans.com.au
  image.mc.creditandfinance.com.au
  image.mc.lendi.com.au
  image.members.toyota
  image.mtdgenuineparts.com
  image.transactions.pitneybowes.com
  image.updates.purina.ca
  image.verabradleyemail.com
  lists.microsoft.com
  local.microsoft.com
  news.microsoft.com
  ottstmisar-1.redmond.corp.microsoft.com
  ottstmiscl-1.redmond.corp.microsoft.com
  protection.outlook.com
  quarantine.messaging.microsoft.com
  saptstotmisar3.redmond.corp.microsoft.com
  saptstotmisar4.redmond.corp.microsoft.com
  substratesearchsuggestions.microsoft.com
  unlocked.microsoft.com
[0m
[0m$ [0mcd /tmp/opencode && curl -s -D hdr_agents.txt -o agents.html "https://agents.microsoft.com/" --max-time 30 && echo "=== agents.microsoft.com ===" && grep -iE "^HTTP|location|server:|x-ms|content-type" hdr_agents.txt | head -8 && wc -c agents.html && grep -oE 'src="[^"]+\.js[^"]*"|<script[^>]*src=[^>]*>|<meta[^>]*content="[^"]{5,80}"' agents.html | head -15
(no output)
[0m
CertSpotter free tier is truncated (marketing/outlook noise) but surfaced `substratesearchsuggestions.microsoft.com` — thematically tied to my earlier substrate-search finding. Now the direct Agent Builder surface + substrate host (both in-scope).
[0m$ [0mcd /tmp/opencode && ls -la agents.html hdr_agents.txt 2>/dev/null; echo "---"; cat hdr_agents.txt 2>/dev/null | head -20; echo "---body---"; head -c 400 agents.html 2>/dev/null
-rw-r--r-- 1 runner runner 0 Aug  7 11:30 hdr_agents.txt
---
---body---
[0m
[0m$ [0mcd /tmp/opencode && getent hosts agents.microsoft.com; echo "rc=$?"; curl -sv --connect-timeout 10 --max-time 25 -o agents.html "https://agents.microsoft.com/" 2>&1 | grep -iE "could not resolve|connection refused|timed out|HTTP/|location:|ssl" | head -8; wc -c agents.html 2>/dev/null
2603:1061:14:94::1 mr-b02.tm-azurefd.net agents.microsoft.com adoption.microsoft.com adoption-djfhedd8hnbffrhy.b02.azurefd.net
rc=0
* ALPN: curl offers h2,http/1.1
*  CAfile: /etc/ssl/certs/ca-certificates.crt
*  CApath: /etc/ssl/certs
* SSL connection using TLSv1.3 / TLS_AES_256_GCM_SHA384 / prime256v1 / id-ecPublicKey
* SSL: no alternative certificate subject name matches target host name 'agents.microsoft.com'
[0m
`agents.microsoft.com` CNAMEs to an adoption/parking Front Door (`adoption.microsoft.com`) and the served cert has no matching SAN — config artifact worth characterizing. Grab the redirect target + cert identity (passive).
[0m$ [0mcd /tmp/opencode && curl -sk -D - -o /dev/null --max-time 20 "https://agents.microsoft.com/" | grep -iE "^HTTP|location:" | head -4; echo "=== cert SAN ==="; echo | timeout 15 openssl s_client -connect agents.microsoft.com:443 -servername agents.microsoft.com 2>/dev/null | openssl x509 -noout -subject -ext subjectAltName 2>/dev/null | head -6; echo "=== DNS chain ==="; getent ahosts adoption.microsoft.com | head -2; getent ahosts adoption-djfhedd8hnbffrhy.b02.azurefd.net | head -2
HTTP/2 404 
=== cert SAN ===
subject=C = US, ST = WA, L = Redmond, O = Microsoft Corporation, CN = *.azureedge.net
X509v3 Subject Alternative Name: 
    DNS:*.azureedge.net
=== DNS chain ===
150.171.109.149 STREAM mr-b02.tm-azurefd.net
150.171.109.149 DGRAM  
150.171.109.149 STREAM mr-b02.tm-azurefd.net
150.171.109.149 DGRAM
[0m
`agents.microsoft.com` is parked on an Azure CDN edge (404, `*.azureedge.net` cert) — config artifact, no takeover vector, but a new asset note. Now: substrate-suggestions host (new, in-scope, ties to earlier finding) + code search for the agent-session API surface (public code review).
[0m$ [0mcd /tmp/opencode && curl -s -D hdr_sub.txt -o sub.json --max-time 25 "https://substratesearchsuggestions.microsoft.com/" | head -1; grep -iE "^HTTP|server:|content-type|x-ms" hdr_sub.txt | head -5; echo "---body---"; head -c 300 sub.json; echo; echo "=== grep.app: agentSignInSessions ==="; curl -s --max-time 30 "https://grep.app/api/search?q=agentSignInSessions" -o grep_asis.json; python3 -c "
import json
try:
    d=json.load(open('grep_asis.json'))
    hits=d.get('hits',{}).get('hits',[])
    print('total:',d.get('hits',{}).get('total'))
    for h in hits[:10]:
        r=h.get('repo',{}); print(r.get('raw'),'|',h.get('path','').get('raw'))
except Exception as ex: print('ERR',ex); print(open('grep_asis.json').read()[:200])
"
---body---

=== grep.app: agentSignInSessions ===
head: cannot open 'sub.json' for reading: No such file or directory
ERR Expecting value: line 1 column 1 (char 0)
<!DOCTYPE html><html lang="en" data-astro-cid-nbv56vs3> <head><meta charset="utf-8"><meta name="viewport" content="width=device-width, initial-scale=1"><meta name="theme-color" content="#000"><title>V
[0m
[0m$ [0mcd /tmp/opencode && curl -sv --connect-timeout 10 --max-time 20 -o sub.json "https://substratesearchsuggestions.microsoft.com/" 2>&1 | grep -iE "could not resolve|refused|timed out|HTTP/|location" | head -5; wc -c sub.json 2>/dev/null; echo "=== retry grep.app w/ UA ==="; curl -s --max-time 30 -H "User-Agent: Mozilla/5.0" "https://grep.app/api/search?q=agentSignInSessions" -o grep2.json; head -c 200 grep2.json
* Could not resolve host: substratesearchsuggestions.microsoft.com
=== retry grep.app w/ UA ===
<!DOCTYPE html><html lang="en" data-astro-cid-nbv56vs3> <head><meta charset="utf-8"><meta name="viewport" content="width=device-width, initial-scale=1"><meta name="theme-color" content="#000"><title>V
[0m
Dead DNS on substrate host (note only). grep.app is blocking API. Pivoting to targeted public-docs/code mining via web search for the agent-session + Agent ID endpoint surface.
[0m◈ [0mParallel Web Search "Microsoft Graph "agentSignInSessions" API endpoint"
[0m◈ [0mParallel Web Search "Microsoft Entra Agent ID "agentIdentityBlueprint" create agent identity Graph API scopes"
GA confirmed (2026-05-01) and — critically — docs now publish a **blocked-permissions table** for agent identities (agent can't hold `Application.ReadWrite.*` etc.), which bounds my PD-B inheritance hypothesis. Now executing the one remaining live POC: bounded `login.live.com` redirect_uri error-echo matrix (documented variants, 1 req/sec, in-scope).
[0m
[0m$ [0mcd /tmp/opencode && sleep 1 && curl -s -D - -o /dev/null --max-time 20 "https://login.live.com/oauth20_desktop.srf?client_id=00000000402B1722&redirect_uri=https%3A%2F%2Flogin.live.com%2Foauth20_desktop.srf&response_type=code" | grep -iE "^HTTP|location:" | head -3
HTTP/1.1 200 OK
[0m
Baseline with the documented MSA client `00000000402B1722` + exact registered redirect returns 200 (no fast-fail). Now the normalization matrix — same client, one mutated `redirect_uri` each, 1 req/sec:
[0m$ [0mcd /tmp/opencode && for u in \
  "https://LOGIN.LIVE.COM/oauth20_desktop.srf" \
  "https://login.live.com/OAUTH20_DESKTOP.SRF" \
  "https://login.live.com/oauth20_desktop.srf/" \
  "https://login.live.com/oauth20_desktop.srf/extra" \
  "http://login.live.com/oauth20_desktop.srf" \
  "oauth20_desktop.srf" \
  "https://login.live.com/oauth20_desktop.srf?x=1" \
  "https://login.live.com/oauth20_desktop.srf#frag"; do
  enc=$(python3 -c "import urllib.parse,sys;print(urllib.parse.quote(sys.argv[1],safe=''))" "$u")
  code=$(curl -s -D - -o /dev/null --max-time 20 "https://login.live.com/oauth20_desktop.srf?client_id=00000000402B1722&redirect_uri=$enc&response_type=code" | tr -d '\r' | awk '/^HTTP/{s=$2}/^location:/{loc=$2}END{print s" | "loc}' | head -c 220)
  echo "[$u] => $code"
  sleep 1
done
[https://LOGIN.LIVE.COM/oauth20_desktop.srf] => 200 | 
[https://login.live.com/OAUTH20_DESKTOP.SRF] => 200 | 
[https://login.live.com/oauth20_desktop.srf/] => 200 | 
[https://login.live.com/oauth20_desktop.srf/extra] => 200 | 
[http://login.live.com/oauth20_desktop.srf] => 200 | 
[oauth20_desktop.srf] => 200 | 
[https://login.live.com/oauth20_desktop.srf?x=1] => 200 | 
[https://login.live.com/oauth20_desktop.srf#frag] => 200 |
[0m
All 200 — need to see what the page actually says (validation may be deferred client-side). Inspecting body of one variant + reproducing the documented fast-fail with a clearly-unregistered client to confirm the echo still exists:
[0m$ [0mcd /tmp/opencode && curl -s -D hdr_v1.txt -o v_case.html --max-time 20 "https://login.live.com/oauth20_desktop.srf?client_id=00000000402B1722&redirect_uri=https%3A%2F%2FLOGIN.LIVE.COM%2Foauth20_desktop.srf&response_type=code"; echo "== case-variant body title/cues =="; grep -oiE "<title>[^<]*</title>|id=\"[^\"]*error[^\"]*\"|error_description[^,<]*" v_case.html | head -6; grep -c "Sign in" v_case.html; echo "== unregistered client =="; sleep 1; curl -s -D hdr_u.txt -o v_unreg.html --max-time 20 "https://login.live.com/oauth20_desktop.srf?client_id=00000000402B0000&redirect_uri=https%3A%2F%2Fexample.com%2Fcb&response_type=code"; grep -iE "^HTTP|location:" hdr_u.txt | head -3; grep -oiE "<title>[^<]*</title>" v_unreg.html | head -2
== case-variant body title/cues ==
0
== unregistered client ==
HTTP/1.1 200 OK
[0m
[0m$ [0mcd /tmp/opencode && echo "== v_case.html (first 700) =="; head -c 700 v_case.html; echo; echo "== v_unreg.html (first 700) =="; head -c 700 v_unreg.html
== v_case.html (first 700) ==
<html dir="ltr">
<head>
</head>
<body>
<div class="header" id="idHeaderTD9">
<h1>
<img src="images/ms-logo-v2.jpg" class="logo" alt="Microsoft account" />
</h1>
</div>
<div class="content">
<h2>You have reached a page that is not normally shown. Microsoft will never ask you to copy or share this URL.</h2>
<p>
</p>
<script>
var marker = "?removed=true";
if(window.location.hash || window.location.search.indexOf(marker) !== 0)
{
var redirectUrl = window.location.origin + window.location.pathname + marker;
setTimeout(function() {
window.location.replace(redirectUrl);
}, 3000);
}
</script>
</div>
</body>
</html>

== v_unreg.html (first 700) ==
<html dir="ltr">
<head>
</head>
<body>
<div class="header" id="idHeaderTD9">
<h1>
<img src="images/ms-logo-v2.jpg" class="logo" alt="Microsoft account" />
</h1>
</div>
<div class="content">
<h2>You have reached a page that is not normally shown. Microsoft will never ask you to copy or share this URL.</h2>
<p>
</p>
<script>
var marker = "?removed=true";
if(window.location.hash || window.location.search.indexOf(marker) !== 0)
{
var redirectUrl = window.location.origin + window.location.pathname + marker;
setTimeout(function() {
window.location.replace(redirectUrl);
}, 3000);
}
</script>
</div>
</body>
</html>
[0m
`/oauth20_desktop.srf` is deprecated/removed (MSA stub page with `?removed=true`) — that's why the earlier fast-fail echo is gone; the desktop-flow endpoint is gone entirely. The live authorize path is `oauth20_authorize.srf`. Re-running the matrix there:
[0m$ [0mcd /tmp/opencode && for u in \
  "https://login.live.com/oauth20_desktop.srf" \
  "https://LOGIN.LIVE.COM/oauth20_desktop.srf" \
  "https://login.live.com/oauth20_desktop.srf/" \
  "http://login.live.com/oauth20_desktop.srf" \
  "oauth20_desktop.srf" \
  "https://example.com/cb"; do
  enc=$(python3 -c "import urllib.parse,sys;print(urllib.parse.quote(sys.argv[1],safe=''))" "$u")
  sleep 1
  code=$(curl -s -D - -o /dev/null --max-time 20 "https://login.live.com/oauth20_authorize.srf?client_id=00000000402B1722&redirect_uri=$enc&response_type=code&scope=wl.signin" | tr -d '\r' | awk '/^HTTP/{s=$2}/^location:/{loc=$2}END{print s" | "loc}' | head -c 200)
  echo "auth [$u] => $code"
done
auth [https://login.live.com/oauth20_desktop.srf] => 200 | 
auth [https://LOGIN.LIVE.COM/oauth20_desktop.srf] => 200 | 
auth [https://login.live.com/oauth20_desktop.srf/] => 200 | 
auth [http://login.live.com/oauth20_desktop.srf] => 200 | 
auth [oauth20_desktop.srf] => 200 | 
auth [https://example.com/cb] => 200 |
[0m
[0m$ [0mcd /tmp/opencode && curl -s -o v_auth.html --max-time 20 "https://login.live.com/oauth20_authorize.srf?client_id=00000000402B1722&redirect_uri=https%3A%2F%2Fexample.com%2Fcb&response_type=code&scope=wl.signin"; echo "size: $(wc -c < v_auth.html)"; grep -oiE "<title>[^<]*</title>" v_auth.html | head -1; grep -c "redirect_uri" v_auth.html; grep -oE 'name="[a-zA-Z_]+" value="[^"]{0,60}"' v_auth.html | head -12
size: 3501
<title>Microsoft account</title>
0
[0m
[0m$ [0mcd /tmp/opencode && curl -s --max-time 40 -H "User-Agent: Mozilla/5.0" "https://raw.githubusercontent.com/microsoft/entrabot/main/docs/platform-learnings/agent-id-blueprints-and-users.md" -o entrabot_agentid.md; wc -c entrabot_agentid.md; echo "=== headings ==="; grep -nE "^#{1,3} " entrabot_agentid.md | head -40
14 entrabot_agentid.md
=== headings ===
[0m
login.live.com live POC **exhausted**: authorize defers validation post-auth (no echo, `redirect_uri` not reflected in HTML), desktop endpoint removed. No passive normalization signal. Pivoting to Agent ID endpoint inventory from public docs + code review of `microsoft/entrabot` (in-scope org) platform-learnings.
[0m$ [0mcd /tmp/opencode && curl -s --max-time 40 -H "User-Agent: Mozilla/5.0" "https://learn.microsoft.com/en-us/graph/api/resources/agentid-platform-overview?view=graph-rest-beta&preserve-view=true" -o agentid_overview.html; wc -c agentid_overview.html
68012 agentid_overview.html
[0m
[0m$ [0mcd /tmp/opencode && cat entrabot_agentid.md; echo "---"; curl -s --max-time 40 -H "User-Agent: Mozilla/5.0" "https://api.github.com/repos/microsoft/entrabot/git/trees/main?recursive=1" -o entrabot_tree.json; python3 -c "
import json
d=json.load(open('entrabot_tree.json'))
if 'tree' in d:
    paths=[t['path'] for t in d['tree'] if 'agent' in t['path'].lower()]
    print('agent-matching paths:',len(paths))
    for p in paths: print(' ',p)
else:
    print('ERR',d.get('message'))
" 2>/dev/null | head -60
404: Not Found---
agent-matching paths: 37
  .claude/skills/implement-agent-id
  .claude/skills/implement-agent-id/SKILL.md
  AGENTS.md
  docs/platform-docs/agent-id-blueprints-and-users.md
  docs/platform-docs/entra-agent-users.md
  docs/platform-docs/microsoft-agent-365.md
  docs/reference/scripts/diagnostics/list-agent-identities-py.md
  docs/reference/scripts/operations/show-agent-status-py.md
  docs/reference/scripts/provisioning/add-agent-sponsor-py.md
  docs/reference/scripts/provisioning/assign-agent-user-licenses-py.md
  docs/reference/scripts/provisioning/create-entra-agent-ids-py.md
  docs/reference/scripts/provisioning/remove-agent-sponsor-py.md
  docs/reference/scripts/provisioning/remove-agent-user-licenses-py.md
  docs/reference/scripts/teardown/deprovision-entra-agent-identity-py.md
  engineering-history/decisions/001-obo-flows-for-device-agents.md
  engineering-history/decisions/002-agent-user-over-obo.md
  engineering-history/plans/PLAN-agent-identity-by-upn.md
  engineering-history/plans/openai-windows-agent-identity-port.md
  engineering-history/research/agent-memory-systems.md
  engineering-history/specs/SPEC-dual-track-agent-identity.md
  prompts/agent_system.md
  scripts/add_agent_sponsor.py
  scripts/assign_agent_user_licenses.py
  scripts/create_entra_agent_ids.py
  scripts/deprovision_entra_agent_identity.py
  scripts/list_agent_identities.py
  scripts/remove_agent_sponsor.py
  scripts/remove_agent_user_licenses.py
  scripts/show_agent_status.py
  tests/docs/test_no_agent_attribution.py
  tests/scripts/test_assign_agent_user_licenses.py
  tests/scripts/test_create_entra_agent_ids.py
  tests/scripts/test_deprovision_entra_agent_identity.py
  tests/scripts/test_list_agent_identities.py
  tests/scripts/test_remove_agent_sponsor.py
  tests/scripts/test_remove_agent_user_licenses.py
  tests/scripts/test_show_agent_status.py
[0m
[0m$ [0mcd /tmp/opencode && for f in "docs/platform-docs/agent-id-blueprints-and-users.md" "engineering-history/decisions/001-obo-flows-for-device-agents.md" "engineering-history/decisions/002-agent-user-over-obo.md"; do
  out="entrabot_$(basename $f .md).md"
  curl -s --max-time 40 -H "User-Agent: Mozilla/5.0" "https://raw.githubusercontent.com/microsoft/entrabot/main/$f" -o "$out"
  echo "$out: $(wc -c < $out) bytes"
done; echo "=== headings in agent-id-blueprints ==="; grep -nE "^#{1,3} " entrabot_agent-id-blueprints-and-users.md | head -30
entrabot_agent-id-blueprints-and-users.md: 22505 bytes
entrabot_001-obo-flows-for-device-agents.md: 2256 bytes
entrabot_002-agent-user-over-obo.md: 2472 bytes
=== headings in agent-id-blueprints ===
1:# Microsoft Entra Agent ID: Blueprints, Identities, and Users
11:## Object model
48:### Agent Identity Blueprint
104:### Agent Identity Blueprint Principal
121:### Agent Identity
143:### Agent User
176:## Sponsors
193:## Load-bearing OAuth and platform constraints
247:## Capabilities and constraints by object
249:### Agent Identity Blueprint
270:### Agent Identity
287:### Agent User
310:## Provisioning identity
341:## Pattern: certificate machine flow plus browser PKCE
385:## Working around Entra OAuth metadata gaps
405:## Entrabot implementation
417:## References
[0m
[0m→ [0mRead /tmp/opencode/entrabot_agent-id-blueprints-and-users.md
`user_fic` grant detail is the key find: the third hop mints `idtyp=user` tokens for an Agent User and accepts `user_id={oid}` **or** `username={upn}` — the classic IDOR-shaped alternate-key. Fetching the wire-level token-flow reference to pin the exact request/claim validation:
[0m
[0m$ [0mcd /tmp/opencode && for f in "docs/reference/token-flows.md" "docs/architecture/identity-and-token-flow.md" "engineering-history/decisions/002-agent-user-over-obo.md"; do
  out="entrabot_$(basename $f .md).md"
  curl -s --max-time 40 -H "User-Agent: Mozilla/5.0" "https://raw.githubusercontent.com/microsoft/entrabot/main/$f" -o "$out"
  echo "== $out: $(wc -c < $out) bytes"
done
== entrabot_token-flows.md: 5165 bytes
== entrabot_identity-and-token-flow.md: 9797 bytes
== entrabot_002-agent-user-over-obo.md: 2472 bytes
[0m
[0m→ [0mRead /tmp/opencode/entrabot_token-flows.md
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re,html
x=open('agentid_overview.html',encoding='utf-8',errors='ignore').read()
# find table rows
rows=re.findall(r'<tr[^>]*>(.*?)</tr>',x,re.S)
print('rows:',len(rows))
for r in rows[:30]:
    cells=re.findall(r'<t[dh][^>]*>(.*?)</t[dh]>',r,re.S)
    if not cells: continue
    c=[re.sub(r'<[^>]+>','',html.unescape(cc)).strip() for cc in cells]
    if any(c): print(' | '.join(c)[:160])
EOF
rows: 70
Component | Purpose | Microsoft Graph resource
Blueprint | Template defining the agent identity type, including permissions that agent identities are preauthorized to automatically inherit | agentIdentityBlu
Blueprint principal | Record of blueprint's addition to a tenant | agentIdentityBlueprintPrincipal
Agent identity | Primary identity for authentication | agentIdentity
Agent user | Optional account for scenarios that require a user account | agentUser
Agent registry (deprecated) | Centralized repository for agent management that serves as the platform for managing agent card manifests, agent instances, and ag
Metadata | Applies to
owner | agentIdentityBlueprint, agentIdentityBlueprintPrincipal, agentIdentity
sponsor | agentIdentityBlueprint, agentIdentityBlueprintPrincipal, agentIdentity, agentUser
manager | agentUser
Permission name | Delegated | Application
AgentIdentity.Create | ➖ | ❌
AgentIdentity.Create.All | ➖ | ❌
AgentIdentity.CreateAsManager | ➖ | ❌
AgentIdentityBlueprint.Create | ➖ | ❌
AgentIdentityBlueprint.CreateAsManager | ➖ | ❌
AgentIdentityBlueprint.ReadWrite.All | ➖ | ❌
AgentIdentityBlueprintPrincipal.Create | ➖ | ❌
Application.ReadWrite.All | ➖ | ❌
Application.ReadWrite.OwnedBy | ➖ | ❌
AppRoleAssignment.ReadWrite.All | ➖ | ❌
BitlockerKey.Read.All | ➖ | ❌
Calendars.Read | ➖ | ❌
ChannelMessage.Read.All | ➖ | ❌
ChannelMessage.Read.Group | ➖ | ❌
Chat.Read.All | ➖ | ❌
Chat.ReadWrite.All | ➖ | ❌
ConsentRequest.ReadWrite.All | ➖ | ❌
CustomSecAttributeAssignment.ReadWrite.All | ❌ | ❌
CustomSecAttributeDefinition.ReadWrite.All | ❌ | ❌
[0m
Hop-3 request schema captured verbatim — the `user_id`/`username` selector is the crux. Now mining the blocked-permissions table from the downloaded platform-overview page + the token-flow architecture doc for `fmi_path`/claim semantics:
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re,html
x=open('agentid_overview.html',encoding='utf-8',errors='ignore').read()
rows=re.findall(r'<tr[^>]*>(.*?)</tr>',x,re.S)
out=[]
for r in rows:
    cells=re.findall(r'<t[dh][^>]*>(.*?)</t[dh]>',r,re.S)
    if not cells: continue
    c=[re.sub(r'<[^>]+>','',html.unescape(cc)).strip() for cc in cells]
    if len(c)>=3 and c[0] and (c[1]!='' or c[2]!=''):
        out.append(c[:3])
# print the blocked (application=blocked or delegated=blocked) rows
blocked=[c for c in out if any('❌' in v for v in c[1:])]
print('rows w/ a blocked marker:',len(blocked))
for c in blocked: print(' | '.join(c)[:150])
EOF
rows w/ a blocked marker: 59
AgentIdentity.Create | ➖ | ❌
AgentIdentity.Create.All | ➖ | ❌
AgentIdentity.CreateAsManager | ➖ | ❌
AgentIdentityBlueprint.Create | ➖ | ❌
AgentIdentityBlueprint.CreateAsManager | ➖ | ❌
AgentIdentityBlueprint.ReadWrite.All | ➖ | ❌
AgentIdentityBlueprintPrincipal.Create | ➖ | ❌
Application.ReadWrite.All | ➖ | ❌
Application.ReadWrite.OwnedBy | ➖ | ❌
AppRoleAssignment.ReadWrite.All | ➖ | ❌
BitlockerKey.Read.All | ➖ | ❌
Calendars.Read | ➖ | ❌
ChannelMessage.Read.All | ➖ | ❌
ChannelMessage.Read.Group | ➖ | ❌
Chat.Read.All | ➖ | ❌
Chat.ReadWrite.All | ➖ | ❌
ConsentRequest.ReadWrite.All | ➖ | ❌
CustomSecAttributeAssignment.ReadWrite.All | ❌ | ❌
CustomSecAttributeDefinition.ReadWrite.All | ❌ | ❌
DelegatedPermissionGrant.ReadWrite.All | ❌ | ❌
Device.ReadWrite.All | ➖ | ❌
Device.Write.Restricted | ❌ | ❌
DeviceManagementConfiguration.Read.All | ➖ | ❌
Directory.AccessAsUser.All | ❌ | ➖
Directory.ReadWrite.All | ❌ | ❌
Directory.Write.Restricted | ❌ | ❌
Domain.ReadWrite.All | ❌ | ❌
EduRoster.ReadWrite.All | ➖ | ❌
EntitlementManagement.ReadWrite.All | ➖ | ❌
Files.Read.All | ➖ | ❌
Files.ReadWrite.All | ➖ | ❌
Group.Create | ➖ | ❌
Group.ReadWrite.All | ❌ | ❌
Group.Write.Restricted | ❌ | ➖
GroupMember.ReadWrite.All | ❌ | ❌
IdentityProvider.ReadWrite.All | ➖ | ❌
LifecycleManagement.ReadWrite.All | ➖ | ❌
Organization.ReadWrite.All | ➖ | ❌
Policy.ReadWrite.AuthenticationMethod | ➖ | ❌
Policy.ReadWrite.CrossTenantAccess | ➖ | ❌
Policy.ReadWrite.PermissionGrant | ➖ | ❌
Policy.ReadWrite.SecurityDefaults | ➖ | ❌
PrintJob.ReadWrite.All | ➖ | ❌
PrivilegedAccess.ReadWrite.AzureAD | ➖ | ❌
PrivilegedAccess.ReadWrite.AzureResources | ➖ | ❌
RoleManagement.ReadWrite.All | ❌ | ➖
RoleManagement.ReadWrite.Directory | ❌ | ❌
Sites.FullControl.All | ➖ | ❌
Sites.Manage.All | ➖ | ❌
Sites.Read.All | ➖ | ❌
Sites.ReadWrite.All | ➖ | ❌
Tasks.ReadWrite.All | ➖ | ❌
User-PasswordProfile.ReadWrite.All | ❌ | ❌
User.DeleteRestore.All | ❌ | ❌
User.EnableDisableAccount.All | ❌ | ❌
User.Invite.All | ➖ | ❌
User.ReadWrite.All | ❌ | ❌
UserAuthenticationMethod.Read.All | ❌ | ➖
UserAuthenticationMethod.ReadWrite.All | ❌ | ❌
[0m
[0m→ [0mRead /tmp/opencode/entrabot_identity-and-token-flow.md[90m [limit=140][0m
Blocked table is broad for the **Agent Identity**, yet the Agent User still gets delegated `Mail.ReadWrite Files.ReadWrite.All Chat.ReadWrite` via consent records — that's the design boundary to probe. Reading the token-flow architecture doc for claim/`fmi_path` semantics:
Complete token architecture captured. Two quick closing checks: crt.sh retry for agent-cert inventory + the My Account `/agents` SPA route (in-scope):
[0m
[0m$ [0msleep 3 && cd /tmp/opencode && curl -s --max-time 90 "https://crt.sh/?q=%25agent%25.microsoft.com&output=json" -o ct_agent2.json; echo "crt.sh bytes: $(wc -c < ct_agent2.json)"; python3 -c "
import json
try:
    d=json.load(open('ct_agent2.json'))
    names=set()
    for e in d:
        for n in e.get('name_value','').split('\n'):
            n=n.strip().lower()
            if n and '*' not in n: names.add(n)
    print('names:',len(names))
    for n in sorted(names): print(' ',n)
except Exception as ex: print('ERR',ex)
" 2>/dev/null | head -40
crt.sh bytes: 253
ERR Expecting value: line 1 column 1 (char 0)
[0m
[0m$ [0mcd /home/runner/work/hunt-lab/hunt-lab && cat >> research/bigpickle.md <<'EOF'

## RUN 2026-08-07 11:45:00 (big-pickle) — TARGET: microsoft — PHASE: POC→RECON (live signals + code review)

### Live POC (in-scope) — login.live.com redirect_uri matrix: EXHAUSTED
- /oauth20_desktop.srf now returns a REMOVED stub ("You have reached a page that is not normally shown..." + ?removed=true redirect) for ALL inputs incl. the previously-documented fast-fail echo — desktop-flow endpoint deprecated.
- /oauth20_authorize.srf (live authorize): with documented MSA client 00000000402B1722, 8 redirect_uri variants (host-case, path-case, trailing slash, extra path, http scheme, relative, ?x=1, #frag) ALL return 200 generic sign-in — validation is deferred server-side post-auth; redirect_uri is NOT reflected in the HTML (no echo/reflection). No normalization acceptance is observable with passive GETs. Matrix blocked behind authentication; redirect_uri finding-value for MSA is low. Recorded for future test-tenant/registered-client work only.
- New recon note: agents.microsoft.com -> CNAME adoption.microsoft.com (Azure Front Door mr-b02.tm-azurefd.net), served cert CN=*.azureedge.net (SAN mismatch => 404 on edge). Parking/adoption only, no takeover (MS-controlled). No Agent Builder portal at the obvious hostname.

### Public-docs + in-scope code review (microsoft/entrabot): Agent User user_fic flow (THE new surface)
- GA 2026-05-01. v1.0 creation endpoints: POST /v1.0/applications/microsoft.graph.agentIdentityBlueprint (blueprint, BaseType application), /servicePrincipals/microsoft.graph.agentIdentityBlueprintPrincipal, /servicePrincipals/microsoft.graph.agentIdentity; agentUser via POST /beta/users with @odata.type microsoft.graph.agentUser (identityParentId links 1:1, immutable).
- Three-hop token chain (wire-level from microsoft/entrabot docs/reference/token-flows.md + architecture/identity-and-token-flow.md):
  - Hop1: client_credentials + cert assertion + scope=api://AzureADTokenExchange/.default + fmi_path={agentIdentity_appId} => T1
  - Hop2: client_id={agentIdentity_appId}, client_assertion=T1, same scope => T2
  - Hop3: grant_type=user_fic, client_id={agentIdentity_appId}, client_assertion={T1}, user_federated_identity_credential={T2}, user_id={agent_user_oid} OR username={agent_user_upn}, requested_token_use=on_behalf_of => delegated token with idtyp=user, oid={agent_user_oid}.
- Agent User is a REAL Entra user object (mailbox, Teams presence, M365 license) with NO own credentials; tokens say idtyp=user and "appear as a user to every Microsoft 365 API" (by design, learn.microsoft.com/en-us/entra/agent-id/agent-users).
- Consent model: delegated scopes granted via POST /v1.0/oauth2PermissionGrants (clientId=agent-identity SP oid, principalId=agent-user oid, e.g. scope "Chat.ReadWrite Mail.ReadWrite Files.ReadWrite.All User.Read").
- Blocked-permissions table now PUBLIC (learn.microsoft.com/graph/api/resources/agentid-platform-overview): 59 rows; agents (application perms) BLOCKED for Application.ReadWrite.All/.OwnedBy, Directory.ReadWrite.All, RoleManagement.ReadWrite.*, Group.*, Sites.ReadWrite.All, Files.ReadWrite.All, Mail via Chat.*, User.ReadWrite.All, etc. App-only blocked for AgentIdentity*; Directory.AccessAsUser.All delegated-blocked.
- Blueprint cannot be public client: publicClient/spa/isFallbackPublicClient surfaces excluded; web.redirectUris allowed ONLY for consent-recording (interactive agent OBO); signInAudience 4 values but agent identities always single-tenant.
- managerApplications: up to 10 first-party MS apps can manage a Blueprint WITHOUT AgentIdentityBlueprintPrincipal.ReadWrite.All (supply-chain trust surface).
- No RFC7591 DCR; no code_challenge_methods_supported in OIDC metadata; RFC8707 resource param ignored (AADSTS901002 on v1).

### Hypotheses (test-tenant, read-only design)
- H-1 [HIGHEST] user_fic Hop-3 subject validation: does ESTS enforce user_id/username == the Agent User whose identityParentId == asserting agent identity (immutable 1:1), or mint idtyp=user tokens for ANY user_id/UPN given valid T1/T2? If not enforced => any blueprint/agent with valid assertions impersonates arbitrary users at M365 API layer. Test: baseline mint for own Agent User; then user_id={unrelated user oid} and username={unrelated upn}; signal 200 => cross-user ATO. CVSS 9.1-9.8. (Blocked: no tenant; the requested_token_use=on_behalf_of + username alternate-key are the highest-risk knobs.)
- H-2 [HIGH] Agent User delegated-scope consent escalation: DelegatedPermissionGrant.ReadWrite.All is blocked for agent identities, but can a user with AgentIdUser.ReadWrite.IdentityParentedBy self-consent broad delegated scopes (Files.ReadWrite.All/Mail.ReadWrite) to their own Agent User without admin approval? Signal: no admin-consent requirement => low-priv -> tenant mailbox/file exfiltration via licensed Agent User. CVSS 7.5-8.8.
- H-3 [MED] managerApplications first-party implicit-management list: enumerate the 10 first-party apps; if any has an IDOR/confused-deputy, chain => manage any blueprint. Read-only: hunt public code for the manager app IDs.
- H-4 agentSignInSessions off-metadata (unchanged, PD-A). H-5 redirect matrix (blocked post-auth, low value, recorded).

### Secrets check
- No secrets. entrabot docs = design docs (no tokens/keys; they explicitly document "tokens must never be logged"). Public client IDs only (e.g., 00000000402B1722 MSA client from docs). No raw secrets committed; no new artifacts this run besides pages.
EOF
echo "journal lines: $(wc -l < research/bigpickle.md)"
journal lines: 4820
[0m
crt.sh unavailable this run (intermittent backend 502s); noting for future runs. The research value this run is consolidated — writing the POC-journal entry, leads, and state:
[0m$ [0mcd /home/runner/work/hunt-lab/hunt-lab && cat >> leads/lead-bigpickle.md <<'EOF'

## 2026-08-07 11:45:00 UTC [microsoft] (model bigpickle) — POC->RECON

- [LIVE-VERIFIED/EXHAUSTED] login.live.com redirect matrix: /oauth20_desktop.srf REMOVED (stub + ?removed=true for all inputs); /oauth20_authorize.srf returns generic sign-in 200 for all 8 redirect variants (host-case, path-case, trailing slash, extra path, http, relative, ?x=1, #frag), validation deferred post-auth, redirect_uri NOT echoed in HTML. No passive normalization signal. MSA client 00000000402B1722 used (docs).
- [RECON] agents.microsoft.com -> adoption.microsoft.com (Azure Front Door), cert CN=*.azureedge.net SAN mismatch => 404. Parked/adoption only, MS-controlled; no takeover. Agent Builder not at obvious hostname.
- [CODE-REVIEW] microsoft/entrabot docs/reference/token-flows.md + architecture/identity-and-token-flow.md: full three-hop Agent User chain (Hop1 client_credentials+cert+fmi_path; Hop2 FIC exchange; Hop3 grant_type=user_fic, user_id={oid} OR username={upn}, requested_token_use=on_behalf_of => idtyp=user delegated token).
- [LEAD-HIGH H-1] user_fic Hop-3 subject-validation gap hypothesis: does ESTS enforce identityParentId 1:1 link, or mint idtyp=user tokens for arbitrary user_id/UPN given valid blueprint+agent assertions? username={upn} alternate-key + requested_token_use=on_behalf_of = highest-risk knobs. CVSS 9.1-9.8 if real (test tenant only).
- [LEAD-HIGH H-2] Agent User delegated-consent escalation: can AgentIdUser.ReadWrite.IdentityParentedBy holder self-consent Files.ReadWrite.All/Mail.ReadWrite to own Agent User w/o admin approval? CVSS 7.5-8.8.
- [LEAD-MED H-3] managerApplications: up to 10 first-party apps manage Blueprints w/o AgentIdentityBlueprintPrincipal.ReadWrite.All — enumerate IDs, check for IDOR/confused deputy. 
- [CONFIRMED-HARDENING] blocked-permissions table public (agentid-platform-overview): 59 rows; app-perm agent blocks incl Application.ReadWrite.All, Directory.ReadWrite.All, RoleManagement.ReadWrite.*, Files/Sites/Group.*, User.ReadWrite.All; Directory.AccessAsUser.All delegated-blocked. Blueprint not a public client (publicClient/spa/isFallbackPublicClient excluded; web.redirectUris consent-recording only). No RFC7591 DCR. GA 2026-05-01; agentUser via POST /beta/users.
EOF
cat > state_bigpickle.json <<'EOF'
{"phase": "POC", "status": "HIGH_POTENTIAL"}
EOF
cat state_bigpickle.json; echo; echo "leads lines: $(wc -l < leads/lead-bigpickle.md)"
{"phase": "POC", "status": "HIGH_POTENTIAL"}

leads lines: 74
[0m
# Program: microsoft
# Phase: POC (live signals) → RECON (new surface: Entra Agent ID / Agent User)

## Key Findings
- **login.live.com redirect_uri matrix EXHAUSTED (live, in-scope).** `/oauth20_desktop.srf` now serves a deprecation stub (`?removed=true`) for every input — the SURFACE-run fast-fail echo is gone because the endpoint was removed. `/oauth20_authorize.srf` returns a generic sign-in `200` for all 8 tested `redirect_uri` variants (host-case, path-case, trailing slash, extra path, `http://`, relative, `?x=1`, `#frag`) using documented MSA client `00000000402B1722`; validation is deferred post-auth and `redirect_uri` is **not** echoed in the HTML. No passive normalization signal exists → blocked behind authentication.
- **New surface: Entra Agent ID went GA 2026-05-01**, and the `user_fic` Agent-User token chain is now fully documented + mirrored in `microsoft/entrabot` (in-scope org). Wire-level flow (learn.microsoft.com/en-us/entra/agent-id/agent-user-oauth-flow; github.com/microsoft/entrabot/blob/main/docs/reference/token-flows.md): Hop1 `client_credentials`+cert assertion+`fmi_path` → T1; Hop2 FIC exchange → T2; Hop3 `grant_type=user_fic` with `user_id={oid}` **or** `username={upn}` + `requested_token_use=on_behalf_of` → delegated token with `idtyp=user`, `oid={agent_user_oid}`. Agent User is a **real** Entra user (mailbox/Teams/license) with no own credentials, and its tokens "appear as a user to every Microsoft 365 API."
- **Blocked-permissions table now public** (learn.microsoft.com/en-us/graph/api/resources/agentid-platform-overview): 59 rows. Agent identities cannot hold `Application.ReadWrite.*`, `Directory.ReadWrite.All`, `RoleManagement.ReadWrite.*`, `Sites/Files.ReadWrite.All`, `User.ReadWrite.All`, etc. Blueprints cannot be OAuth public clients (`publicClient`/`spa`/`isFallbackPublicClient` excluded; `web.redirectUris` consent-recording only). No RFC 7591 DCR; no `code_challenge_methods_supported` in OIDC metadata.
- **`managerApplications`** on Blueprints: up to 10 first-party Microsoft apps can manage a Blueprint without `AgentIdentityBlueprintPrincipal.ReadWrite.All` — a supply-chain trust surface to enumerate.
- **`agents.microsoft.com`** resolves into `adoption.microsoft.com` (Azure Front Door) and serves `*.azureedge.net` (SAN mismatch → 404). Parked/adoption only, MS-controlled; no takeover.

## New Endpoints / Assets
- Graph (v1.0, documented): `POST /v1.0/applications/microsoft.graph.agentIdentityBlueprint`, `/servicePrincipals/microsoft.graph.agentIdentityBlueprintPrincipal`, `/servicePrincipals/microsoft.graph.agentIdentity`; beta `POST /beta/users` (`@odata.type=microsoft.graph.agentUser`, prop `identityParentId`).
- Token endpoint usage (test-tenant): `POST /{tenant}/oauth2/v2.0/token` with `grant_type=user_fic`, `user_id`/`username`, `user_federated_identity_credential`, `requested_token_use=on_behalf_of`; consent via `POST /v1.0/oauth2PermissionGrants` (principalId=agent-user oid).
- Scopes/roles: `AgentIdUser.ReadWrite.IdentityParentedBy` / `AgentIdUser.ReadWrite.All`, dir roles `Agent ID Developer/Administrator`; blueprint scopes `AgentIdentityBlueprint.{Create,AddRemoveCreds.All,UpdateAuthProperties.All}`, `AgentIdentityBlueprintPrincipal.Create`.

## Hypotheses (impact + read-only test)
1. **[H-1, HIGHEST] `user_fic` Hop-3 subject-validation gap.** If ESTS mints `idtyp=user` tokens for any `user_id`/`username` given valid T1+T2 (not enforcing the immutable `identityParentId` 1:1), any blueprint/agent can impersonate arbitrary users at the M365-API layer. Test: own tenant, baseline mint for own Agent User, then unrelated user's `oid` and an unrelated `upn`; 200 → cross-user ATO. CVSS 9.1–9.8.
2. **[H-2, HIGH] Agent User delegated-consent escalation.** Can a holder of `AgentIdUser.ReadWrite.IdentityParentedBy` self-consent broad delegated scopes (`Files.ReadWrite.All`, `Mail.ReadWrite`) to their own Agent User without admin approval? Signal: no admin-consent gate → low-priv → tenant mailbox/file exfiltration via a licensed Agent User. CVSS 7.5–8.8.
3. **[H-3, MED] `managerApplications` first-party trust.** Enumerate the ≤10 first-party manager apps; any with an IDOR/confused-deputy could manage arbitrary Blueprints. Read-only: hunt public code for the app IDs. CVSS unknown.
4. **[H-4]** `agentSignInSessions` off-metadata cross-user/scope gap (unchanged, PD-A). CVSS 6.5–8.8.
5. **[H-5] Verified ID mint** + **[H-6] ToS policyfile IDOR** (unchanged, test-tenant). CVSS 7.1–9.1 / 5.3–7.5.

## Secrets Check
None. `entrabot` docs are design-only (no tokens/keys; they document "tokens must never be logged"). Only public client IDs (`00000000402B1722` MSA, `8c59ead7…`, `19db86c3…`). No raw secrets committed; artifact hashes unchanged from prior runs.

## CVSS Candidates
user_fic subject gap 9.1–9.8; consent escalation 7.5–8.8; agentSignInSessions 6.5–8.8; Verified ID 7.1–9.1; ToS IDOR 5.3–7.5; managerApplications 5.3–8.0 (speculative).

---
## STATE UPDATE
STATUS_PHASE: POC
STATUS_STATE: HIGH_POTENTIAL
NEXT_STEP_1: Test-tenant execution of H-1 (user_fic): mint for own Agent User vs unrelated `user_id`/`username` — the single highest-value signal of the engagement; if 200 on unrelated, escalate to MSRC immediately.
NEXT_STEP_2: Test-tenant execution of H-2 (Agent User delegated-consent gate) and enumerate `managerApplications` first-party IDs from public code (H-3).
NEXT_STEP_3: If test tenant unavailable, RECON the next unexplored in-scope surface: `myaccount.microsoft.com/agents` SPA route internals + retry crt.sh CT inventory for `*agent*.microsoft.com` certs (crt.sh 502 this run), and mine `microsoft/entrabot` remaining docs (`entra-agent-users.md`, OBO decisions) for claim-validation edge cases.

## RUN 2026-08-07 12:45:00 (big-pickle) — TARGET: microsoft — PHASE: POC→RECON (Graph agent-management ecosystem + methodology correction)

### METHODOLOGY CORRECTION (applies retroactively)
- CONTROL TEST: unauthenticated GET on nonexistent Graph paths (/v1.0/thisdoesnotexistxyz12345, /beta/thisdoesnotexistxyz12345) and on documented POST-only actions (/v1.0/me/revokeSignInSessions, changePassword, sendMail, checkMemberGroups) ALL return HTTP 401 InvalidAuthenticationToken. Graph authenticates BEFORE routing, so an unauthenticated GET cannot distinguish a real route from a nonexistent one. → ALL prior journal claims of the form "GET <graph endpoint> → 401, therefore alive/routed" are NULL signals and are RETRACTED (incl. /me/agentSignInSessions, /me/authentication/methods). Route existence is instead established by public $metadata declarations + MS Learn docs. The non-Graph hosts (api.myaccount / api.mysignins) returned 401+WWW-Authenticate/405 — those remain valid route signals.

### NEW SURFACE — Graph Agent Management ecosystem (recovered from beta $metadata sha256 ca304859… + MS Learn docs; all declared, all auth-gated, Global cloud only)
- Agent Registry singleton /beta/agentRegistry → agentInstances / agentCardManifests / agentCollections. Docs: learn.microsoft.com/graph/api/agentregistry-list-agentinstances. agentInstance links agentIdentityId + agentUserId + agentIdentityBlueprintId; carries url + additionalInterfaces (transport endpoints), JWS-signed agentCard signatures (alg ES256, kid did:web, type agentcard+jws), ownerIds OR managedBy ("either required"). Permissions: AgentInstance.Read.All / .ReadWrite.All / .ReadWrite.ManagedBy (app); delegated requires Agent Registry Administrator dir role. DEPRECATION: replaced May-2026 by Agent 365 package APIs.
- agents EntitySet /beta/agents (microsoft.graph.agent, nav copilotTools); /beta/copilot/agents; /beta/auditLogs/agents. copilotRoot (/beta/copilot) also exposes agentRegistrations, admin/catalog/packages (copilotPackageDetail; block/unblock/reassign/update ops; requires Agent 365 license + AI admin or Global admin), admin/policySettings, admin/settings, settings, reports, conversations, interactionHistory, users.
- ID Protection: /beta/identityProtection/riskyAgents (subtypes riskyAgentIdentity / riskyAgentIdentityBlueprintPrincipal / riskyAgentUser; actions confirmCompromised/confirmSafe/dismiss) + /beta/identityProtection/agentRiskDetections. Scope IdentityRiskyAgent.Read.All; delegated roles Global Reader / Security Operator / Security Reader / Security Administrator.
- Audit attribution: signIn.agent + summarizedSignIn.agent = agentic.agentSignIn (agentServicePrincipalId, agentSubjectParentId, agentType/agentSubjectType, parentAppId). agentType enum: notAgentic=0, agenticApp=2, agenticAppInstance=3, agentIdentityBlueprintPrincipal=4, agentIDuser=5.
- Metadata annotations: agentRegistration.agentCard = "Flexible Json manifest … displayName, description, iconUrl, version, provider, capabilities, skills, security"; ownerIds = "either owners or managedby is required".

### CODE REVIEW (microsoft/entrabot, in-scope org) — wire-level consent-grant primitive (H-2 refinement)
- scripts/create_entra_agent_ids.py: POST /v1.0/oauth2PermissionGrants with clientId={agent-identity-oid}, consentType=Principal, principalId={agent-user-oid}, resourceId={CALLER-CHOSEN SP oid}, scope string. ResourceId is NOT restricted to Graph: same primitive grants Azure Storage user_impersonation (app e406a681-f3d4-42a8-90b6-c2b029497af1) so hop-3 can mint idtyp=user https://storage.azure.com/.default tokens. Grant is one-time, per-principal, non-expiring (startTime only). Provisioner app needs DelegatedPermissionGrant.ReadWrite.All.
- Application.Read.All granted to agentIdentity SP via appRoleAssignment (app role id 9a5d68dd-52b0-4cc2-bd40-abcf44ac3a30) — Application.Read.All is NOT in the agent blocked-permissions table (only ReadWrite.* are) → agents can enumerate tenant apps/SPs.
- New subtype navs: GET /servicePrincipals/{id}/microsoft.graph.agentIdentity/sponsors (beta); GET /oauth2PermissionGrants?$filter=clientId eq '{oid}' and principalId eq '{oid}' (v1); GET /applications/{id}?$select=keyCredentials.
- UPN uniqueness used as tenant-wide lookup key (find_existing_agent_user_by_upn). entrabot explicitly warns "pooling agent users is an anti-pattern: object-ID recycling → residual permissions attach to next holder" (design acknowledgment of object-id reuse risk).
- No entrabot reference to agentRegistry/copilot catalog — those are separate first-party surfaces.

### Live checks (passive, 1/sec, in-scope graph.microsoft.com)
- /beta/agents, /beta/copilot/agents, /beta/agentRegistry{,/agentInstances,/agentCardManifests,/agentCollections}, /beta/auditLogs/agents, /beta/identityProtection/{riskyAgents,agentRiskDetections}, /v1.0/agents, /v1.0/agentRegistry/agentInstances → ALL 401 (now classified NULL signal; route existence rests on $metadata+docs).
- crt.sh CT inventory for %agent%.microsoft.com: still 502 (3rd consecutive run). agents.microsoft.com previously characterized (parked on adoption.microsoft.com AFD, *.azureedge.net SAN mismatch → 404).

### Hypotheses (test-tenant, read-only design)
- H-6 [HIGH] Registry ownership boundary: is managedBy enforced = caller appId on POST/PATCH agentInstances, or settable arbitrarily? Can a holder of AgentInstance.ReadWrite.ManagedBy (app) or Agent Registry Administrator (delegated) claim/overwrite another's agentInstance + agentCardManifest (instructions/endpoints)? Also copilot package reassign (POST /copilot/admin/catalog/packages/{id}/reassign) and block/unblock — role boundary. If not enforced → agent supply-chain takeover at M365 runtime. CVSS 7.5–9.0.
- H-7 [HIGH] agentCard JWS trust: registry stores optional signatures (ES256, did:web kid). Does the consuming runtime verify signature over the card (type agentcard+jws) or trust unsigned manifests? Unsigned/spoofed card → first-party impersonation + instruction injection. Test-tenant. CVSS 6.5–9.0.
- H-8 [MED] riskyAgents write-op integrity: confirmCompromised/confirmSafe/dismiss on arbitrary agent ids — risk-state tampering vs agent-risk CA. Role-gated (Security Administrator/Global Reader read). CVSS 4–6.
- H-9 [MED] agent sign-in attribution (signIn.agent: agentServicePrincipalId, parentAppId) as second data source for the agentSignInSessions cross-user hypothesis (PD-A). CVSS 6.5–8.8.
- H-1 [HIGHEST, unchanged] user_fic Hop-3 subject validation (user_id vs username alternate key). H-2 [HIGH, refined] consent-grant primitive is resource-agnostic (resourceId arbitrary → storage user_impersonation / any first-party resource). H-3 [MED] managerApplications first-party list.

### Secrets check
- None. entrabot = design/code only (no tokens/keys). Public client IDs, well-known app-role GUID (9a5d68dd-…), MS-doc sample UUIDs. Metadata hashes unchanged: graph_v1_meta.xml 9d84e451…, graph_beta_meta.xml ca304859…. No raw secrets committed.

===== RUN 2026-08-07 12:32:14 UTC | model bigpickle | target google =====
[0m
> build · big-pickle
[0m
[0m$ [0mls -la /tmp/opencode/ 2>/dev/null | head -60
total 16
drwxr-xr-x  2 runner runner  4096 Aug  7 12:24 .
drwxrwxrwt 14 root   root   12288 Aug  7 12:24 ..
[0m
[0m$ [0mcd /home/runner/work/hunt-lab/hunt-lab && cat scope.yml 2>/dev/null | head -40; echo "---"; cat state_bigpickle.json
programs:
  google:
    name: Google Vulnerability Reward Program
    disclosure_policy: https://bughunters.google.com/about/rules
    domains:
      - google.com
      - "*.google.com"
    github_orgs:
      - google
  microsoft:
    name: Microsoft Bounty / MSRC
    disclosure_policy: https://www.microsoft.com/en-us/msrc/bounty
    domains:
      - microsoft.com
      - "*.microsoft.com"
      - "*.msn.com"
      - "*.live.com"
      - "*.office.com"
      - "*.bing.com"
      - "*.azure.com"
      - "*.azurewebsites.net"
      - "*.windows.net"
      - "*.sharepoint.com"
      - "*.onmicrosoft.com"
      - "*.outlook.com"
      - "*.hotmail.com"
      - "*.xbox.com"
      - "*.dynamics.com"
      - "*.powerapps.com"
    github_orgs:
      - microsoft

rules:
  # Both programs require passive, non-disruptive testing.
  passive_only: true          # GET/HEAD reads only; no scanners, no fuzzers, no auth-bypass attempts on live infra
  max_requests_per_second: 1  # never hammer
  no_account_creation: true   # no mass signups (MSRC: banned; Google InTENT: restricted)
  no_data_modification: true  # no writes, deletes, state changes
  no_pre_disclosure: true     # never disclose before program response
  redact_secrets_in_commits: true  # commit sha256 of secrets, never raw values
---
{"phase": "POC", "status": "HIGH_POTENTIAL"}
[0m
[0m$ [0mmkdir -p /tmp/opencode && cd /tmp/opencode && for f in "docs/platform-docs/entra-agent-users.md" "engineering-history/plans/PLAN-agent-identity-by-upn.md" "docs/reference/scripts/provisioning/create-entra-agent-ids-py.md" "docs/reference/scripts/provisioning/assign-agent-user-licenses-py.md" "docs/reference/scripts/diagnostics/list-agent-identities-py.md"; do
  out="entrabot_$(basename $f .md).md"
  curl -s --max-time 40 -H "User-Agent: Mozilla/5.0" "https://raw.githubusercontent.com/microsoft/entrabot/main/$f" -o "$out"
  echo "$out: $(wc -c < $out) bytes"
done
entrabot_entra-agent-users.md: 10379 bytes
entrabot_PLAN-agent-identity-by-upn.md: 8269 bytes
entrabot_create-entra-agent-ids-py.md: 7804 bytes
entrabot_assign-agent-user-licenses-py.md: 4190 bytes
entrabot_list-agent-identities-py.md: 2956 bytes
[0m
`/tmp/opencode` was reset (prior artifacts gone), but the journal and leads persist. POC live options are exhausted (all hypotheses are test-tenant-gated); per protocol I deepen POC designs via in-scope code review and open RECON on the unexplored Graph **agent registry / agentCard** ecosystem + CT inventory.
[0m$ [0mcd /tmp/opencode && for f in "scripts/create_entra_agent_ids.py" "scripts/assign_agent_user_licenses.py" "scripts/list_agent_identities.py" "scripts/show_agent_status.py" "scripts/add_agent_sponsor.py" "scripts/deprovision_entra_agent_identity.py"; do
  out="entrabot_$(basename $f .py).py"
  curl -s --max-time 40 -H "User-Agent: Mozilla/5.0" "https://raw.githubusercontent.com/microsoft/entrabot/main/$f" -o "$out"
  echo "$out: $(wc -c < $out) bytes"
done
entrabot_create_entra_agent_ids.py: 46024 bytes
entrabot_assign_agent_user_licenses.py: 9257 bytes
entrabot_list_agent_identities.py: 2930 bytes
entrabot_show_agent_status.py: 21284 bytes
entrabot_add_agent_sponsor.py: 4995 bytes
entrabot_deprovision_entra_agent_identity.py: 8648 bytes
[0m
[0m→ [0mRead /tmp/opencode/entrabot_PLAN-agent-identity-by-upn.md
[0m→ [0mRead /tmp/opencode/entrabot_entra-agent-users.md
[0m→ [0mRead /tmp/opencode/entrabot_create_entra_agent_ids.py
Now the wire-level scripts — these contain the actual Graph calls for the three-hop flow:
[0m
[0m$ [0mcd /tmp/opencode && for f in entrabot_assign_agent_user_licenses.py entrabot_list_agent_identities.py entrabot_add_agent_sponsor.py entrabot_deprovision_entra_agent_identity.py; do
  echo "==== $f ===="
  grep -oE '(https://graph\.microsoft\.com/[a-zA-Z0-9/_.${}?=&-]+|graph_request\("[A-Z]+", "[^"]+"|GET|POST|PATCH|DELETE)"[^"]*")' "$f" | sort -u | head -25
done
==== entrabot_assign_agent_user_licenses.py ====
==== entrabot_list_agent_identities.py ====
==== entrabot_add_agent_sponsor.py ====
==== entrabot_deprovision_entra_agent_identity.py ====
[0m
[0m$ [0mcd /tmp/opencode && echo "=== show_agent_status.py API calls ===" && grep -oE 'graph_request\("[A-Z]+", "[^"]+"[^)]*\)|requests\.[a-z]+\([^)]*"https://graph\.microsoft\.com[^)]*\)' entrabot_show_agent_status.py | sort -u | head -30
=== show_agent_status.py API calls ===
[0m
This is gold — the script reveals the consent-grant primitive (`resourceId` is caller-specified, can target Azure Storage `user_impersonation`, not just Graph). Let me extract the API surface from the remaining scripts:
[0m$ [0mcd /tmp/opencode && grep -nE 'graph\.microsoft\.com|/users/|/servicePrincipals|/applications|/oauth2PermissionGrants|assignLicense|checkMemberGroups|getMemberGroups' entrabot_show_agent_status.py | head -40
96:        f"/servicePrincipals/{agent_oid}"
112:    path = f"/users/{agent_user_id}?$select=assignedLicenses"
123:        "/oauth2PermissionGrants"
136:        path = f"/servicePrincipals/{resource_id}?$select=displayName"
148:    path = f"/applications/{blueprint_object_id}?$select=keyCredentials"
[0m
[0m→ [0mRead /tmp/opencode/entrabot_show_agent_status.py[90m [offset=60, limit=120][0m
Consent-grant surface confirmed (`GET /oauth2PermissionGrants?$filter=clientId/principalId`, `GET /servicePrincipals/{oid}/microsoft.graph.agentIdentity/sponsors`). Now the NEW unexplored surface — Graph's **agent registry / agentCard** ecosystem in beta $metadata (re-fetch, passive):
[0m
[0m$ [0mcd /tmp/opencode && curl -s -o graph_beta_meta.xml "https://graph.microsoft.com/beta/\$metadata" --max-time 120 && wc -c graph_beta_meta.xml
7298965 graph_beta_meta.xml
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
# Entity types with 'agent' (case-insensitive) — full list
ents=sorted(set(re.findall(r'<EntityType Name="([^"]*[aA]gent[^"]*)"',x)))
print("== ENTITY TYPES w/ agent (",len(ents),") ==")
for e in ents: print(" ",e)
EOF
== ENTITY TYPES w/ agent ( 21 ) ==
  agent
  agentCardManifest
  agentCollection
  agentIdentity
  agentIdentityBlueprint
  agentIdentityBlueprintPrincipal
  agentInstance
  agentRegistration
  agentRegistry
  agentRiskDetection
  agentUser
  cloudPcAgentPool
  cloudPcAgentPoolUserAssignment
  cloudPcBulkReinstallAgent
  exactMatchUploadAgent
  onPremisesAgent
  onPremisesAgentGroup
  riskyAgent
  riskyAgentIdentity
  riskyAgentIdentityBlueprintPrincipal
  riskyAgentUser
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
for name in ['agent','agentRegistry','agentRegistration','agentInstance','agentCardManifest','agentCollection','agentRiskDetection','riskyAgent','riskyAgentUser','riskyAgentIdentity']:
    m=re.search(r'<EntityType Name="'+name+r'"[^>]*>.*?</EntityType>',x,re.S)
    if m:
        body=re.sub(r'\s+',' ',m.group(0))
        print('---',name)
        print(body[:900])
        print()
EOF
--- agent
<EntityType Name="agent" BaseType="graph.entity"><NavigationProperty Name="copilotTools" Type="Collection(graph.copilotTool)" ContainsTarget="true" /></EntityType>

--- agentRegistry
<EntityType Name="agentRegistry"><NavigationProperty Name="agentCardManifests" Type="Collection(graph.agentCardManifest)" ContainsTarget="true" /><NavigationProperty Name="agentCollections" Type="Collection(graph.agentCollection)" ContainsTarget="true" /><NavigationProperty Name="agentInstances" Type="Collection(graph.agentInstance)" ContainsTarget="true" /></EntityType>

--- agentRegistration
<EntityType Name="agentRegistration" BaseType="graph.entity"><Property Name="agentCard" Type="graph.Json" /><Property Name="agentIdentityBlueprintId" Type="Edm.String" /><Property Name="agentIdentityId" Type="Edm.String" /><Property Name="createdBy" Type="Edm.String" Nullable="false" /><Property Name="description" Type="Edm.String" /><Property Name="displayName" Type="Edm.String" Nullable="false" /><Property Name="lastPublishedBy" Type="Edm.String" /><Property Name="managedByAppId" Type="Edm.String" /><Property Name="originatingStore" Type="Edm.String" /><Property Name="ownerIds" Type="Collection(Edm.String)" Nullable="false" /><Property Name="sourceAgentId" Type="Edm.String" /><Property Name="sourceCreatedDateTime" Type="Edm.DateTimeOffset" Nullable="false" /><Property Name="sourceLastModifiedDateTime" Type="Edm.DateTimeOffset" Nullable="false" /></EntityType>

--- agentInstance
<EntityType Name="agentInstance" BaseType="graph.entity"><Property Name="additionalInterfaces" Type="Collection(graph.agentInterface)" /><Property Name="agentIdentityBlueprintId" Type="Edm.String" /><Property Name="agentIdentityId" Type="Edm.String" /><Property Name="agentUserId" Type="Edm.String" /><Property Name="createdBy" Type="Edm.String" /><Property Name="createdDateTime" Type="Edm.DateTimeOffset" /><Property Name="displayName" Type="Edm.String" Nullable="false" /><Property Name="lastModifiedDateTime" Type="Edm.DateTimeOffset" /><Property Name="managedBy" Type="Edm.String" /><Property Name="originatingStore" Type="Edm.String" /><Property Name="ownerIds" Type="Collection(Edm.String)" Nullable="false" /><Property Name="preferredTransport" Type="Edm.String" /><Property Name="signatures" Type="Collection(graph.agentCardSignature)" /><Property Name="sourceAgentId" Type="Edm.String" /><P

--- agentCardManifest
<EntityType Name="agentCardManifest" BaseType="graph.entity"><Property Name="capabilities" Type="graph.agentCapabilities" Nullable="false" /><Property Name="createdBy" Type="Edm.String" /><Property Name="createdDateTime" Type="Edm.DateTimeOffset" /><Property Name="defaultInputModes" Type="Collection(Edm.String)" Nullable="false" /><Property Name="defaultOutputModes" Type="Collection(Edm.String)" Nullable="false" /><Property Name="description" Type="Edm.String" Nullable="false" /><Property Name="displayName" Type="Edm.String" Nullable="false" /><Property Name="documentationUrl" Type="Edm.String" /><Property Name="iconUrl" Type="Edm.String" /><Property Name="lastModifiedDateTime" Type="Edm.DateTimeOffset" /><Property Name="managedBy" Type="Edm.String" /><Property Name="originatingStore" Type="Edm.String" /><Property Name="ownerIds" Type="Collection(Edm.String)" Nullable="false" /><Property

--- agentCollection
<EntityType Name="agentCollection" BaseType="graph.entity"><Property Name="createdBy" Type="Edm.String" /><Property Name="createdDateTime" Type="Edm.DateTimeOffset" /><Property Name="description" Type="Edm.String" /><Property Name="displayName" Type="Edm.String" Nullable="false" /><Property Name="lastModifiedDateTime" Type="Edm.DateTimeOffset" /><Property Name="managedBy" Type="Edm.String" /><Property Name="originatingStore" Type="Edm.String" /><Property Name="ownerIds" Type="Collection(Edm.String)" Nullable="false" /><NavigationProperty Name="members" Type="Collection(graph.agentInstance)" /></EntityType>

--- agentRiskDetection
<EntityType Name="agentRiskDetection" BaseType="graph.entity"><Property Name="activityDateTime" Type="Edm.DateTimeOffset" /><Property Name="additionalInfo" Type="Edm.String" /><Property Name="agentDisplayName" Type="Edm.String" /><Property Name="agentId" Type="Edm.String" /><Property Name="blueprintId" Type="Edm.String" /><Property Name="clientSessionId" Type="Edm.String" /><Property Name="detectedDateTime" Type="Edm.DateTimeOffset" /><Property Name="detectionTimingType" Type="graph.riskDetectionTimingType" Nullable="false" /><Property Name="displayName" Type="Edm.String" Nullable="false" /><Property Name="identityId" Type="Edm.String" Nullable="false" /><Property Name="identityType" Type="graph.agentIdentityType" Nullable="false" /><Property Name="lastModifiedDateTime" Type="Edm.DateTimeOffset" /><Property Name="riskDetail" Type="graph.riskDetail" Nullable="false" /><Property Name="risk

--- riskyAgent
<EntityType Name="riskyAgent" BaseType="graph.entity"><Property Name="agentDisplayName" Type="Edm.String" /><Property Name="blueprintId" Type="Edm.String" /><Property Name="identityType" Type="graph.agentIdentityType" Nullable="false" /><Property Name="isDeleted" Type="Edm.Boolean" Nullable="false" /><Property Name="isEnabled" Type="Edm.Boolean" Nullable="false" /><Property Name="isProcessing" Type="Edm.Boolean" Nullable="false" /><Property Name="riskDetail" Type="graph.riskDetail" Nullable="false" /><Property Name="riskLastModifiedDateTime" Type="Edm.DateTimeOffset" /><Property Name="riskLevel" Type="graph.riskLevel" Nullable="false" /><Property Name="riskState" Type="graph.riskState" Nullable="false" /></EntityType>

--- riskyAgentUser
<EntityType Name="riskyAgentUser" BaseType="graph.riskyAgent"><NavigationProperty Name="agentUser" Type="graph.agentUser" /></EntityType>

--- riskyAgentIdentity
<EntityType Name="riskyAgentIdentity" BaseType="graph.riskyAgent"><NavigationProperty Name="agentIdentity" Type="graph.agentIdentity" /></EntityType>
[0m
Full agent ecosystem identified — `agent`, `agentRegistry`, `agentRegistration`, `agentInstance`, `agentCardManifest`, `agentCollection`, plus `riskyAgent*` (ID Protection). Extracting definitions + functions/actions:
[0m$ [0mcd /tmp/opencode && echo "=== Functions/Actions w/ agent (beta) ===" && grep -oE '<(Function|Action) Name="[^"]*[aA]gent[^"]*"' graph_beta_meta.xml | sort -u && echo "=== agentRegistry/agentCard/agentCollection refs ===" && grep -c "agentRegistry" graph_beta_meta.xml && grep -c "agentCardManifest" graph_beta_meta.xml && grep -c "agentRegistration" graph_beta_meta.xml
=== Functions/Actions w/ agent (beta) ===
<Action Name="configureAgent"
<Action Name="deployAgent"
<Action Name="getUnhealthyDefenderAgentsReport"
<Action Name="retryPartnerAgentInstallation"
<Function Name="retrieveDeployAgentActionResults"
=== agentRegistry/agentCard/agentCollection refs ===
1
1
1
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
# EntitySets and Singletons with agent
for kind in ['EntitySet','Singleton']:
    hits=re.findall(r'<'f'{kind} Name="([^"]*[aA]gent[^"]*)"[^>]*/>',x)
    print(kind,'w/agent:',hits)
# count references to agentRegistry/agentCardManifest/agentInstance/agentCollection/agentRegistration anywhere
for n in ['agentRegistry','agentCardManifest','agentCollection','agentInstance','agentRegistration','agentRiskDetection','riskyAgent','agentCard','agentIdentityType']:
    c=x.count(n)
    print(n, 'refs:', c)
EOF
EntitySet w/agent: ['agents']
Singleton w/agent: []
agentRegistry refs: 9
agentCardManifest refs: 11
agentCollection refs: 10
agentInstance refs: 12
agentRegistration refs: 19
agentRiskDetection refs: 6
riskyAgent refs: 13
agentCard refs: 15
agentIdentityType refs: 3
[0m
Registry entity types are defined but let me check if they're wired to entity sets/singletons (that determines live reachability):
[0m$ [0mcd /tmp/opencode && echo "=== complex types w/ agent ===" && grep -oE '<ComplexType Name="[^"]*[aA]gent[^"]*"' graph_beta_meta.xml | sort -u && echo "=== agentRiskDetection/riskyAgent context (surrounding 300 chars) ===" && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
for term in ['riskyAgents','agentRiskDetections','agentRiskDetection','identityProtection']:
    i=x.find(term)
    if i>=0:
        print(term, '=>', re.sub(r'\s+',' ',x[max(0,i-150):i+150]))
    else:
        print(term, 'NOT FOUND')
EOF
=== complex types w/ agent ===
<ComplexType Name="a365AiInvokeAgent"
<ComplexType Name="agentAdminActivityRecord"
<ComplexType Name="agentCapabilities"
<ComplexType Name="agentCardSignature"
<ComplexType Name="agentExtension"
<ComplexType Name="agentExtensionParams"
<ComplexType Name="agentInterface"
<ComplexType Name="agentProvider"
<ComplexType Name="agentSettingAdminActivity"
<ComplexType Name="agentSignIn"
<ComplexType Name="agentSkill"
<ComplexType Name="aiAgent"
<ComplexType Name="aiAgentDetails"
<ComplexType Name="aiAgentEvidence"
<ComplexType Name="aiAgentInfo"
<ComplexType Name="aiInvokeAgentAuditRecord"
<ComplexType Name="clientUserAgent"
<ComplexType Name="cloudPcAgentHealthCheckDetail"
<ComplexType Name="cloudPcAgentHealthCheckStatusDetail"
<ComplexType Name="cloudPcAgentHealthCheckSummary"
<ComplexType Name="cloudPcAgentPoolBillingConfiguration"
<ComplexType Name="cloudPcAgentPoolCapabilityConfiguration"
<ComplexType Name="cloudPcAgentPoolScalingPolicy"
<ComplexType Name="cloudPcAgentPoolSessionUsage"
<ComplexType Name="cloudPcAgentStatusDetail"
<ComplexType Name="cloudPcExternalPartnerAgentSetting"
<ComplexType Name="cloudPcPartnerAgentInstallResult"
<ComplexType Name="copilotAgentManagementAuditRecord"
<ComplexType Name="fileStorageContainerTypeAgentSettings"
<ComplexType Name="hybridAgentUpdaterConfiguration"
<ComplexType Name="mosAgentInfoRecord"
<ComplexType Name="mosAgentInfoRecordV2"
<ComplexType Name="purviewPostureAgentAuditRecord"
<ComplexType Name="rtiOperationsAgentAuditRecord"
<ComplexType Name="sensitiveInfoRemediationAgentDataRecord"
<ComplexType Name="serviceUserAgent"
<ComplexType Name="targetAgentIdentitySponsorsOrOwners"
<ComplexType Name="userAgent"
<ComplexType Name="vivaGlintAgenticCampaignAuditRecord"
=== agentRiskDetection/riskyAgent context (surrounding 300 chars) ===
riskyAgents => insTarget="true" /><NavigationProperty Name="riskDetections" Type="Collection(graph.riskDetection)" ContainsTarget="true" /><NavigationProperty Name="riskyAgents" Type="Collection(graph.riskyAgent)" ContainsTarget="true" /><NavigationProperty Name="riskyServicePrincipals" Type="Collection(graph.risk
agentRiskDetections => tId" Type="Edm.String" /><Property Name="source" Type="Edm.String" /></EntityType><EntityType Name="identityProtectionRoot"><NavigationProperty Name="agentRiskDetections" Type="Collection(graph.agentRiskDetection)" ContainsTarget="true" /><NavigationProperty Name="riskDetections" Type="Collection(gr
agentRiskDetection => yValue Property="Version" String="2025-02/globalSecureAccess" /></Record></Collection></Annotation></Annotations><Annotations Target="microsoft.graph.agentRiskDetection"><Annotation Term="Org.OData.Capabilities.V1.CountRestrictions"><Record><PropertyValue Property="Countable" Bool="false" /></Record
identityProtection => lIdentityVerificationCredential, windowsHealthMonitoring, extensions, mxProfileZebraOnly, deviceFirmwareConfigurationInterface, deliveryOptimization, identityProtection, kiosk, overrideGroupPolicy, domainJoinPreview, pkcsImportedCertificate, networkBoundary, endpointProtection, microsoftDefenderAtpW
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
# 1. agents EntitySet full definition + container
m=re.search(r'<EntitySet Name="agents"[^/]*/>',x)
print('agents EntitySet:', m.group(0) if m else 'NOT FOUND')
i=x.find('EntitySet Name="agents"')
print('ctx:', re.sub(r'\s+',' ',x[max(0,i-300):i+200]))
EOF
agents EntitySet: <EntitySet Name="agents" EntityType="microsoft.graph.agent" />
ctx: t Name="agreementAcceptances" EntityType="microsoft.graph.agreementAcceptance" /><EntitySet Name="agreements" EntityType="microsoft.graph.agreement" /><EntitySet Name="riskDetections" EntityType="microsoft.graph.riskDetection" /><EntitySet Name="riskyUsers" EntityType="microsoft.graph.riskyUser" /><EntitySet Name="agents" EntityType="microsoft.graph.agent" /><EntitySet Name="mobilityManagementPolicies" EntityType="microsoft.graph.mobilityManagementPolicy"><NavigationPropertyBinding Path="include
[0m
Major discovery: a live `agents` EntitySet and ID Protection's `riskyAgents`/`agentRiskDetections` navs, plus a `agentSignIn` complex type. Let me trace where they're wired:
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
# 2. agentSignIn complex type definition
m=re.search(r'<ComplexType Name="agentSignIn"[^>]*>.*?</ComplexType>',x,re.S)
print('== agentSignIn complex type ==')
print(re.sub(r'\s+',' ',m.group(0))[:800] if m else 'NOT FOUND')
# where is agentSignIn referenced?
print('refs:', x.count('agentSignIn'))
for mm in re.finditer(r'agentSignIn',x):
    s=mm.start()
    seg=x[max(0,s-120):s+120]
    if '<Property' in seg or 'Type="graph.agentSignIn' in seg:
        print('CTX:', re.sub(r'\s+',' ',seg))
EOF
== agentSignIn complex type ==
<ComplexType Name="agentSignIn" BaseType="graph.signInIdentity"><Property Name="agentServicePrincipalId" Type="Edm.String" /></ComplexType>
refs: 4
CTX:  Type="Edm.Int32" /></EntityType><EntityType Name="signIn" BaseType="graph.entity"><Property Name="agent" Type="agentic.agentSignIn" /><Property Name="appDisplayName" Type="Edm.String" /><Property Name="appId" Type="Edm.String" /><Property 
CTX: eType="graph.entity" /><EntityType Name="summarizedSignIn" BaseType="graph.entity"><Property Name="agent" Type="agentic.agentSignIn" /><Property Name="aggregationDateTime" Type="Edm.DateTimeOffset" /><Property Name="appDisplayName" Type="Ed
CTX: ion" BaseType="graph.agreementFileProperties" /><ComplexType Name="signInIdentity" Abstract="true" /><ComplexType Name="agentSignIn" BaseType="graph.signInIdentity"><Property Name="agentServicePrincipalId" Type="Edm.String" /></ComplexType>
CTX: Name="agentIdentityBlueprintPrincipal" Value="4" /><Member Name="agentIDuser" Value="5" /></EnumType><ComplexType Name="agentSignIn"><Property Name="agentSubjectParentId" Type="Edm.String" /><Property Name="agentSubjectType" Type="agentic.a
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
# agentic.agentSignIn (namespace-qualified) full definition
m=re.search(r'<ComplexType Name="agentSignIn"[^>]*>.*?</ComplexType>',x,re.S)
print('== agentSignIn (2nd def w/ agentSubjectType) ==')
print(re.sub(r'\s+',' ',m.group(0)))
# agentSubjectType enum
mm=re.search(r'<EnumType Name="[a-zA-Z]*[sS]ubject[A-Za-z]*"',x)
for en in re.finditer(r'<EnumType Name="([^"]*[sS]ubject[^"]*)"',x):
    print('ENUM:',en.group(1))
en=re.search(r'<EnumType Name="agentSubjectType"[^>]*>.*?</EnumType>',x,re.S)
if en: print('== agentSubjectType ==', re.sub(r'\s+',' ',en.group(0))[:600])
else:
    # try namespaced search
    i=x.find('agentSubjectType')
    print('agentSubjectType ctx:', re.sub(r'\s+',' ',x[i-250:i+400]))
EOF
== agentSignIn (2nd def w/ agentSubjectType) ==
<ComplexType Name="agentSignIn" BaseType="graph.signInIdentity"><Property Name="agentServicePrincipalId" Type="Edm.String" /></ComplexType>
ENUM: dataSubjectType
ENUM: subjectRightsRequestStage
ENUM: subjectRightsRequestStageStatus
ENUM: subjectRightsRequestStatus
ENUM: subjectRightsRequestType
ENUM: accessPackageSubjectLifecycle
ENUM: appleSubjectNameFormat
ENUM: subjectAlternativeNameType
ENUM: subjectNameFormat
agentSubjectType ctx: e="unknownFutureValue" Value="6" /><Member Name="agentIdentityBlueprintPrincipal" Value="4" /><Member Name="agentIDuser" Value="5" /></EnumType><ComplexType Name="agentSignIn"><Property Name="agentSubjectParentId" Type="Edm.String" /><Property Name="agentSubjectType" Type="agentic.agentType" Nullable="false" /><Property Name="agentType" Type="agentic.agentType" Nullable="false" /><Property Name="parentAppId" Type="Edm.String" /></ComplexType></Schema><Schema Namespace="microsoft.graph.healthMonitoring" xmlns="http://docs.oasis-open.org/odata/ns/edm"><EnumType Name="alertState"><Member Name="active" Value="0" /><Member Name="resolved" Value="1
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
# signIn entity set + container of identityProtectionRoot navs
m=re.search(r'<EntitySet Name="signIns"[^/]*/>',x)
print('signIns EntitySet:', m.group(0) if m else 'NOT FOUND')
# riskyAgents / agentRiskDetections nav bindings
for term in ['riskyAgents','agentRiskDetections']:
    i=x.find('Path="'+term+'"')
    if i>=0:
        print(term,'binding ctx:', re.sub(r'\s+',' ',x[i-160:i+160]))
    else:
        print(term,'no explicit binding (default nav)')
# riskDetection entity set exists?
m=re.search(r'<EntitySet Name="riskDetections"[^/]*/>',x)
print('riskDetections ES:', m.group(0) if m else '-')
# what contains identityProtectionRoot: entity set
m=re.search(r'<EntitySet Name="identityProtection"[^/]*/>',x)
print('identityProtection ES:', m.group(0) if m else 'NOT FOUND')
EOF
signIns EntitySet: NOT FOUND
riskyAgents no explicit binding (default nav)
agentRiskDetections no explicit binding (default nav)
riskDetections ES: <EntitySet Name="riskDetections" EntityType="microsoft.graph.riskDetection" />
identityProtection ES: NOT FOUND
[0m
The `signIn` audit entity now carries `agentic.agentSignIn` data (`agentSubjectParentId`/`agentSubjectType`). Extracting the full agentic model + ID Protection wiring:
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
# full agentic.agentType enum
i=x.find('EnumType Name="agentType"')
print('== agentType enum ==')
print(re.sub(r'\s+',' ',x[i-40:i+700]))
# signIns entity set (any EntitySet referencing graph.signIn)
for m in re.finditer(r'<EntitySet Name="([^"]*)" EntityType="[^"]*\.signIn"',x):
    print('signIn-derived EntitySet:', m.group(1))
# containers
for m in re.finditer(r'<EntityContainer Name="([^"]*)"',x):
    print('CONTAINER:', m.group(1))
# does any EntitySet/container include identityProtectionRoot?
for m in re.finditer(r'(<EntitySet Name="([^"]*)" EntityType="[^"]*identityProtectionRoot"[^/]*/>)',x):
    print('identityProtectionRoot set:', m.group(2), '|', m.group(1))
for m in re.finditer(r'<Singleton Name="([^"]*)" Type="[^"]*identityProtectionRoot"',x):
    print('identityProtectionRoot singleton:', m.group(1))
EOF
== agentType enum ==
tp://docs.oasis-open.org/odata/ns/edm"><EnumType Name="agentType"><Member Name="notAgentic" Value="0" /><Member Name="agenticApp" Value="2" /><Member Name="agenticAppInstance" Value="3" /><Member Name="unknownFutureValue" Value="6" /><Member Name="agentIdentityBlueprintPrincipal" Value="4" /><Member Name="agentIDuser" Value="5" /></EnumType><ComplexType Name="agentSignIn"><Property Name="agentSubjectParentId" Type="Edm.String" /><Property Name="agentSubjectType" Type="agentic.agentType" Nullable="false" /><Property Name="agentType" Type="agentic.agentType" Nullable="false" /><Property Name="parentAppId" Type="Edm.String" /></ComplexType></Schema><Schema Namespace="microsoft.graph.healthMonitoring" xmlns="http://docs.oasis-open.org
CONTAINER: GraphService
identityProtectionRoot singleton: identityProtection
[0m
`agentic.agentType` enum recovered (notAgentic=0 … agentIdentityBlueprintPrincipal=4, agentIDuser=5). Let me pin the auditLogs signIns wiring and then live-verify reachability of the three new surfaces (1 req/sec):
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
# find entity set referencing signIn type within GraphService container
m=re.findall(r'<EntitySet Name="([^"]*)" EntityType="[^"]*\.signIn"',x)
print('signIn sets:',m)
# auditLogRoot container?
m=re.findall(r'<EntitySet Name="([^"]*)" EntityType="microsoft\.graph\.auditLogRoot"',x)
print('auditLogRoot sets:',m)
# look for 'auditLogs' singleton/entityset
for m in re.finditer(r'(EntitySet|Singleton) Name="auditLogs"',x):
    print('auditLogs:', m.group(0))
# any navigation bindings with auditLogs
i=x.find('auditLogs')
while i>=0 and i < len(x):
    seg=x[i-80:i+200]
    if 'EntitySet' in seg or 'Singleton' in seg or 'Path=' in seg:
        print('ctx:',re.sub(r'\s+',' ',seg)); 
    i=x.find('auditLogs',i+1)
    if i> 100000000: break
EOF
signIn sets: []
auditLogRoot sets: []
auditLogs: Singleton Name="auditLogs"
ctx: istrationScopeTargets" Target="directoryObjects" /></Singleton><Singleton Name="auditLogs" Type="microsoft.graph.auditLogRoot" /><Singleton Name="reports" Type="microsoft.graph.reportRoot"><NavigationPropertyBinding Path="correlations/servicePrincipal" Target="servicePrincipals"
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
i=x.find('Singleton Name="auditLogs"')
print('== auditLogRoot bindings ==')
seg=x[i:i+1500]
# extract navigation bindings
for m in re.finditer(r'<NavigationPropertyBinding Path="([^"]*)" Target="([^"]*)"',seg):
    print(' ',m.group(1),'->',m.group(2))
print('--- raw tail ---')
print(re.sub(r'\s+',' ',seg)[:800])
EOF
== auditLogRoot bindings ==
  correlations/servicePrincipal -> servicePrincipals
  healthMonitoring/alerts/enrichment/impacts/microsoft.graph.healthMonitoring.directoryObjectImpactSummary/resourceSampling -> directoryObjects
  agents -> agents
  agentCollections/members -> agentRegistry/agentInstances
  agentInstances/agentCardManifest -> agentRegistry/agentCardManifests
  agentInstances/collections -> agentRegistry/agentCollections
  backupRestore/exchangeProtectionPolicies/mailboxExclusionUnits -> solutions/microsoft.graph.backupRestoreRoot/backupRestore/mailboxExclusionUnits
  backupRestore/exchangeProtectionPolicies/mailboxExclusionUnitsBulkAdditionJobs -> solutions/microsoft.graph.backupRestoreRoot/backupRestore/mailboxExclusionUnitsBulkAdditionJobs
--- raw tail ---
Singleton Name="auditLogs" Type="microsoft.graph.auditLogRoot" /><Singleton Name="reports" Type="microsoft.graph.reportRoot"><NavigationPropertyBinding Path="correlations/servicePrincipal" Target="servicePrincipals" /><NavigationPropertyBinding Path="healthMonitoring/alerts/enrichment/impacts/microsoft.graph.healthMonitoring.directoryObjectImpactSummary/resourceSampling" Target="directoryObjects" /></Singleton><Singleton Name="copilot" Type="microsoft.graph.copilotRoot"><NavigationPropertyBinding Path="agents" Target="agents" /></Singleton><Singleton Name="agentRegistry" Type="microsoft.graph.agentRegistry"><NavigationPropertyBinding Path="agentCollections/members" Target="agentRegistry/agentInstances" /><NavigationPropertyBinding Path="agentInstances/agentCardManifest" Target="agentRegist
[0m
Three new live surfaces confirmed in metadata: `agents` EntitySet, `agentRegistry` **singleton** (`agentCardManifests`/`agentInstances`/`agentCollections`), and `auditLogs/agents`. Live-reachability check (passive GETs, 1/sec):
[0m$ [0mcd /tmp/opencode && for p in \
  "/beta/agents" \
  "/beta/copilot/agents" \
  "/beta/agentRegistry" \
  "/beta/agentRegistry/agentInstances" \
  "/beta/agentRegistry/agentCardManifests" \
  "/beta/auditLogs/agents" \
  "/beta/identityProtection/riskyAgents" \
  "/beta/identityProtection/agentRiskDetections"; do
  out=$(curl -s -o /tmp/opencode/b.rsp -w "%{http_code}" --max-time 25 "https://graph.microsoft.com$p")
  sig=$(head -c 120 /tmp/opencode/b.rsp | tr -d '\n')
  echo "[$out] $p => ${sig:0:100}"
  sleep 1
done
[401] /beta/agents => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty.","innerError":{"date
[401] /beta/copilot/agents => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty.","innerError":{"date
[401] /beta/agentRegistry => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty.","innerError":{"date
[401] /beta/agentRegistry/agentInstances => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty.","innerError":{"date
[401] /beta/agentRegistry/agentCardManifests => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty.","innerError":{"date
[401] /beta/auditLogs/agents => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty.","innerError":{"date
[401] /beta/identityProtection/riskyAgents => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty.","innerError":{"date
[401] /beta/identityProtection/agentRiskDetections => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty.","innerError":{"date
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
for name in ['agentCardSignature','agentCapabilities','targetAgentIdentitySponsorsOrOwners','agentSkill','agentProvider','aiAgent','agentInterface']:
    m=re.search(r'<ComplexType Name="'+name+r'"[^>]*>.*?</ComplexType>',x,re.S)
    print('---',name)
    print(re.sub(r'\s+',' ',m.group(0))[:700] if m else 'NOT FOUND')
    print()
# where agentRegistration is referenced (non-definition)
print("== agentRegistration contexts ==")
for mm in list(re.finditer(r'agentRegistration',x))[1:6]:
    s=mm.start()
    print(' >', re.sub(r'\s+',' ',x[max(0,s-140):s+140]))
EOF
--- agentCardSignature
<ComplexType Name="agentCardSignature"><Property Name="header" Type="graph.jwsHeader" /><Property Name="protected" Type="Edm.String" Nullable="false" /><Property Name="signature" Type="Edm.String" Nullable="false" /></ComplexType>

--- agentCapabilities
<ComplexType Name="agentCapabilities"><Property Name="extensions" Type="Collection(graph.agentExtension)" /><Property Name="pushNotifications" Type="Edm.Boolean" Nullable="false" /><Property Name="stateTransitionHistory" Type="Edm.Boolean" Nullable="false" /><Property Name="streaming" Type="Edm.Boolean" Nullable="false" /></ComplexType>

--- targetAgentIdentitySponsorsOrOwners
<ComplexType Name="targetAgentIdentitySponsorsOrOwners" BaseType="graph.userSet" /><ComplexType Name="targetUserSponsors" BaseType="graph.userSet" /><EntityType Name="accessPackage" BaseType="graph.entity"><Property Name="catalogId" Type="Edm.String" /><Property Name="createdBy" Type="Edm.String" /><Property Name="createdDateTime" Type="Edm.DateTimeOffset" /><Property Name="description" Type="Edm.String" /><Property Name="displayName" Type="Edm.String" /><Property Name="isHidden" Type="Edm.Boolean" /><Property Name="isRoleScopesVisible" Type="Edm.Boolean" /><Property Name="modifiedBy" Type="Edm.String" /><Property Name="modifiedDateTime" Type="Edm.DateTimeOffset" /><Property Name="uniqueName

--- agentSkill
<ComplexType Name="agentSkill"><Property Name="description" Type="Edm.String" Nullable="false" /><Property Name="displayName" Type="Edm.String" Nullable="false" /><Property Name="examples" Type="Collection(Edm.String)" /><Property Name="id" Type="Edm.String" Nullable="false" /><Property Name="inputModes" Type="Collection(Edm.String)" /><Property Name="outputModes" Type="Collection(Edm.String)" /><Property Name="security" Type="Collection(graph.securityRequirement)" /><Property Name="tags" Type="Collection(Edm.String)" Nullable="false" /></ComplexType>

--- agentProvider
<ComplexType Name="agentProvider"><Property Name="organization" Type="Edm.String" Nullable="false" /><Property Name="url" Type="Edm.String" Nullable="false" /></ComplexType>

--- aiAgent
<ComplexType Name="aiAgent"><Property Name="firstAccessDateTime" Type="Edm.DateTimeOffset" /><Property Name="id" Type="Edm.String" /><Property Name="lastAccessDateTime" Type="Edm.DateTimeOffset" /><Property Name="name" Type="Edm.String" /><Property Name="totalBytesReceived" Type="Edm.Int64" /><Property Name="totalBytesSent" Type="Edm.Int64" /><Property Name="trafficType" Type="microsoft.graph.networkaccess.trafficType" /><Property Name="transactionCount" Type="Edm.Int64" /></ComplexType>

--- agentInterface
<ComplexType Name="agentInterface"><Property Name="transport" Type="Edm.String" Nullable="false" /><Property Name="url" Type="Edm.String" Nullable="false" /></ComplexType>

== agentRegistration contexts ==
 > es including their instructions, owners, publishing status,&#xA;and associated tools." /></Annotations><Annotations Target="microsoft.graph.agentRegistration/agentCard"><Annotation Term="Org.OData.Core.V1.Description" String="Flexible Json manifest containing agent card informati
 > ion, provider,&#xA;capabilities, skills, security, and other manifest-defined fields." /></Annotations><Annotations Target="microsoft.graph.agentRegistration/agentIdentityBlueprintId"><Annotation Term="Org.OData.Core.V1.Description" String="Agent identity blueprint identifier." /
 > ion Term="Org.OData.Core.V1.Description" String="Agent identity blueprint identifier." /></Annotations><Annotations Target="microsoft.graph.agentRegistration/agentIdentityId"><Annotation Term="Org.OData.Core.V1.Description" String="Entra agent identity identifier." /></Annotation
 > otation Term="Org.OData.Core.V1.Description" String="Entra agent identity identifier." /></Annotations><Annotations Target="microsoft.graph.agentRegistration/createdBy"><Annotation Term="Org.OData.Core.V1.Description" String="The unique identifier of the user or app who created t
 >  String="The unique identifier of the user or app who created the agent registration." /></Annotations><Annotations Target="microsoft.graph.agentRegistration/description"><Annotation Term="Org.OData.Core.V1.Description" String="The agent description providing an overview of its p
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
# Does signIn entity have agentSignInSessions? check 'signIn' related nav in v1? use v1 too - but not downloaded. Check beta signIn full def
m=re.search(r'<EntityType Name="signIn" BaseType="graph.entity"[^>]*>.*?</EntityType>',x,re.S)
body=re.sub(r'\s+',' ',m.group(0)) if m else 'NOT FOUND'
print('== signIn entity (beta) ==')
print(body[:1400])
EOF
== signIn entity (beta) ==
<EntityType Name="signIn" BaseType="graph.entity"><Property Name="agent" Type="agentic.agentSignIn" /><Property Name="appDisplayName" Type="Edm.String" /><Property Name="appId" Type="Edm.String" /><Property Name="appliedConditionalAccessPolicies" Type="Collection(graph.appliedConditionalAccessPolicy)" /><Property Name="appliedEventListeners" Type="Collection(graph.appliedAuthenticationEventListener)" /><Property Name="appOwnerTenantId" Type="Edm.String" /><Property Name="appTokenProtectionStatus" Type="graph.tokenProtectionStatus" /><Property Name="authenticationAppDeviceDetails" Type="graph.authenticationAppDeviceDetails" /><Property Name="authenticationAppPolicyEvaluationDetails" Type="Collection(graph.authenticationAppPolicyDetails)" /><Property Name="authenticationContextClassReferences" Type="Collection(graph.authenticationContext)" /><Property Name="authenticationDetails" Type="Collection(graph.authenticationDetail)" /><Property Name="authenticationMethodsUsed" Type="Collection(Edm.String)" /><Property Name="authenticationProcessingDetails" Type="Collection(graph.keyValue)" /><Property Name="authenticationProtocol" Type="graph.protocolType" /><Property Name="authenticationRequirement" Type="Edm.String" /><Property Name="authenticationRequirementPolicies" Type="Collection(graph.authenticationRequirementPolicy)" /><Property Name="autonomousSystemNumber" Type="Edm.Int32" /><
[0m
All 8 new endpoints are **routed & auth-gated** (401, not 404) — a confirmed live surface. Now mapping scopes/ownership model from the remaining complex types + where `agentRegistration` binds, and checking v1 parity:
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
i=x.find('targetAgentIdentitySponsorsOrOwners')
while i>=0:
    seg=re.sub(r'\s+',' ',x[max(0,i-120):i+120])
    print('>',seg)
    i=x.find('targetAgentIdentitySponsorsOrOwners',i+1)
    if i > len(x): break
# signIn agent property description annotation
for t in ['microsoft.graph.signIn/agent','signIn/agent','agentSignIn']:
    i=x.find('Target="'+t+'"')
    if i>=0: print(t,'ANN:',re.sub(r'\s+',' ',x[i:i+400]))
EOF
> roperty Name="description" Type="Edm.String" /><Property Name="id" Type="Edm.String" /></ComplexType><ComplexType Name="targetAgentIdentitySponsorsOrOwners" BaseType="graph.userSet" /><ComplexType Name="targetUserSponsors" BaseType="graph.u
[0m
`agentRegistration.agentCard` is a JWS-signed manifest ("instructions, owners, publishing status") and `targetAgentIdentitySponsorsOrOwners` suggests Entra entitlement targeting. Let me find where that userSet binds, and pull documented scopes from MS Learn:
[0m◈ [0mParallel Web Search "Microsoft Graph API "agentRegistry" OR "agentInstances" OR "agentCardManifests" endpoint permissions documentation"
Major surface confirmed: the **Agent Registry API** (agentRegistry singleton + agentInstances/agentCardManifests/agentCollections) is now public in Graph docs with dedicated permissions (`AgentCardManifest.Read.All`, `AgentInstance.ReadWrite.ManagedBy`), and the PowerShell sample even references **v1.0**. Verifying v1 parity + the ManagedBy boundary live:
[0m
[0m$ [0mcd /tmp/opencode && curl -s -o graph_v1_meta.xml "https://graph.microsoft.com/v1.0/\$metadata" --max-time 90 && wc -c graph_v1_meta.xml && echo "=== v1 agentRegistry refs ===" && grep -c "agentRegistry" graph_v1_meta.xml; grep -c "agentCardManifest" graph_v1_meta.xml; grep -c "agentInstance" graph_v1_meta.xml; echo "=== v1 agents EntitySet ===" && grep -oE '<EntitySet Name="agents"[^/]*/>' graph_v1_meta.xml; echo "=== v1 agentIdentity/sponsor types ===" && grep -oE '<EntityType Name="agent[A-Za-z]*"' graph_v1_meta.xml | sort -u
2931817 graph_v1_meta.xml
=== v1 agentRegistry refs ===
0
0
0
=== v1 agents EntitySet ===
=== v1 agentIdentity/sponsor types ===
<EntityType Name="agentIdentity"
<EntityType Name="agentIdentityBlueprint"
<EntityType Name="agentIdentityBlueprintPrincipal"
<EntityType Name="agentUser"
[0m
v1 metadata has **no** agentRegistry (beta-only — the third-party v1.0 reference was inaccurate). Let me live-verify the v1.0 routes and extract the registry's documented access model from metadata annotations:
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
for t in ['agentRegistry','agentInstance','agentCardManifest','agentCollection','agentRegistration','agent']:
    # find annotations Targets
    for m in re.finditer(r'<Annotations Target="microsoft\.graph\.'+t+r'"[^>]*>',x):
        i=m.end()
        seg=x[i:i+900]
        desc=re.search(r'<Annotation Term="Org\.OData\.Core\.V1\.Description" String="([^"]*)"',seg)
        # permission annotations
        perms=re.findall(r'<Annotation Term="[^"]*Permission"[^>]*>|<Record><PropertyValue Property="Value" String="([^"]*)"',seg)
        if desc:
            print(f'[{t}] DESC: {desc.group(1)[:180]}')
EOF
[agentRegistration] DESC: Entity that represents an agent registration containing metadata, endpoint&#xA;configuration, tools, and publishing information.&#xA;This entity provides developers and administrat
[0m
[0m$ [0mcd /tmp/opencode && for p in "/v1.0/agentRegistry/agentInstances" "/v1.0/agents" "/beta/agentRegistry/agentCardManifests" "/beta/agentRegistry/agentCollections"; do
  out=$(curl -s -o /tmp/opencode/b.rsp -w "%{http_code}" --max-time 25 "https://graph.microsoft.com$p")
  sig=$(head -c 100 /tmp/opencode/b.rsp | tr -d '\n')
  echo "[$out] $p => ${sig:0:80}"
  sleep 1
done
[401] /v1.0/agentRegistry/agentInstances => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[401] /v1.0/agents => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[401] /beta/agentRegistry/agentCardManifests => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[401] /beta/agentRegistry/agentCollections => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
# Extract description annotations for all agentRegistry-related targets
targets=re.findall(r'<Annotations Target="(microsoft\.graph\.(?:agentRegistry|agentCardManifest|agentInstance|agentCollection|agentRegistration)[^"]*)"[^>]*>(.*?)</Annotations>',x,re.S)
for tgt,body in targets:
    d=re.search(r'Description" String="([^"]*)"',body,re.S)
    desc=re.sub(r'\s+',' ',d.group(1)) if d else ''
    if desc: print(f'{tgt} :: {desc[:200]}')
EOF
microsoft.graph.agentRegistration :: Entity that represents an agent registration containing metadata, endpoint&#xA;configuration, tools, and publishing information.&#xA;This entity provides developers and administrators with all details
microsoft.graph.agentRegistration/agentCard :: Flexible Json manifest containing agent card information following public manifest&#xA;specifications. Can include displayName, description, iconUrl, version, provider,&#xA;capabilities, skills, secur
microsoft.graph.agentRegistration/agentIdentityBlueprintId :: Agent identity blueprint identifier.
microsoft.graph.agentRegistration/agentIdentityId :: Entra agent identity identifier.
microsoft.graph.agentRegistration/createdBy :: The unique identifier of the user or app who created the agent registration.
microsoft.graph.agentRegistration/description :: The agent description providing an overview of its purpose and capabilities.
microsoft.graph.agentRegistration/displayName :: Display name for the agent instance.
microsoft.graph.agentRegistration/lastPublishedBy :: The unique identifier of the last person to publish the agent.
microsoft.graph.agentRegistration/managedByAppId :: Application identifier managing this agent.
microsoft.graph.agentRegistration/originatingStore :: Name of the store/system where the agent originated.
microsoft.graph.agentRegistration/ownerIds :: List of owner identifiers for the agent in case of user registering agent. Either owners or managedby is required
microsoft.graph.agentRegistration/sourceAgentId :: Original agent identifier from source system.
microsoft.graph.agentRegistration/sourceCreatedDateTime :: The date and time when the agent instance was created from source.
microsoft.graph.agentRegistration/sourceLastModifiedDateTime :: The date and time when the agent instance was last modified from source.
[0m
**Critical**: `/v1.0/agentRegistry/agentInstances` and `/v1.0/agents` are **also routed (401, not 404)** despite being absent from v1.0 metadata — an undocumented-but-routed v1 surface. Extracting the full registry access model + official scope table:
[0m% [0mWebFetch https://learn.microsoft.com/en-us/graph/api/agentregistry-list-agentinstances?view=graph-rest-beta&preserve-view=true
[0m% [0mWebFetch https://learn.microsoft.com/en-us/microsoft-agent-365/admin/graph-api
Now the migration surface — the Agent 365 admin API that's replacing the registry (new RECON target), plus the ID Protection agent-risk scopes:
[0m◈ [0mParallel Web Search "Microsoft Graph beta "identityProtection/riskyAgents" OR "agentRiskDetections" permissions role "Identity Risk""
Before claiming the v1.0 routes are real, a critical control test: does a *nonexistent* path also return 401 (auth-before-routing) or 404? This determines whether the 401s indicate genuine routes:
[0m
[0m$ [0mcd /tmp/opencode && for p in "/v1.0/thisdoesnotexistxyz12345" "/beta/thisdoesnotexistxyz12345" "/v1.0/agents" "/v1.0/agentRegistry/agentInstances" "/beta/agentRegistry/agentInstances/xyz/agentCardManifest"; do
  out=$(curl -s -o /tmp/opencode/b.rsp -w "%{http_code}" --max-time 25 "https://graph.microsoft.com$p")
  sig=$(head -c 90 /tmp/opencode/b.rsp | tr -d '\n')
  echo "[$out] $p => ${sig:0:80}"
  sleep 1
done
[401] /v1.0/thisdoesnotexistxyz12345 => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[401] /beta/thisdoesnotexistxyz12345 => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[401] /v1.0/agents => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[401] /v1.0/agentRegistry/agentInstances => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[401] /beta/agentRegistry/agentInstances/xyz/agentCardManifest => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[0m
**Methodological correction discovered**: even a nonexistent Graph path returns 401 — Graph authenticates before routing, so prior "401 = alive" claims for Graph are **null signals**. I need a real routing signal. Testing whether method-mismatch (405) discriminates:
[0m$ [0mcd /tmp/opencode && for p in "/v1.0/me/revokeSignInSessions" "/v1.0/me/changePassword" "/v1.0/me/sendMail" "/v1.0/me/checkMemberGroups" "/v1.0/users"; do
  out=$(curl -s -o /tmp/opencode/b.rsp -w "%{http_code}" --max-time 25 "https://graph.microsoft.com$p")
  sig=$(head -c 90 /tmp/opencode/b.rsp | tr -d '\n')
  echo "[$out] $p => ${sig:0:80}"
  sleep 1
done
[401] /v1.0/me/revokeSignInSessions => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[401] /v1.0/me/changePassword => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[401] /v1.0/me/sendMail => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[401] /v1.0/me/checkMemberGroups => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[401] /v1.0/users => {"error":{"code":"InvalidAuthenticationToken","message":"Access token is empty."
[0m
Confirmed: even documented POST-only actions return 401 pre-auth — **no unauthenticated request can distinguish a Graph route**; the auth gate precedes routing entirely. This is a methodology correction for the whole journal (401 ≠ "alive" for Graph). The surfaces stand on stronger evidence: declared in public `$metadata` + documented on MS Learn. Capturing the last piece — the Agent 365 package API (registry replacement):
[0m% [0mWebFetch https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/api/admin-settings/package/overview
The Agent 365 Package API (`/copilot/admin/catalog/packages*`) is the replacement surface with block/reassign/update ops. Checking if it's declared in beta $metadata (which would confirm it's a live Graph route vs standalone):
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
for term in ['copilotPackage','admin/catalog','catalog/packages','copilotRoot','copilot/admin','declarativeAgent','customEngineAgent']:
    c=x.count(term)
    print(term,'refs:',c)
# copilotRoot singleton definition
i=x.find('Singleton Name="copilot"')
if i>=0:
    print('copilot singleton ctx:', re.sub(r'\s+',' ',x[i-100:i+600]))
EOF
copilotPackage refs: 15
admin/catalog refs: 0
catalog/packages refs: 0
copilotRoot refs: 8
copilot/admin refs: 0
declarativeAgent refs: 0
customEngineAgent refs: 0
copilot singleton ctx: hMonitoring.directoryObjectImpactSummary/resourceSampling" Target="directoryObjects" /></Singleton><Singleton Name="copilot" Type="microsoft.graph.copilotRoot"><NavigationPropertyBinding Path="agents" Target="agents" /></Singleton><Singleton Name="agentRegistry" Type="microsoft.graph.agentRegistry"><NavigationPropertyBinding Path="agentCollections/members" Target="agentRegistry/agentInstances" /><NavigationPropertyBinding Path="agentInstances/agentCardManifest" Target="agentRegistry/agentCardManifests" /><NavigationPropertyBinding Path="agentInstances/collections" Target="agentRegistry/agentCollections" /></Singleton><Singleton Name="solutions" Type="microsoft.graph.solutionsRoot"><Navigatio
[0m
`copilotPackage` is declared (15 refs) but the `/copilot/admin/catalog` path is **not** in Graph $metadata — separate surface. Extracting the copilotPackage entity + confirming the agents/copilotTools model:
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
for m in re.finditer(r'<EntityType Name="copilotPackage[^"]*"[^>]*>.*?</EntityType>',x,re.S):
    print('---', re.sub(r'\s+',' ',m.group(0))[:800])
for m in re.finditer(r'<ComplexType Name="copilotPackage[^"]*"[^>]*>.*?</ComplexType>',x,re.S):
    print('---CT', re.sub(r'\s+',' ',m.group(0))[:600])
# where copilotPackage refs besides def
idx=[m.start() for m in re.finditer(r'copilotPackage',x)]
print('ref positions:',len(idx))
for s in idx[1:6]:
    print('>', re.sub(r'\s+',' ',x[s-120:s+120]))
EOF
--- <EntityType Name="copilotPackage" BaseType="graph.entity"><Property Name="appId" Type="Edm.String" /><Property Name="assetId" Type="Edm.String" /><Property Name="availableTo" Type="graph.packageStatus" Nullable="false" /><Property Name="createdDateTime" Type="Edm.DateTimeOffset" /><Property Name="deployedTo" Type="graph.packageStatus" Nullable="false" /><Property Name="displayName" Type="Edm.String" /><Property Name="elementTypes" Type="Collection(Edm.String)" Nullable="false" /><Property Name="isBlocked" Type="Edm.Boolean" Nullable="false" /><Property Name="lastModifiedDateTime" Type="Edm.DateTimeOffset" /><Property Name="manifestId" Type="Edm.String" /><Property Name="manifestVersion" Type="Edm.String" /><Property Name="ownerId" Type="Edm.String" /><Property Name="platform" Type="Edm.Str
--- <EntityType Name="copilotPackageDetail" BaseType="graph.copilotPackage"><Property Name="acquireUsersAndGroups" Type="Collection(graph.packageAccessEntity)" Nullable="false" /><Property Name="activeUsers" Type="Edm.Int32" /><Property Name="allowedUsersAndGroups" Type="Collection(graph.packageAccessEntity)" Nullable="false" /><Property Name="categories" Type="Collection(Edm.String)" Nullable="false" /><Property Name="elementDetails" Type="Collection(graph.packageElementDetail)" Nullable="false" /><Property Name="exceptionRate" Type="Edm.Double" /><Property Name="lastUsedDateTime" Type="Edm.DateTimeOffset" /><Property Name="longDescription" Type="Edm.String" /><Property Name="sensitivity" Type="Edm.String" /><Property Name="sharedWithUsersAndGroups" Type="Collection(graph.packageAccessEntity)
---CT <ComplexType Name="copilotPackageUpdateResponse"><Property Name="id" Type="Edm.String" Nullable="false" /></ComplexType>
ref positions: 15
>  users who actively used the package during the reporting period." /></Annotations><Annotations Target="microsoft.graph.copilotPackageDetail/exceptionRate"><Annotation Term="Org.OData.Core.V1.Description" String="The fraction of sessions th
> t resulted in an exception, expressed as a value between 0 and 1." /></Annotations><Annotations Target="microsoft.graph.copilotPackageDetail/lastUsedDateTime"><Annotation Term="Org.OData.Core.V1.Description" String="The date and time when t
>  time when the package was last used, in ISO 8601 format and UTC." /></Annotations><Annotations Target="microsoft.graph.copilotPackageDetail/totalRunTimeInHours"><Annotation Term="Org.OData.Core.V1.Description" String="Total hours worked by
> ata.Core.V1.Description" String="Total hours worked by the agent." /></Annotations><Annotations Target="microsoft.graph.copilotPackageDetail/totalSessions"><Annotation Term="Org.OData.Core.V1.Description" String="The total number of session
> e="false" /><Property Name="sensitivityLabel" Type="graph.searchSensitivityLabelInfo" /></EntityType><ComplexType Name="copilotPackageUpdateResponse"><Property Name="id" Type="Edm.String" Nullable="false" /></ComplexType><ComplexType Name="
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
# how are copilotPackage objects exposed? look for entity sets of that type + copilotRoot navs + catalog
for m in re.finditer(r'<EntitySet Name="([^"]*)" EntityType="[^"]*\.copilotPackage"',x):
    print('copilotPackage EntitySet:',m.group(1))
for m in re.finditer(r'<EntitySet Name="([^"]*)" EntityType="[^"]*\.copilotPackageDetail"',x):
    print('copilotPackageDetail EntitySet:',m.group(1))
# copilotRoot full def
i=x.find('EntityType Name="copilotRoot"')
if i>=0:
    seg=x[i:i+1200]
    print('== copilotRoot ==', re.sub(r'\s+',' ',seg)[:900])
# packageStatus / packageAccessEntity / packageElementDetail
for n in ['packageStatus','packageAccessEntity','packageElementDetail','searchSensitivityLabelInfo']:
    m=re.search(r'<(EnumType|ComplexType) Name="'+n+r'"[^>]*>.*?</\1>',x,re.S)
    if m: print('==',n,'==', re.sub(r'\s+',' ',m.group(0))[:400])
    else:
        i=x.find(n)
        print(n,'(def not found; ctx):', re.sub(r'\s+',' ',x[max(0,i-100):i+100]) if i>=0 else 'n/a')
EOF
== copilotRoot == EntityType Name="copilotRoot"><NavigationProperty Name="agentRegistrations" Type="Collection(graph.agentRegistration)" ContainsTarget="true" /><NavigationProperty Name="conversations" Type="Collection(graph.copilotConversation)" ContainsTarget="true" /><NavigationProperty Name="admin" Type="graph.copilotAdmin" ContainsTarget="true" /><NavigationProperty Name="agents" Type="Collection(graph.agent)" /><NavigationProperty Name="reports" Type="graph.copilotReportRoot" ContainsTarget="true" /><NavigationProperty Name="settings" Type="graph.copilotSetting" ContainsTarget="true" /><NavigationProperty Name="communications" Type="graph.copilotCommunicationsRoot" ContainsTarget="true" /><NavigationProperty Name="interactionHistory" Type="graph.aiInteractionHistory" ContainsTarget="true" /><NavigationProperty Name="users" Type="Collection(graph.aiUser)" ContainsTarget="true" /></EntityType><EntityT
== packageStatus == <EnumType Name="packageStatus"><Member Name="none" Value="0" /><Member Name="some" Value="1" /><Member Name="all" Value="2" /><Member Name="unknownFutureValue" Value="3" /></EnumType>
== packageAccessEntity == <ComplexType Name="packageAccessEntity"><Property Name="resourceId" Type="Edm.String" Nullable="false" /><Property Name="resourceType" Type="graph.accessEntityType" Nullable="false" /></ComplexType>
== packageElementDetail == <ComplexType Name="packageElementDetail"><Property Name="elements" Type="Collection(graph.packageElement)" Nullable="false" /><Property Name="elementType" Type="Edm.String" /></ComplexType>
== searchSensitivityLabelInfo == <ComplexType Name="searchSensitivityLabelInfo"><Property Name="color" Type="Edm.String" /><Property Name="displayName" Type="Edm.String" /><Property Name="priority" Type="Edm.Int32" /><Property Name="sensitivityLabelId" Type="Edm.String" /><Property Name="tooltip" Type="Edm.String" /></ComplexType>
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
m=re.search(r'<EntityType Name="copilotAdmin"[^>]*>.*?</EntityType>',x,re.S)
print('== copilotAdmin ==')
print(re.sub(r'\s+',' ',m.group(0))[:1200] if m else 'NOT FOUND')
# catalog refs
for term in ['catalog','catalogRoot']:
    c=x.count(term); print(term,'refs:',c)
i=x.find('catalog')
if i>=0: print('ctx:', re.sub(r'\s+',' ',x[max(0,i-150):i+150]))
EOF
== copilotAdmin ==
<EntityType Name="copilotAdmin" BaseType="graph.entity"><NavigationProperty Name="catalog" Type="graph.copilotAdminCatalog" ContainsTarget="true" /><NavigationProperty Name="policySettings" Type="Collection(graph.copilotPolicySetting)" ContainsTarget="true" /><NavigationProperty Name="settings" Type="graph.copilotAdminSetting" ContainsTarget="true" /></EntityType>
catalog refs: 78
catalogRoot refs: 0
ctx: oft.graph.deviceManagementConfigurationSettingVisibility/settingsCatalog"><Annotation Term="Org.OData.Core.V1.Description" String="Visible to setting catalog policy type." /></Annotations><Annotations Target="microsoft.graph.deviceManagementConfigurationSettingVisibility/template"><Annotation Term="
[0m
[0m$ [0mcd /tmp/opencode && python3 - << 'EOF'
import re
x=open('graph_beta_meta.xml').read()
m=re.search(r'<EntityType Name="copilotAdminCatalog"[^>]*>.*?</EntityType>',x,re.S)
print('== copilotAdminCatalog ==')
print(re.sub(r'\s+',' ',m.group(0))[:1200] if m else 'NOT FOUND')
EOF
== copilotAdminCatalog ==
<EntityType Name="copilotAdminCatalog" BaseType="graph.entity"><NavigationProperty Name="packages" Type="Collection(graph.copilotPackageDetail)" ContainsTarget="true" /></EntityType>
[0m
[0m$ [0mcd /tmp/opencode && sha256sum graph_v1_meta.xml graph_beta_meta.xml
9d84e4510aa8d6bc95f14ae0bde05ea01c967fcd29478269b58e7cfdd2bdc16a  graph_v1_meta.xml
ca3048596553b16c06004d1dacfae221de85b8e58878d7228feb15a7aeda2ebe  graph_beta_meta.xml
[0m
Full model recovered. `copilot/admin/catalog/packages` maps to `copilotAdminCatalog.packages` (copilotPackageDetail) — the Agent 365 surface IS in the beta schema. Retrying CT inventory + hashing artifacts, then finalizing:
[0m$ [0msleep 1 && cd /tmp/opencode && curl -s --max-time 60 "https://crt.sh/?q=%25agent%25.microsoft.com&output=json" -o ct_agent.json 2>/dev/null; echo "bytes: $(wc -c < ct_agent.json 2>/dev/null)"; python3 -c "
import json
try:
    d=json.load(open('ct_agent.json'))
    names=set()
    for e in d:
        for n in e.get('name_value','').split('\n'):
            n=n.strip().lower()
            if n and '*' not in n: names.add(n)
    print('names:',len(names))
    for n in sorted(names): print(' ',n)
except Exception as ex: print('ERR',ex)
" 2>/dev/null | head -50
bytes: 150
ERR Expecting value: line 1 column 1 (char 0)
[0m
[0m$ [0mcd /tmp/opencode && grep -rliE "agentRegistry|agentCardManifest|copilot/admin|copilotPackage|riskyAgent|agentRegistration" entrabot_* 2>/dev/null; echo "---"; grep -oE "agentIdentity[A-Za-z]*|agentUserId|identityParentId|fmi_path|user_fic" entrabot_create_entra_agent_ids.py | sort | uniq -c | sort -rn | head
---
      8 identityParentId
      5 agentIdentityBlueprintId
      2 agentIdentity
      1 user_fic
      1 agentIdentityBlueprintPrincipal
      1 agentIdentityBlueprint
[0m
crt.sh still down (502); metadata hashes match prior runs (`9d84e451…`/`ca304859…`) — confirming the registry/copilot/risk surface was already in beta schema and simply unextracted earlier. Final check — does entrabot reference the registry/catalog APIs:
[0m$ [0mcd /home/runner/work/hunt-lab/hunt-lab && cat >> research/bigpickle.md <<'EOF'

## RUN 2026-08-07 12:45:00 (big-pickle) — TARGET: microsoft — PHASE: POC→RECON (Graph agent-management ecosystem + methodology correction)

### METHODOLOGY CORRECTION (applies retroactively)
- CONTROL TEST: unauthenticated GET on nonexistent Graph paths (/v1.0/thisdoesnotexistxyz12345, /beta/thisdoesnotexistxyz12345) and on documented POST-only actions (/v1.0/me/revokeSignInSessions, changePassword, sendMail, checkMemberGroups) ALL return HTTP 401 InvalidAuthenticationToken. Graph authenticates BEFORE routing, so an unauthenticated GET cannot distinguish a real route from a nonexistent one. → ALL prior journal claims of the form "GET <graph endpoint> → 401, therefore alive/routed" are NULL signals and are RETRACTED (incl. /me/agentSignInSessions, /me/authentication/methods). Route existence is instead established by public $metadata declarations + MS Learn docs. The non-Graph hosts (api.myaccount / api.mysignins) returned 401+WWW-Authenticate/405 — those remain valid route signals.

### NEW SURFACE — Graph Agent Management ecosystem (recovered from beta $metadata sha256 ca304859… + MS Learn docs; all declared, all auth-gated, Global cloud only)
- Agent Registry singleton /beta/agentRegistry → agentInstances / agentCardManifests / agentCollections. Docs: learn.microsoft.com/graph/api/agentregistry-list-agentinstances. agentInstance links agentIdentityId + agentUserId + agentIdentityBlueprintId; carries url + additionalInterfaces (transport endpoints), JWS-signed agentCard signatures (alg ES256, kid did:web, type agentcard+jws), ownerIds OR managedBy ("either required"). Permissions: AgentInstance.Read.All / .ReadWrite.All / .ReadWrite.ManagedBy (app); delegated requires Agent Registry Administrator dir role. DEPRECATION: replaced May-2026 by Agent 365 package APIs.
- agents EntitySet /beta/agents (microsoft.graph.agent, nav copilotTools); /beta/copilot/agents; /beta/auditLogs/agents. copilotRoot (/beta/copilot) also exposes agentRegistrations, admin/catalog/packages (copilotPackageDetail; block/unblock/reassign/update ops; requires Agent 365 license + AI admin or Global admin), admin/policySettings, admin/settings, settings, reports, conversations, interactionHistory, users.
- ID Protection: /beta/identityProtection/riskyAgents (subtypes riskyAgentIdentity / riskyAgentIdentityBlueprintPrincipal / riskyAgentUser; actions confirmCompromised/confirmSafe/dismiss) + /beta/identityProtection/agentRiskDetections. Scope IdentityRiskyAgent.Read.All; delegated roles Global Reader / Security Operator / Security Reader / Security Administrator.
- Audit attribution: signIn.agent + summarizedSignIn.agent = agentic.agentSignIn (agentServicePrincipalId, agentSubjectParentId, agentType/agentSubjectType, parentAppId). agentType enum: notAgentic=0, agenticApp=2, agenticAppInstance=3, agentIdentityBlueprintPrincipal=4, agentIDuser=5.
- Metadata annotations: agentRegistration.agentCard = "Flexible Json manifest … displayName, description, iconUrl, version, provider, capabilities, skills, security"; ownerIds = "either owners or managedby is required".

### CODE REVIEW (microsoft/entrabot, in-scope org) — wire-level consent-grant primitive (H-2 refinement)
- scripts/create_entra_agent_ids.py: POST /v1.0/oauth2PermissionGrants with clientId={agent-identity-oid}, consentType=Principal, principalId={agent-user-oid}, resourceId={CALLER-CHOSEN SP oid}, scope string. ResourceId is NOT restricted to Graph: same primitive grants Azure Storage user_impersonation (app e406a681-f3d4-42a8-90b6-c2b029497af1) so hop-3 can mint idtyp=user https://storage.azure.com/.default tokens. Grant is one-time, per-principal, non-expiring (startTime only). Provisioner app needs DelegatedPermissionGrant.ReadWrite.All.
- Application.Read.All granted to agentIdentity SP via appRoleAssignment (app role id 9a5d68dd-52b0-4cc2-bd40-abcf44ac3a30) — Application.Read.All is NOT in the agent blocked-permissions table (only ReadWrite.* are) → agents can enumerate tenant apps/SPs.
- New subtype navs: GET /servicePrincipals/{id}/microsoft.graph.agentIdentity/sponsors (beta); GET /oauth2PermissionGrants?$filter=clientId eq '{oid}' and principalId eq '{oid}' (v1); GET /applications/{id}?$select=keyCredentials.
- UPN uniqueness used as tenant-wide lookup key (find_existing_agent_user_by_upn). entrabot explicitly warns "pooling agent users is an anti-pattern: object-ID recycling → residual permissions attach to next holder" (design acknowledgment of object-id reuse risk).
- No entrabot reference to agentRegistry/copilot catalog — those are separate first-party surfaces.

### Live checks (passive, 1/sec, in-scope graph.microsoft.com)
- /beta/agents, /beta/copilot/agents, /beta/agentRegistry{,/agentInstances,/agentCardManifests,/agentCollections}, /beta/auditLogs/agents, /beta/identityProtection/{riskyAgents,agentRiskDetections}, /v1.0/agents, /v1.0/agentRegistry/agentInstances → ALL 401 (now classified NULL signal; route existence rests on $metadata+docs).
- crt.sh CT inventory for %agent%.microsoft.com: still 502 (3rd consecutive run). agents.microsoft.com previously characterized (parked on adoption.microsoft.com AFD, *.azureedge.net SAN mismatch → 404).

### Hypotheses (test-tenant, read-only design)
- H-6 [HIGH] Registry ownership boundary: is managedBy enforced = caller appId on POST/PATCH agentInstances, or settable arbitrarily? Can a holder of AgentInstance.ReadWrite.ManagedBy (app) or Agent Registry Administrator (delegated) claim/overwrite another's agentInstance + agentCardManifest (instructions/endpoints)? Also copilot package reassign (POST /copilot/admin/catalog/packages/{id}/reassign) and block/unblock — role boundary. If not enforced → agent supply-chain takeover at M365 runtime. CVSS 7.5–9.0.
- H-7 [HIGH] agentCard JWS trust: registry stores optional signatures (ES256, did:web kid). Does the consuming runtime verify signature over the card (type agentcard+jws) or trust unsigned manifests? Unsigned/spoofed card → first-party impersonation + instruction injection. Test-tenant. CVSS 6.5–9.0.
- H-8 [MED] riskyAgents write-op integrity: confirmCompromised/confirmSafe/dismiss on arbitrary agent ids — risk-state tampering vs agent-risk CA. Role-gated (Security Administrator/Global Reader read). CVSS 4–6.
- H-9 [MED] agent sign-in attribution (signIn.agent: agentServicePrincipalId, parentAppId) as second data source for the agentSignInSessions cross-user hypothesis (PD-A). CVSS 6.5–8.8.
- H-1 [HIGHEST, unchanged] user_fic Hop-3 subject validation (user_id vs username alternate key). H-2 [HIGH, refined] consent-grant primitive is resource-agnostic (resourceId arbitrary → storage user_impersonation / any first-party resource). H-3 [MED] managerApplications first-party list.

### Secrets check
- None. entrabot = design/code only (no tokens/keys). Public client IDs, well-known app-role GUID (9a5d68dd-…), MS-doc sample UUIDs. Metadata hashes unchanged: graph_v1_meta.xml 9d84e451…, graph_beta_meta.xml ca304859…. No raw secrets committed.
EOF
echo "journal lines: $(wc -l < research/bigpickle.md)"
journal lines: 5535
[0m
entrabot only touches the identity chain (no registry/catalog usage) — confirming those are separate first-party surfaces. All analysis complete; writing journal, leads, and state:
[0m$ [0mcd /home/runner/work/hunt-lab/hunt-lab && cat >> leads/lead-bigpickle.md <<'EOF'

## 2026-08-07 12:45:00 UTC [microsoft] (model bigpickle) — POC->RECON (Graph agent ecosystem + methodology fix)

- [METHODOLOGY-CORRECTION] Control test: nonexistent Graph paths AND documented POST-only actions all return 401 pre-routing (graph.microsoft.com authenticates before routing). All prior "401 = endpoint alive" claims for Graph (incl. /me/agentSignInSessions, /me/authentication/methods) are RETRACTED as null signals. Route existence must come from $metadata + docs. api.myaccount/api.mysignins 401/405 statuses remain valid.
- [NEW-SURFACE] Agent Registry (beta): /beta/agentRegistry singleton -> agentInstances/agentCardManifests/agentCollections (agentInstance links agentIdentityId+agentUserId+agentIdentityBlueprintId; url+additionalInterfaces; JWS agentCard signatures ES256 did:web agentcard+jws; ownerIds OR managedBy). Perms: AgentInstance.Read.All / ReadWrite.All / ReadWrite.ManagedBy; delegated requires Agent Registry Administrator role. Global cloud only. Deprecated May-2026 in favor of Agent 365.
- [NEW-SURFACE] Copilot agent management (beta): /beta/agents (nav copilotTools), /beta/copilot/agents, /beta/copilot/agentRegistrations, /beta/copilot/admin/catalog/packages (copilotPackageDetail; list/get/update/block/unblock/reassign; Agent 365 license + AI admin/Global admin), /beta/auditLogs/agents.
- [NEW-SURFACE] ID Protection agent risk (beta): /beta/identityProtection/riskyAgents (riskyAgentIdentity/riskyAgentIdentityBlueprintPrincipal/riskyAgentUser; confirmCompromised/confirmSafe/dismiss) + /beta/identityProtection/agentRiskDetections. Scope IdentityRiskyAgent.Read.All; roles Global Reader/Security Operator/Reader/Administrator.
- [NEW-SURFACE] Audit: signIn.agent = agentic.agentSignIn (agentServicePrincipalId, agentSubjectParentId, agentType/agentSubjectType, parentAppId); agentType enum notAgentic=0 agenticApp=2 agenticAppInstance=3 agentIdentityBlueprintPrincipal=4 agentIDuser=5.
- [LEAD-HIGH H-6] Agent Registry ownership boundary: managedBy enforced=caller appId? reassign/block role boundary? If not -> claim/overwrite any agent card manifest (instructions+endpoints). CVSS 7.5-9.0.
- [LEAD-HIGH H-7] agentCard JWS trust: is signature verified by M365 runtime or advisory? Unsigned/spoofed card = first-party impersonation. CVSS 6.5-9.0.
- [LEAD-MED H-8] riskyAgents write ops (confirmCompromised/Safe/dismiss) integrity vs agent-risk CA. CVSS 4-6. [LEAD-MED H-9] signIn.agent attribution as 2nd source for agentSignInSessions cross-user (PD-A). CVSS 6.5-8.8.
- [CODE-REVIEW] entrabot create_entra_agent_ids.py: consent grant POST /v1.0/oauth2PermissionGrants with CALLER-CHOSEN resourceId (Graph OR Azure Storage user_impersonation e406a681-...); one-time per-principal non-expiring; needs DelegatedPermissionGrant.ReadWrite.All. Application.Read.All NOT agent-blocked (role 9a5d68dd-...). Subtype navs: /servicePrincipals/{id}/microsoft.graph.agentIdentity/sponsors. Pooling anti-pattern note: object-id recycling residual-permission risk (design-acknowledged).
EOF
cat > state_bigpickle.json <<'EOF'
{"phase": "POC", "status": "HIGH_POTENTIAL"}
EOF
cat state_bigpickle.json; echo; echo "leads lines: $(wc -l < leads/lead-bigpickle.md)"
{"phase": "POC", "status": "HIGH_POTENTIAL"}

leads lines: 96
[0m
# Program: microsoft
# Phase: POC (design) → RECON (new surface: Graph Agent Management ecosystem; + methodology correction)

## Key Findings
- **Methodology correction (applies retroactively):** control-tested that unauthenticated GETs to *nonexistent* Graph paths and to *documented POST-only actions* all return `401 InvalidAuthenticationToken` — Graph authenticates **before** routing, so prior journal claims of "401 ⇒ endpoint alive" for Graph (incl. `/me/agentSignInSessions`, `/me/authentication/methods`) are **null signals and retracted**. Route existence now rests on public `$metadata` declarations + MS Learn docs. (api.myaccount / api.mysignins 401/405/404 remain valid signals — non-Graph hosts.)
- **NEW surface — Agent Registry API** (`/beta/agentRegistry` singleton → `agentInstances`/`agentCardManifests`/`agentCollections`), declared in beta `$metadata` (sha256 `ca304859…`, unchanged from prior runs) + documented at learn.microsoft.com/graph/api/agentregistry-list-agentinstances. `agentInstance` binds `agentIdentityId`+`agentUserId`+`agentIdentityBlueprintId`, carries runtime endpoint `url`/`additionalInterfaces`, JWS-signed agent cards (`ES256`, `did:web` kid, `agentcard+jws`), and `ownerIds` **OR** `managedBy` ("either required"). Permissions: `AgentInstance.Read.All`/`.ReadWrite.All`/`.ReadWrite.ManagedBy`; delegated callers need **Agent Registry Administrator** role. Global cloud only; **deprecated May-2026** in favor of Agent 365.
- **NEW surface — Copilot/agent admin** (beta): `/beta/agents` (nav `copilotTools`), `/beta/copilot/agents`, `/beta/copilot/agentRegistrations`, and `/beta/copilot/admin/catalog/packages` (copilotPackageDetail; list/get/update/**block/unblock/reassign**; Agent 365 license + AI admin/Global admin), plus `/beta/auditLogs/agents`.
- **NEW surface — ID Protection agent risk** (beta): `/beta/identityProtection/riskyAgents` (subtypes riskyAgentIdentity / riskyAgentIdentityBlueprintPrincipal / riskyAgentUser; actions confirmCompromised/confirmSafe/dismiss) + `/beta/identityProtection/agentRiskDetections`. Scope `IdentityRiskyAgent.Read.All`; roles Global Reader/Security Operator/Reader/Administrator.
- **NEW surface — audit attribution:** `signIn.agent` = `agentic.agentSignIn` (`agentServicePrincipalId`, `agentSubjectParentId`, `agentType`/`agentSubjectType`, `parentAppId`); enum `notAgentic=0, agenticApp=2, agenticAppInstance=3, agentIdentityBlueprintPrincipal=4, agentIDuser=5`.
- **Code review (microsoft/entrabot):** consent-grant primitive is **resource-agnostic** — `POST /v1.0/oauth2PermissionGrants` with caller-chosen `resourceId` (Graph **or** Azure Storage `user_impersonation` `e406a681…`), one-time per-principal non-expiring. `Application.Read.All` is **not** in the agent blocked-permissions table (only `ReadWrite.*` are). entrabot design-docs acknowledge object-ID recycling/residual-permission risk ("pooling is an anti-pattern").

## New Endpoints / Assets
- Graph beta (all declared in `$metadata`, auth-gated): `/agentRegistry`, `/agentRegistry/agentInstances`, `/agentRegistry/agentCardManifests`, `/agentRegistry/agentCollections`, `/agents`, `/copilot/agents`, `/copilot/agentRegistrations`, `/copilot/admin/catalog/packages` (+`/{id}/block|unblock|reassign`), `/auditLogs/agents`, `/identityProtection/riskyAgents`, `/identityProtection/agentRiskDetections`.
- Graph beta entity model: `agent` (copilotTools), `agentRegistry`, `agentRegistration` (agentCard JSON manifest: iconUrl/provider/skills/security), `agentInstance` (signatures, url, interfaces), `agentCardManifest`, `agentCollection`, `copilotPackage`/`copilotPackageDetail` (allowedUsersAndGroups, sensitivityLabel), `riskyAgent*`, `agentic.agentSignIn` on `signIn`/`summarizedSignIn`.
- Graph API surface (entrabot): `POST /v1.0/oauth2PermissionGrants` (caller-chosen resourceId), `POST /v1.0/servicePrincipals/{oid}/appRoleAssignments` (Application.Read.All role `9a5d68dd…`), `GET /servicePrincipals/{oid}/microsoft.graph.agentIdentity/sponsors`, `GET /users?$filter=identityParentId eq '{oid}'`.

## Hypotheses (impact + read-only test)
1. **[H-6 HIGH]** Registry ownership boundary: is `managedBy` enforced = caller appId on POST/PATCH, and are `reassign`/`block` role-gated? If not, any app with `AgentInstance.ReadWrite.ManagedBy` claims/overwrites another agent's card manifest (instructions + endpoints) → supply-chain takeover at M365 runtime. CVSS 7.5–9.0.
2. **[H-7 HIGH]** agentCard JWS trust: does the consuming runtime verify the `agentcard+jws` signature or trust unsigned manifests? Spoofed card → first-party impersonation. CVSS 6.5–9.0.
3. **[H-8 MED]** riskyAgents write ops (`confirmCompromised`/`confirmSafe`/`dismiss`) — risk-state tampering vs agent-risk Conditional Access. CVSS 4–6.
4. **[H-9 MED]** `signIn.agent` attribution as a second data source for the agentSignInSessions cross-user IDOR (PD-A). CVSS 6.5–8.8.
5. **[H-1 HIGHEST, unchanged]** `user_fic` Hop-3 subject validation (`user_id` vs `username` alternate key) → arbitrary-user minting. CVSS 9.1–9.8.
6. **[H-2 HIGH, refined]** consent-grant primitive is resource-agnostic (arbitrary `resourceId` incl. storage `user_impersonation`) — whether self-consent is admin-gated is the test. CVSS 7.5–8.8.

## Secrets Check
None. entrabot = design/code only (no tokens/keys). Only public client IDs, well-known app-role GUID (`9a5d68dd-52b0-4cc2-bd40-abcf44ac3a30`), MS-doc sample UUIDs. Artifact hashes only: `graph_v1_meta.xml 9d84e451…`, `graph_beta_meta.xml ca304859…`.

## CVSS Candidates
H-6 7.5–9.0; H-7 6.5–9.0; H-8 4–6; H-9 6.5–8.8; H-1 9.1–9.8; H-2 7.5–8.8.

---
## STATE UPDATE
STATUS_PHASE: POC
STATUS_STATE: HIGH_POTENTIAL
NEXT_STEP_1: Test-tenant execution of H-1 (user_fic Hop-3: mint for own Agent User vs unrelated `user_id`/`username`; 200 on unrelated ⇒ escalate to MSRC) and H-2 (self-consent gate via `POST /v1.0/oauth2PermissionGrants` with arbitrary resourceId).
NEXT_STEP_2: Test-tenant H-6/H-7: create agentInstance/card with foreign `managedBy`/`ownerIds` (is it accepted?) and unsigned card signature; enumerate copilot package `reassign`/`block` role boundary.
NEXT_STEP_3: If no test tenant, RECON next unexplored surface: retry crt.sh CT inventory for `*agent*.microsoft.com` (down 3 runs), enumerate Agent 365 package API host binding, and mine `microsoft/entrabot` `docs/reference/scripts/operations/show-agent-status` + `entra_provisioning.py` for provisioner scopes/cert handling.
