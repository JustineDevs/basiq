# CONTRIBUTING

## Welcome
Thank you for contributing to Basiq.

Basiq is building open-source infrastructure for Philippine logistics. Contributions should strengthen the shared standard, improve developer experience, and preserve the integrity of the provider-driven architecture.

## Ways to Contribute
- Improve documentation
- Fix bugs
- Add tests
- Improve schemas
- Build new provider adapters
- Improve UI components
- Report issues and edge cases
- Propose better normalization strategies

## Ground Rules
- Keep the core stable.
- Do not hardcode provider logic into shared packages.
- Prefer narrow, composable changes over broad rewrites.
- Preserve typed interfaces and validation quality.
- Document tradeoffs clearly in pull requests.

## Repo Principles
### 1. Provider-Driven Design
All provider-specific behavior belongs in adapter packages.

### 2. Stable Contracts
Changes to shared schemas and adapter contracts must be deliberate and backward-aware.

### 3. PH-First Modeling
Philippine geographic and logistics realities are first-class concerns, not edge cases.

### 4. Graceful Degradation
Capability differences should be explicit and well-documented.

## Development Setup
```bash
pnpm install
pnpm dev
pnpm test
```

## Branching
Use small focused branches.

Recommended naming:
- `feat/provider-jrs`
- `fix/core-validation`
- `docs/quickstart`
- `test/webhook-normalizer`

## Pull Request Checklist
Before opening a PR:
- [ ] Code is scoped to one concern
- [ ] Tests were added or updated
- [ ] Docs were updated when behavior changed
- [ ] Provider-specific logic stays inside adapter packages
- [ ] No breaking contract changes were introduced silently
- [ ] TypeScript passes in strict mode

## Adapter Contribution Rules
If you are adding a provider adapter, include:
- credential requirements
- capability declarations
- payload translation logic
- error mapping
- webhook verification or parsing when supported
- test coverage for the adapter contract

## Documentation Standards
Good docs should answer:
- What does this package do?
- Why does it exist?
- What are the constraints?
- How do I verify behavior?
- What is intentionally unsupported?

## Issue Reports
Please include:
- environment
- package version
- provider name
- reproduction steps
- expected behavior
- actual behavior
- logs or payload examples when safe to share

## Commit Style
Suggested prefixes:
- `feat:` new functionality
- `fix:` bug fixes
- `docs:` documentation changes
- `refactor:` internal improvements
- `test:` test changes
- `chore:` maintenance work

## Code Style
- TypeScript strict mode
- Small functions
- Explicit types on public interfaces
- No hidden provider assumptions in shared packages
- Raw provider payloads only as debug metadata, not the main contract

## Decision Making
Large changes should be proposed first through an issue or design note before implementation begins.

## Code of Conduct
Be respectful, specific, and collaborative. The goal is to build shared infrastructure that other Philippine developers can trust.
