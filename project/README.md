### Project Name
**Basiq**

### Problem Statement
Philippine logistics integrations are fragmented. Each courier exposes different payloads, authentication models, tracking formats, and operational assumptions, forcing developers to write custom glue code for every provider.

This affects:
- E-commerce teams
- SaaS builders
- Marketplace developers
- Internal ops platforms

Consequences:
- Slow integration time
- Higher maintenance cost
- Vendor lock-in
- Inconsistent tracking and webhook handling
- Rework when switching providers

### Solution Overview
Basiq is an open-source universal logistics layer for the Philippines. It gives developers a standardized SDK, unified schemas, provider adapters, and optional UI components for shipment creation, address validation, rate lookup, and tracking.

Core differentiators:
- BYOK design, developers keep their own courier accounts
- Universal PH address schema
- Provider adapter contract
- Normalized tracking and webhook events
- Plug-and-play UI primitives
- Framework-ready package design

### Project Description
**Tech Stack**
- TypeScript 5+
- Turborepo
- Zod
- TanStack Query
- TSUP
- Tailwind CSS
- Headless UI
- Mitosis

**Architecture**
- `@basiq/core`: schemas, orchestration, errors, registry, webhook normalizer
- `@basiq/react`: hooks, provider context, UI bindings
- `@basiq/provider-*`: courier-specific adapters
- `docs`: API-style docs site with three-panel layout

**User Stories**
- As a developer, I want to create a shipment with one payload regardless of courier.
- As a merchant platform, I want to compare rates across enabled providers.
- As a frontend team, I want reusable PH-ready shipping UI components.
- As an ops team, I want normalized tracking events from different courier webhooks.

**Technical Challenges**
- Mapping courier-specific payloads into one contract
- PSGC-backed address normalization
- Handling uneven provider capabilities
- Standardizing statuses and warnings without hiding provider limitations

**Scalability**
- Add providers through adapter packages
- Keep core courier-agnostic
- Support more frameworks later through compiled UI outputs
- Expand from shipping flows into labels, COD, returns, and analytics

### Community Engagement Features
- Open-source provider adapter contributions
- Public schema discussions
- Courier capability matrix in docs
- Example integrations for storefronts and admin dashboards
- Feedback-driven roadmap voting

### Monetization Strategy
Start open-core.

Possible revenue layers:
- Free: core SDK, basic adapters, docs
- Pro: advanced dashboards, hosted webhook relay, analytics, premium support
- Enterprise: onboarding, custom adapters, SLA support, private packages

### Roadmap
| Phase | Scope |
|---|---|
| V1 | `@basiq/core`, one express adapter, one on-demand adapter, React hooks, basic docs |
| V1.1 | Address picker, tracking timeline, capability matrix, webhook normalization |
| V1.2 | COD flows, labels/waybills, better error handling, sandbox examples |
| V2 | More providers, more framework targets, richer theming, admin utilities |
| V3 | Hosted services, analytics, enterprise workflows |

### Success Metrics
**Technical**
- Time to first shipment under 30 minutes
- Adapter test coverage above 80%
- Stable schema validation across supported providers

**User**
- Number of GitHub stars
- Number of installs
- Number of successful sample integrations
- Developer feedback on setup time

**Business**
- Number of active teams using Basiq
- Paid support inquiries
- Enterprise adapter requests
