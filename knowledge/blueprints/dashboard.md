# Dashboard Blueprint

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** Architect, Planner, Tech Lead, Security & Quality Auditor

Opinionated starting architecture and decision framework for authenticated operational interfaces.  
Use this blueprint when the primary product type is **Dashboard / Internal Tool** (see [knowledge/classification/website-types.md](../classification/website-types.md)).

This file does **not** prescribe a single stack. It supplies a coherent default shape, the critical decision points that must be resolved early, and the knowledge links agents are expected to follow.

## When to Apply

**Strong signals that this blueprint is appropriate**
- Almost every screen requires authentication; there is little or no public marketing surface.
- Primary users are known (employees, operators, partners, admins) rather than anonymous visitors.
- Core value is viewing metrics, managing operational data, performing workflows, or administering systems.
- Data tables, filters, charts, forms, and CRUD dominate the interface.
- Correctness, freshness, and supportability of data presentation are first-class concerns.
- Role-based or permission-gated views are expected from the start.

**Counter-signals (consider a different blueprint)**
- Primary goal is lead capture or brand presentation → [landing-page.md](landing-page.md)
- Multi-user product with ongoing feature use, accounts, and usually billing → [saas.md](saas.md)
- Transactional catalogue + cart + fulfilment dominates → [ecommerce.md](ecommerce.md)
- Pure content / documentation delivery → [documentation-site.md](documentation-site.md)

If the product is a hybrid (e.g. a SaaS product that also has a heavy internal admin console), treat the **SaaS surface** as the primary architectural driver and apply this blueprint’s decision points selectively to the admin surface. Record the dual nature explicitly.

## Recommended Starting Shape

A typical modern dashboard architecture that balances speed of delivery with operational sanity for complexity bands S–L:

| Layer | Default posture | Notes |
|-------|-----------------|-------|
| **Entry** | Authenticated app shell (SSR or hybrid preferred) | No public marketing surface, or a minimal login / status page only |
| **Application shell** | Role-aware layout with navigation, global filters, and contextual actions | Sidebar or top-nav patterns; permission-gated menu items |
| **API boundary** | Versioned HTTP API (REST or RPC-style) behind the same origin or a dedicated internal domain | Prefer a single coherent API surface; avoid premature microservices |
| **Identity** | First-party credentials, SSO (SAML/OIDC), or enterprise IdP | See [patterns/authentication.md](../patterns/authentication.md) |
| **Authorization** | Explicit roles or policy checks on every sensitive action and data scope | See [patterns/authorization.md](../patterns/authorization.md) |
| **Data** | Primary relational store + optional specialised stores (search, time-series, analytics) | Start with one primary database; introduce others only when justified |
| **Background work** | Queue + workers for exports, reports, bulk operations, notifications | Never block request paths on long-running work |
| **Observability** | Structured logs, metrics, traces, error tracking, all user- and role-tagged | Required from day one for supportability and audit |
| **Audit** | Immutable activity / change log for sensitive actions | Especially important for internal tools that touch production data |

**Default complexity target:** Small-to-Medium. Many internal tools stay small; the shape above scales to Large with disciplined boundaries. Avoid over-engineering the smallest single-team tools.

## Core Decision Points (must be resolved in Architecture Blueprint)

These decisions are high-leverage and expensive to reverse. Record the choice **and** the decisive criteria.

### 1. Audience & trust boundary
- Who are the users (employees only, partners, customers with limited admin access)?
- Is the tool internal-only, or does it cross organisational boundaries?
- Network exposure: private VPC / VPN, IP allow-list, or public internet with strong auth.
- Link: [patterns/authentication.md](../patterns/authentication.md)

