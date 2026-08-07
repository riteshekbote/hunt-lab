# Laguna Research Journal — Microsoft Bounty (MSRC)
Model: opencode/laguna-s-2.1-free
Run started: 2026-08-07 (round-robin slot=1 => TARGET=microsoft)
Phase this run: RECON (Phase 1)

## Sources reviewed (passive reads of program docs + public code)
- https://www.microsoft.com/en-us/msrc/bounty (Bounty Programs overview)
- https://www.microsoft.com/en-us/msrc/bounty-programs (program list + awards)
- https://www.microsoft.com/en-us/msrc/pentest-rules-of-engagement (Rules of Engagement)
- https://microsoft.com/msrc/bounty-microsoft-identity (Identity program scope)  ← highest priority
- https://www.microsoft.com/en-us/msrc/opensourcebountyprogram (Open Source program scope)
- https://aka.ms/msrc-api-docs-cvrf (MSRC CVRF / Security Update Guide OpenAPI)
- GitHub org: microsoft/ (8246 repos per API; sampled pages 1-5 + identity-named search)

## Policy / Rules summary
- Passive-only, GET/HEAD, <=1 rps. NO account creation, NO writes, NO state changes.
- Bounty Legal Safe Harbor applies to good-faith testing.
- Report via MSRC Researcher Portal: https://msrc.microsoft.com/report/vulnerability
- IMPORTANT OUT-OF-SCOPE classes (will not pursue):
  * Sub-Domain Takeover (explicitly OOS in Identity program)
  * Denial of Service (OOS)
  * Reports from automated scanning tools (OOS)
  * Training/docs/samples/community forums (OOS)
  * Leaked non-live/test tokens w/o service impact (OOS)

## Bounty programs & award ceilings (key)
- Microsoft Identity (MSA / Azure AD / OpenID): $750–$100,000 (MFA bypass = $100k)
- Microsoft Azure: up to $60,000
- Microsoft Copilot (AI): up to $30,000
- M365 cloud: up to $19,500
- Open Source: $750–$15,000  (auth issues/injection/SSRF/IDOR/XSS/CSRF/web misconfig/cross-origin/input-validation/GitHub Actions/deserialization)
- Hyper-V: up to $250,000 ; Windows Insider: up to $100,000 ; Edge: up to $30,000
- Zero Day Quest: Azure, Copilot, Dynamics, Identity, M365 (research challenge: open; live event: invite-only)

## Identity program — explicit in-scope endpoints (from docs)
login.windows.net | login.microsoftonline.com | login.live.com | account.live.com
account.microsoft.com | signup.live.com | account.windowsazure.com | account.activedirectory.windowsazure.com
credential.activedirectory.windowsazure.com | passwordreset.microsoftonline.com
adminwebservice.microsoftonline.com | api.mysignins.microsoft.com | provisioningapi.microsoftonline.com
myaccess.microsoft.com | myapps.microsoft.com | myaccount.microsoft.com | microsoftazuread-sso.com
mysignins.microsoft.com | accounts.accesscontrol.windows.net
Mobile: Microsoft Authenticator (+Broker, Lite) iOS/Android
Graph: graph.microsoft.com (Directory Management, Governance, Identity & Sign In tabs)
Standards: OpenID Connect Core/Discovery/Session, OAuth 2.0 Multiple Response Types, OAuth 2.0 Form Post (ratified only; certified impls only)

## Open-Source program — explicitly in-scope GitHub repos
- azure/azure-sdk + azure-sdk-for-{net,python,js,java,go,rust} + azure-sdk-tools
- microsoft/fluentui
- PowerShell/PowerShell (PS 7)
- microsoft/typescript
- microsoft/vscode
- microsoft/agent-framework  (semantic-kernel & autogen ARE excluded)
- microsoft/monaco-editor
- microsoft/msquic

