## Top-level layout

Use this root layout so Basiq stays organized as a logistics standardization platform with separate docs, example apps, and reusable packages.

```txt
basiq/
├─ apps/
│  ├─ docs/
│  ├─ web-playground/
│  └─ example-react/
├─ packages/
│  ├─ core/
│  ├─ react/
│  ├─ ui/
│  ├─ provider-lalamove/
│  ├─ provider-jt/
│  ├─ provider-ninjavan/
│  ├─ psgc/
│  ├─ config/
│  └─ test-utils/
├─ tooling/
│  ├─ eslint/
│  ├─ typescript/
│  └─ scripts/
├─ .changeset/
├─ .github/
│  ├─ workflows/
│  └─ ISSUE_TEMPLATE/
├─ docs/
│  ├─ adr/
│  ├─ specs/
│  └─ diagrams/
├─ examples/
│  ├─ node-basic/
│  ├─ react-basic/
│  └─ webhook-handler/
├─ package.json
├─ pnpm-workspace.yaml
├─ turbo.json
├─ tsconfig.base.json
├─ README.md
├─ CONTRIBUTING.md
├─ LICENSE
└─ .gitignore
```

## Apps and packages

Put user-facing surfaces in `apps/`, and keep reusable shipping logic in `packages/`, because Basiq is centered on a shared core registry, hooks, UI components, and provider adapters rather than on one single app.

### `apps/`
- `apps/docs` should be the public documentation site with guides, API references, capability matrix, and branding examples.
- `apps/web-playground` should be a safe demo surface for trying schemas, payloads, and adapter responses without polluting the docs app.
- `apps/example-react` should be the canonical integration sample for `@basiq/react` and `@basiq/ui`.

### `packages/`
- `packages/core` should contain schemas, errors, registry, orchestration, webhook normalization, and shared domain types.
- `packages/react` should contain hooks, query bindings, and framework-specific integration logic only.
- `packages/ui` should contain reusable components like Address Picker, Tracking Timeline, and Rate Comparison Card.
- `packages/provider-lalamove`, `packages/provider-jt`, and `packages/provider-ninjavan` should contain only adapter logic for those providers.
- `packages/psgc` should hold the compressed Philippine geographic dataset and lookup helpers, because PH-first logistics normalization is part of the product identity.
- `packages/config` should centralize shared tsconfig, lint presets, env helpers, and package conventions for the monorepo.
- `packages/test-utils` should hold fixtures, mock providers, webhook samples, and shared contract-test helpers.

## Concrete file tree

This is the concrete V1 tree I would use.