### 2. Identity & access
- Session vs. token vs. hybrid model.
- First-party credentials, enterprise SSO (SAML/OIDC), passkeys, or service accounts.
- Role model: simple roles, RBAC, or more expressive policy (ABAC / ReBAC).
- How permissions are evaluated at the UI (hide vs. disable) and at the API (enforce).
- Impersonation / “view as” support needs and its security model.
- Links: [patterns/authentication.md](../patterns/authentication.md), [patterns/authorization.md](../patterns/authorization.md)

### 3. Data scope & multi-tenancy (if applicable)
- Is data global, organisation-scoped, team-scoped, or user-scoped?
- If multi-tenant: isolation strength and how the current scope is identified and bound.
- Row-level security or equivalent enforcement expectations.
- Link: [patterns/multi-tenancy.md](../patterns/multi-tenancy.md) (when relevant)

### 4. Rendering & delivery strategy
- Must align with [classification/rendering-strategies.md](../classification/rendering-strategies.md).
- Typical dashboard choice: authenticated SSR/hybrid or SPA/CSR (SEO is usually irrelevant).
- Preference for server-driven UI when data freshness and permission gating matter heavily.
- Link: [technologies/frontend.md](../technologies/frontend.md)

### 5. Data presentation & interaction model
- Dominant patterns: tables with filters/sort/pagination, charts, detail drawers, multi-step forms, bulk actions.
- Real-time or near-real-time requirements (WebSocket, polling, SSE).
- Export, print, and scheduled-report needs.
- Links: [patterns/forms.md](../patterns/forms.md), [patterns/search.md](../patterns/search.md), [patterns/real-time.md](../patterns/real-time.md)

### 6. Background & asynchronous work
- Which operations are synchronous vs. queued (exports, bulk updates, report generation).
- Retry, dead-letter, and observability strategy for workers.
- User-facing status and notification of long-running jobs.

### 7. Observability, audit & supportability
- User- and role-aware logging, metrics, and tracing from the first deploy.
- Immutable audit trail for sensitive mutations and access to sensitive data.
- Support tooling needs (impersonation, feature-flag overrides, data inspection) and their security model.
- Retention and access policy for logs and audit records.

### 8. Environment & deployment posture
- Single environment vs. multiple (dev / staging / prod) with clear promotion path.
- Secrets management and least-privilege service accounts.
- Backup, restore, and disaster-recovery expectations for operational data.

## Technology Family Guidance

Prefer coherence over novelty. Record the concrete choices and the criteria that selected them.

| Concern | Preferred starting families (2026) | Avoid early unless justified |
|---------|------------------------------------|------------------------------|
| Frontend application | React meta-framework (Next.js App Router, Remix) or solid Vue/Svelte equivalents; strong component libraries for tables/charts | Exotic frameworks with thin ecosystems; pure CSR when server-driven permission gating is simpler |
| Backend / API | Type-safe full-stack or dedicated API layer (Node/TS, Go, or the language the team already ships reliably) | Premature microservices or polyglot explosion |
| Primary database | PostgreSQL (or compatible) with strong migration discipline | Multiple primary stores before the data model stabilises |
| Auth | Battle-tested library or managed service; enterprise SSO when the audience requires it | Homegrown crypto or session stores |
| Tables / data grids | Mature, accessible data-table components with server-side pagination/filtering support | Building a full data-grid from scratch |
| Charts / visualisation | Established charting library that supports the required chart types and accessibility | Over-custom visualisation engines for simple metric displays |
| Background jobs | Proven queue + worker (or platform equivalent) | Ad-hoc cron or fire-and-forget without observability |
| Search (if needed) | Managed or self-hosted search that supports permission filters | Full-text only in the primary DB once scale or relevance demands more |
| File storage / exports | Object storage with signed URLs; careful handling of sensitive exports | Serving large or sensitive binaries from the application servers |

Deeper criteria live in [knowledge/technologies/](../technologies/index.md).

## Essential Patterns to Apply

Every Dashboard Architecture Blueprint should explicitly reference (and decide against where appropriate) at least:

