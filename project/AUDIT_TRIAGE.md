# AUDIT_TRIAGE

## Purpose
This document defines how Basiq performs audit triage for dependency health, vulnerabilities, license review, configuration risks, and release-readiness findings.

Audit triage is separate from implementation and separate from the release gate, but its results feed the release decision.

## Scope
Audit triage covers:
- dependency vulnerabilities
- outdated packages with material security or maintenance risk
- license compatibility concerns
- leaked secret risks
- unsafe CI/CD configuration
- package ownership or publish-risk issues
- provider adapter security assumptions

## Triage Frequency
- On every release candidate
- Weekly for active repositories
- Immediately after dependency upgrades affecting core or adapters
- Immediately after a security advisory affecting direct or transitive dependencies

## Severity Model

### P0 — Critical
Examples:
- known exploitable RCE in a production dependency
- secret committed to git history
- package publish token leak
- broken auth or signature verification in webhook handling

Action:
- block release immediately
- hotfix or rollback path required

### P1 — High
Examples:
- high-severity dependency with realistic exploit path
- provider credential mishandling
- incorrect environment scoping in CI
- missing verification for signed webhooks on an official adapter

Action:
- block release unless exception is approved and documented

### P2 — Medium
Examples:
- moderate dependency risk with limited exploitability
- stale package with no immediate exploit but poor maintenance signal
- weak logging or missing audit evidence

Action:
- can release only if tracked with owner and deadline

### P3 — Low
Examples:
- cosmetic config drift
- optional upgrades
- non-runtime documentation issues

Action:
- backlog

## Audit Areas

### 1. Dependency Health
Check:
- direct dependencies
- transitive vulnerabilities
- abandoned packages
- major-version lag for critical build or security packages

Suggested commands:
```bash
pnpm audit
pnpm outdated
pnpm licenses list
```

### 2. Secret and Config Exposure
Check:
- committed `.env` files
- exposed API keys in source or examples
- unsafe logging of tokens or secrets
- over-permissive CI environment usage

Suggested commands:
```bash
git grep -n "API_KEY\|SECRET\|TOKEN\|PRIVATE_KEY"
```

### 3. CI/CD Integrity
Check:
- branch protection assumptions
- unsafe publish conditions
- workflows that publish on untrusted events
- missing permission scoping in GitHub Actions

### 4. Provider Adapter Review
Check:
- credential handling
- request signing logic
- webhook verification logic
- raw payload logging policy
- error mapping that could hide provider failures

### 5. Documentation Truthfulness
Check:
- whether README and specs reflect real current behavior
- whether public contracts changed without docs updates
- whether release gate docs still match actual commands

## Triage Table Template
```md
| ID | Area | Finding | Severity | Status | Owner | Fix | Release Impact |
|---|---|---|---|---|---|---|---|
| AT-001 | Dependency | Example vulnerability in package X | P1 | OPEN | @owner | Upgrade to version Y | BLOCK |
```

## Status Values
- OPEN
- IN_PROGRESS
- MITIGATED
- ACCEPTED_RISK
- CLOSED

## Release Impact Rules
- P0 always blocks release
- P1 usually blocks release
- P2 may ship only with documented owner and deadline
- P3 never blocks release by itself

## Output Artifacts
Every triage cycle should produce:
- findings table
- blocked vs non-blocked summary
- owner assignment
- due dates for non-blockers
- release verdict input

## Suggested Monorepo Workflow
1. Run dependency and security checks.
2. Inspect results for exploitability, not just raw severity.
3. Map findings to Basiq runtime areas: core, adapters, UI, docs, CI.
4. Decide whether each finding blocks release.
5. Record decisions in the triage table.
6. Feed blockers into `RELEASE_GATE.md` evidence.

## Basiq-Specific Audit Priorities
Pay extra attention to:
- webhook verification paths
- BYOK credential handling
- adapter translation correctness
- logging of provider payloads
- package publishing permissions
- PSGC data provenance and update process

## Audit Verdict Template
```md
Audit Date: <date>
Commit: <sha>
Critical Findings: <count>
High Findings: <count>
Medium Findings: <count>
Low Findings: <count>
Release Blockers: <yes/no>
Final Audit Verdict: PASS | WARN | FAIL
Notes: <short rationale>
```

## Operating Rule
Audit triage is not a checkbox. It is the written reasoning layer between raw scan output and a real ship decision.