```txt
basiq/
├─ apps/
│  ├─ docs/
│  │  ├─ app/
│  │  │  ├─ (marketing)/
│  │  │  ├─ docs/
│  │  │  ├─ api/
│  │  │  └─ playground/
│  │  ├─ components/
│  │  ├─ content/
│  │  ├─ public/
│  │  ├─ package.json
│  │  └─ tsconfig.json
│  ├─ web-playground/
│  │  ├─ src/
│  │  │  ├─ app/
│  │  │  ├─ components/
│  │  │  ├─ lib/
│  │  │  └─ mocks/
│  │  ├─ package.json
│  │  └─ tsconfig.json
│  └─ example-react/
│     ├─ src/
│     │  ├─ app/
│     │  ├─ components/
│     │  ├─ lib/
│     │  └─ providers/
│     ├─ package.json
│     └─ tsconfig.json
├─ packages/
│  ├─ core/
│  │  ├─ src/
│  │  │  ├─ address/
│  │  │  │  ├─ address.types.ts
│  │  │  │  ├─ address.schema.ts
│  │  │  │  └─ address.normalizer.ts
│  │  │  ├─ shipment/
│  │  │  │  ├─ shipment.types.ts
│  │  │  │  ├─ shipment.schema.ts
│  │  │  │  ├─ shipment.status.ts
│  │  │  │  └─ shipment.mapper.ts
│  │  │  ├─ contact/
│  │  │  │  ├─ contact.types.ts
│  │  │  │  └─ contact.schema.ts
│  │  │  ├─ registry/
│  │  │  │  ├─ provider-registry.ts
│  │  │  │  ├─ provider-context.ts
│  │  │  │  └─ byok-manager.ts
│  │  │  ├─ adapters/
│  │  │  │  ├─ adapter-contract.ts
│  │  │  │  ├─ capability.types.ts
│  │  │  │  └─ adapter-errors.ts
│  │  │  ├─ webhooks/
│  │  │  │  ├─ webhook.types.ts
│  │  │  │  ├─ webhook-normalizer.ts
│  │  │  │  └─ webhook-events.ts
│  │  │  ├─ errors/
│  │  │  │  ├─ basiq-error.ts
│  │  │  │  ├─ validation-error.ts
│  │  │  │  ├─ provider-error.ts
│  │  │  │  └─ capability-error.ts
│  │  │  ├─ utils/
│  │  │  │  ├─ invariant.ts
│  │  │  │  ├─ deep-merge.ts
│  │  │  │  └─ assert-never.ts
│  │  │  └─ index.ts
│  │  ├─ tests/
│  │  ├─ package.json
│  │  ├─ tsup.config.ts
│  │  └─ tsconfig.json
│  ├─ react/
│  │  ├─ src/
│  │  │  ├─ provider/
│  │  │  │  ├─ basiq-provider.tsx
│  │  │  │  └─ basiq-context.ts
│  │  │  ├─ hooks/
│  │  │  │  ├─ use-shipment.ts
│  │  │  │  ├─ use-rates.ts
│  │  │  │  ├─ use-tracking.ts
│  │  │  │  └─ use-provider-capabilities.ts
│  │  │  ├─ query/
│  │  │  │  ├─ query-keys.ts
│  │  │  │  └─ query-client.ts
│  │  │  └─ index.ts
│  │  ├─ tests/
│  │  ├─ package.json
│  │  └─ tsconfig.json
│  ├─ ui/
│  │  ├─ src/
│  │  │  ├─ components/
│  │  │  │  ├─ address-picker/
│  │  │  │  ├─ tracking-timeline/
│  │  │  │  ├─ rate-comparison-card/
│  │  │  │  └─ shipment-status-badge/
│  │  │  ├─ theme/
│  │  │  │  ├─ tokens.ts
│  │  │  │  ├─ basiq-theme.ts
│  │  │  │  └─ provider-theme.tsx
│  │  │  ├─ styles/
│  │  │  │  └─ index.css
│  │  │  └─ index.ts
│  │  ├─ package.json
│  │  └─ tsconfig.json
│  ├─ provider-lalamove/
│  │  ├─ src/
│  │  │  ├─ client/
│  │  │  │  ├─ lalamove-client.ts
│  │  │  │  └─ lalamove-auth.ts
│  │  │  ├─ mapping/
│  │  │  │  ├─ shipment-to-lalamove.ts
│  │  │  │  ├─ rate-from-lalamove.ts
│  │  │  │  ├─ tracking-from-lalamove.ts
│  │  │  │  └─ webhook-from-lalamove.ts
│  │  │  ├─ capabilities/
│  │  │  │  └─ lalamove-capabilities.ts
│  │  │  ├─ adapter/
│  │  │  │  └─ lalamove-adapter.ts
│  │  │  └─ index.ts
│  │  ├─ tests/
│  │  └─ package.json
│  ├─ provider-jt/
│  │  ├─ src/
│  │  │  ├─ client/
│  │  │  ├─ mapping/
│  │  │  ├─ capabilities/
│  │  │  ├─ adapter/
│  │  │  └─ index.ts
│  │  ├─ tests/
│  │  └─ package.json
│  ├─ provider-ninjavan/
│  │  ├─ src/
│  │  │  ├─ client/
│  │  │  ├─ mapping/
│  │  │  ├─ capabilities/
│  │  │  ├─ adapter/
│  │  │  └─ index.ts
│  │  ├─ tests/
│  │  └─ package.json
│  ├─ psgc/
│  │  ├─ src/
│  │  │  ├─ data/
│  │  │  ├─ lookup/
│  │  │  ├─ types/
│  │  │  └─ index.ts
│  │  ├─ scripts/
│  │  │  └─ build-psgc.ts
│  │  └─ package.json
│  ├─ config/
│  │  ├─ eslint/
│  │  ├─ tsconfig/
│  │  ├─ env/
│  │  └─ package.json
│  └─ test-utils/
│     ├─ src/
│     │  ├─ fixtures/
│     │  ├─ mocks/
│     │  ├─ samples/
│     │  └─ contract-test-kit.ts
│     └─ package.json
├─ docs/
│  ├─ adr/
│  │  ├─ 0001-provider-driven-architecture.md
│  │  ├─ 0002-universal-shipment-schema.md
│  │  └─ 0003-graceful-degradation.md
│  ├─ specs/
│  │  ├─ PROJECT_BRIEF.md
│  │  ├─ ROADMAP.md
│  │  ├─ ARCHITECTURE.md
│  │  ├─ ADAPTER_CONTRACT.md
│  │  ├─ UNIVERSAL_SCHEMA.md
│  │  ├─ CONTRIBUTING.md
│  │  └─ DEMO_SCRIPT.md
│  └─ diagrams/
│     ├─ system-context.mmd
│     ├─ request-flow.mmd
│     └─ webhook-flow.mmd
└─ tooling/
   ├─ eslint/
   ├─ typescript/
   └─ scripts/
```

