# ADAPTER_CONTRACT

## Purpose
Every official Basiq adapter must implement the same contract so the core can orchestrate providers through a stable runtime interface.

## Design Goals
- Consistent provider lifecycle
- Strong typing
- Capability discovery
- Predictable error handling
- Independent adapter packages

## TypeScript Contract
```ts
export type ProviderKey = 'lalamove' | 'jt' | 'ninjavan' | string;

export interface ProviderCapabilities {
  createShipment: boolean;
  getRates: boolean;
  trackShipment: boolean;
  cancelShipment: boolean;
  cod: boolean;
  scheduledDelivery: boolean;
  labelGeneration: boolean;
  webhookVerification: boolean;
  multiStop: boolean;
  heavyLoad: boolean;
}

export interface AdapterContext {
  providerKey: ProviderKey;
  environment: 'sandbox' | 'production';
  credentials: Record<string, string>;
  config?: Record<string, unknown>;
}

export interface CreateShipmentInput {
  shipment: PHShipment;
}

export interface CreateShipmentResult {
  shipmentId: string;
  providerShipmentId: string;
  status: ShipmentStatus;
  raw?: unknown;
}

export interface RateQuoteInput {
  shipment: PHShipment;
}

export interface RateQuote {
  provider: ProviderKey;
  serviceName: string;
  amount: number;
  currency: 'PHP';
  eta?: string;
  raw?: unknown;
}

export interface TrackingInput {
  shipmentId?: string;
  providerShipmentId?: string;
  trackingNumber?: string;
}

export interface TrackingEvent {
  status: ShipmentStatus;
  label: string;
  description?: string;
  occurredAt?: string;
  location?: string;
  raw?: unknown;
}

export interface TrackingResult {
  shipmentId?: string;
  providerShipmentId?: string;
  trackingNumber?: string;
  status: ShipmentStatus;
  events: TrackingEvent[];
  raw?: unknown;
}

export interface WebhookParseResult {
  eventType: string;
  shipmentId?: string;
  providerShipmentId?: string;
  status?: ShipmentStatus;
  payload: unknown;
  raw?: unknown;
}

export interface BasiqAdapter {
  key: ProviderKey;
  version: string;
  getCapabilities(): ProviderCapabilities;
  createShipment(input: CreateShipmentInput, ctx: AdapterContext): Promise<CreateShipmentResult>;
  getRates(input: RateQuoteInput, ctx: AdapterContext): Promise<RateQuote[]>;
  trackShipment(input: TrackingInput, ctx: AdapterContext): Promise<TrackingResult>;
  cancelShipment?(input: TrackingInput, ctx: AdapterContext): Promise<{ success: boolean; raw?: unknown }>;
  verifyWebhook?(headers: Headers | Record<string, string>, body: string, ctx: AdapterContext): Promise<boolean>;
  parseWebhook?(headers: Headers | Record<string, string>, body: string, ctx: AdapterContext): Promise<WebhookParseResult>;
}
```

## Required Methods
### `getCapabilities()`
Returns feature support flags so the core and UI can adapt behavior without guessing.

### `createShipment()`
Creates a provider-native shipment from the normalized Basiq shipment input.

### `getRates()`
Returns one or more rate quotes in normalized Basiq format.

### `trackShipment()`
Resolves shipment tracking state and timeline data.

## Optional Methods
### `cancelShipment()`
Implemented only when the provider supports shipment cancellation.

### `verifyWebhook()`
Verifies provider webhook authenticity.

### `parseWebhook()`
Parses incoming webhook payloads into normalized Basiq events.

## Adapter Rules
- Adapters must never expose provider-only responses as the primary return shape.
- Adapters may attach raw provider payloads under `raw` for debugging.
- Adapters must map provider-specific errors into typed Basiq errors.
- Adapters must declare missing support through capability flags instead of silent assumptions.
- Adapters must not mutate the input shipment payload.

## Capability Behavior
If a provider does not support a requested feature:
- return a typed `CapabilityError`, or
- ignore the field only if it has no material effect on user expectations.

## Testing Requirements
Every adapter should have:
- contract tests
- payload translation tests
- error mapping tests
- webhook parsing tests when supported
- capability snapshot tests

## Certification Checklist
An adapter is considered official when it:
- implements the full required contract,
- documents credential requirements,
- passes contract tests,
- normalizes statuses into Basiq enums,
- and clearly documents unsupported behavior.
