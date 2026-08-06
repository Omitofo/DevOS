# SaaS Blueprint

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** Architect, Planner, Tech Lead, Security & Quality Auditor

Opinionated starting architecture and decision framework for multi-user products delivered as a service.  
Use this blueprint when the primary product type is **SaaS Application** (see [knowledge/classification/website-types.md](../classification/website-types.md)).

This file does **not** prescribe a single stack. It supplies a coherent default shape, the critical decision points that must be resolved early, and the knowledge links agents are expected to follow.

## When to Apply

**Strong signals that this blueprint is appropriate**
- Users (or organisations) create accounts and return repeatedly.
- Core value is ongoing feature use rather than one-time conversion or content consumption.
- Organisations / workspaces, roles, and multi-tenancy are present or planned.
- Billing, plans, usage metering, or free-to-paid conversion is part of the product model.
- Onboarding, activation, retention, and feature adoption are first-class concerns.

**Counter-signals (consider a different blueprint)**
- Primary goal is lead capture or brand presentation → [landing-page.md](landing-page.md)
- Pure internal operational interface with no external customers → [dashboard.md](dashboard.md)
- Transactional catalogue + cart + fulfilment dominates → [ecommerce.md](ecommerce.md)

If the product is a clear hybrid (public marketing site + authenticated SaaS), treat the **SaaS surface** as the primary architectural driver and note the marketing surface as secondary.

## Recommended Starting Shape

A typical modern SaaS architecture that balances speed of delivery with operational sanity for complexity bands M–L:

| Layer | Default posture | Notes |
|-------|-----------------|-------|
| **Public surface** | Marketing / docs / pricing pages (SSG or hybrid) | Separate from the authenticated application where possible; share design system tokens |
| **Application shell** | Authenticated multi-tenant app (SSR or hybrid preferred) | Workspace / organisation as the primary tenancy unit |
| **API boundary** | Versioned HTTP API (REST or RPC-style) behind the same origin or a dedicated API domain | Prefer a single coherent API surface over many microservices at the start |
| **Identity** | First-party + optional social / enterprise IdP | See [patterns/authentication.md](../patterns/authentication.md) |
| **Tenancy** | Row-level (shared-everything) with strong enforcement, escalating to stronger isolation for higher tiers | See [patterns/multi-tenancy.md](../patterns/multi-tenancy.md) |
| **Data** | Primary relational store + optional specialised stores (search, analytics, files) | Start with one primary database; introduce others only when justified |
| **Background work** | Queue + workers for email, billing webhooks, heavy jobs, provisioning | Never block request paths on long-running work |
| **Billing** | External billing provider + local entitlement / plan cache | See [patterns/payments.md](../patterns/payments.md) |
| **Observability** | Structured logs, metrics, traces, error tracking, all tenant-tagged | Required from day one for supportability |

**Default complexity target:** Medium. The shape above scales to Large with disciplined boundaries; it is overkill for the smallest internal tools.

## Core Decision Points (must be resolved in Architecture Blueprint)

These decisions are high-leverage and expensive to reverse. Record the choice **and** the decisive criteria.

### 1. Tenancy model
- Organisation / workspace as the top-level tenant (recommended default) vs. purely user-centric.
- Isolation strength: shared-everything (row-level) vs. schema/database/silo per tenant or per tier.
- How the current tenant is identified (subdomain, path, header, token claim) and how it is bound to the authenticated principal.
- Link: [patterns/multi-tenancy.md](../patterns/multi-tenancy.md)

### 2. Identity & access
- Session vs. token vs. hybrid model.
- First-party credentials, social login, enterprise SSO (SAML/OIDC), passkeys.
- Role model: simple roles, RBAC, or more expressive policy.
- Invitation, seat management, and ownership transfer flows.
- Links: [patterns/authentication.md](../patterns/authentication.md), [patterns/authorization.md](../patterns/authorization.md)

### 3. Billing & entitlement
- Who owns the source of truth for plans and invoices (external provider vs. local).
- How entitlements are evaluated at runtime (feature flags, plan limits, usage counters).
- Free tier, trials, and conversion paths.
- Webhook handling, idempotency, and reconciliation.
- Link: [patterns/payments.md](../patterns/payments.md)

### 4. Rendering & delivery strategy
- Must align with [classification/rendering-strategies.md](../classification/rendering-strategies.md).
- Typical SaaS choice: hybrid (SSR/SSG for public + authenticated app shell) or fully authenticated SPA/CSR when SEO is irrelevant.
- Link: [technologies/frontend.md](../technologies/frontend.md)

### 5. Data ownership & boundaries
- What data is global vs. tenant-scoped vs. user-scoped.
- Soft-delete, hard-delete, export, and portability requirements (especially for regulated or enterprise customers).
- Audit / activity log expectations.

### 6. Background & asynchronous work
- Which operations are synchronous vs. queued.
- Retry, dead-letter, and observability strategy for workers.
- Provisioning and de-provisioning of tenant resources.

