## MVP scope

The MVP should include only these packages: `@basiq/core`, `@basiq/react`, `@basiq/provider-lalamove`, `@basiq/provider-jt`, `@basiq/psgc`, one docs app, and one example React app.
The MVP should support only four app-facing flows: `createShipment`, `getRates`, `trackShipment`, and `parseWebhook`, because those are the smallest useful surface area for proving a universal logistics layer.
Do **not** include Vue/Svelte output, hosted services, broad provider coverage, advanced theming, returns, or enterprise dashboards in this sprint.

## Week 1

### Days 1–2: Freeze contracts

- Finalize `UNIVERSAL_SCHEMA.md`, `ADAPTER_CONTRACT.md`, and `ARCHITECTURE.md` before coding deeper features, because your workflow works best when boundaries are defined first.
- Lock the internal domain model for `PHAddress`, `PHContact`, `PHShipment`, `ShipmentStatus`, `ProviderCapabilities`, and normalized errors.
- Define only one provider key format: `lalamove` and `jt`, and keep display names out of core logic.


### Days 3–4: Build core package

- Implement `packages/core/src/address`, `contact`, `shipment`, `registry`, `adapters`, `webhooks`, and `errors` exactly as the monorepo structure expects.
- Add Zod schemas for address, contact, shipment, and webhook payload normalization.
- Implement the provider registry, BYOK manager, capability checks, and typed error classes first, because everything else depends on them.


### Day 5: PSGC package

- Build `@basiq/psgc` with compressed lookup helpers for region, province, city, and barangay validation so PH-specific address normalization is present from V1.
- Expose simple helpers like `findRegion`, `findProvince`, `findCity`, and `findBarangayByCity`.


## Week 2

### Days 6–7: Lalamove adapter

- Implement `provider-lalamove` with client, auth, capability definition, shipment mapping, rate mapping, tracking mapping, and webhook parsing.
- Treat the adapter as an isolated translation layer only, because provider-specific behavior should stay outside shared packages in your architecture model.
- Focus first on sandbox-safe shipment creation, rate quotes, and tracking rather than every optional Lalamove feature.


### Days 8–9: J\&T adapter

- Implement `provider-jt` with the same internal structure as Lalamove so both adapters satisfy one contract.
- Normalize provider outputs into one shared Basiq result format, even if some fields are missing or need capability warnings.
- Add `getCapabilities()` so unsupported features are explicit rather than hidden.


### Day 10: Contract tests

- Write contract tests that run the same expectations against both adapters for `createShipment`, `getRates`, `trackShipment`, and `parseWebhook`.
- Add fixtures for raw request, response, and webhook payloads so mapping drift is easy to catch later.


## Week 3

### Days 11–12: React package

- Implement `@basiq/react` with `BasiqProvider`, `useShipment`, `useRates`, `useTracking`, and `useProviderCapabilities`.
- Use TanStack Query patterns for loading, caching, and errors so app integrations stay consistent.


### Days 13–14: UI package

- Build only three UI components: `AddressPicker`, `TrackingTimeline`, and `RateComparisonCard`.
- Keep the UI minimal and wired to normalized core outputs, not provider-native payloads.
- Use default styling tokens only; postpone full branding API polish until after the core is validated.


### Day 15: Example app

- Create `apps/example-react` that proves the full loop: configure credentials, create shipment, fetch rates, track shipment, and render the timeline.
- This app is the MVP proof, so it matters more than extra components or visual polish.


## Week 4

### Days 16–17: Docs app

- Ship a minimal docs site with Quickstart, Concepts, Provider Setup, Universal Schema, Adapter Capability Matrix, and Example App walkthrough.
- Add one architecture diagram and one request flow diagram so developers understand the core-to-adapter model quickly.


### Days 18–19: Hardening

- Add CI checks for lint, typecheck, unit tests, contract tests, and package builds so the monorepo stays release-safe from the start.
- Add release gate and audit triage docs or scripts only at the level needed to protect the MVP, not as a giant enterprise process.
- Verify that provider-specific auth, token handling, and webhook paths are isolated, because real courier integrations can involve bearer tokens, token expiry handling, and onboarding complexity.[^1]


### Day 20: MVP ship checklist

- Confirm that a developer can install the packages, register credentials, create a shipment with one payload, fetch rates, track a shipment, and receive a normalized webhook event through the example app.
- Confirm that both adapters pass the same contract tests and that docs match actual package behavior.
- If any provider feature is unsupported, surface it through capability flags or warnings instead of pretending parity exists.


## Ship criteria

Your MVP is done when these deliverables exist and work end to end:

- `@basiq/core` with schemas, registry, errors, and webhook normalizer.
- `@basiq/provider-lalamove` and `@basiq/provider-jt` both passing shared contract tests.
- `@basiq/react` with the four main hooks and provider wrapper.
- `@basiq/ui` with Address Picker, Tracking Timeline, and Rate Comparison Card.
- `@basiq/psgc` with usable PH geographic lookup helpers.
- `apps/docs` and `apps/example-react` proving real developer onboarding.


## Non-goals

Do not build these in the MVP sprint:

- Ninja Van adapter.
- Full cross-framework component compilation.
- Hosted webhook relay or analytics.
- Enterprise billing or reseller workflows.
- Large provider marketplace claims.
