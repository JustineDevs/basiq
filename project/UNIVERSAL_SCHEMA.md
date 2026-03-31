# UNIVERSAL_SCHEMA

## Purpose
The universal schema is the app-facing contract for all Basiq shipping operations. It encodes the minimum complete data model needed to support Philippine logistics providers while remaining stable across adapter implementations.

## Design Goals
- PH-first address modeling
- Strong validation
- Provider-agnostic payloads
- Extensible but narrow core
- Support for COD and common logistics flows

## Primary Shipment Type
```ts
export type ServiceType = 'instant' | 'next_day' | 'scheduled';
export type PaymentMethod = 'CASH_SENDER' | 'CASH_RECIPIENT' | 'WALLET' | 'COD';
export type ParcelCategory = 'food' | 'document' | 'parcel' | 'fragile' | 'bulky';

export enum ShipmentStatus {
  PENDING = 'PENDING',
  CONFIRMED = 'CONFIRMED',
  PICKED_UP = 'PICKED_UP',
  IN_TRANSIT = 'IN_TRANSIT',
  OUT_FOR_DELIVERY = 'OUT_FOR_DELIVERY',
  DELIVERED = 'DELIVERED',
  FAILED = 'FAILED',
  CANCELED = 'CANCELED',
}

export interface PHCoordinates {
  lat: number;
  lng: number;
}

export interface PHAddress {
  line1: string;
  barangay: string;
  city: string;
  province: string;
  region: string;
  postalCode: string;
  coordinates?: PHCoordinates;
}

export interface PHContact {
  name: string;
  mobile: string;
  email?: string;
  address: PHAddress;
}

export interface PHParcel {
  name: string;
  description?: string;
  weight: number;
  dimensions: {
    length: number;
    width: number;
    height: number;
  };
  category: ParcelCategory;
  declaredValue: number;
}

export interface PHShipmentOptions {
  isInsulatedBox?: boolean;
  requiresHelper?: boolean;
  isDoorToDoor?: boolean;
}

export interface PHShipment {
  sender: PHContact;
  recipient: PHContact;
  serviceType: ServiceType;
  scheduleAt?: string;
  parcel: PHParcel;
  paymentMethod: PaymentMethod;
  codAmount?: number;
  options?: PHShipmentOptions;
}
```

## Field Rationale
### `sender` and `recipient`
Every provider depends on participant data. Basiq keeps this normalized so adapters can translate to provider-native shapes.

### `serviceType`
Supports the broad classes of same-day, express, and scheduled logistics.

### `parcel`
Captures core fulfillment data needed for routing, pricing, and restrictions.

### `paymentMethod` and `codAmount`
Supports PH commerce realities, especially COD-based orders.

### `options`
Holds provider-conditional extras while keeping the base shipment model stable.

## Validation Rules
### Mobile
Accept PH mobile formats such as:
- `09XXXXXXXXX`
- `+639XXXXXXXXX`

### Address
Required fields:
- line1
- barangay
- city
- province
- region
- postalCode

### Coordinates
Optional at the core level, but required by specific providers or service classes when needed.

### COD
If `paymentMethod` is `COD`, then `codAmount` must be present and greater than zero.

### Scheduled Delivery
If `serviceType` is `scheduled`, then `scheduleAt` must be present.

## Example Payload
```json
{
  "sender": {
    "name": "Store Admin",
    "mobile": "+639171234567",
    "address": {
      "line1": "Unit 5, Rizal Street",
      "barangay": "San Antonio",
      "city": "Makati",
      "province": "Metro Manila",
      "region": "NCR",
      "postalCode": "1203",
      "coordinates": { "lat": 14.567, "lng": 121.02 }
    }
  },
  "recipient": {
    "name": "Juan Dela Cruz",
    "mobile": "09181234567",
    "address": {
      "line1": "Blk 2 Lot 3",
      "barangay": "Santiago",
      "city": "General Trias",
      "province": "Cavite",
      "region": "Region IV-A",
      "postalCode": "4107"
    }
  },
  "serviceType": "next_day",
  "parcel": {
    "name": "T-Shirt Order",
    "description": "Two cotton shirts",
    "weight": 1.2,
    "dimensions": { "length": 30, "width": 20, "height": 10 },
    "category": "parcel",
    "declaredValue": 899
  },
  "paymentMethod": "COD",
  "codAmount": 899,
  "options": {
    "isDoorToDoor": true
  }
}
```

## Graceful Degradation Rule
Adapters should ignore optional fields that a provider does not support only when doing so does not mislead the developer. Otherwise, the adapter should return a warning or typed capability error.

## Future Extensions
Possible future additions:
- pickup instructions
- return shipment metadata
- insurance options
- multi-stop route support
- package item arrays for complex orders
