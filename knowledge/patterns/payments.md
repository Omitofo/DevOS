# Payments

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, Tech Lead, Security & Quality Auditor

Payment orchestration, provider integration, webhooks, reconciliation, and compliance boundaries.

**Important:** DevOS does not implement payment processing. This pattern defines the engineering decisions that must be recorded so that a later implementation session can be correct and auditable.

## Core Responsibilities (system under design)

| Responsibility | Notes |
|----------------|-------|
| **Checkout initiation** | Create a payment intent / session with the provider |
| **Client confirmation** | Collect payment method details via provider-hosted or PCI-compliant fields |
| **Webhook / callback handling** | Authoritative source of payment state changes |
| **Internal order / entitlement state** | Map provider events to product entitlements |
| **Reconciliation & reporting** | Detect mismatches between provider and internal records |
| **Refunds & disputes** | Controlled flows with audit trail |

## Preference Guidance

**Prefer a mature payment service provider (PSP) when**
- Card / wallet / local payment methods are required.
- PCI scope must be minimised (use provider-hosted fields, Checkout, or Elements-style components).
- Complexity band is any size; rolling your own card handling is almost never justified.

**Prefer provider-hosted checkout when**
- Speed to market and minimal PCI burden are primary.
- Customisation needs are modest.

**Prefer embedded / Elements-style when**
- Brand control and UX continuity justify the additional integration work.
- The team can still keep sensitive data out of their own servers.

## Essential Decision Points

1. **Provider selection criteria**  
   - Supported geographies and payment methods.  
   - Pricing, settlement, and dispute tooling.  
   - Webhook reliability and idempotency support.  
   - Record the chosen provider and the decisive criteria (do not invent requirements).

2. **Source of truth for payment state**  
   - Provider events (via webhooks) are the authoritative driver of paid / failed / refunded state.  
   - Internal records must be updated from those events, not from client-side success callbacks alone.

3. **Idempotency**  
   - Creating a payment intent and processing webhooks must be idempotent.  
   - Use idempotency keys for client-initiated actions that may be retried.

4. **Webhook security**  
   - Verify signatures.  
   - Reject replays outside an acceptable window.  
   - Process asynchronously where possible; acknowledge quickly.

5. **Entitlement mapping**  
   - Define how a successful payment maps to product access (subscription period, one-time unlock, credit, etc.).  
   - Handle partial failures (payment succeeded, entitlement grant failed) with a recovery path.

6. **PCI & data minimisation**  
   - Never store raw card numbers or CVV.  
   - Prefer tokenisation and provider-side storage.  
   - Document the residual PCI scope.

7. **Refunds, cancellations, disputes**  
   - Explicit flows with who may initiate and what internal state changes.  
   - Dispute / chargeback handling must surface to operations.

## Anti-Patterns

- Trusting only the client-side “payment succeeded” redirect or callback.
- Storing card data on application servers.
- Non-idempotent webhook handlers that double-grant entitlements.
- Ignoring reconciliation; assuming provider and internal state never diverge.
- Hard-coding a single provider’s data model into core domain entities without an anti-corruption layer.

## Recording Requirements

In the Architecture Blueprint:

- Chosen PSP and integration style (hosted vs. embedded)
- Webhook handling and signature verification approach
- How payment events map to entitlements
- Idempotency and reconciliation strategy
- Residual PCI scope statement
- Link to this file and the criteria used

## Related Patterns

- [authentication.md](authentication.md) / [authorization.md](authorization.md) — who may pay and who receives entitlements
- [api-design.md](api-design.md) — webhook endpoints and error contracts
- [multi-tenancy.md](multi-tenancy.md) — tenant-scoped billing where applicable
- knowledge/technologies/ — concrete provider SDKs and tooling
