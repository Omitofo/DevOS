# Multi-Tenancy

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, Tech Lead, Security & Quality Auditor

Tenant isolation models, data partitioning, and cross-cutting concerns that arise when a single deployment serves multiple independent customers (tenants).

## Isolation Models

| Model | Description | Isolation strength | Operational cost |
|-------|-------------|--------------------|------------------|
| **Shared everything (row-level)** | Single database/schema; every row carries a tenant_id; queries always filter by it | Logical | Lowest |
| **Shared database, separate schemas** | One database, schema per tenant | Logical + some administrative | Medium |
| **Separate databases** | Database (or cluster) per tenant | Stronger | Higher |
| **Separate deployments / silos** | Fully independent stacks per tenant or per tier | Strongest | Highest |

Hybrids (e.g. shared for free tier, siloed for enterprise) are common and should be documented explicitly.

## Preference Guidance

**Prefer shared everything (row-level) when**
- Tenant count is high and per-tenant data volume is modest.
- Cost efficiency and operational simplicity dominate.
- The team can enforce tenant filters rigorously (code review, query middleware, tests, row-level security).

**Prefer separate schemas or databases when**
- Regulatory, contractual, or noise-isolation requirements demand stronger boundaries.
- Per-tenant backup, restore, or performance isolation is required.
- Complexity band is L–XL and the operational overhead is accepted.

**Prefer siloed deployments when**
- Enterprise contracts require complete isolation or dedicated infrastructure.
- Custom domains, custom code, or wildly different scaling profiles exist per tenant.

## Essential Decision Points

1. **Tenant identification**  
   - How the current tenant is determined (subdomain, path, header, token claim, session).  
   - The identifier must be available early in the request lifecycle and must be unforgeable by the client.

2. **Enforcement of isolation**  
   - Application-level filters are necessary but not sufficient for high-assurance needs.  
   - Prefer additional database-level controls (RLS, separate credentials) where the risk justifies them.  
   - Cross-tenant queries must be impossible by construction for ordinary application paths.

3. **Shared vs. tenant-specific resources**  
   - Which data and configuration are global (feature flags, system users) vs. tenant-scoped.  
   - Global resources must not become accidental cross-tenant leakage points.

4. **Onboarding & provisioning**  
   - How a new tenant is created, what resources are allocated, and what the default configuration is.  
   - Idempotent provisioning and clear failure modes.

5. **Data export, deletion, and portability**  
   - Tenant off-boarding, GDPR-style erasure, and data export must be designed, not retrofitted.  
   - Soft-delete vs. hard-delete and retention windows.

6. **Noisy neighbour & quotas**  
   - Rate limits, storage quotas, and compute isolation (where applicable) prevent one tenant from degrading others.

7. **Observability**  
   - Logs, metrics, and traces must be taggable by tenant for support and debugging without leaking data across tenants.

## Anti-Patterns

- Forgetting the tenant filter on even one query path (classic data leak).
- Using a tenant identifier that the client can freely set without server-side validation against the authenticated principal.
- Mixing tenant data in caches without including tenant_id in the cache key.
- Treating “admin” super-users as exempt from isolation without explicit, audited break-glass procedures.
- Designing multi-tenancy after the data model is already mature and widely used.

## Recording Requirements

In the Architecture Blueprint:

- Chosen isolation model (and any hybrid tiers)
- Tenant identification mechanism
- Enforcement strategy (application + database controls)
- Provisioning and off-boarding approach
- Link to this file and the decisive criteria

## Related Patterns

- [authentication.md](authentication.md) — establishing the principal that carries tenant membership
- [authorization.md](authorization.md) — permissions evaluated inside a tenant context
- [caching.md](caching.md) — tenant-aware cache keys
- [search.md](search.md) — tenant-scoped indexes or filters
- knowledge/technologies/ — concrete database and isolation features
