# Ecommerce Blueprint

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** Architect, Planner, Tech Lead, Security & Quality Auditor

Opinionated starting architecture and decision framework for transactional catalogue + cart + checkout products.  
Use this blueprint when the primary product type is **Ecommerce / Storefront** (see [knowledge/classification/website-types.md](../classification/website-types.md)).

This file does **not** prescribe a single stack. It supplies a coherent default shape, the critical decision points that must be resolved early, and the knowledge links agents are expected to follow.

## When to Apply

**Strong signals that this blueprint is appropriate**
- Core value is browsing a catalogue, adding items to a cart, and completing a purchase (or booking).
- Inventory, pricing, promotions, taxes, shipping, and order fulfilment are first-class concerns.
- Payment processing, fraud signals, and post-purchase order management are required.
- Guest checkout and/or account-based purchase history are expected.
- Product discovery (search, filters, collections, merchandising) drives conversion.

**Counter-signals (consider a different blueprint)**
- Primary goal is lead capture or brand presentation with no transactional cart → [landing-page.md](landing-page.md)
- Persistent multi-user workspaces, collaboration, or subscription feature access dominates → [saas.md](saas.md)
- Showcase of work / case studies with editorial focus and no cart → [portfolio.md](portfolio.md)
- Pure internal operational interface for staff (order admin, inventory tools) with no customer-facing storefront → [dashboard.md](dashboard.md)
- Organised, searchable, versioned knowledge is the core value → [documentation-site.md](documentation-site.md)

If the product is a clear hybrid (public marketing site + transactional storefront, or storefront + subscription SaaS), treat the **transactional commerce surface** as the primary architectural driver and note secondary surfaces explicitly.

## Recommended Starting Shape

A typical modern ecommerce architecture that balances conversion performance, operational reliability, and merchandising flexibility for complexity bands M–L:

| Layer | Default posture | Notes |
|-------|-----------------|-------|
| **Storefront** | High-performance public catalogue + cart + checkout (SSG/ISR for product & collection pages + selective SSR or edge for personalised / cart state) | Core Web Vitals and perceived speed are conversion-critical |
| **Admin / back-office** | Authenticated operational interface for products, orders, inventory, promotions, customers | Can share design system; often a separate deploy unit or protected route group |
| **API boundary** | Versioned HTTP API (or full-stack server actions + dedicated endpoints) for cart, checkout, orders, and admin mutations | Prefer coherent commerce API surface; avoid premature microservices |
| **Catalogue & inventory** | Primary relational store for products, variants, pricing, stock; optional search index for discovery | Single source of truth for sellable items; eventual consistency only where deliberately chosen |
| **Cart & session** | Server-side or hybrid cart with clear guest → authenticated merge rules | Never lose cart on login; support both guest and account checkout |
| **Payments** | External payment provider (PCI scope reduction) + local order & payment-intent records | See [patterns/payments.md](../patterns/payments.md) |
| **Orders & fulfilment** | Durable order aggregate with state machine; background workers for fulfilment, notifications, inventory adjustments | Never block checkout response on slow downstream systems |
| **Search & discovery** | Dedicated search (or highly optimised primary DB + filters) with faceting, ranking, and merchandising rules | Critical for conversion once catalogue size grows |
| **Media** | Object storage + CDN + image transformation pipeline for product imagery | Aggressive optimisation and responsive variants required |
| **Observability** | Structured logs, metrics, traces, error tracking; order- and customer-tagged where possible | Supportability and fraud investigation depend on it |

**Default complexity target:** Medium. The shape above scales to Large with disciplined boundaries (especially around inventory, promotions, and multi-channel). It is overkill for a simple single-product or very small catalogue site.

## Core Decision Points (must be resolved in Architecture Blueprint)

These decisions are high-leverage and expensive to reverse. Record the choice **and** the decisive criteria.

### 1. Catalogue & product model
- Simple products vs. variants (size/colour/SKU), bundles, configurable products, digital goods, or subscriptions-as-products.
- Pricing model: fixed, tiered, customer-group, dynamic, or promotional overlays.
- Inventory ownership: single warehouse, multi-location, or external OMS; reservation vs. soft-hold vs. real-time decrement strategy.
- How product data is authored and published (admin UI, import, headless PIM).

