# PullGuard GitHub Action

Code quality, security & compliance on every pull request.

## Quick Start

```yaml
# .github/workflows/pullguard.yml
name: PullGuard
on: [pull_request]
permissions:
  contents: read
  pull-requests: write
jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: pullguard-dev/pullguard-action@v1
```

That's it. 14 analyzers run on every PR — no license key, no configuration, no account needed.

## Unlock Pro / Enterprise

Add your license key as a repo secret to unlock the Pro / Enterprise analyzer set (Pro: 31 of 32; Enterprise: all 32 incl. custom-rules):

```yaml
      - uses: pullguard-dev/pullguard-action@v1
        with:
          license-key: ${{ secrets.PULLGUARD_LICENSE_KEY }}
          fail-on-severity: critical
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `license-key` | Unlocks Pro (31 of 32 analyzers; all except custom-rules) or Enterprise (all 32 incl. custom-rules + SOC 2) | Empty (free tier, 14 analyzers) |
| `fail-on-severity` | Fail workflow at this severity: `info`, `minor`, `moderate`, `major`, `critical` | Empty (never fail) |
| `hourly-rate` | Developer hourly rate for cost estimation | `150` |
| `config` | Path to `.driftrc.yml` config file | Auto-detected |
| `path` | Path to scan | `.` (repo root) |

## Outputs

| Output | Description |
|--------|-------------|
| `score` | Drift score (0-100, lower is better) |
| `grade` | Drift grade (A-F) |
| `findings` | Total finding count |

## What's Included

### Free (14 analyzers)
Core code quality + supply-chain hygiene: complexity, dead code, duplication, monolithic files, naming, nesting, imports, module systems, file structure, error handling, test patterns, API consistency, **dangerous tracked files** (.env, id_rsa, etc.), **repo hygiene** (SECURITY.md, CODEOWNERS, LICENSE, CI workflow presence).

### Pro — $29/repo/mo (31 of 32 analyzers; custom-rules is Enterprise-only)
Everything free + SQL injection, XSS, SSRF, command injection, path traversal, hardcoded secrets, insecure crypto, prototype pollution, ReDoS, dependency CVEs, type coverage, architecture violations, API contract, AI code detection, cost estimation, CORS, CSP, NoSQL injection, **Dockerfile IaC** (runs-as-root, :latest, ADD-from-URL, embedded secrets), **git-history secret scan**, **GitHub Actions security** (pwn-request, script injection, unpinned actions, excessive permissions).

### Enterprise — $99/repo/mo
Everything Pro + SOC 2 security evidence (CC3.1, CC3.2, CC6.1, CC6.8), custom rules, knowledge silo detection, breaking change detection, compliance trends, dependency freshness.

## Contact

hello@pullguard.dev | [pullguard.dev](https://www.pullguard.dev)