- [authentication.md](../patterns/authentication.md)
- [authorization.md](../patterns/authorization.md)
- [api-design.md](../patterns/api-design.md)
- [forms.md](../patterns/forms.md)
- [caching.md](../patterns/caching.md) — especially permission-aware and freshness-aware keys

Optional but frequent:
- [multi-tenancy.md](../patterns/multi-tenancy.md) — when data is organisation- or team-scoped
- [search.md](../patterns/search.md)
- [real-time.md](../patterns/real-time.md)
- [file-uploads.md](../patterns/file-uploads.md) — for imports or attachments

## Recommended Project Skeleton (high-level)

```
/
├── apps/ or packages/
│   ├── web/                 # authenticated dashboard frontend
│   ├── api/                 # or colocated with web in full-stack setups
│   └── workers/             # background jobs (exports, reports, bulk ops)
├── packages/                # shared types, ui, config, design tokens
├── infrastructure/          # IaC, environments, secrets
└── ...
```

Exact layout is language- and monorepo-tool dependent; the important invariant is clear separation of the authenticated application, API, and workers, plus a single source of truth for shared types, permissions, and design tokens. Public marketing content is intentionally absent or minimal.

## Anti-Patterns

- Treating authorization as a UI-only concern; every sensitive API endpoint and data query must enforce it.
- Omitting audit trails for mutations and access to sensitive operational data.
- Building a custom data-grid or charting engine when mature libraries already solve the problem.
- Blocking the request path on long-running exports or bulk operations.
- Ignoring soft-delete, export, and retention policies until a compliance or support incident forces them.
- Designing roles and permissions after the screens are already built.
- Sharing the same deployment unit and credentials between a public marketing site and a privileged internal tool without clear boundaries.
- Skipping tenant / scope context from logs, metrics, caches, and background jobs when multi-tenancy is present.
- Over-engineering a simple single-team tool with microservices, multiple databases, or complex event architectures.

## Recording Requirements

In the project’s Architecture Blueprint the Architect must record at minimum:

```markdown
## Classification
- Primary type: Dashboard / Internal Tool
- Knowledge: knowledge/classification/website-types.md
- Blueprint: knowledge/blueprints/dashboard.md

## Audience & Trust Boundary
- Users: <employees | partners | …>
- Exposure: <private | allow-listed | public + strong auth>
- …

## Identity & Access
- Auth model: <session | token | hybrid>
- IdP approach: …
- Role / policy model: …
- Impersonation: <yes/no + constraints>

## Data Scope
- Scope model: <global | organisation | team | user | hybrid>
- Multi-tenancy (if any): …

## Key Technology Decisions
- Frontend family + rendering strategy
- Backend / API approach
- Primary datastore
- Background work approach
- Audit / activity-log approach
- Links to the decisive knowledge files and criteria
```

Deviations from this blueprint’s recommendations are allowed; they must be explicit and justified.

## Related Knowledge

- [classification/website-types.md](../classification/website-types.md) — type definition
- [classification/project-complexity.md](../classification/project-complexity.md) — sizing
- [classification/rendering-strategies.md](../classification/rendering-strategies.md)
- [classification/architecture-selection.md](../classification/architecture-selection.md)
- [patterns/](../patterns/index.md) — authentication, authorization, api-design, forms, multi-tenancy, …
- [technologies/](../technologies/index.md) — concrete stack criteria
- [standards/](../standards/index.md) — naming, documentation, testing conventions that apply to the resulting codebase
- [design/](../design/index.md) — composition, typography, spacing, accessibility patterns relevant to dense operational UIs

## Evolution Notes

This blueprint targets the common case of authenticated internal or partner-facing operational tools with tables, charts, forms, and role-based access.  
Specialised variants (real-time operations centres, heavy analytics workbenches, multi-sided partner portals) should extend or override sections rather than fork the entire file. When a recurring specialised shape appears, promote it to its own blueprint.