### 2. Cart, session & identity
- Guest cart persistence mechanism and lifetime.
- Rules for merging guest cart into authenticated cart on login / signup.
- Whether accounts are required, optional, or incentivised; passwordless / social options.
- Link: [patterns/authentication.md](../patterns/authentication.md)

### 3. Checkout & payments
- Single-page vs. multi-step checkout; address, shipping, payment, review sequence.
- Payment provider(s), capture strategy (auth + capture vs. immediate), and support for wallets / local methods.
- Tax calculation (built-in, provider, or external tax service) and shipping rate calculation.
- Idempotency, retry, and failure recovery for payment intents and order creation.
- Link: [patterns/payments.md](../patterns/payments.md)

### 4. Order lifecycle & fulfilment
- Order state machine (created → paid → fulfilled / cancelled / refunded, plus custom states).
- Who owns fulfilment (internal, 3PL, drop-ship) and how status updates flow back.
- Inventory adjustment timing relative to payment and fulfilment.
- Refunds, partial refunds, exchanges, and cancellation policies and their technical enforcement.

### 5. Rendering, performance & conversion
- Must align with [classification/rendering-strategies.md](../classification/rendering-strategies.md).
- Typical ecommerce choice: hybrid (SSG/ISR for catalogue + selective dynamic for cart, personalised pricing, or inventory-sensitive pages).
- Explicit Core Web Vitals and conversion-path performance budgets.
- Link: [technologies/frontend.md](../technologies/frontend.md)

### 6. Search, filtering & merchandising
- Search engine choice and ranking / relevance strategy.
- Faceted navigation, collections, and curated merchandising rules.
- Personalisation scope (if any) and its data/privacy implications.

### 7. Multi-channel, multi-store, or multi-currency (if in scope)
- Whether the system must support multiple storefronts, currencies, languages, or sales channels from day one.
- Isolation and configuration model for each.

### 8. Observability, fraud & supportability
- Order- and payment-intent correlation across logs and traces.
- Fraud signal capture points and integration with external tools if needed.
- Admin tooling requirements (order search, customer lookup, manual adjustments) and their audit model.

## Technology Family Guidance

Prefer coherence and battle-tested commerce primitives over novelty. Record the concrete choices and the criteria that selected them.

| Concern | Preferred starting families (2026) | Avoid early unless justified |
|---------|------------------------------------|------------------------------|
| Storefront / frontend | React meta-framework with strong SSG/ISR + server capabilities (Next.js App Router, Remix, or solid Vue/Svelte equivalents) or purpose-built commerce storefronts | Pure CSR SPA for the primary catalogue and product pages; heavy client-only state for cart |
| Backend / API | Type-safe full-stack or dedicated API layer (Node/TS, Go, or the language the team already ships reliably) | Premature microservices or polyglot explosion around catalogue / cart / orders |
| Primary database | PostgreSQL (or compatible) with strong migration discipline and careful transaction boundaries around inventory and orders | Multiple primary stores before the data model stabilises |
| Search | Managed search or highly capable primary-DB full-text + filters; introduce dedicated search when relevance or scale demands it | Building a custom ranking engine |
| Payments | Established provider (Stripe, Adyen, etc.) with webhook-driven reconciliation | Building a payment gateway or storing card data |
| Media / images | Object storage + CDN + on-the-fly or build-time transformation | Serving large product images from application servers |
| Background work | Proven queue + workers for order processing, emails, inventory sync, webhooks | Fire-and-forget without observability or retry |
| Admin UI | Same design system as storefront or a focused internal tool; protect with strong auth | Exposing powerful admin mutations without audit trails |

Deeper criteria live in [knowledge/technologies/](../technologies/index.md).

## Essential Patterns to Apply

Every Ecommerce Architecture Blueprint should explicitly reference (and decide against where appropriate) at least:

