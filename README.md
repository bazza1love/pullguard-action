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

That's it. 12 analyzers run on every PR — no license key, no configuration, no account needed.

## Unlock Pro / Enterprise

Add your license key as a repo secret to unlock all 27 analyzers:

```yaml
      - uses: pullguard-dev/pullguard-action@v1
        with:
          license-key: ${{ secrets.PULLGUARD_LICENSE_KEY }}
          fail-on-severity: critical
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `license-key` | Unlocks Pro (27 analyzers) or Enterprise (27 + SOC 2, custom rules) | Empty (free tier) |
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

### Free (12 analyzers)
Complexity, dead code, duplication, god files, naming, nesting, imports, module systems, file structure, error handling, test patterns, API consistency

### Pro — $29/repo/mo (27 analyzers)
Everything free + SQL injection, XSS, SSRF, command injection, path traversal, hardcoded secrets, insecure crypto, prototype pollution, ReDoS, dependency CVEs, type coverage, architecture violations, API contract, AI code detection, cost estimation, CORS, CSP, NoSQL injection

### Enterprise — $99/repo/mo
Everything Pro + SOC 2 compliance, custom rules, knowledge silo detection, breaking change detection, compliance trends, dependency freshness

## Privacy

Your code never leaves your GitHub runners. PullGuard runs as a Docker container in your CI — zero data transmitted to external servers. See [Privacy Policy](https://www.pullguard.dev/privacy.html).

## Links

[Website](https://www.pullguard.dev) · [Terms](https://www.pullguard.dev/terms.html) · [Privacy](https://www.pullguard.dev/privacy.html) · hello@pullguard.dev
