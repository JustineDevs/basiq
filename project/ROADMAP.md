# ROADMAP

## Product Direction
Basiq should evolve as a provider-driven logistics kernel first, then expand into UI, ecosystem tooling, and enterprise services.

## Phase Overview

| Phase | Name | Focus | Outcome |
|---|---|---|---|
| V1 | MVP Kernel | Core contracts and two adapters | One standardized shipping flow |
| V1.1 | Developer Experience | Better docs, examples, and UI primitives | Faster integration time |
| V1.2 | Operational Flows | COD, labels, webhook maturity | More real-world usefulness |
| V2 | Coverage Expansion | More providers and richer capabilities | Broader PH logistics compatibility |
| V3 | Platform Layer | Hosted services and enterprise features | Sustainable business layer |

## V1 — MVP Kernel
### Goal
Prove that one Basiq contract can support multiple Philippine logistics providers without forcing app rewrites.

### Deliverables
- Universal shipment schema
- Universal contact and address schema
- Registry and BYOK manager
- Standardized error model
- Provider capability map
- Shipment creation API
- Tracking lookup API
- Rate lookup API
- Lalamove adapter
- J&T adapter
- Basic React integration
- Minimal docs site

### Exit Criteria
- End-to-end demo works with at least two providers
- Shared payload passes schema validation
- Provider-specific translation is reliable
- Webhook normalization works for supported event types

## V1.1 — Developer Experience
### Goal
Make Basiq easy to adopt and evaluate.

### Deliverables
- Address Picker UI
- Tracking Timeline UI
- Rate Comparison Card UI
- Capability matrix in docs
- Quickstart guide
- Example apps
- Better copy, diagrams, and API docs

### Exit Criteria
- First-time developer can complete setup in under 30 minutes
- Example app demonstrates create shipment, tracking, and rates

## V1.2 — Operational Flows
### Goal
Support real operational workflows common in PH commerce.

### Deliverables
- COD normalization
- Labels and waybill support where available
- Better warning system for unsupported features
- Standardized shipment status timeline
- Retry-safe webhook parsing helpers
- Logging and observability hooks

### Exit Criteria
- Basiq can support a realistic merchant flow from booking to tracking
- Unsupported provider features degrade gracefully without runtime confusion

## V2 — Coverage Expansion
### Goal
Expand compatibility while protecting the stability of the core contract.

### Deliverables
- More courier adapters
- More webhook mappings
- More rate and service class coverage
- More provider capability flags
- More UI package targets
- Theme customization API

### Exit Criteria
- New adapters can be added without changing app-facing contracts
- Capability differences are clearly documented and testable

## V3 — Platform Layer
### Goal
Turn Basiq from an SDK into durable infrastructure.

### Deliverables
- Hosted webhook relay
- Team dashboards
- Adapter certification process
- Analytics and delivery observability
- Enterprise support workflows
- Private adapter packages

### Exit Criteria
- Paid support or hosted offerings become viable
- Enterprise teams can adopt Basiq with lower operational overhead

## Milestones
### Milestone 1
Finalize schemas and adapter contract.

### Milestone 2
Ship first two adapters with tests.

### Milestone 3
Publish docs and example app.

### Milestone 4
Normalize COD and webhook flows.

### Milestone 5
Expand provider coverage.

## Key Metrics
- Time to first shipment
- Successful adapter test coverage
- Number of active providers
- Number of example app completions
- GitHub stars and package installs
- Number of teams running Basiq in production

## Strategic Rule
Do not expand providers faster than the core contract can remain stable.
