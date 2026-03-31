# DEMO_SCRIPT

## Demo Goal
Show that Basiq lets a developer integrate Philippine logistics once and use a standardized workflow across providers.

## Demo Length
2 to 3 minutes

## Demo Story
A developer wants to add shipping to an application without learning different courier payloads, tracking formats, and webhook systems one by one.

## Demo Structure

### Scene 1 — Opening Problem (0:00 to 0:20)
Voiceover:
"Philippine logistics integrations are fragmented. Every courier has different payloads, status models, and requirements. Basiq solves that by providing one universal logistics layer for shipments, rates, tracking, and webhooks."

Show on screen:
- Basiq logo and short tagline
- Diagram of app -> Basiq -> providers

### Scene 2 — Quick Architecture (0:20 to 0:40)
Voiceover:
"Instead of integrating each courier directly, the app talks to Basiq. The core validates and orchestrates requests, then provider adapters translate them into courier-specific APIs."

Show on screen:
- Architecture diagram
- Highlight core, registry, and adapters

### Scene 3 — Register Providers (0:40 to 1:00)
Voiceover:
"Basiq uses a BYOK model. Developers connect their own provider credentials, so they keep direct relationships and rates with couriers."

Show on screen:
- Config file or setup UI
- Register Lalamove and J&T credentials

### Scene 4 — Create Shipment Once (1:00 to 1:30)
Voiceover:
"Here we create a shipment using one standardized payload with Philippine address fields, parcel details, and payment settings."

Show on screen:
- Code editor with one `createShipment()` call
- Highlight sender, recipient, barangay, city, province, and COD fields

### Scene 5 — Compare Rates (1:30 to 1:50)
Voiceover:
"Using the same shipment data, Basiq can request rates from supported providers and normalize the result into one predictable format."

Show on screen:
- Rate response or rate comparison UI
- Multiple providers listed side by side

### Scene 6 — Track Shipment (1:50 to 2:15)
Voiceover:
"Tracking works the same way. Provider-specific events are mapped into a shared shipment timeline so the app does not have to understand each courier separately."

Show on screen:
- Tracking timeline UI
- Normalized statuses such as PICKED_UP, IN_TRANSIT, and DELIVERED

### Scene 7 — Webhook Normalization (2:15 to 2:35)
Voiceover:
"When couriers send webhook updates, Basiq verifies and normalizes the event so downstream systems can use one status model."

Show on screen:
- Raw webhook on one side
- Normalized event on the other side

### Scene 8 — Close (2:35 to 3:00)
Voiceover:
"Basiq turns fragmented PH logistics APIs into one developer-first standard. Integrate once, support multiple providers, and build shipping flows faster."

Show on screen:
- Repo URL
- Docs URL placeholder
- Short mission statement

## Screen Checklist
- Logo or project name
- Architecture diagram
- Config setup
- Shipment creation example
- Rates example
- Tracking timeline
- Webhook normalization example
- Final CTA slide

## Recording Tips
- Use large zoom in code editor
- Keep cursor movement deliberate
- Avoid switching too many windows
- Show only the happy path in the main cut
- Prepare backup screenshots for unstable provider demos

## Recommended Assets
- One polished diagram
- One short example file
- One sample payload
- One tracking timeline screen
- One docs landing screen

## Final Message
Basiq is not just another courier wrapper. It is a standardized logistics layer for the Philippine developer ecosystem.