- [payments.md](../patterns/payments.md)
- [authentication.md](../patterns/authentication.md) — guest + account flows
- [authorization.md](../patterns/authorization.md) — especially admin and customer data access
- [api-design.md](../patterns/api-design.md)
- [forms.md](../patterns/forms.md) — checkout, address, account
- [file-uploads.md](../patterns/file-uploads.md) — product media
- [caching.md](../patterns/caching.md) — catalogue, pricing, and inventory-aware caches
- [search.md](../patterns/search.md) — when catalogue discovery is non-trivial

Optional but frequent: [real-time.md](../patterns/real-time.md) (inventory or order status), [multi-tenancy.md](../patterns/multi-tenancy.md) (multi-store / marketplace variants).

## Recommended Project Skeleton (high-level)

```
/
├── apps/ or packages/
│   ├── storefront/          # public catalogue, cart, checkout
│   ├── admin/               # operational back-office (or route group)
│   ├── api/                 # or colocated commerce API surface
│   └── workers/             # order processing, notifications, sync jobs
├── packages/                # shared types, ui, config, pricing/tax helpers
├── content/ or cms/         # marketing / editorial content (may be separate)
├── infrastructure/          # IaC, environments, secrets
└── ...
```

Exact layout is language- and monorepo-tool dependent; the important invariants are clear separation of public storefront, admin surface, commerce API/mutations, and asynchronous work, plus a single source of truth for product, cart, and order models and design tokens.

## Anti-Patterns

- Treating inventory and order creation as simple database writes without transactional guarantees or clear reservation semantics.
- Building a custom payment or tax engine.
- Making the cart purely client-side with no server-side durability or guest-merge strategy.
- Coupling the public storefront and high-privilege admin UI so tightly that a storefront compromise escalates easily.
- Ignoring Core Web Vitals and image performance on product and collection pages.
- Starting with microservices or multiple databases “for future scale” before the commerce domain model is stable.
- Omitting webhook idempotency, reconciliation, and dead-letter handling for payments and fulfilment events.
- Designing promotions, discounts, and pricing rules as ad-hoc code instead of an explicit, testable model.
- Skipping audit trails for order adjustments, refunds, and inventory changes.
- Assuming a single currency, single warehouse, and single language will remain true without recording the assumption.

## Recording Requirements

In the project’s Architecture Blueprint the Architect must record at minimum:

```markdown
## Classification
- Primary type: Ecommerce / Storefront
- Knowledge: knowledge/classification/website-types.md
- Blueprint: knowledge/blueprints/ecommerce.md

## Catalogue & Inventory
- Product model: <simple | variants | configurable | …>
- Inventory strategy: …
- Pricing model: …

## Cart & Identity
- Guest cart: …
- Account model: …
- Merge rules: …

## Checkout & Payments
- Provider(s): …
- Capture strategy: …
- Tax / shipping approach: …

## Order Lifecycle
- State machine summary: …
- Fulfilment ownership: …

## Key Technology Decisions
- Storefront rendering strategy + performance budget
- Backend / API approach
- Primary datastore + search approach
- Background work approach
- Links to the decisive knowledge files and criteria
```

Deviations from this blueprint’s recommendations are allowed; they must be explicit and justified.

## Related Knowledge

- [classification/website-types.md](../classification/website-types.md) — type definition
- [classification/project-complexity.md](../classification/project-complexity.md) — sizing
- [classification/rendering-strategies.md](../classification/rendering-strategies.md)
- [classification/architecture-selection.md](../classification/architecture-selection.md)
- [patterns/](../patterns/index.md) — payments, authentication, authorization, api-design, forms, search, caching, …
- [technologies/](../technologies/index.md) — concrete stack criteria
- [design/](../design/index.md) — composition, storytelling, motion, and accessibility patterns that affect conversion
- [standards/](../standards/index.md) — naming, documentation, testing conventions that apply to the resulting codebase

## Evolution Notes

This blueprint targets the common case of a single-store or small multi-store ecommerce business with a customer-facing catalogue, cart, and checkout.  
Specialised variants (multi-sided marketplaces, heavy B2B quoting + approval flows, pure digital-goods platforms, or highly regulated industries) should extend or override sections rather than fork the entire file. When a recurring specialised shape appears, promote it to its own blueprint.
