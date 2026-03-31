# RELEASE_GATE

## Purpose
This document defines the release gate for shipping Basiq packages, docs, and official adapters.

The release gate exists to ensure Basiq ships only when the monorepo, shared contracts, and supported provider adapters are internally consistent.

## Ship Rule
Ship only after both commands pass on the release candidate commit:

```bash
pnpm release-gate
pnpm release-gate:full
```

## Gate Levels

### Baseline Gate — `pnpm release-gate`
This is the required PR and release-candidate baseline.

It must verify:
- lint
- typecheck
- unit tests
- integration tests
- package builds
- audit triage
- docs integrity for public contract changes

### Full Gate — `pnpm release-gate:full`
This is the ship gate.

It must verify:
- everything in `pnpm release-gate`
- docs app build under release conditions
- example app sanity verification
- end-to-end tests for official flows
- release packaging or publish dry run

## Release Scope
The release gate applies to:
- `@basiq/core`
- `@basiq/react`
- `@basiq/ui`
- official provider adapters
- docs app when public APIs or onboarding behavior changed

## Blocking Conditions
Release is blocked if any of the following are true:
- `pnpm release-gate` fails
- `pnpm release-gate:full` fails
- changeset metadata is missing for publishable package changes
- public contracts changed without documentation update
- adapter contract tests fail
- audit triage contains unresolved release blockers
- critical secrets or unsafe env handling are detected

## Release Checklist
- [ ] Branch is up to date with `development`
- [ ] Release PR into `main` is approved
- [ ] Changesets are present and reviewed
- [ ] `pnpm release-gate` passes
- [ ] `pnpm release-gate:full` passes
- [ ] Release notes are generated
- [ ] Docs reflect final public behavior
- [ ] Version bumps are correct
- [ ] Publish dry run is clean

## Gate Evidence
Every release should attach evidence for:
- CI run URL
- commit SHA
- package versions
- changeset summary
- audit triage result
- known non-blockers, if any

## Required Test Categories

### Core
- schema validation tests
- registry resolution tests
- error classification tests
- webhook normalizer tests

### Adapters
- capability declaration tests
- create shipment contract tests
- rate quote tests
- tracking normalization tests
- webhook verification and parsing tests when supported

### UI and React
- hook behavior tests
- provider wrapper tests
- component rendering smoke tests

## Contract Change Rule
If any of these change, release is blocked until docs and examples are updated:
- adapter contract
- universal shipment schema
- error model
- capability flags
- environment configuration required by developers

## Allowed Non-Blockers
The following may be documented but do not automatically block a release:
- improvement ideas with no runtime effect
- unsupported future providers
- non-public internal refactors
- docs copy polish unrelated to behavior

## Emergency Patch Rule
Emergency fixes may skip non-essential checks only if:
- the patch is scoped and documented,
- the skipped checks are explicitly listed,
- the patch is followed by a full validation run,
- and maintainers record the exception in release notes.

## Final Release Verdict Template
```md
Release Candidate: <version>
Commit: <sha>
Baseline Gate: PASS | FAIL
Full Gate: PASS | FAIL
Audit Triage: PASS | WARN | FAIL
Docs Sync: PASS | FAIL
Publish Dry Run: PASS | FAIL
Final Verdict: SHIP | NO SHIP
Notes: <short rationale>
```