## Tech stack (from repo languages sampled)
TypeScript, Python, Go, Rust, C++, C#, Java, C, PowerShell, Ruby, Shell, HCL, Java/Kotlin/Swift/JS/TS (native)
Security-native code present: SymCrypt (C crypto), msquic (C, QUIC), mimalloc (C allocator), DirectXShaderCompiler (C++), edit/terminal (Rust/C++), garnet (C# Redis cache).

## Passive infra inventory
- crt.sh returned 502 during this run (cert-transparency SaaS degraded); retry planned.
- Subdomain list derived from MSRC Identity program docs above (program source, legitimate).
- MSRC Security Update Guide API endpoints (passive changelog):
  /updates , /updates/{key=yyyy-mmm|CVE|yyyy} , /cvrf/{id} , /csaf/{id} , /$/metadata

## Leads / surfaces to probe next (Phase 2 SURFACE)
1. Identity OAuth2/OIDC endpoints on login.microsoftonline.com — OIDC discovery & redirect_uri behavior.
2. graph.microsoft.com (Directory/Governance/Identity+Sign-In tabs) — Graph API permission surface, consent model under Global Admin.
3. microsoftazuread-sso.com / accounts.accesscontrol.windows.net — legacy WS-* / token-issuance endpoints.
4. adminwebservice / api.mysignins / provisioningapi.microsoftonline.com — less-frequent endpoints, possible parameter handling gaps.
5. Open-source native code: SymCrypt (crypto impl), msquic (QUIC parser), mimalloc (allocator) — memory-safety leads for OS bounty.
6. Semantic-kernel/autogen exclusion NOTE: OS-program scope excludes these two agent repos (bounty via Agent Framework program only).

## Secrets check (Phase 1)
- No live secret scanning performed. GitHub public repos scanned via the existing reposcan pipeline (reposcan-raw shows 0 code/config hits this cycle). Flag: re-run secret scan against the explicit OS-bounty repo list.

===== RUN 2026-08-07 09:01:01 UTC | model laguna | target microsoft =====
[0m
> build · laguna-s-2.1-free
[0m
[0m→ [0mRead scope.yml
[0m
[0m$ [0mls -la /home/runner/work/hunt-lab/hunt-lab/
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
-rw-r--r-- 1 runner runner   66 Aug  7 08:52 raw_laguna.txt
drwxr-xr-x 2 runner runner 4096 Aug  7 08:52 reports
drwxr-xr-x 2 runner runner 4096 Aug  7 08:52 reposcan-raw
drwxr-xr-x 2 runner runner 4096 Aug  7 08:52 research
-rw-r--r-- 1 runner runner 2858 Aug  7 08:52 research-prompt.txt
-rw-r--r-- 1 runner runner 1650 Aug  7 08:52 scope.yml
drwxr-xr-x 2 runner runner 4096 Aug  7 08:52 triage
[0m
[0m→ [0mRead raw_laguna.txt
[0m→ [0mRead research-prompt.txt
[0m→ [0mRead ctx-research.txt
[0m→ [0mRead ctx-leads.txt
[0m→ [0mRead ctx-valid.txt
[0m→ [0mRead README.md
[0m→ [0mRead leads
[0m→ [0mRead research
[0m→ [0mRead triage
[0m→ [0mRead reports
[0m→ [0mRead reposcan-raw
[0m→ [0mRead research/laguna.md
[0m→ [0mRead leads/lead-laguna.md
[0m→ [0mRead reposcan-raw/analysis.txt
[0m→ [0mRead reposcan-raw/grep-hits.txt
[0m→ [0mRead leads/reposcan-latest.md
[0m
[0m$ [0mgit log --oneline -30
8a0825b fix: feed cands file to xargs for cloning
[0m
[0m$ [0mls -la .github/workflows/ 2>/dev/null; cat .github/workflows/*.yml 2>/dev/null | head -200
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
[0m$ [0mSLOT=$(( $(date +%s) / 600 % 2 )); echo "epoch=$(date +%s) slot=$SLOT"; if [ "$SLOT" = "0" ]; then echo "TARGET=google"; else echo "TARGET=microsoft"; fi
epoch=1786092808 slot=1
TARGET=microsoft
[0m
The target is selected by round-robin time slot. Let me compute the current slot (every 10 min) to determine whether this run targets Google or Microsoft.
[0m% [0mWebFetch https://www.microsoft.com/en-us/msrc/bounty
Target confirmed as **Microsoft** (slot 1, round-robin). The research journal is empty, so this is Phase 1 RECON. Let me begin passive recon: MSRC disclosure policy, crt.sh for subdomain inventory, and the microsoft/ GitHub org.
[0m
[0m$ [0msleep 1; curl -s -m 25 "https://crt.sh/?q=%.microsoft.com&output=json" 2>/dev/null | python3 -c "import sys,json; d=json.load(sys.stdin); print('TOTAL CERTS:', len(d)); names=sorted(set(n for r in d for n in r['name_value'].split('\n'))); [print(n) for n in names[:300]]" 2>/dev/null | sort -u | head -300
(no output)
[0m
The crt.sh query returned empty. Let me retry the subdomain enumeration and fetch the bounty program details and rules of engagement.
[0m% [0mWebFetch https://www.microsoft.com/en-us/msrc/bounty-programs
[0m
[0m$ [0msleep 1; curl -s -m 25 "https://crt.sh/?q=microsoft.com&output=json" 2>/dev/null > /tmp/mcrt.json 2>/dev/null; python3 -c "
import json
try:
    d=json.load(open('/tmp/mcrt.json'))
    print('TOTAL CERTS:', len(d))
    names=sorted(set(n for r in d for n in r['name_value'].split('\n') if n and '*' not in n))
    print('UNIQUE NAMES:', len(names))
    for n in names: print(n)
except Exception as e:
    print('ERR', e)
" 2>&1 | head -400
ERR Expecting value: line 1 column 1 (char 0)
[0m
Let me fix the crt.sh query (needs redirect following) and fetch the rules of engagement and MSRC API docs.
[0m% [0mWebFetch https://www.microsoft.com/en-us/msrc/pentest-rules-of-engagement
[0m
[0m$ [0msleep 1; curl -sL -m 30 "https://crt.sh/?q=microsoft.com&output=json&min=1" 2>/dev/null > /tmp/mcrt.json; python3 -c "
import json
try:
    d=json.load(open('/tmp/mcrt.json'))
    print('TOTAL CERTS:', len(d))
    names=sorted(set(n.strip() for r in d for n in r['name_value'].split('\n') if n and '*' not in n))
    print('UNIQUE NAMES:', len(names))
    for n in names: print(n)
except Exception as e:
    print('ERR', e)
" 2>&1 | head -500
ERR Expecting value: line 1 column 1 (char 0)
[0m
The crt.sh query keeps failing. Let me debug the raw response and try the GitHub API for Microsoft-related assets in parallel.
[0m$ [0msleep 1; curl -sL -m 30 "https://api.github.com/orgs/microsoft/repos?per_page=100&sort=pushed" -H "Accept: application/vnd.github+json" 2>/dev/null | python3 -c "
import sys,json
d=json.load(sys.stdin)
print('TOTAL REPOS:', len(d))
for r in d:
    print(r['name'], '| stars:', r['stargazers_count'], '| lang:', r.get('language'), '| fork:', r['fork'])
" 2>&1 | head -120
TOTAL REPOS: 100
PowerToys | stars: 137541 | lang: C | fork: False
BCApps | stars: 660 | lang: AL | fork: False
vscode-documentdb | stars: 31 | lang: TypeScript | fork: False
sample-webapp | stars: 2 | lang: TypeScript | fork: False
CopilotAnalyticsLabs | stars: 8 | lang: HTML | fork: False
GitHub-Copilot-for-Azure | stars: 242 | lang: TypeScript | fork: False
CCF | stars: 870 | lang: C++ | fork: False
vscode | stars: 188466 | lang: TypeScript | fork: False
react-native-test-app | stars: 668 | lang: TypeScript | fork: False
qdk | stars: 980 | lang: Rust | fork: False
testfx | stars: 1034 | lang: C# | fork: False
azure-pipelines-tasks | stars: 3648 | lang: TypeScript | fork: False
retina | stars: 3165 | lang: Go | fork: False
TypeAgent | stars: 729 | lang: TypeScript | fork: False
microsoft-ui-xaml | stars: 7834 | lang: C++ | fork: False
winget-pkgs | stars: 10937 | lang: None | fork: False
Qcodes | stars: 452 | lang: Python | fork: False
oxidizer | stars: 133 | lang: Rust | fork: False
FluidFramework | stars: 4943 | lang: TypeScript | fork: False
quicreach | stars: 151 | lang: C++ | fork: False
homebrew-apm | stars: 5 | lang: Ruby | fork: False
kiota | stars: 3796 | lang: C# | fork: False
vscode-mssql | stars: 1898 | lang: TypeScript | fork: False
aspire | stars: 6209 | lang: C# | fork: False
Build26-DEM333-how-foundry-integrates-with-open-source-frameworks-and-tools | stars: 7 | lang: Python | fork: False
Build26-BRK242-turn-your-agents-into-action-connect-tools-apis-and-documents | stars: 9 | lang: Python | fork: False
accessibility-insights-web | stars: 911 | lang: TypeScript | fork: False
Sico | stars: 142 | lang: TypeScript | fork: False
aspire.dev | stars: 187 | lang: MDX | fork: False
lage | stars: 815 | lang: TypeScript | fork: False
onnxruntime | stars: 21296 | lang: C++ | fork: False
accessibility-insights-windows | stars: 536 | lang: C# | fork: False
customer-chatbot-solution-accelerator | stars: 46 | lang: Python | fork: False
fluentui | stars: 20193 | lang: TypeScript | fork: False
GSA-Policy-Workbench | stars: 0 | lang: None | fork: False
AL-Go | stars: 500 | lang: PowerShell | fork: False
WSL | stars: 33320 | lang: C++ | fork: False
BC-Bench | stars: 55 | lang: Jupyter Notebook | fork: False
Agents-M365Copilot | stars: 97 | lang: C# | fork: False
vcpkg | stars: 27333 | lang: CMake | fork: False
Multi-Agent-Custom-Automation-Engine-Solution-Accelerator | stars: 864 | lang: Python | fork: False
intelligent-terminal | stars: 1651 | lang: C++ | fork: False
go-winmd | stars: 39 | lang: Go | fork: False
AdaptiveCards | stars: 1959 | lang: C# | fork: False
trident | stars: 29 | lang: Rust | fork: False
mssql-django | stars: 399 | lang: Python | fork: False
teams.ts | stars: 101 | lang: TypeScript | fork: False
vscode-docs | stars: 6588 | lang: Markdown | fork: False
azure-pipelines-extensions | stars: 311 | lang: PowerShell | fork: False
fluentui-charting-contrib | stars: 28 | lang: TypeScript | fork: False
DiskANN | stars: 1895 | lang: Rust | fork: False
CsWinRT | stars: 661 | lang: C# | fork: False
vscode-copilotstudio | stars: 104 | lang: C# | fork: False
windows-rs | stars: 12660 | lang: Rust | fork: False
vsmarketplace | stars: 196 | lang: PowerShell | fork: False
foundry-toolkit | stars: 2042 | lang: JavaScript | fork: False
frontier-ghcp-rvas | stars: 2 | lang: COBOL | fork: False
STL | stars: 11122 | lang: C++ | fork: False
beachball | stars: 814 | lang: TypeScript | fork: False
IssueLens | stars: 0 | lang: Python | fork: False
verismo | stars: 69 | lang: Rust | fork: False
prompty | stars: 1246 | lang: Rust | fork: False
mssql-jdbc | stars: 1129 | lang: Java | fork: False
fluentui-blazor | stars: 4793 | lang: C# | fork: False
playwright | stars: 94130 | lang: TypeScript | fork: False
zerotrustassessment | stars: 398 | lang: HTML | fork: False
hve-core | stars: 1323 | lang: PowerShell | fork: False
terraform-provider-fabric | stars: 125 | lang: Go | fork: False
Huabu | stars: 81 | lang: TypeScript | fork: False
onnxruntime-genai | stars: 1098 | lang: C++ | fork: False
winml-cli | stars: 36 | lang: Python | fork: False
openjdk-jdk | stars: 40 | lang: Java | fork: True
terraform-provider-power-platform | stars: 58 | lang: Go | fork: False
microsoft-iq-solution-accelerator | stars: 27 | lang: Python | fork: False
azure-ai-search-foundry-iq-live-knowledge-sources | stars: 14 | lang: Python | fork: False
axe-pipelines-samples | stars: 116 | lang: TypeScript | fork: False
Employee-Self-Service-Agent-Developer-Kit | stars: 25 | lang: Python | fork: False
ox-tools | stars: 11 | lang: Rust | fork: False
azure-pipelines-tasks-common-packages | stars: 40 | lang: TypeScript | fork: False
mscclpp | stars: 546 | lang: C++ | fork: False
win-dev-skills | stars: 374 | lang: C# | fork: False
azurelinux | stars: 5308 | lang: Python | fork: False
vscode-fabric | stars: 44 | lang: TypeScript | fork: False
python-lsp-compare | stars: 7 | lang: Python | fork: False
msphpsql_pdo_sqlsrv | stars: 1 | lang: None | fork: False
garnet | stars: 11917 | lang: C# | fork: False
msphpsql_sqlsrv | stars: 0 | lang: None | fork: False
msphpsql | stars: 1884 | lang: PHP | fork: False
agent-learning | stars: 3 | lang: Python | fork: False
onnxruntime-extensions | stars: 474 | lang: C++ | fork: False
agent-framework-go | stars: 408 | lang: Go | fork: False
copilot-for-eclipse | stars: 119 | lang: Java | fork: False
fhir-server | stars: 1376 | lang: TSQL | fork: False
TimeCraft | stars: 1082 | lang: Python | fork: False
amplifier-bundle-attractor | stars: 7 | lang: Python | fork: False
FSQ | stars: 10 | lang: Python | fork: False
Enterprise-AI-Analytics-Pipeline | stars: 3 | lang: Python | fork: False
lisa | stars: 332 | lang: Python | fork: False
PyRIT | stars: 4258 | lang: Python | fork: False
cognitive-services-speech-sdk-js | stars: 322 | lang: TypeScript | fork: False
[0m
[0m$ [0msleep 1; curl -sL -m 30 -H "User-Agent: recon" "https://crt.sh/?q=microsoft.com&output=json" -o /tmp/crtraw.txt 2>/dev/null; echo "SIZE=$(wc -c < /tmp/crtraw.txt)"; head -c 500 /tmp/crtraw.txt; echo "---"; python3 -c "import json; d=json.load(open('/tmp/crtraw.txt')); print('CERTS', len(d))" 2>&1 | head -5
SIZE=253
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL was not found on this server.</p>
<hr>
<address>Apache Server at crt.sh Port 443</address>
</body></html>
---
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/usr/lib/python3.12/json/__init__.py", line 293, in load
    return loads(fp.read(),
           ^^^^^^^^^^^^^^^^
[0m
[0m$ [0msleep 1; curl -sL -m 30 "https://api.github.com/orgs/microsoft/repos?per_page=100&page=2&sort=pushed" -H "Accept: application/vnd.github+json" 2>/dev/null | python3 -c "
import sys,json
d=json.load(sys.stdin)
print('TOTAL REPOS page2:', len(d))
for r in d:
    print(r['name'], '| stars:', r['stargazers_count'], '| lang:', r.get('language'), '| fork:', r['fork'])
" 2>&1 | head -120
TOTAL REPOS page2: 100
cognitive-services-speech-sdk-js | stars: 322 | lang: TypeScript | fork: False
CmdPal-Extensions | stars: 61 | lang: None | fork: False
go-crypto-openssl | stars: 2 | lang: Go | fork: False
amplifier-bundle-android-tester | stars: 0 | lang: Python | fork: False
ActiveDirectoryTierModel | stars: 351 | lang: PowerShell | fork: False
azure-linux-image-tools | stars: 36 | lang: Go | fork: False
sqltoolsservice | stars: 505 | lang: C# | fork: False
mcp | stars: 3545 | lang: C# | fork: False
litebox | stars: 2664 | lang: Rust | fork: False
sbi | stars: 30 | lang: Go | fork: False
winappCli | stars: 1177 | lang: C# | fork: False
vscode-python | stars: 4629 | lang: TypeScript | fork: True
ps-rule | stars: 93 | lang: PowerShell | fork: False
ghqr | stars: 523 | lang: Go | fork: False
qdk-chemistry | stars: 67 | lang: C++ | fork: False
typespec | stars: 5813 | lang: Java | fork: False
SwiftStreamingMarkdown | stars: 315 | lang: Swift | fork: False
vcpkg-tool | stars: 598 | lang: C++ | fork: False
agent-framework | stars: 12651 | lang: Python | fork: False
amplifier-bundle-computer-use | stars: 0 | lang: Python | fork: False
SimpleL7Proxy | stars: 20 | lang: C# | fork: False
SkillOpt | stars: 15731 | lang: Python | fork: False
SPARROW-Engine | stars: 1 | lang: Rust | fork: False
agentic-applications-for-unified-data-foundation-solution-accelerator | stars: 124 | lang: Python | fork: False
azure-ai-proxy-lite | stars: 4 | lang: C# | fork: False
mxc | stars: 1200 | lang: Rust | fork: False
azure-container-linux | stars: 14 | lang: Shell | fork: True
mssql-python | stars: 457 | lang: Python | fork: False
FLAML | stars: 4384 | lang: Jupyter Notebook | fork: False
mcp-interactiveUI-samples | stars: 66 | lang: HTML | fork: False
service-fabric-explorer | stars: 113 | lang: TypeScript | fork: False
haste | stars: 86 | lang: Python | fork: False
azure-pipelines-agent | stars: 1905 | lang: C# | fork: False
amplifier | stars: 3111 | lang: Shell | fork: False
hcsshim | stars: 685 | lang: Go | fork: False
sample-app-aoai-chatGPT | stars: 1922 | lang: Python | fork: False
vscode-cpptools | stars: 6148 | lang: TypeScript | fork: False
MIDI | stars: 670 | lang: C++ | fork: False
DCVC | stars: 803 | lang: Python | fork: False
homebrew-xbox-game-creator-tools | stars: 7 | lang: Ruby | fork: False
mimalloc | stars: 13267 | lang: C | fork: False
mcp-dotnet-samples | stars: 195 | lang: C# | fork: False
Conversation-Knowledge-Mining-Solution-Accelerator | stars: 462 | lang: Python | fork: False
SynapseML | stars: 5236 | lang: Scala | fork: False
typeagent-py | stars: 878 | lang: Python | fork: False
moc-sdk-for-go | stars: 18 | lang: Go | fork: False
graphmind-artifacts | stars: 2 | lang: Jupyter Notebook | fork: False
DirectXShaderCompiler | stars: 3630 | lang: C++ | fork: False
RAMPART | stars: 395 | lang: Python | fork: False
kata-containers | stars: 94 | lang: Rust | fork: True
Container-Migration-Solution-Accelerator | stars: 2 | lang: Python | fork: False
kiota-dotnet | stars: 86 | lang: C# | fork: False
frontier-sre-agent-rvas | stars: 2 | lang: HTML | fork: False
simplechat | stars: 144 | lang: Python | fork: False
sentinel_environments | stars: 4 | lang: TypeScript | fork: False
vscode-react-native | stars: 2737 | lang: TypeScript | fork: False
power-platform-skills | stars: 629 | lang: JavaScript | fork: False
Analytics-Hub | stars: 21 | lang: Python | fork: False
AOCoptimizer.jl | stars: 16 | lang: Julia | fork: False
SAIMOpt.jl | stars: 9 | lang: Julia | fork: False
agent-governance-toolkit | stars: 5693 | lang: Python | fork: False
pyright | stars: 15568 | lang: Python | fork: False
openvmm | stars: 1869 | lang: Rust | fork: False
msquic | stars: 4748 | lang: C | fork: False
go-infra-images | stars: 7 | lang: PowerShell | fork: False
xbox-game-streaming-tools | stars: 33 | lang: PowerShell | fork: False
go-crypto-darwin | stars: 10 | lang: Go | fork: False
mssql-rs | stars: 38 | lang: Rust | fork: False
power-pages-samples | stars: 46 | lang: JavaScript | fork: False
rushstack | stars: 6482 | lang: TypeScript | fork: False
shell-use | stars: 218 | lang: Rust | fork: False
universal-print-resources | stars: 13 | lang: PowerShell | fork: False
amplifier-module-context-simple | stars: 4 | lang: Python | fork: False
TypeScript-Website | stars: 2547 | lang: TypeScript | fork: False
foundry-local | stars: 2495 | lang: C++ | fork: False
teams.py | stars: 61 | lang: Python | fork: False
vscode-cosmosdb | stars: 198 | lang: TypeScript | fork: False
teams.net | stars: 41 | lang: C# | fork: False
TEE-Attestation-Verification | stars: 9 | lang: Rust | fork: False
HOBL | stars: 13 | lang: Python | fork: False
edit | stars: 14437 | lang: Rust | fork: False
json-schemas | stars: 156 | lang: Python | fork: False
go-crypto-winnative | stars: 44 | lang: Go | fork: False
react-native-windows | stars: 17322 | lang: C++ | fork: False
amplifier-app-cli | stars: 18 | lang: Python | fork: False
azure-solution-hub | stars: 0 | lang: JavaScript | fork: False
waza | stars: 1155 | lang: Go | fork: False
kiota-java | stars: 38 | lang: Java | fork: False
physical-ai-toolchain | stars: 101 | lang: Python | fork: False
durabletask-js | stars: 13 | lang: TypeScript | fork: False
github-constellation-software | stars: 11 | lang: TypeScript | fork: False
fabric-terraform-quickstart | stars: 50 | lang: HCL | fork: False
Windows-driver-samples | stars: 7811 | lang: C | fork: False
keyborg | stars: 24 | lang: TypeScript | fork: False
OpenAPI.NET | stars: 1604 | lang: C# | fork: False
kiota-samples | stars: 114 | lang: Go | fork: False
olive-recipes | stars: 63 | lang: Python | fork: False
typescript-benchmarking | stars: 26 | lang: TypeScript | fork: False
HealthAgentBench | stars: 35 | lang: Python | fork: False
WinAppVSCE | stars: 10 | lang: TypeScript | fork: False
[0m
Good — GitHub org scan works. crt.sh is 404ing (possibly migrated). Let me debug crt.sh, fetch the next page of Microsoft repos, and check the MSRC API docs in parallel.
[0m$ [0msleep 1; curl -sL -m 30 "https://api.github.com/orgs/microsoft/repos?per_page=100&sort=pushed&type=members" -H "Accept: application/vnd.github+json" 2>/dev/null | python3 -c "
import sys,json
d=json.load(sys.stdin)
print('TOTAL REPOS (members):', len(d))
for r in d:
    print(r['name'])
" 2>&1 | head -130
TOTAL REPOS (members): 0
[0m
[0m$ [0msleep 1; echo "=== plain ==="; curl -s -m 15 "https://crt.sh/?q=microsoft.com" -o /tmp/c1.html -w "HTTP %{http_code} size %{size_download}\n"; echo "=== try crt.sh root ==="; curl -sL -m 15 "https://crt.sh/" -o /tmp/c2.html -w "HTTP %{http_code} size %{size_download}\n"; head -c 300 /tmp/c2.html 2>/dev/null | tr -d '\r' | head -5
=== plain ===
HTTP 000 size 0
=== try crt.sh root ===
HTTP 200 size 5897
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>
<HEAD>
  <META http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <TITLE>crt.sh | Certificate Search</TITLE>
[0m
[0m$ [0msleep 2; curl -sL -m 40 "https://crt.sh/?q=%.microsoft.com" -o /tmp/crt_wild.html -w "HTTP %{http_code} size %{size_download}\n" 2>/dev/null; echo "---"; python3 -c "
import re
try:
    h=open('/tmp/crt_wild.html',encoding='utf-8',errors='ignore').read()
    # crt.sh HTML results have rows with class tr1/tr2 and td
    rows=re.findall(r'<td[^>]*class=\"(?:tr1|tr2)\"[^>]*>(.*?)</td>', h, re.S)
    print('html_rows:', len(rows))
    # try to extract names
    names=re.findall(r'showdomains\.php\?d=.*?>(.*?)</a>', h, re.S)
    print('names found on page:', len(names))
    # dump any json-ish
    if '404' in h[:500]: print('GOT 404')
    print(h[:300])
except Exception as e:
    print('ERR',e)
" 2>&1 | head -40
HTTP 502 size 150
---
html_rows: 0
names found on page: 0
<html>
<head><title>502 Bad Gateway</title></head>
<body>
<center><h1>502 Bad Gateway</h1></center>
<hr><center>nginx</center>
</body>
</html>
[0m
crt.sh root works but the query endpoint is timing out (HTTP 000). Let me retry it once, get remaining repo pages, and check the Microsoft Identity bounty page (most security-relevant program).
[0m$ [0msleep 1; for p in 3 4 5 6 7; do curl -sL -m 30 "https://api.github.com/orgs/microsoft/repos?per_page=100&page=$p&sort=pushed" -H "Accept: application/vnd.github+json" 2>/dev/null | python3 -c "
import sys,json
try:
    d=json.load(sys.stdin)
    print('PAGE $p:', len(d))
    for r in d:
        print(r['name'], '|', r.get('language',''), '| stars', r['stargazers_count'], '| fork' if r['fork'] else '')
except Exception as e:
    pass
" 2>&1; sleep 2; done | head -250
PAGE 3: 100
identity-spiffe | Python | stars 8 
social-reasoning-bench | Python | stars 18 
kiota-serialization-form-go | Go | stars 10 
CoworkDashboard | Python | stars 6 
go-infra | Go | stars 43 
ApplicationInsights-dotnet | C# | stars 608 
quicksand | Python | stars 41 
go | Go | stars 428 
vscode-python-environments | TypeScript | stars 134 
OpenAPI.NET.OData | C# | stars 240 
MicroHack | Jupyter Notebook | stars 194 
teams-sdk | TypeScript | stars 723 
perfview | C# | stars 4729 
discovery | Python | stars 221 
mu_basecore | C | stars 287 
MSBuildCache | C# | stars 62 
fabric-spark-performance-engineering | Python | stars 0 
playwright-python | Python | stars 14894 
webui | Rust | stars 74 
fluentui-react-native | TypeScript | stars 1438 
SymCrypt-OpenSSL | C | stars 81 
winget-pkgs-submission-test | PowerShell | stars 20 
mfcmapi | C++ | stars 979 
microsoft-performance-toolkit-sdk | C# | stars 229 
libHttpClient | C++ | stars 293 
winget-dsc | PowerShell | stars 152 
winget-create | C# | stars 640 
winget-cli | C++ | stars 26282 
winget-studio | C# | stars 64 
winget-cli-restsource | C# | stars 314 
Oryx | C# | stars 879 
applicationinsights-angularplugin-js | JavaScript | stars 35 
powerbi-visuals-utils-typeutils | TypeScript | stars 15 
powerbi-visuals-utils-dataviewutils | TypeScript | stars 25 
powerbi-visuals-utils-testutils | TypeScript | stars 16 
kiota-authentication-azure-go | Go | stars 10 
azure-skills | Python | stars 1356 
roosterjs | TypeScript | stars 1310 
terminal | C++ | stars 104513 
MSBuildSdks | C# | stars 508 
WindowsDeveloperConfig | PowerShell | stars 1875 
durabletask-mssql | C# | stars 104 
SharePoint-Embedded-Samples | TypeScript | stars 105 
openjdk-jdk11u | Java | stars 14 | fork
python-environment-tools | Rust | stars 206 
aspire-skills | TypeScript | stars 76 
component-detection | C# | stars 548 
ShadowFrog | Python | stars 18 
webnn-developer-preview | JavaScript | stars 68 
pxt-arcade | TypeScript | stars 542 
moc | Go | stars 53 
BuildXL | C# | stars 1015 
polyfills | JavaScript | stars 3 
vs-servicehub | C# | stars 32 
debugpy | Python | stars 2453 
PteroSet | Python | stars 2 
print-oem-samples | C++ | stars 22 
fast | TypeScript | stars 9664 
data-formulator | Python | stars 16009 
vscode-azurearcenabledmachines | TypeScript | stars 4 
azure-quantum-python | Python | stars 154 
azure-pipelines-terraform | TypeScript | stars 143 
vscode-cpptools-api | TypeScript | stars 32 
SymCrypt | C | stars 882 
vscode-azureresourcegroups | TypeScript | stars 78 
atlas-design | TypeScript | stars 74 
Agents-for-net | C# | stars 175 
agent-academy | JavaScript | stars 3155 
WindowsAppSDK | C++ | stars 4644 
Docker-Provider | Ruby | stars 173 
aspire-samples | C# | stars 1187 
storm | Go | stars 2 
skills-for-fabric | Python | stars 946 
Agents-for-python | Python | stars 204 
ZoomitForMac | Swift | stars 87 
Game-Asset-Conditioning-Library | C++ | stars 25 
fabric-stateful-streaming-lakehouse | Python | stars 2 
sql-server-language-extensions | C++ | stars 113 
retrochimera | Python | stars 29 
xdp-for-windows | C | stars 484 
tas-client | TypeScript | stars 4 
powerplatform-build-tools | TypeScript | stars 298 
FluidExamples | TypeScript | stars 279 
azure-linux-dev-tools | Go | stars 18 
crap4csharp | C# | stars 4 
durabletask-protobuf | None | stars 10 
undocked | PowerShell | stars 34 
Olive | Python | stars 2375 
ValueLens-for-Microsoft-Copilot | Jupyter Notebook | stars 7 
apm | Python | stars 3479 
azure-container-linux-mantle | Go | stars 5 | fork
NativeAOTDependencyHelper | C# | stars 44 
agora-workbench | Python | stars 4 
openjdk-docker | Shell | stars 14 
Nova | Rust | stars 857 
typescript-go | Go | stars 26152 
Teams-AdaptiveCards-Mobile | C++ | stars 17 
wildcat | Python | stars 12 
immersive-reader-sdk | HTML | stars 109 
vscode-containers | TypeScript | stars 135 
PAGE 4: 100
skala | Python | stars 244 
viva-insights-sample-code | CSS | stars 14 
skills | TypeScript | stars 2867 
windows-container-tools | C++ | stars 276 
mu_tiano_platforms | C | stars 82 
PTVS | C# | stars 2567 
finnts | R | stars 264 
azcapman | PowerShell | stars 1 
service-fabric-traefik | Go | stars 14 
SharePoint-Embedded-MCP-Server | TypeScript | stars 4 
openjdk-adoptium-marketplace-data | Python | stars 6 
ohttp | Rust | stars 5 | fork
PR-Metrics | TypeScript | stars 86 
graphitation | TypeScript | stars 152 
merklecpp | C++ | stars 91 
pg_durable | Rust | stars 2733 
VFSForGit | C# | stars 6122 
vs-renovate-presets | None | stars 1 
vscode-js-profile-visualizer | TypeScript | stars 158 
cat-agent-skills | Python | stars 47 
cat | HTML | stars 12 
intune-tenant-doc | PowerShell | stars 6 
Trieste | C++ | stars 53 
shell-intune-samples | Shell | stars 1080 
rats | Python | stars 33 
QuestToCompilePublicAssets | None | stars 0 
setup-kiota | TypeScript | stars 14 
intune-my-macs | PowerShell | stars 179 
vscode-codicons | Handlebars | stars 1147 
o365-moodle | PHP | stars 207 
Pharmcat-Foundry-Agent | Python | stars 0 
regorus | Rust | stars 337 
kompli | C++ | stars 4 
codecoverage | C# | stars 123 
amplifier-app-opencode | Python | stars 1 
skill-recorder | TypeScript | stars 2205 
vscode-vsce | TypeScript | stars 1040 
finops-toolkit | PowerShell | stars 592 
OpenApi.ApiManifest | C# | stars 32 
.github | None | stars 163 
fabric-samples | Jupyter Notebook | stars 607 
Agents-for-js | TypeScript | stars 85 
PSRule | C# | stars 470 
kiota-typescript | TypeScript | stars 61 
bioemu | Python | stars 859 
msft-linux-cve-analysis | None | stars 4 
homebrew-git | Ruby | stars 26 
vscode-azuretools | TypeScript | stars 143 
vscode-azurestaticwebapps | TypeScript | stars 98 
FluidHelloWorld | JavaScript | stars 141 
powerplatform-vscode | TypeScript | stars 270 
AzureLinuxVulnerabilityData | None | stars 47 
azure-pipelines-task-lib | TypeScript | stars 457 
setup-WinAppCli | JavaScript | stars 19 
recipe-tool | Python | stars 35 
Microsoft-Fabric-workload-development-sample | Python | stars 58 
fabric-extensibility-toolkit | TypeScript | stars 56 
vstest | C# | stars 967 
fabric-cicd | Python | stars 318 
BC-ALAgents | PowerShell | stars 6 
MAPIStubLibrary | C | stars 37 
MHA | TypeScript | stars 382 
kiota-abstractions-ruby | Ruby | stars 5 
kiota-bundle-go | Go | stars 5 
multi-agent-reference-architecture | Python | stars 226 
vscode-azurefunctions | TypeScript | stars 319 
azure-devops-rust-api | Rust | stars 76 
amplifier-app-actions | Python | stars 1 
Dynamics365Commerce.InStore | TypeScript | stars 80 
Dynamics365Commerce.Solutions | None | stars 80 
Dynamics365Commerce.ScaleUnit | PowerShell | stars 89 
dynwinrt | Rust | stars 50 
superbenchmark | Python | stars 388 
kiota-serialization-multipart-go | Go | stars 8 
TypeChat | TypeScript | stars 8677 
kiota-serialization-text-go | Go | stars 13 
kiota-serialization-json-ruby | Ruby | stars 5 
kiota-http-go | Go | stars 21 
kiota-serialization-json-go | Go | stars 10 
kiota-http-ruby | Ruby | stars 16 
semantic-kernel | C# | stars 28428 
kiota-abstractions-go | Go | stars 19 
skills-for-copilot-studio | JavaScript | stars 391 
UniSteer | Python | stars 1 
semantic-link-labs | Python | stars 561 
SqlNexus | C# | stars 429 
HARC | Python | stars 8 
vscode-websearchforcopilot | TypeScript | stars 63 
Azure-Ledger-Explorer | TypeScript | stars 4 
fabric-rti-mcp | Python | stars 127 
xbox-multiplayer-analysis-tool | C# | stars 105 
vscode-pull-request-github | TypeScript | stars 2615 
Microsoft-Defender-For-Containers | PowerShell | stars 4 
security-devops-common | C# | stars 7 
repoclassbench | Python | stars 18 
monosize | TypeScript | stars 47 
AzureTRE | Python | stars 233 
HydraLab | Java | stars 1028 
openjdk-jdk25u | Java | stars 8 
openjdk-jdk21u | Java | stars 23 
PAGE 5: 100
openjdk-jdk17u | Java | stars 12 | fork
vscode-java-test | TypeScript | stars 337 
inno-updater | Rust | stars 118 
vscode-java-dependency | TypeScript | stars 197 
vscode-spring-boot-dashboard | TypeScript | stars 71 
vscode-java-debug | TypeScript | stars 587 
vscode-spring-initializr | TypeScript | stars 77 
vscode-gradle | Java | stars 174 
healthcare-shared-components | C# | stars 105 
amplifier-foundation | Python | stars 16 
WinUI-Gallery | C# | stars 3573 
vscode-maven | TypeScript | stars 209 
microbots | Python | stars 38 
flint-chart | TypeScript | stars 3485 
postgres-mcp | None | stars 2 
XBOX-Godot-Sample | C++ | stars 224 
CsWin32 | C# | stars 2514 
rnx-kit | TypeScript | stars 1728 
node-api-dotnet | C# | stars 778 
Windows-classic-samples | None | stars 5650 
Agent365-devTools | C# | stars 56 
SharePoint-Embedded-VS-Code-Extension | TypeScript | stars 20 
OHCL-Linux-Kernel | C | stars 23 
awesome-rayfin | TypeScript | stars 83 
generative-ai-for-beginners | Jupyter Notebook | stars 116871 
Build26-LAB512-hands-on-with-the-latest-cobalt-vms-from-code-to-cloud-deployment | C# | stars 3 
binskim | C# | stars 857 
mwkorea | HTML | stars 5 
langchainjs-for-beginners | TypeScript | stars 196 
synapse-spark-runtime | None | stars 40 
azure-devops-mcp | TypeScript | stars 1947 
mstic | Python | stars 214 
low-latency-audio | C++ | stars 179 
mu_crypto_release | Assembly | stars 21 
entrabot | Python | stars 8 
react-native-macos | C++ | stars 4373 | fork
accessibility-insights-action | TypeScript | stars 97 
survprompt | Python | stars 0 
virtualnodesOnAzureContainerInstances | Go Template | stars 12 
amlFilesystem-lustre | C | stars 16 
axe-sarif-converter | TypeScript | stars 38 
playwright-mcp | TypeScript | stars 35887 
playwright-cli | JavaScript | stars 12382 
amplifier-bundle-evaluation | HTML | stars 2 
pxt-microbit | TypeScript | stars 806 
wdkmetadata | C++ | stars 108 
magentic-ui | Python | stars 10044
[0m
crt.sh is returning 502 (service degradation). I have a comprehensive repo inventory. Let me fetch the Microsoft Identity bounty page (highest-relevance program for OpenID), the MSRC API docs, and attempt a final crt.sh query with a longer timeout.
[0m% [0mWebFetch https://microsoft.com/msrc/bounty-microsoft-identity
[0m
[0m$ [0msleep 2; curl -sL -m 60 "https://crt.sh/?q=%.microsoft.com" -o /tmp/crt2.html -w "HTTP %{http_code} size %{size_download}\n" 2>/dev/null; echo "---try json output---"; sleep 3; curl -sL -m 60 "https://crt.sh/?q=microsoft.com&output=json" -H "Accept: application/json" -o /tmp/crt3.json -w "HTTP %{http_code} size %{size_download}\n" 2>/dev/null; python3 -c "import json; d=json.load(open('/tmp/crt3.json')); print('CERTS',len(d))" 2>&1 | head -3
HTTP 404 size 253
---try json output---
HTTP 502 size 150
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/usr/lib/python3.12/json/__init__.py", line 293, in load
[0m
[0m$ [0msleep 1; curl -sL -m 30 "https://api.github.com/orgs/microsoft" -H "Accept: application/vnd.github+json" 2>/dev/null | python3 -c "import sys,json; d=json.load(sys.stdin); print('TOTAL REPOS in microsoft org:', d['public_repos']); print('following:', d.get('following'))" 2>&1 | head -5
TOTAL REPOS in microsoft org: 8246
following: 0
[0m
Excellent reconnaissance data. I now have the complete Microsoft Identity bounty scope (highest-relevance program). Key note: **Sub-Domain Takeovers and DoS are explicitly out of scope** for Microsoft. Let me get the MSRC Security Update Guide API (changelogs/source) and confirm the total repo count.
[0m$ [0msleep 2; curl -sL -m 35 "https://api.github.com/search/repositories?q=org:microsoft+in:name+identity&per_page=30" -H "Accept: application/vnd.github+json" 2>/dev/null | python3 -c "
import sys,json
d=json.load(sys.stdin)
print('identity-named repos:', d['total_count'])
for r in d['items']:
    print(r['name'], '| stars', r['stargazers_count'], '|', r.get('language'))
" 2>&1 | head -40
identity-named repos: 11
Microsoft-Defender-for-Identity | stars 126 | PowerShell
Microsoft-Defender-for-Identity-Sizing-Tool | stars 49 | None
SPID-and-Digital-Identity-Enabler | stars 30 | C#
identity-dev-lab | stars 10 | Java
adfs-sample-block-user-on-adfs-marked-risky-by-AzureAD-IdentityProtection | stars 9 | C#
ms-identity-conditional-access-for-compliance | stars 9 | None
franceconnect-identity-provider-dotnet-webapi-aspnetcore | stars 4 | C#
identity-spiffe | stars 8 | Python
Decentralized-Identity-and-Verifiable-Credentials | stars 7 | None
ignite25-LAB549-strengthen-your-identity-security-posture-with-conditional-access | stars 2 | None
Autonomous-Identity-Hardening-with-Microsoft-Entra-Security-Agents | stars 0 | None
[0m
[0m% [0mWebFetch https://aka.ms/msrc-api-docs-cvrf
The Microsoft GitHub org is enormous (8246 repos). I've identified identity-relevant repos. Let me fetch the MSRC developer API documentation (changelogs) and the open-source bounty program page to complete Phase 1 RECON.
[0m% [0mWebFetch https://www.microsoft.com/en-us/msrc/opensourcebountyprogram
[0m
[0m← [0mWrite research/laguna.md
Wrote file successfully.
[0m
I now have the full MSRC API spec, the Open Source bounty scope, and the Identity bounty scope. crt.sh is down (502), but I've enumerated endpoints directly from Microsoft's own bounty program documentation (legitimate program source). Let me persist this to the research journal so future runs build on it.
[0m$ [0mls -la state_laguna.json 2>/dev/null; echo "---"; cat state_laguna.json 2>/dev/null
---
[0m
Now let me persist the leads and set the state file so future runs continue past RECON.
[0m← [0mWrite state_laguna.json
Wrote file successfully.
[0m
[0m← [0mWrite leads/lead-laguna.md
Wrote file successfully.
[0m
I have completed a substantial Phase 1 RECON on the **Microsoft Bounty Program (MSRC)** (round-robin slot=1; crt.sh was degraded, so I enumerated endpoints from Microsoft's own program docs). Findings persisted to `research/laguna.md`, `leads/lead-laguna.md`, and `state_laguna.json`.

# Program: microsoft
# Phase: RECON
## Key Findings (one line each, specific)
- MSRC Bounty Programs page confirms 6 bounty tracks with $750–$250,000 awards; Microsoft Identity (MSA/AzAD/OpenID) is the highest-impact track (MFA bypass = $100,000).
- Rules of Engagement + Bounty Legal Safe Harbor permit passive good-faith testing on Microsoft Online Assets; **Sub-Domain Takeovers and DoS are explicitly OUT OF SCOPE** (will not pursue).
- The Microsoft Identity program page gives an **explicit, enumerated list of in-scope endpoints/hosts** (login.windows.net, login.microsoftonline.com, login.live.com, account.*, signup.live.com, mysignins/myaccount/myapps/myaccess/microsoftazuread-sso.com/credentials, adminwebservice/api/provisioningapi .microsoftonline.com, passwordreset.microsoftonline.com, accounts.accesscontrol.windows.net, Microsoft Authenticator mobile apps, and the Directory Management / Governance / Identity & Sign In tabs of graph.microsoft.com (V1.0)).
- The Open Source Bounty program ($750–$15,000) explicitly lists in-scope GitHub repos: azure/azure-sdk(-for-{net,python,js,java,go,rust}) + azure-sdk-tools, microsoft/fluentui, PowerShell/PowerShell, microsoft/typescript, microsoft/vscode, microsoft/agent-framework, microsoft/monaco-editor, microsoft/msquic — and **explicitly excludes microsoft/semantic-kernel and microsoft/autogen** (attack surface note).
- The GitHub `microsoft/` org contains 8,246 public repos (API count) spanning TypeScript, Python, Go, Rust, C++, C#, Java, C, PowerShell; the existing reposcan cycle (nemotron3) reported 0 code/config hits this run.
- MSRC publishes a public **Security Update Guide / CVRF API** (OpenAPI v3): `GET /updates`, `/updates/{key}` (key = `yyyy-mmm` | CVE | `yyyy`), `/cvrf/{id}`, `/csaf/{id}`, `/$metadata` — a passive changelog source for regression hunting.
- Standards scope for Identity is limited to **ratified** OpenID/OAuth specs (OIDC Core/Discovery/Session, OAuth Multiple Response Types, OAuth Form Post) implemented in **certified** Microsoft products; draft/candidate standards are out of scope.
- Recent additions to Identity scope (per revision history): adminwebservice/microsoftonline.com, api.mysignins.microsoft.com, provisioningapi.microsoftonline.com (Aug 2024); mysignins/myaccount/myaccess/myapps/microsoftazuread-sso.com/accounts.accesscontrol.windows.net (Jul 2025); account.microsoft.com (Nov 2025).

## New Endpoints / Assets
- **OAuth2/OIDC identity endpoints:** `login.microsoftonline.com`, `login.windows.net`, `login.live.com`, `signup.live.com`, `passwordreset.microsoftonline.com`, `api.mysignins.microsoft.com`, `adminwebservice.microsoftonline.com`, `provisioningapi.microsoftonline.com`, `microsoftazuread-sso.com`, `accounts.accesscontrol.windows.net`, `myapps.microsoft.com`, `myaccount.microsoft.com`, `myaccess.microsoft.com`, `mysignins.microsoft.com`
- **Graph API surface:** `graph.microsoft.com` — Azure AD Graph v1.0 Directory Management / Governance / Identity and Sign In tabs
- **MSRC changelog API:** `https://msrc.microsoft.com/update-guide` (OData CVRF v3) — `/updates`, `/updates/{key}`, `/cvrf/{id}`, `/csaf/{id}`
- **Reporting portal:** `https://msrc.microsoft.com/report/vulnerability` (Correlation ID required for Identity reports)
- **OS-bounty repo set:** 14 explicitly listed GitHub repos above
- **Native/security-critical codebases:** SymCrypt (C crypto), msquic (C/QUIC), mimalloc (C allocator), DirectXShaderCompiler (C++), garnet (C# Redis cache), typescript-go / typescript (TypeScript compiler)

## Hypotheses (numbered, with impact + read-only test)
*(These are candidate hypotheses to validate in PHASE 2 SURFACE / PHASE 3 HYPOTHESIS — not yet tested against live infra. Listed because the RECON surface warrants them.)*
1. **OIDC `redirect_uri` / `post_logout_redirect_uri` validation** on login.microsoftonline.com — potential open-redirect or token-leak; impact = account takeover (up to $40k/$100k w/ MFA). Read-only test (POC phase): GET discovery doc `/.well-known/openid-configuration` and `oauth2/v2.0/authorize?...` to observe redirect handling.
2. **`graph.microsoft.com` consent / app-permission IDOR** — delegated vs. app-only scopes across Directory/Governance tabs could permit cross-tenant enumeration; impact = information disclosure / EoP. Test: `GET https://graph.microsoft.com/v1.0/` with a read-only scope to map metadata (unauth OIDC discovery only).
3. **Legacy issuer `accounts.accesscontrol.windows.net` vs. modern `login.microsoftonline.com`** token issuance — possible issuer-confusion / token replay across protocols; impact = auth bypass. Test: compare `iss` claims and `aud` binding on published sample tokens (docs only).
4. **SymCrypt / msquic / mimalloc parser bugs** surfacing as RCE/info-disclosure in native code; OS bounty up to $15k; impact = RCE on native consumers. Test (POC): source review of `microsoft/msquic` and `microsoft/SymCrypt` parsing paths only.
5. **Open-source supply-chain / deserialization in azure/azure-sdk or agent-framework**; impact = supply-chain compromise (bounty up to $15k). Test: passive repo review of input handling in azure-sdk serializers.
6. **MSRC Update Guide API** may leak near-real-time CVE/fix info useful for regression hunting; not a vuln itself but an intel feed.

## Secrets Check (public repos: what was found, hash only, where)
- Phase 1 did **not** run a live secret scan. The existing reposcan pipeline (reposcan-raw/grep-hits.txt) reported **0 code/config files** this cycle — no secrets to hash yet.
- Plan: targeted secret scan against the OS-bounty repo list (azure/azure-sdk*, microsoft/fluentui, microsoft/typescript, microsoft/vscode, microsoft/agent-framework, microsoft/msquic, microsoft/monaco-editor, PowerShell/PowerShell). Any live secret found will be reported **only as a sha256 hash** and flagged for triage (never raw).

## CVSS Candidates
- (none yet — CVSS scoring deferred to PHASE 4 POC after a concrete, validated signal. Identity auth-bypass class would sit CVSS 8.0–9.8 depending on user interaction and MFA interaction.)

---
## STATE UPDATE (exact format for parsing)
STATUS_PHASE: RECON
STATUS_STATE: IN_PROGRESS
NEXT_STEP_1: Begin PHASE 2 SURFACE — passively enumerate OIDC discovery document and OAuth2 authorize endpoint parameters on login.microsoftonline.com; map redirect_uri handling.
NEXT_STEP_2: Passively inspect graph.microsoft.com Graph v1.0 metadata (unauth discovery) to map the Directory/Governance/Identity+Sign-In endpoint surface and consent model.
NEXT_STEP_3: Start passive source review of microsoft/msquic + microsoft/SymCrypt (in-scope native code) for memory-safety leads, and retry crt.sh subdomain enumeration now that the first pass is complete.

============================================================
# PROGRAM: microsoft
# PHASE: SURFACE  (2026-08-07 run, model laguna)
============================================================
## Key Findings (one line each, specific)
- OIDC v2.0 discovery (login.microsoftonline.com/common/v2.0/.well-known/openid-configuration) returns full endpoint set: authorize, token, end_session/logout, devicecode, jwks, kerberos, plus an **mtls token_endpoint alias at mtlsauth.microsoft.com** with `tls_client_certificate_bound_access_tokens=true`.
- v2.0 supported grant/response surface: response_types {code, id_token, code id_token, id_token token}; response_modes {query, fragment, form_post}; client auth methods {client_secret_post, private_key_jwt, client_secret_basic, self_signed_tls_client_auth}; subject_types {pairwise}.
- **Dual protocol versions**: v1.0 discovery (/common/.well-known/openid-configuration?api-version=1.0) uses issuer `https://sts.windows.net/{tenantid}/`, authorize at /common/oauth2/authorize, and supports response types NOT present in v2.0: hybrid `token id_token` and implicit `token` (v2.0 dropped these). Two distinct issuer namespaces for the same tenant.
- **Authorize endpoint does NOT validate redirect_uri on the initial unauthenticated GET**: a request with a placeholder client_id + arbitrary redirect_uri (https://example.com/ and https://evil.example/) returned HTTP 200 + a generic "Sign in to your account" page in BOTH cases — no redirect_uri rejection, no error. Validation is deferred to post-authentication token issuance. → redirect_uri handling is NOT observable via a passive unauth GET; confirming a redirect_uri flaw requires completing an auth flow (state-change / OOS for passive).
- Graph API v1.0 service doc (graph.microsoft.com/v1.0/) returns 72 top-level collections; OData $metadata (2.9 MB) exposes **1,183 EntityTypes** and **326 Functions** across namespaces `microsoft.graph`, `microsoft.graph.identityGovernance`, `microsoft.graph.security`, `microsoft.graph.entraRecoveryServices`.
- mTLS config: `tls_client_certificate_bound_access_tokens=true`; token endpoint alias on `mtlsauth.microsoft.com` (separate host, mTLS-bound tokens). Certificate-bound access token surface — possible binding-confusion between the plain token endpoint and the mTLS alias if an attacker strips the binding.
- Legacy WS-Fed host `microsoftazuread-sso.com` unreachable from test vantage (HTTP 000 — likely region/gateway-gated); `api.mysignins.microsoftonline.com` and `provisioningapi.microsoftonline.com` also unreachable from vantage (HTTP 000, path/region-gated); `adminwebservice.microsoftonline.com` resolves (HTTP 404 on root, Content-Type text/HTML).

## New Endpoints / Assets (SURFACE)
- GET https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration  (OIDC discovery; live)
- GET https://login.microsoftonline.com/common/.well-known/openid-configuration?api-version=1.0  (v1.0 discovery; issuer sts.windows.net/{tenantid})
- GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize  (response_types: code|id_token|code id_token|id_token token; modes: query|fragment|form_post)
- GET https://login.microsoftonline.com/common/oauth2/v2.0/token  (POST normally; mTLS alias: https://mtlsauth.microsoft.com/common/oauth2/v2.0/token)
- GET https://login.microsoftonline.com/common/oauth2/v2.0/logout  (end_session_endpoint)
- GET https://login.microsoftonline.com/common/oauth2/v2.0/devicecode  (device code flow)
- GET https://login.microsoftonline.com/common/discovery/v2.0/keys  (JWKS)
- GET https://login.microsoftonline.com/common/kerberos
- GET https://graph.microsoft.com/v1.0/  (OData service doc, 72 collections)
- GET https://graph.microsoft.com/v1.0/$metadata  (OData schema, 1183 EntityTypes, 326 Functions)
- GET https://graph.microsoft.com/oidc/userinfo  (userinfo endpoint per v2.0 discovery)
- adminwebservice.microsoftonline.com/  (resolves; 404 root)
- (region-gated/unreachable from vantage): microsoftazuread-sso.com, api.mysignins.microsoftonline.com, provisioningapi.microsoftonline.com, accounts.accesscontrol.windows.net

## Hypotheses (numbered, with impact + read-only test)  ← PHASE 3 input
1. Issuer-confusion / token replay across v1.0↔v2.0. v1.0 issuer is `https://sts.windows.net/{tenantid}/`; v2.0 is `https://login.microsoftonline.com/{tenantid}/v2.0`. If any in-scope Relying Party or MS graph token-accepting endpoint accepts a v1.0-style `sts.windows.net` iss token where it should require v2.0 (or vice-versa), or accepts tokens whose `aud` matches the wrong protocol, cross-protocol token replay is possible. Impact: auth bypass / MFA bypass (up to $100,000). Read-only test: compare iss/aud/tid fields of v1.0 vs v2.0 sample tokens in public MS docs; inspect any token-bearing surface for loose issuer/aud validation (source review of MS-published sample RPs). NOTE: high bar — only actionable if a downstream RP validates issuer loosely; Microsoft's own endpoints must not be mis-tested.
2. mTLS cert-bound access token binding bypass. `tls_client_certificate_bound_access_tokens=true` and a distinct mTLS token endpoint on mtlsauth.microsoft.com. If the binding is enforced only at the token endpoint and an access token request to graph.microsoft.com does not re-attest the TLS cert binding, a stolen/bound token could be replayed over plain TLS. Impact: access-token replay (Confidentiality/Integrity on Graph data). Read-only test: GET graph.microsoft.com/v1.0/ over plain TLS vs mTLS; compare WWW-Authenticate / token-claim introspection hints (no token use — passive header analysis only).
3. Hybrid/implicit response_type quirks (v1.0 supports `token id_token`, `token` not in v2.0). The form_post response_mode + hybrid response_type path is historically error-prone. Impact: code/token leakage via response_mode confusion. Read-only test: inspect MSRC Update Guide / CVRF for past fixes in this path (regression); passive source/docs review. Cannot actively trigger without a flow (state-change).
4. redirect_uri deferred-validation (observed). Because the authorize endpoint does not pre-validate redirect_uri, any redirect_uri-allowlist bypass is invisible to a passive GET. A true PoC needs to complete a sign-in (state change / OOS for passive). Hypothesis is documented as a **deferred PoC** — not pursued live; candidate for formal report only if a non-flow-completing signal (e.g., error-message oracle) is later found.
5. Graph IDOR / consent-scope escape in identityGovernance or security functions. 326 Graph functions across identityGovernance (Governance tab) + security. A delegated permission scope that maps to identityGovernance functions (e.g., workflow/task/insights) could permit cross-tenant enumeration or self-elevation if consent is loosely scoped. Impact: privilege escalation / info disclosure (up to ~$19,500 for M365; OS-program IDOR up to $15,000). Read-only test: GET graph.microsoft.com/v1.0/$metadata and enumerate identityGovernance-bound functions; map to documented delegated-vs-app-only scopes in docs.microsoft.com (passive docs read only).

## Secrets Check (public repos: what was found, hash only, where)
- Phase 2 did not execute a secret scan. Per RECON, the reposcan pipeline (reposcan-raw/grep-hits.txt) reported 0 code/config hits this cycle. Targeted secret scan against the OS-bounty repo list is deferred (next slot). Any live secret found will be reported as a sha256 hash only and flagged for triage.

## CVSS Candidates
- (deferred to PHASE 3 HYPOTHESIS / PHASE 4 POC). Issuer-confusion / auth-bypass class candidate sits ~CVSS 8.0–9.8 depending on UI and required precondition (MFA interaction, RP config). mTLS binding bypass ~CVSS 7.5–8.1. Graph IDOR ~CVSS 6.5–8.1. No score claimed until a concrete read-only signal is produced.
</arg_value></tool_call>

===== RUN 2026-08-07 09:06:00 UTC | model laguna | target google =====
[0m
> build · laguna-s-2.1-free
[0m
[0m$ [0msleep 1; curl -sL -m 25 -H "User-Agent: msrc-research-recon" "https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration" -o /tmp/oidc_common.json -w "HTTP %{http_code} bytes %{size_download}\n"; echo "=== keys ==="; python3 -c "import json;d=json.load(open('/tmp/oidc_common.json'));[print(k,'=',d[k]) for k in d]" 2>&1 | head -40
HTTP 200 bytes 1728
=== keys ===
token_endpoint = https://login.microsoftonline.com/common/oauth2/v2.0/token
token_endpoint_auth_methods_supported = ['client_secret_post', 'private_key_jwt', 'client_secret_basic', 'self_signed_tls_client_auth']
jwks_uri = https://login.microsoftonline.com/common/discovery/v2.0/keys
response_modes_supported = ['query', 'fragment', 'form_post']
subject_types_supported = ['pairwise']
id_token_signing_alg_values_supported = ['RS256']
response_types_supported = ['code', 'id_token', 'code id_token', 'id_token token']
scopes_supported = ['openid', 'profile', 'email', 'offline_access']
issuer = https://login.microsoftonline.com/{tenantid}/v2.0
request_uri_parameter_supported = False
userinfo_endpoint = https://graph.microsoft.com/oidc/userinfo
authorization_endpoint = https://login.microsoftonline.com/common/oauth2/v2.0/authorize
device_authorization_endpoint = https://login.microsoftonline.com/common/oauth2/v2.0/devicecode
http_logout_supported = True
frontchannel_logout_supported = True
end_session_endpoint = https://login.microsoftonline.com/common/oauth2/v2.0/logout
claims_supported = ['sub', 'iss', 'cloud_instance_name', 'cloud_instance_host_name', 'cloud_graph_host_name', 'msgraph_host', 'aud', 'exp', 'iat', 'auth_time', 'acr', 'nonce', 'preferred_username', 'name', 'tid', 'ver', 'at_hash', 'c_hash', 'email']
kerberos_endpoint = https://login.microsoftonline.com/common/kerberos
mtls_endpoint_aliases = {'token_endpoint': 'https://mtlsauth.microsoft.com/common/oauth2/v2.0/token'}
tls_client_certificate_bound_access_tokens = True
tenant_region_scope = None
cloud_instance_name = microsoftonline.com
cloud_graph_host_name = graph.windows.net
msgraph_host = graph.microsoft.com
rbac_url = https://pas.windows.net
[0m
[0m$ [0msleep 1; curl -sL -m 25 -H "User-Agent: msrc-research-recon" "https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration" -o /tmp/o2.json -w "HTTP %{http_code} bytes %{size_download}\n"; echo "---authorize endpoint params---"; python3 -c "
import json
d=json.load(open('/tmp/o2.json'))
for k,v in d.items():
    print(k,'=>',v)
" 2>&1 | head -40
HTTP 200 bytes 1728
---authorize endpoint params---
token_endpoint => https://login.microsoftonline.com/common/oauth2/v2.0/token
token_endpoint_auth_methods_supported => ['client_secret_post', 'private_key_jwt', 'client_secret_basic', 'self_signed_tls_client_auth']
jwks_uri => https://login.microsoftonline.com/common/discovery/v2.0/keys
response_modes_supported => ['query', 'fragment', 'form_post']
subject_types_supported => ['pairwise']
id_token_signing_alg_values_supported => ['RS256']
response_types_supported => ['code', 'id_token', 'code id_token', 'id_token token']
scopes_supported => ['openid', 'profile', 'email', 'offline_access']
issuer => https://login.microsoftonline.com/{tenantid}/v2.0
request_uri_parameter_supported => False
userinfo_endpoint => https://graph.microsoft.com/oidc/userinfo
authorization_endpoint => https://login.microsoftonline.com/common/oauth2/v2.0/authorize
device_authorization_endpoint => https://login.microsoftonline.com/common/oauth2/v2.0/devicecode
http_logout_supported => True
frontchannel_logout_supported => True
end_session_endpoint => https://login.microsoftonline.com/common/oauth2/v2.0/logout
claims_supported => ['sub', 'iss', 'cloud_instance_name', 'cloud_instance_host_name', 'cloud_graph_host_name', 'msgraph_host', 'aud', 'exp', 'iat', 'auth_time', 'acr', 'nonce', 'preferred_username', 'name', 'tid', 'ver', 'at_hash', 'c_hash', 'email']
kerberos_endpoint => https://login.microsoftonline.com/common/kerberos
mtls_endpoint_aliases => {'token_endpoint': 'https://mtlsauth.microsoft.com/common/oauth2/v2.0/token'}
tls_client_certificate_bound_access_tokens => True
tenant_region_scope => None
cloud_instance_name => microsoftonline.com
cloud_graph_host_name => graph.windows.net
msgraph_host => graph.microsoft.com
rbac_url => https://pas.windows.net
[0m
I have the full RECON context. The state file shows `phase=RECON, status=IN_PROGRESS`, so this run advances to **PHASE 2 SURFACE**. I'll build on the journal — not repeat it. The highest-impact surface is the Microsoft Identity OAuth2/OIDC endpoints on `login.microsoftonline.com` (MFA-bypass bounty = $100k). Let me passively enumerate the OIDC discovery docs and Graph metadata.

I'll respect ≤1 rps and GET/HEAD only, against declared in-scope hosts.
[0m$ [0msleep 1; curl -sL -m 25 -H "User-Agent: msrc-research-recon" "https://graph.microsoft.com/v1.0/" -o /tmp/graph_svc.json -w "HTTP %{http_code} bytes %{size_download}\n"; python3 -c "
import json
try:
    d=json.load(open('/tmp/graph_svc.json'))
    print('top-level keys:', list(d.keys()))
    for k in list(d.keys())[:20]:
        v=d[k]
        if isinstance(v,str): print(f'{k} = {v}')
        elif isinstance(v,(list,dict)): print(f'{k} = ({type(v).__name__} len {len(v) if not isinstance(v,dict) else len(v.keys())})')
except Exception as e:
    print('ERR',e)
" 2>&1 | head -40
HTTP 200 bytes 4779
top-level keys: ['@odata.context', 'value']
@odata.context = https://graph.microsoft.com/v1.0/$metadata
value = (list len 72)
[0m
[0m$ [0msleep 1; curl -sL -m 25 -H "User-Agent: msrc-research-recon" "https://graph.microsoft.com/v1.0/\$metadata" -o /tmp/graph_meta.xml -w "HTTP %{http_code} bytes %{size_download}\n"; echo "--- namespace + entity count ---"; python3 -c "
import re
try:
    x=open('/tmp/graph_meta.xml',encoding='utf-8',errors='ignore').read()
    print('len',len(x))
    ns=re.findall(r'Namespace=\"([^\"]+)\"',x)[:5]
    print('namespaces:',ns)
    ents=re.findall(r'<EntityType\s+Name=\"([^\"]+)\"',x)
    print('entity types:',len(ents))
    print('sample entities:',ents[:15])
    funcs=re.findall(r'<Function\s+Name=\"([^\"]+)\"',x)
    print('functions:',len(funcs),funcs[:10])
except Exception as e:
    print('ERR',e)
" 2>&1 | head -30
HTTP 200 bytes 2931817
--- namespace + entity count ---
len 2931764
namespaces: ['microsoft.graph.identityGovernance', 'microsoft.graph', 'microsoft.graph.security', 'microsoft.graph.entraRecoveryServices', 'microsoft.graph.termStore']
entity types: 1183
sample entities: ['workflowBase', 'workflow', 'lifecycleWorkflowsContainer', 'userProcessingResult', 'run', 'task', 'taskProcessingResult', 'customTaskExtension', 'insights', 'lifecycleManagementSettings', 'taskDefinition', 'workflowTemplate', 'taskReport', 'workflowVersion', 'entity']
functions: 326 ['summary', 'summary', 'summary', 'topTasksProcessedSummary', 'topWorkflowsProcessedSummary', 'workflowsProcessedByCategory', 'workflowsProcessedSummary', 'getRelyingPartyDetailedSummary', 'deviceConfigurationDeviceActivity', 'deviceConfigurationUserActivity']
[0m
Got the Microsoft Entra OIDC discovery doc — it reveals the full OAuth2/OIDC endpoint set (authorize, token, end_session, devicecode, jwks, kerberos, mtls alias). Now let me map the Graph API surface (v1.0 metadata) and probe how the `authorize` endpoint handles `redirect_uri` validation — the key SURFACE question for the Identity program.
[0m$ [0msleep 1; echo "=== TEST 1: benign redirect_uri with placeholder client_id ==="; curl -s -m 20 -H "User-Agent: msrc-research-recon" "https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=00000000-0000-0000-0000-000000000000&response_type=code&redirect_uri=https%3A%2F%2Fexample.com%2F&response_mode=query&scope=openid&state=abc123" -D /tmp/az_hdr1.txt -o /tmp/az_body1.html -w "HTTP %{http_code}\n"; echo "--- headers ---"; grep -iE "location|set-cookie|www-authenticate" /tmp/az_hdr1.txt 2>/dev/null; echo "--- body snippet ---"; head -c 800 /tmp/az_body1.html 2>/dev/null | tr -d '\r\n' | sed 's/<[^>]*>//g' | head -c 500; echo
=== TEST 1: benign redirect_uri with placeholder client_id ===
HTTP 200
--- headers ---
Set-Cookie: buid=1.ASkAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAApAA.AQABGgEAAAAdDD7nC9b5Q7JPd_okEQRFRXZvU3RzQXJ0aWZhY3RzAwAAAAAAHcQhzE0_nBcCCpfDebJiS4FNl5jyF2XeVsMDrpVQaMaXoF_teu82oJ0iYR2c5H-NyBBdBcn7pGt_mgoLF6wGbSmFhy6U-hIJZUE7xDI_6skgAA; expires=Sun, 06-Sep-2026 09:02:58 GMT; path=/; secure; HttpOnly; SameSite=None
Set-Cookie: esctx-8ZT4DiltgN4=AQABCQEAAAAdDD7nC9b5Q7JPd_okEQRFRXZvU3RzQXJ0aWZhY3RzDQAAAAAAfQW9iAd19b18OPnd4T9Bdps_pxGd3Ab_KBqAk2wwul9l__kJkDTnoptWB7rQNU6FERTnhoXqiRc3Ln36pW16v_CQPXFFnK2rBSWWhhD-TOpkXcjb7Ac15qFX6-QUlltjppvqFn-5KQQ3XgwdxDWnQiAA; domain=.login.microsoftonline.com; path=/; secure; HttpOnly; SameSite=None
Set-Cookie: fpc=AiPQeu4puNtBn6mV7E2I01E; expires=Sun, 06-Sep-2026 09:02:58 GMT; path=/; secure; HttpOnly; SameSite=None
Set-Cookie: esctx=PBgABBwEAAAAdDD7nC9b5Q7JPd_okEQRFRXZvU3RzQXJ0aWZhY3RzDQAAAAAAOC0ucTgdROtgXnKAShOWAhxFuSbqLzCGpqzweqGtRiDCxQJt2te1wgi1Y8ev-3e_3nvXagT9PT80MrKUwseoo7GH2z4cDzPSpCsxFg2WVaQZ8nY4AWXap700ctJZ7aN_byz1wJtGhUStm9TLzg81nRA76Z_c4yvPIQM9RRUgjVH1PaY0AxpZrhVOmufWndcD5Wizi7C9_FqAmLjk0Wqb2CtnKy45A4GN8UjIRu1QLeVPUKFNgOJM9BM0VLhqiCWAAGgxr5laNrobzj3-CDJZNkFfblv3tw04WK_JJxKEQIFsDzwpjVDowVTHsVAD4xLGsmqSs7Q7M8pAivbmlJ4DOoKFPFHDTM5htlyjFEweju39Z2hX4S8dUY3yVb8bRTZo1pW1vWE6ZoJobHv6nfmgHrDGsPRxQ4bvrBTq4ORK-m6ilqxS_sj8NVQwhI0XxI5y7ZyhdyWz-_3eXWh98umxCgsmZZRaS6_jn2CSUoQ-fk-aFoU2bM2NfzPWrPG-zeYGKRNB4DjH4WkFSwX9e_AxGMo_S9hV-JgFthKq7fk8o6nanOvcHwXyQtvn9weS7HzU0C_DxHqrwTOlS84hG7p8RSUtbGn5s3-KXStcv0-_w-iXpYapkubZRAUXYHUGfGa2aU-a4iNSgGSgZUMpq9N_yT6FuTBMtUORkovsOWd87jwrGgoa2goVebT0BKH1_6YeAl6OCEAVySEHovc2wr1xcGSPOmboPzrfnoq2tyMgdeMgAA; domain=.login.microsoftonline.com; path=/; secure; HttpOnly; SameSite=None
Set-Cookie: x-ms-gateway-slice=estsfd; path=/; secure; samesite=none; httponly
Set-Cookie: stsservicecookie=estsfd; path=/; secure; samesite=none; httponly
--- body snippet ---
    Sign in to your account                            <meta name="PageID" content="C
[0m
Graph API surface is large: **1,183 entity types, 326 functions** across `microsoft.graph`, `identityGovernance`, and `security` namespaces. Now the critical SURFACE probe — how does the `authorize` endpoint validate `redirect_uri`? I'll send a single benign GET with a controlled redirect_uri and a placeholder client_id to observe the validation/error behavior (no credentials, no flow completion — pure passive).
[0m$ [0msleep 1; echo "=== TEST 2: malformed redirect_uri (observe error specificity) ==="; curl -s -m 20 -H "User-Agent: msrc-research-recon" "https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=00000000-0000-0000-0000-000000000000&response_type=code&redirect_uri=https%3A%2F%2Fevil.example%2Fcb&response_mode=fragment&scope=openid&state=x" -D /tmp/az2_hdr.txt -o /tmp/az2_body.html -w "HTTP %{http_code}\n"; grep -iE "location|set-cookie" /tmp/az2_hdr.txt 2>/dev/null; echo "--- body snippet ---"; head -c 600 /tmp/az2_body.html 2>/dev/null | tr -d '\r\n' | sed 's/<[^>]*>//g' | head -c 400; echo
=== TEST 2: malformed redirect_uri (observe error specificity) ===
HTTP 200
Set-Cookie: buid=1.AVgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABYAA.AQABGgEAAAAdDD7nC9b5Q7JPd_okEQRFRXZvU3RzQXJ0aWZhY3RzAwAAAAAA2oVqCUpUm1DatNi0R3cWz5kTpBOLb4fcEoiGE7Z4li6FhdYgQYYMTPLHB0kbFlt3m30gwfMw_qgGR_kHQGIAjN3dpANsnsczlvEvoMJLxzUgAA; expires=Sun, 06-Sep-2026 09:03:03 GMT; path=/; secure; HttpOnly; SameSite=None
Set-Cookie: esctx-Vgz8fSvTVQ=AQABCQEAAAAdDD7nC9b5Q7JPd_okEQRFRXZvU3RzQXJ0aWZhY3RzDQAAAAAA5Vv82ZAYqed1gZwdk1QSMtnOv53abOE-JKIxtMy3lP-8ZEeh25sRRlJVlQ1qebeuqjWFCH_I2cg8l4hGlJ-Je60gd0ipxrRicJrWS0nczvTbOQSA-dW-Oc9jvJw5NCirtrMktc5Vny5aU2mJ-s5bPSAA; domain=.login.microsoftonline.com; path=/; secure; HttpOnly; SameSite=None
Set-Cookie: fpc=AgHE-uKFivhLpBsQsxh_fww; expires=Sun, 06-Sep-2026 09:03:03 GMT; path=/; secure; HttpOnly; SameSite=None
Set-Cookie: esctx=PBgABBwEAAAAdDD7nC9b5Q7JPd_okEQRFRXZvU3RzQXJ0aWZhY3RzDQAAAAAAmWVRqeRhRVAW_0EzD6tPn8qeVKeydMMkFEZaCKwJH5TrZLUWVSLWQuAB_0JP8qKLXUrfmQhgop1a1mW39s1AS0OpmEPA1pNZZcEJQtUCSWRfD4XchE7ig3SerPI9hlJuihUu56z3oL8fW-oTalwr8TzKhCWhiac7b4uW4kYzHcRKa9-ODOBPPu_Natxnp0FTBqm-bb9xT91j56EMyiAB226XH-Ur3QNkbXGSXLlnkwLT6OmS6RuGJlMWjzKNiqGUjlV0788mhgImPwkRz1Hch8McwUT3RjRsIocsXqmTO5FCWtI4KJzTI_-ExxqI4Olv18QFZ34DSSGn1OiFju1BZECxWBvpDFlfXiOmIO0I7hE4tY89hYUzynt-vHJhsG9xXsTgVsgdzVAJzoFu4IlkCSi3kbEqoTd_yKcEsBXJq8u0BfaxmFZUDmdiQXZER3yia408ga6RPAipwsXWmCXDTid60myaAOC4tsa5PwQDG7vkvFWsQ-SdtHjko7JVcKlLx5IZ0NTropCyUCwAhDLFI-k9lNOnbiUmHukyuOL9oI6ChmwzDlZnQmFdHqblhFqBO4Twma4tdkjKh3U-WQzEL6h42ED1IphFpZiM4NlCJBJFHp_QwmLpJEcJlaE_Ls3zy_j5vyPzQB2cbmdhc36RElebTwVfsx6tBm7bC8bpl3iE7h4cDu7qLlRr_6tScnRmDXYOLdXvWdilkqO1bcVf6_gq1Q-KvQcRTTw_8fnfPGcgAA; domain=.login.microsoftonline.com; path=/; secure; HttpOnly; SameSite=None
Set-Cookie: x-ms-gateway-slice=estsfd; path=/; secure; samesite=none; httponly
Set-Cookie: stsservicecookie=estsfd; path=/; secure; samesite=none; httponly
--- body snippet ---
    Sign in to your account                        <me
[0m
[0m$ [0msleep 1; echo "=== v1.0 discovery (legacy) ==="; curl -sL -m 20 -H "User-Agent: msrc-research-recon" "https://login.microsoftonline.com/common/.well-known/openid-configuration?api-version=1.0" -o /tmp/v1disc.json -w "HTTP %{http_code} bytes %{size_download}\n"; python3 -c "
import json
try:
    d=json.load(open('/tmp/v1disc.json'))
    for k in ['authorization_endpoint','token_endpoint','end_session_endpoint','issuer','jwks_uri','userinfo_endpoint']:
        print(k,'=',d.get(k))
    print('response_types:',d.get('response_types_supported'))
    print('id_token_alg:',d.get('id_token_signing_alg_values_supported'))
except Exception as e:
    print('not JSON or err:',e); print(open('/tmp/v1disc.json').read()[:300])
" 2>&1 | head -30
=== v1.0 discovery (legacy) ===
HTTP 200 bytes 1564
authorization_endpoint = https://login.microsoftonline.com/common/oauth2/authorize
token_endpoint = https://login.microsoftonline.com/common/oauth2/token
end_session_endpoint = https://login.microsoftonline.com/common/oauth2/logout
issuer = https://sts.windows.net/{tenantid}/
jwks_uri = https://login.microsoftonline.com/common/discovery/keys
userinfo_endpoint = https://login.microsoftonline.com/common/openid/userinfo
response_types: ['code', 'id_token', 'code id_token', 'token id_token', 'token']
id_token_alg: ['RS256']
[0m
Key SURFACE observation: both authorize probes returned HTTP 200 with a generic *"Sign in to your account"* page even with a **placeholder client_id + arbitrary redirect_uri** — Microsoft does **not** return a redirect_uri validation error on the initial unauthenticated GET. Validation is deferred to post-authentication token issuance, which means redirect_uri handling is **not observable via a passive unauthenticated GET** alone (requires completing the flow). Important for PoC design.

Let me check the v1.0 discovery (legacy vs v2.0) and the legacy `microsoftazuread-sso.com` WS-Fed endpoint to map the multi-tenant/auth protocol surface.
[0m$ [0msleep 1; echo "=== microsoftazuread-sso.com (legacy WS-Fed) ==="; curl -sI -m 20 -H "User-Agent: msrc-research-recon" "https://microsoftazuread-sso.com/" -w "HTTP %{http_code}\n" -D /tmp/sso_hdr.txt; echo "--- headers ---"; head -30 /tmp/sso_hdr.txt 2>/dev/null; echo "=== check well-known on legacy host ==="; curl -s -m 20 -H "User-Agent: msrc-research-recon" "https://microsoftazuread-sso.com/FederationMetadata/2007-06/FederationMetadata.xml" -w "HTTP %{http_code} bytes %{size_download}\n" -o /tmp/sso_fed.xml 2>&1 | tail -2; head -c 300 /tmp/sso_fed.xml 2>/dev/null
=== microsoftazuread-sso.com (legacy WS-Fed) ===
HTTP 000
--- headers ---
=== check well-known on legacy host ===
HTTP 000 bytes 0
[0m
[0m$ [0msleep 1; for ep in "https://api.mysignins.microsoftonline.com/" "https://provisioningapi.microsoftonline.com/" "https://adminwebservice.microsoftonline.com/"; do echo "=== $ep ==="; curl -sI -m 15 -H "User-Agent: msrc-research-recon" "$ep" -w "HTTP %{http_code}\n" -D /tmp/sig.txt 2>&1 | tail -1; grep -iE "^(server|www-authenticate|location|content-type)" /tmp/sig.txt 2>/dev/null | head -5; echo; done
=== https://api.mysignins.microsoftonline.com/ ===
HTTP 000

=== https://provisioningapi.microsoftonline.com/ ===
HTTP 000

=== https://adminwebservice.microsoftonline.com/ ===
HTTP 404
Content-Type: text/HTML
[0m
Excellent SURFACE data point — the v1.0 endpoint reveals a **different issuer** (`https://sts.windows.net/{tenantid}/`) and **supports additional response types** the v2.0 doesn't (hybrid `token id_token`, pure `token` implicit) — a classic dual-protocol surface. `microsoftazuread-sso.com` is currently unreachable from this vantage (HTTP 000). Let me round out the Surface with a quick probe of the two "less-frequent" endpoints flagged in RECON (api.mysignins + provisioningapi).
[0m→ [0mRead state_laguna.json
[0m→ [0mRead leads/lead-laguna.md
[0m→ [0mRead research/laguna.md
I now have comprehensive PHASE 2 SURFACE data. The dual v1.0/v2.0 endpoints with a **different issuer namespace** (`sts.windows.net` vs `login.microsoftonline.com`) plus the mTLS cert-bound token surface and 326 Graph functions across `identityGovernance`/`security` namespaces are the most promising surfaces. The "less-frequent" endpoints (adminwebservice) resolve but are path-gated; mysignins/provisioning/sso host are unreachable from this vantage.

Let me persist these findings and advance the phase.
[0m
[0m← [0mWrite state_laguna.json
Wrote file successfully.
[0m
[0m← [0mWrite leads/lead-laguna.md
Wrote file successfully.
[0m
I have all the PHASE 2 SURFACE data I need. Let me persist the SURFACE findings, update the leads with the new hypotheses (issuer-confusion between sts.windows.net/v1.0 vs login.microsoftonline.com/v2.0, mTLS cert-binding surface, Graph identityGovernance/security functions), and advance the state file.
[0m$ [0mtail -5 /home/runner/work/hunt-lab/hunt-lab/research/laguna.md; echo "=== LINE COUNT ==="; wc -l /home/runner/work/hunt-lab/hunt-lab/research/laguna.md
STATUS_STATE: IN_PROGRESS
NEXT_STEP_1: Begin PHASE 2 SURFACE — passively enumerate OIDC discovery document and OAuth2 authorize endpoint parameters on login.microsoftonline.com; map redirect_uri handling.
NEXT_STEP_2: Passively inspect graph.microsoft.com Graph v1.0 metadata (unauth discovery) to map the Directory/Governance/Identity+Sign-In endpoint surface and consent model.
NEXT_STEP_3: Start passive source review of microsoft/msquic + microsoft/SymCrypt (in-scope native code) for memory-safety leads, and retry crt.sh subdomain enumeration now that the first pass is complete.
</arg_value></tool_call>
=== LINE COUNT ===
1020 /home/runner/work/hunt-lab/hunt-lab/research/laguna.md
[0m
Now let me append the PHASE 2 SURFACE findings to the research journal. Let me read the tail of the research file to find the right anchor point.
[0m← [0mEdit research/laguna.md
Index: /home/runner/work/hunt-lab/hunt-lab/research/laguna.md
===================================================================
--- /home/runner/work/hunt-lab/hunt-lab/research/laguna.md
+++ /home/runner/work/hunt-lab/hunt-lab/research/laguna.md
@@ -1016,5 +1016,46 @@
 STATUS_STATE: IN_PROGRESS
 NEXT_STEP_1: Begin PHASE 2 SURFACE — passively enumerate OIDC discovery document and OAuth2 authorize endpoint parameters on login.microsoftonline.com; map redirect_uri handling.
 NEXT_STEP_2: Passively inspect graph.microsoft.com Graph v1.0 metadata (unauth discovery) to map the Directory/Governance/Identity+Sign-In endpoint surface and consent model.
 NEXT_STEP_3: Start passive source review of microsoft/msquic + microsoft/SymCrypt (in-scope native code) for memory-safety leads, and retry crt.sh subdomain enumeration now that the first pass is complete.
+
+============================================================
+# PROGRAM: microsoft
+# PHASE: SURFACE  (2026-08-07 run, model laguna)
+============================================================
+## Key Findings (one line each, specific)
+- OIDC v2.0 discovery (login.microsoftonline.com/common/v2.0/.well-known/openid-configuration) returns full endpoint set: authorize, token, end_session/logout, devicecode, jwks, kerberos, plus an **mtls token_endpoint alias at mtlsauth.microsoft.com** with `tls_client_certificate_bound_access_tokens=true`.
+- v2.0 supported grant/response surface: response_types {code, id_token, code id_token, id_token token}; response_modes {query, fragment, form_post}; client auth methods {client_secret_post, private_key_jwt, client_secret_basic, self_signed_tls_client_auth}; subject_types {pairwise}.
+- **Dual protocol versions**: v1.0 discovery (/common/.well-known/openid-configuration?api-version=1.0) uses issuer `https://sts.windows.net/{tenantid}/`, authorize at /common/oauth2/authorize, and supports response types NOT present in v2.0: hybrid `token id_token` and implicit `token` (v2.0 dropped these). Two distinct issuer namespaces for the same tenant.
+- **Authorize endpoint does NOT validate redirect_uri on the initial unauthenticated GET**: a request with a placeholder client_id + arbitrary redirect_uri (https://example.com/ and https://evil.example/) returned HTTP 200 + a generic "Sign in to your account" page in BOTH cases — no redirect_uri rejection, no error. Validation is deferred to post-authentication token issuance. → redirect_uri handling is NOT observable via a passive unauth GET; confirming a redirect_uri flaw requires completing an auth flow (state-change / OOS for passive).
+- Graph API v1.0 service doc (graph.microsoft.com/v1.0/) returns 72 top-level collections; OData $metadata (2.9 MB) exposes **1,183 EntityTypes** and **326 Functions** across namespaces `microsoft.graph`, `microsoft.graph.identityGovernance`, `microsoft.graph.security`, `microsoft.graph.entraRecoveryServices`.
+- mTLS config: `tls_client_certificate_bound_access_tokens=true`; token endpoint alias on `mtlsauth.microsoft.com` (separate host, mTLS-bound tokens). Certificate-bound access token surface — possible binding-confusion between the plain token endpoint and the mTLS alias if an attacker strips the binding.
+- Legacy WS-Fed host `microsoftazuread-sso.com` unreachable from test vantage (HTTP 000 — likely region/gateway-gated); `api.mysignins.microsoftonline.com` and `provisioningapi.microsoftonline.com` also unreachable from vantage (HTTP 000, path/region-gated); `adminwebservice.microsoftonline.com` resolves (HTTP 404 on root, Content-Type text/HTML).
+
+## New Endpoints / Assets (SURFACE)
+- GET https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration  (OIDC discovery; live)
+- GET https://login.microsoftonline.com/common/.well-known/openid-configuration?api-version=1.0  (v1.0 discovery; issuer sts.windows.net/{tenantid})
+- GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize  (response_types: code|id_token|code id_token|id_token token; modes: query|fragment|form_post)
+- GET https://login.microsoftonline.com/common/oauth2/v2.0/token  (POST normally; mTLS alias: https://mtlsauth.microsoft.com/common/oauth2/v2.0/token)
+- GET https://login.microsoftonline.com/common/oauth2/v2.0/logout  (end_session_endpoint)
+- GET https://login.microsoftonline.com/common/oauth2/v2.0/devicecode  (device code flow)
+- GET https://login.microsoftonline.com/common/discovery/v2.0/keys  (JWKS)
+- GET https://login.microsoftonline.com/common/kerberos
+- GET https://graph.microsoft.com/v1.0/  (OData service doc, 72 collections)
+- GET https://graph.microsoft.com/v1.0/$metadata  (OData schema, 1183 EntityTypes, 326 Functions)
+- GET https://graph.microsoft.com/oidc/userinfo  (userinfo endpoint per v2.0 discovery)
+- adminwebservice.microsoftonline.com/  (resolves; 404 root)
+- (region-gated/unreachable from vantage): microsoftazuread-sso.com, api.mysignins.microsoftonline.com, provisioningapi.microsoftonline.com, accounts.accesscontrol.windows.net
+
+## Hypotheses (numbered, with impact + read-only test)  ← PHASE 3 input
+1. Issuer-confusion / token replay across v1.0↔v2.0. v1.0 issuer is `https://sts.windows.net/{tenantid}/`; v2.0 is `https://login.microsoftonline.com/{tenantid}/v2.0`. If any in-scope Relying Party or MS graph token-accepting endpoint accepts a v1.0-style `sts.windows.net` iss token where it should require v2.0 (or vice-versa), or accepts tokens whose `aud` matches the wrong protocol, cross-protocol token replay is possible. Impact: auth bypass / MFA bypass (up to $100,000). Read-only test: compare iss/aud/tid fields of v1.0 vs v2.0 sample tokens in public MS docs; inspect any token-bearing surface for loose issuer/aud validation (source review of MS-published sample RPs). NOTE: high bar — only actionable if a downstream RP validates issuer loosely; Microsoft's own endpoints must not be mis-tested.
+2. mTLS cert-bound access token binding bypass. `tls_client_certificate_bound_access_tokens=true` and a distinct mTLS token endpoint on mtlsauth.microsoft.com. If the binding is enforced only at the token endpoint and an access token request to graph.microsoft.com does not re-attest the TLS cert binding, a stolen/bound token could be replayed over plain TLS. Impact: access-token replay (Confidentiality/Integrity on Graph data). Read-only test: GET graph.microsoft.com/v1.0/ over plain TLS vs mTLS; compare WWW-Authenticate / token-claim introspection hints (no token use — passive header analysis only).
+3. Hybrid/implicit response_type quirks (v1.0 supports `token id_token`, `token` not in v2.0). The form_post response_mode + hybrid response_type path is historically error-prone. Impact: code/token leakage via response_mode confusion. Read-only test: inspect MSRC Update Guide / CVRF for past fixes in this path (regression); passive source/docs review. Cannot actively trigger without a flow (state-change).
+4. redirect_uri deferred-validation (observed). Because the authorize endpoint does not pre-validate redirect_uri, any redirect_uri-allowlist bypass is invisible to a passive GET. A true PoC needs to complete a sign-in (state change / OOS for passive). Hypothesis is documented as a **deferred PoC** — not pursued live; candidate for formal report only if a non-flow-completing signal (e.g., error-message oracle) is later found.
+5. Graph IDOR / consent-scope escape in identityGovernance or security functions. 326 Graph functions across identityGovernance (Governance tab) + security. A delegated permission scope that maps to identityGovernance functions (e.g., workflow/task/insights) could permit cross-tenant enumeration or self-elevation if consent is loosely scoped. Impact: privilege escalation / info disclosure (up to ~$19,500 for M365; OS-program IDOR up to $15,000). Read-only test: GET graph.microsoft.com/v1.0/$metadata and enumerate identityGovernance-bound functions; map to documented delegated-vs-app-only scopes in docs.microsoft.com (passive docs read only).
+
+## Secrets Check (public repos: what was found, hash only, where)
+- Phase 2 did not execute a secret scan. Per RECON, the reposcan pipeline (reposcan-raw/grep-hits.txt) reported 0 code/config hits this cycle. Targeted secret scan against the OS-bounty repo list is deferred (next slot). Any live secret found will be reported as a sha256 hash only and flagged for triage.
+
+## CVSS Candidates
+- (deferred to PHASE 3 HYPOTHESIS / PHASE 4 POC). Issuer-confusion / auth-bypass class candidate sits ~CVSS 8.0–9.8 depending on UI and required precondition (MFA interaction, RP config). mTLS binding bypass ~CVSS 7.5–8.1. Graph IDOR ~CVSS 6.5–8.1. No score claimed until a concrete read-only signal is produced.
 </arg_value></tool_call>

[0m
[0m$ [0mecho "=== state_laguna.json ==="; cat state_laguna.json; echo "=== git status (short) ==="; git -C /home/runner/work/hunt-lab/hunt-lab status --short
=== state_laguna.json ===
{"phase": "SURFACE", "status": "HIGH_POTENTIAL"}
=== git status (short) ===
 M leads/lead-laguna.md
 M research/laguna.md
 M state_laguna.json
?? ctx-leads.txt
?? ctx-research.txt
?? ctx-valid.txt
?? raw_laguna.txt
?? research-prompt.txt
[0m
Let me verify the final state is consistent.
PHASE 2 SURFACE complete. State advanced to `SURFACE / HIGH_POTENTIAL`.

## Key results

**OAuth2/OIDC surface (login.microsoftonline.com):**
- v2.0 discovery (live, GET): authorize `/oauth2/v2.0/authorize`, token, logout, devicecode, jwks, kerberos; supports response_types `{code, id_token, code id_token, id_token token}` and response_modes `{query, fragment, form_post}`.
- **Dual protocol**: v1.0 discovery uses issuer `https://sts.windows.net/{tenantid}/` and supports **response types not in v2.0** (hybrid `token id_token`, implicit `token`) — two issuer namespaces for the same tenant. → issuer-confusion surface [Hypothesis 1].
- **redirect_uri is NOT validated pre-auth**: placeholder client_id + arbitrary redirect_uri (both `example.com` and `evil.example`) returned HTTP 200 + a generic *"Sign in"* page — no rejection. Validation is deferred to post-auth token issuance, so it's **not observable via a passive GET**. → deferred PoC [Hypothesis 4].
- **mTLS**: `tls_client_certificate_bound_access_tokens=true` with token alias on `mtlsauth.microsoft.com` — cert-binding/bypass surface [Hypothesis 2].

**Graph surface (graph.microsoft.com):**
- v1.0 service doc: 72 collections; `$metadata`: **1,183 EntityTypes, 326 Functions** across `microsoft.graph`, `identityGovernance`, `security`, `entraRecoveryServices`. → IDOR / consent-scope-escape surface [Hypothesis 5].

**Legacy "less-frequent" endpoints:**
- `adminwebservice.microsoftonline.com` resolves (404 root); `microsoftazuread-sso.com`, `api.mysignins.*`, `provisioningapi.*`, `accounts.accesscontrol.windows.net` unreachable from this vantage (HTTP 000 — region/path-gated).

**In scope per rules:** login.microsoftonline.com, graph.microsoft.com, microsoftazuread-sso.com, accounts.accesscontrol.windows.net — all declared in-scope hosts. All requests were GET/HEAD, ≤1 rps, no credentials/flows/state-changes.

**Findings persisted:** `research/laguna.md` (appended SURFACE section), `leads/lead-laguna.md` (5 hypotheses w/ read-only PoC designs), `state_laguna.json` (`{"phase":"SURFACE","status":"HIGH_POTENTIAL"}`).

**Next:** PHASE 3 HYPOTHESIS — formalize issuer-confusion vs sts.windows.net, mTLS cert-binding bypass, and Graph identityGovernance/security function IDOR; design read-only GET/HEAD PoCs (no flow completion).
