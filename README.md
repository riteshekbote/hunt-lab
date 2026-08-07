# hunt-lab

24/7 multi-model bug-hunting automation for the **Google VRP** and **Microsoft Bounty (MSRC)** programs.

- 5 opencode models (Big Pickle, Nemotron 3 Ultra, Longcat, Ling 3.0, Laguna) hunt in parallel every **10 minutes**, rotating targets (google / microsoft)
- Deep public-repo scan of the `google/` and `microsoft/` GitHub orgs every **30 minutes**
- Triager job validates every lead with a second model + live passive probe, keeping a running count in `reports/valid-bugs.md`
- All testing is **passive and read-only** (GET/HEAD, ≤1 rps, no account creation, no data modification) per program rules
- Secrets are committed only as hashes; findings must be reported through the program channels listed in `scope.yml`

| Artifact | Purpose |
|---|---|
| `research/` | Per-model research journals |
| `leads/` | Candidate findings (UNVALIDATED) |
| `triage/` | Validation verdicts |
| `reports/valid-bugs.md` | Validated findings + running count |
| `scope.yml` | Program scope and rules (edit to adjust) |