### 7. Observability & supportability
- Tenant-aware logging, metrics, and tracing from the first deploy.
- Support tooling needs (impersonation, audit trails, feature-flag overrides) and their security model.

## Technology Family Guidance

Prefer coherence over novelty. Record the concrete choices and the criteria that selected them.

| Concern | Preferred starting families (2026) | Avoid early unless justified |
|---------|------------------------------------|------------------------------|
| Frontend application | React meta-framework (Next.js App Router, Remix) or solid Vue/Svelte equivalents | Pure CSR SPA for public-facing surfaces; exotic frameworks with thin ecosystems |
| Backend / API | Type-safe full-stack or dedicated API layer (Node/TS, Go, or the language the team already ships reliably) | Premature microservices or polyglot explosion |
| Primary database | PostgreSQL (or compatible) with strong migration discipline | Multiple primary stores before the data model stabilises |
| Auth | Battle-tested library or managed service; passkeys where audience supports them | Homegrown crypto or session stores |
| Billing | Established provider (Stripe, Paddle, etc.) + local entitlement cache | Building a billing engine |
| Background jobs | Proven queue + worker (or platform equivalent) | Ad-hoc cron or fire-and-forget without observability |
| Search (if needed) | Managed or self-hosted search that supports tenant filters | Full-text only in the primary DB once scale or relevance demands more |
| File storage | Object storage with signed URLs / tenant-prefixed keys | Serving large binaries from the application servers |

Deeper criteria live in [knowledge/technologies/](../technologies/index.md).

## Essential Patterns to Apply

Every SaaS Architecture Blueprint should explicitly reference (and decide against where appropriate) at least:

- [authentication.md](../patterns/authentication.md)
- [authorization.md](../patterns/authorization.md)
- [multi-tenancy.md](../patterns/multi-tenancy.md)
- [payments.md](../patterns/payments.md) — when monetisation is in scope
- [api-design.md](../patterns/api-design.md)
- [caching.md](../patterns/caching.md) — especially tenant-aware keys
- [forms.md](../patterns/forms.md) and [file-uploads.md](../patterns/file-uploads.md) for common surfaces

Optional but frequent: [real-time.md](../patterns/real-time.md), [search.md](../patterns/search.md).

## Recommended Project Skeleton (high-level)

```
/
├── apps/ or packages/
│   ├── web/                 # public + authenticated frontend
│   ├── api/                 # or colocated with web in full-stack setups
│   └── workers/             # background jobs
├── packages/                # shared types, ui, config, etc.
├── docs/ or content/        # marketing / docs (may be separate deploy)
├── infrastructure/          # IaC, environments
└── ...
```

Exact layout is language- and monorepo-tool dependent; the important invariant is clear separation of public surface, authenticated application, API, and workers, plus a single source of truth for shared types and design tokens.

## Anti-Patterns

- Starting with microservices or multiple databases “for future scale”.
- Treating multi-tenancy as a later add-on; leakage is extremely costly to retrofit.
- Building a custom billing or subscription engine.
- Omitting tenant context from logs, metrics, caches, and background jobs.
- Making the public marketing site and the product application share a single, tightly coupled deployment unit without clear boundaries.
- Skipping background job infrastructure until the first long-running request already hurts latency.
- Designing roles and permissions after the UI is already built.
- Ignoring soft-delete, export, and erasure paths until a customer or regulator asks.

## Recording Requirements

In the project’s Architecture Blueprint the Architect must record at minimum:

```markdown
## Classification
- Primary type: SaaS Application
- Knowledge: knowledge/classification/website-types.md
- Blueprint: knowledge/blueprints/saas.md

## Tenancy
- Model: <row-level | schema | database | silo | hybrid>
- Identification: <subdomain | path | …>
- Enforcement: …

## Identity & Access
- Auth model: <session | token | hybrid>
- IdP approach: …
- Role / policy model: …

## Billing & Entitlement (if in scope)
- Provider: …
- Entitlement evaluation: …

## Key Technology Decisions
- Frontend family + rendering strategy
- Backend / API approach
- Primary datastore
- Background work approach
- Links to the decisive knowledge files and criteria
```

Deviations from this blueprint’s recommendations are allowed; they must be explicit and justified.

## Related Knowledge

- [classification/website-types.md](../classification/website-types.md) — type definition
- [classification/project-complexity.md](../classification/project-complexity.md) — sizing
- [classification/rendering-strategies.md](../classification/rendering-strategies.md)
- [classification/architecture-selection.md](../classification/architecture-selection.md)
- [patterns/](../patterns/index.md) — authentication, authorization, multi-tenancy, payments, api-design, …
- [technologies/](../technologies/index.md) — concrete stack criteria
- [standards/](../standards/index.md) — naming, documentation, testing conventions that apply to the resulting codebase

## Evolution Notes

This blueprint targets the common case of B2B or B2C multi-user SaaS with organisation-level tenancy.  
Specialised variants (marketplace-style multi-sided platforms, heavy real-time collaboration, regulated single-tenant enterprise) should extend or override sections rather than fork the entire file. When a recurring specialised shape appears, promote it to its own blueprint.