## Naming rules

The naming should stay boring, explicit, and package-first, because that makes a provider-driven monorepo easier to scale and audit over time.

- Use `kebab-case` for folders and package names, such as `provider-lalamove`, `web-playground`, and `test-utils`.
- Use `@basiq/*` for all publishable packages, such as `@basiq/core`, `@basiq/react`, and `@basiq/provider-jt`.
- Use `*.types.ts`, `*.schema.ts`, `*.adapter.ts`, `*.client.ts`, `*.mapper.ts`, and `*.error.ts` suffixes consistently so file purpose is obvious at a glance.
- Name provider mapping files by direction, such as `shipment-to-lalamove.ts` and `tracking-from-lalamove.ts`, because that avoids vague names like `transform.ts`.
- Reserve `index.ts` only for package or module exports, not for business logic.
- Keep provider names normalized in code as `lalamove`, `jt`, and `ninjavan`, and do not mix display names with internal keys.
- Put all docs that define long-term product truth under `docs/specs/` and architectural decisions under `docs/adr/`, because you already prefer structured reusable technical guides and decision documents.

## Package naming map

This is the naming set I would lock for V1 so the repo stays stable as you add more adapters and surfaces later.

| Package | Purpose |
|---|---|
| `@basiq/core` | Schemas, domain models, registry, errors, orchestration.  |
| `@basiq/react` | React provider, hooks, query integration.  |
| `@basiq/ui` | Reusable shipping UI components.  |
| `@basiq/provider-lalamove` | Lalamove adapter package.  |
| `@basiq/provider-jt` | J&T adapter package.  |
| `@basiq/provider-ninjavan` | Ninja Van adapter package.  |
| `@basiq/psgc` | Philippine geographic lookup dataset and helpers.  |
| `@basiq/config` | Shared monorepo config presets.  |
| `@basiq/test-utils` | Shared mocks, fixtures, and contract-test helpers.  |

The main rule is simple: keep `core` courier-agnostic, keep adapters isolated, and keep app surfaces thin, because that is the cleanest match for the provider-driven layered system you are building.
