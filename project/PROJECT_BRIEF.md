# PROJECT_BRIEF

## Project Name
Basiq

## Status
Draft

## Owner
@JustineDevs

## Mission
Basiq standardizes Philippine logistics integrations through open-source code.

## One-Line Pitch
Basiq is a universal logistics layer for the Philippines that gives developers one standardized SDK for shipments, rates, tracking, and webhook normalization across courier providers.

## Problem
Philippine logistics integrations are fragmented. Each courier exposes different payload shapes, address requirements, status enums, authentication flows, and operational constraints. Developers who want to support more than one provider end up writing custom glue code, duplicating business logic, and maintaining courier-specific edge cases across their stack.

## Solution
Basiq provides a provider-driven abstraction layer that sits between developer applications and official courier APIs. It standardizes shipment creation, address validation, tracking, rates, and webhook handling while preserving the developer's direct business relationship with logistics providers through a BYOK model.

## Why It Matters
- Reduces integration time from weeks to hours.
- Removes courier lock-in by standardizing the app-facing contract.
- Encodes Philippine-specific logistics realities such as barangay structure, COD, and local mobile validation.
- Makes it easier to build shipping experiences for storefronts, admin panels, marketplaces, and internal operations tools.

## Who It Is For
- E-commerce platforms
- Marketplace builders
- SaaS teams with fulfillment workflows
- Operations dashboards
- Developers building PH-first delivery flows

## Core Value Proposition
Integrate once, switch providers later, and keep a consistent shipping developer experience.

## Product Pillars
### 1. Standardization Layer
Basiq exposes one shared contract for creating and tracking shipments regardless of provider.

### 2. BYOK Infrastructure
Developers use their own courier credentials, rates, and business accounts.

### 3. PH-First Validation
Addresses, mobile numbers, and COD rules are modeled for Philippine logistics workflows.

### 4. Developer Experience
The SDK includes strong typing, shared schemas, normalized errors, and optional UI components.

## Initial Scope
### In Scope
- Universal shipment schema
- Universal contact and address schema
- Provider registry
- Provider capability flags
- Shipment creation
- Rate fetching
- Tracking lookup
- Webhook normalization
- React hooks and basic UI components
- API-style documentation site

### Out of Scope for V1
- Full provider coverage in the Philippines
- Hosted dashboard services
- Billing and reseller logistics model
- Multi-framework compiled UI output beyond the first supported target
- Full returns management

## Business Model
Basiq starts as open-core infrastructure.

Possible future monetization:
- Paid support
- Enterprise onboarding
- Premium adapters
- Hosted webhook relay
- Analytics and observability tooling

## Success Definition
Basiq succeeds if a developer can:
1. Install the SDK.
2. Register a supported provider.
3. Create a shipment with one typed payload.
4. Swap providers without rewriting app logic.
5. Render shipping UI with minimal custom code.

## V1 Deliverables
- `@basiq/core`
- `@basiq/react`
- `@basiq/provider-lalamove`
- `@basiq/provider-jt`
- Core docs site
- One example integration app

## Risks
- Uneven API maturity across PH courier providers
- Business onboarding requirements for provider access
- Payload and webhook differences more complex than public docs suggest
- Maintaining abstraction quality without hiding provider limitations

## Guiding Principle
Basiq should be narrow, reliable, and extensible before it becomes broad.
