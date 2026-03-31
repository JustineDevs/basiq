# ARCHITECTURE

## Overview
Basiq uses a layered, provider-driven architecture. The system separates app-facing contracts from courier-facing implementations so developers can depend on one stable interface while adapters handle provider-specific complexity.

## Design Goals
- Stable app-facing API
- Strong typing and validation
- Provider isolation
- Graceful degradation
- PH-first data modeling
- Extensible package system

## Monorepo Structure
```txt
apps/
  docs/
  example-react/

packages/
  core/
  react/
  ui/
  provider-lalamove/
  provider-jt/
  provider-ninjavan/
```

## Layer Model

### Layer A — Core Kernel
Responsibilities:
- Universal domain models
- Validation schemas
- Error model
- Registry and orchestration
- Capability negotiation
- Webhook normalization

Primary package:
- `@basiq/core`

### Layer B — Provider Adapters
Responsibilities:
- Translate Basiq payloads into provider payloads
- Map provider responses into normalized Basiq responses
- Verify and parse provider webhooks
- Expose supported capabilities

Primary packages:
- `@basiq/provider-lalamove`
- `@basiq/provider-jt`
- `@basiq/provider-ninjavan`

### Layer C — Framework Bindings
Responsibilities:
- State synchronization
- Hooks for rates, tracking, and shipment lifecycle
- Query caching and mutation patterns
- Provider-aware context wrappers

Primary package:
- `@basiq/react`

### Layer D — UI Components
Responsibilities:
- Address Picker
- Tracking Timeline
- Rate Comparison Card
- Theming and branding hooks

Primary package:
- `@basiq/ui`

### Layer E — Documentation and Examples
Responsibilities:
- Quickstart docs
- API references
- Capability matrix
- Working example integrations

Primary app:
- `apps/docs`

## Primary Request Flow
1. Developer app calls a Basiq method such as `createShipment()`.
2. Core validates the payload using shared schemas.
3. Registry resolves the target provider instance.
4. Adapter translates the payload into provider-specific format.
5. Provider API responds.
6. Adapter normalizes the response into a Basiq result.
7. App receives a stable typed response.

## Webhook Flow
1. Provider sends a webhook.
2. Adapter verifies authenticity.
3. Adapter parses provider event shape.
4. Core maps provider state into `ShipmentStatus`.
5. App receives a normalized event object.

## Key Internal Modules
### Registry
Stores provider instances and credentials, and resolves adapters for runtime operations.

### Validator
Zod-based schemas validate app-facing payloads before provider translation begins.

### Normalizer
Maps provider-specific responses and statuses into stable Basiq domain objects.

### Capability Engine
Exposes provider support flags such as COD, same-day delivery, label generation, scheduled delivery, or helper-required transport.

## Error Strategy
Errors are standardized into categories:
- ValidationError
- AuthError
- ProviderError
- CapabilityError
- NetworkError
- UnknownError

Each adapter is responsible for mapping provider-native errors into Basiq categories.

## Graceful Degradation
If a payload includes a feature unsupported by the selected provider, the adapter must:
- ignore the field when safe,
- warn when behavior changes materially, or
- throw a typed `CapabilityError` when execution would be misleading.

## Package Boundaries
### `@basiq/core`
Must not import framework-specific UI concerns.

### `@basiq/react`
Must not contain provider-specific translation logic.

### `@basiq/provider-*`
Must implement the shared adapter contract.

### `@basiq/ui`
Must stay presentation-first while consuming normalized core outputs.

## Example Architecture Diagram
```mermaid
graph TD
    App[Developer App] --> React[@basiq/react]
    React --> Core[@basiq/core]
    Core --> Registry[Provider Registry]
    Core --> Validator[Zod Schemas]
    Core --> Normalizer[Webhook Normalizer]
    Registry --> Lala[@basiq/provider-lalamove]
    Registry --> JT[@basiq/provider-jt]
    Registry --> Ninja[@basiq/provider-ninjavan]
    Lala --> LalaAPI[Lalamove API]
    JT --> JTAPI[J&T API]
    Ninja --> NinjaAPI[Ninja Van API]
```

## Architectural Rule
Basiq must preserve one stable developer contract even when provider behavior differs underneath.
