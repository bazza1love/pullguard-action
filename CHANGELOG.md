# PullGuard changelog

All notable customer-visible changes. Earlier releases predate this
changelog; this file is the canonical record going forward.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Live release notes for the hosted scanner: [pullguard.dev](https://www.pullguard.dev).

## [Unreleased]

_Customer-visible changes already live on `:latest` but not yet bundled into a cut image tag. Pin a specific release below for change-controlled, reproducible scans._

---

## [1.0.0] — 2026-05-20 (Spring 2026 release)

PullGuard's first pinnable release. 43 analyzers across security, code
quality, compliance, and supply-chain; five compliance frameworks (SOC 2,
HIPAA, PCI DSS 4.0, NIST 800-53 Rev 5, ISO 27001:2022); endpoint-risk
prioritisation; and actionable-vs-observational cost reporting. Pin it with
`image-pin: v1.0.0`.

### Added

- **11 new security analyzers** — coverage now includes CSRF protection,
  cookie security flags, JWT confusion attacks, HTTP security headers,
  server-side template injection, unsafe reflection, file-upload
  validation, cryptographic hygiene, generic injection sinks, Kubernetes
  IaC security (CIS Level 1), and missing-authentication detection on
  framework routes (Express / NestJS / Spring / Django / Flask / Rails).

- **Endpoint Risk Engine** — composes per-endpoint authentication state,
  data-flow taint, and call-graph reachability into a unified risk
  tier. An unauthenticated endpoint reachable from a tainted source
  becomes a **critical** finding rather than three disconnected
  medium-severity findings, giving security teams a clearer
  prioritisation signal.

- **Compliance framework expansion** — added HIPAA Technical Safeguards
  (45 CFR §164.312), PCI DSS 4.0, NIST 800-53 Rev 5, and ISO 27001:2022
  evidence mappings alongside the existing SOC 2 coverage. SOC 2 itself
  expanded from 4 to 8 in-scope controls (added CC4.1, CC6.2, CC6.7,
  CC8.2).

- **`.pullguardignore` suppression file** with PR-comment workflow —
  comment `/pullguard ignore <rule-id>` on a pull request and PullGuard
  opens a suggested edit to your `.pullguardignore`. Security-category
  analyzers cannot be suppressed by this workflow.

- **Cost partition (actionable vs observational)** — fix-cost is now
  reported as **actionable** (Moderate severity and above) versus
  **observations** (Info / Minor). Headlines reflect what an
  engineering leader would actually sign off on fixing rather than the
  inflated all-findings total.

- **`image-pin` action input** — pin the scanner to an immutable release
  tag (`image-pin: v1.0.0`) or content digest (`image-pin: sha256:…`)
  instead of the rolling `:latest`. Recommended for enterprise
  change-control and reproducible builds. Defaults to `latest`, so
  existing workflows keep their current behaviour with no change required.

### Changed

- **Tier analyzer counts**: Free **14** (unchanged), Pro **31 → 42**,
  Team & Enterprise **32 → 43**. License keys auto-pick up the new
  analyzers; no customer action required.

- **PR comment + Step Summary headers** — display
  `N actionable findings (+M observations) · $X actionable` instead of
  the previous all-findings aggregate. Each section in the PR comment
  is now collapsible; full finding descriptions render inline rather
  than truncating at 80 characters.

- **Per-language file-size thresholds** — Java tolerates 1000-line
  files (idiomatic for content-management codebases), Go 400,
  TypeScript 600, etc., replacing a single global threshold that
  unfairly penalised verbose-by-convention languages.

- **Honest line counts** — file-size and function-length checks no
  longer count comment-only or blank lines. Heavily-documented code
  is no longer flagged for documentation density. Aligned with the
  industry-standard NCLOC (Non-Commented Lines of Code) metric.

- **GitHub Actions Marketplace listing** — analyzer count, pricing
  tier descriptions, and capability claims updated across all
  customer-facing surfaces.

### Fixed

- **CI reliability** on macOS runners for cross-repo verification —
  improved authentication path reduces transient failures.

- **False positives on documented code** — typical web-framework
  codebases see roughly 30 spurious "monolithic file / function"
  findings removed per repository as a result of the line-count
  change above.

- **PR comment rendering** — descriptions no longer truncated mid-word;
  overflow findings are reachable via collapsible sections instead of
  dead "and N more findings" text.

### Compliance

- **Multi-framework evidence sections** appear in the Step Summary +
  PR comment when the relevant analyzers run. Each control is mapped
  to the analyzers that produce evidence for it; the report shows
  PASS / CONCERN / FAIL per control with the underlying violation
  count. Auditors get a one-screenshot answer; engineering teams get
  the line-by-line drill-in.

---

## How to read this changelog

- **`Added`** — new capabilities customers can use
- **`Changed`** — behaviour or output changes affecting existing scans
- **`Fixed`** — bug fixes that resolve incorrect or missing output
- **`Compliance`** — changes to compliance-evidence reporting

PullGuard is continuously deployed: pushing to the `pullguard-dev/pullguard:latest`
container image happens on every merge to the scanner repository's
main branch. Customers using `uses: pullguard-dev/pullguard-action@v1`
pull the latest image automatically on each scan.

For change-controlled or reproducible scans, pin a specific image with the
`image-pin` input (e.g. `image-pin: v1.0.0`). You then get exactly the
capabilities listed under that release section above, frozen until you
choose to move the pin — each numbered release here corresponds to an
immutable `ghcr.io/pullguard-dev/pullguard:vX.Y.Z` image tag.

For policy questions, security disclosures, or to discuss enterprise
deployment options: [hello@pullguard.dev](mailto:hello@pullguard.dev).
