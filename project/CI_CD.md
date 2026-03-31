# CI_CD

## Purpose
This document defines the continuous integration and continuous delivery workflow for the Basiq monorepo.

Basiq uses a provider-driven monorepo architecture, so CI/CD must validate shared contracts, adapter isolation, docs correctness, and package release safety before code reaches production.

## Objectives
- Keep `main` releasable at all times.
- Validate app-facing contracts before publishing packages.
- Prevent provider-specific regressions from leaking into shared packages.
- Enforce consistent package quality across apps and packages.
- Support fast PR feedback and stricter pre-release verification.

## Branch Strategy
- `feature/*` branches for isolated work
- `development` for integration testing
- `main` for production-ready releases

### Branch Rules
- No direct push to `main`
- No direct push to `development`
- Every feature branch must be rebased or merged from `development` before opening a release-ready PR
- Release PRs merge from `development` into `main`

## CI Levels

### Level 1 — Pull Request CI
Runs on:
- pull requests to `development`
- pull requests to `main`

Checks:
- Install dependencies with pnpm
- Validate lockfile integrity
- Typecheck
- Lint
- Unit tests
- Contract tests
- Build packages
- Validate docs links if docs changed

### Level 2 — Integration CI
Runs on:
- push to `development`

Checks:
- All Level 1 checks
- Example app build
- Docs app build
- Cross-package integration tests
- Adapter capability snapshot tests
- Webhook normalization tests

### Level 3 — Release CI
Runs on:
- release candidate PRs to `main`
- tag creation
- manual dispatch

Checks:
- All Level 2 checks
- Full release gate
- Changeset validation
- Publish dry run
- Release notes generation
- Artifact integrity checks

## Workflow Commands
```bash
pnpm install --frozen-lockfile
pnpm lint
pnpm typecheck
pnpm test
pnpm build
pnpm release-gate
pnpm release-gate:full
```

## Required Root Scripts
```json
{
  "scripts": {
    "lint": "turbo run lint",
    "typecheck": "turbo run typecheck",
    "test": "turbo run test",
    "build": "turbo run build",
    "release-gate": "pnpm lint && pnpm typecheck && pnpm test && pnpm build && pnpm audit:triage",
    "release-gate:full": "pnpm release-gate && turbo run test:e2e --continue"
  }
}
```

## Recommended GitHub Actions

### `ci-pr.yml`
Purpose:
- Fast PR feedback for code quality and correctness.

Jobs:
- setup
- lint
- typecheck
- test
- build

### `ci-integration.yml`
Purpose:
- Validate development branch after merge.

Jobs:
- install
- lint
- typecheck
- test
- integration
- docs-build
- example-build

### `release.yml`
Purpose:
- Run publish-safe verification for tags or manual releases.

Jobs:
- release-gate
- release-gate-full
- changesets
- publish-dry-run
- publish

## Package-Level Expectations

### Shared Packages
Packages such as `@basiq/core`, `@basiq/react`, and `@basiq/ui` must pass:
- typecheck
- lint
- unit tests
- build

### Provider Adapters
Packages such as `@basiq/provider-lalamove` and `@basiq/provider-jt` must also pass:
- adapter contract tests
- payload mapping tests
- error mapping tests
- webhook parsing tests when supported

## Deployment Targets

### Docs App
- Deploy from `main`
- Preview deploys from PRs

### Example Apps
- Preview deploys only
- Never treated as release blockers unless they are official public demos

### NPM Packages
- Publish only from tagged releases or approved release workflow

## CI Failure Policy
- Red PR checks block merge
- Release gate failure blocks publishing
- Audit triage failures above threshold block `main`
- Docs drift does not block feature PRs unless the change modifies public contracts

## Environment Policy
Store secrets only in CI provider secrets.

Required examples:
- `NPM_TOKEN`
- `TURBO_TOKEN` if remote cache is used
- provider sandbox credentials only in protected environments

## Caching
- Use pnpm store cache
- Use Turbo remote or local cache
- Cache should never bypass test execution on protected branches when changed files affect runtime contracts

## Deliverable Standard
A PR is considered CI-complete only when:
- code passes
- tests pass
- relevant docs are updated
- release gate assumptions remain true
