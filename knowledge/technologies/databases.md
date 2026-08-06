# Database Technologies

**Status:** Active  
**Authority:** core/contract.md · runtime/agents/architect.md · runtime/agents/tech-lead.md · knowledge/patterns/multi-tenancy.md  
**Last review:** 2026-08-06  
**Confidence:** High

Decision framework for selecting primary and supporting data stores.

The goal is a coherent, operable data layer—not the maximum number of specialised databases.

## Scope

- Relational (SQL) systems
- Document / JSON stores
- Key-value and cache stores
- Search and analytics engines
- Specialised stores (time-series, graph, vector) when justified

Out of scope: ORM vs query-builder debates (implementation detail), backup/restore runbooks (ops), and multi-tenancy isolation models (see patterns/multi-tenancy.md).

## Primary Decision Axes

1. **Data shape and access patterns** (relational integrity vs flexible documents vs pure key lookup)
2. **Consistency and transaction requirements**
3. **Scale and latency targets**
4. **Operational maturity of the team**
5. **Multi-tenancy and isolation needs**
6. **Ecosystem fit** with the chosen backend runtime

## Recommended Defaults

### Primary store for most business applications

**Prefer a relational database** (PostgreSQL as the strong default) when:

- Entities have relationships that must be consistent.
- Transactions, constraints, and declarative integrity matter.
- The team benefits from mature tooling, ORMs, migrations, and reporting.
- Complexity band S–L for the majority of product domains.

PostgreSQL covers relational data, JSONB for semi-structured fields, full-text search for moderate needs, and extensions (PostGIS, pgvector, etc.) that often remove the need for a second system.

**Acceptable alternatives:**
- MySQL / MariaDB when organisational standards or existing expertise dictate it.
- SQLite for single-node, embedded, or very small deployments (CLI tools, local-first, edge).

### Document stores

**Prefer when:**
- The dominant access pattern is document-centric and the schema is genuinely fluid.
- Horizontal scaling of the document model is a proven requirement.
- The team already operates the chosen engine at scale.

**Caution:** Document stores do not remove the need for data modelling; they change where the constraints live. Avoid them as a default “because JSON”.

### Key-value / cache

**Prefer Redis (or compatible) for:**
- Session stores, rate limiting, short-lived locks, pub/sub, and hot-path caches.
- Work queues when the primary runtime ecosystem supports it well.

Do not treat Redis as the system of record for durable business data unless the durability and persistence model has been explicitly accepted.

### Search

**Prefer dedicated search (OpenSearch, Elasticsearch, Typesense, Meilisearch, or PostgreSQL FTS) when:**
- Full-text relevance, faceting, or typo-tolerance exceed what the primary store can deliver.
- Search load must be isolated from transactional load.

Start with PostgreSQL full-text or a lightweight engine; introduce a dedicated search cluster only when metrics justify the operational cost.

### Specialised stores

| Need | Candidate | Justification required |
|------|-----------|------------------------|
| Vector / semantic search | pgvector, dedicated vector DB | Embedding workload and recall requirements |
| Time-series | TimescaleDB, dedicated TSDB | High cardinality + retention policies |
| Graph | Neo4j, PG + recursive CTEs | Traversal-heavy domain that SQL struggles with |
| Analytics / OLAP | Columnar store, lakehouse | Separation of analytical from transactional load |

Each specialised store adds operational surface. Record why the primary store is insufficient.

## Multi-Store Guidance

- **Default:** one primary relational store + Redis for cache/sessions if needed.
- Add a second durable store only when a clear bounded context and access pattern justify it.
- Keep dual-writes and eventual consistency explicit; prefer change-data-capture or outbox patterns over ad-hoc synchronisation.
- Document ownership: which service or module is the source of truth for each entity.

## Migration & Schema Management

- Prefer versioned, expandable migrations (expand/contract) over destructive changes in production paths.
- Schema changes must be compatible with zero-downtime deployment goals when the complexity band or availability targets require it.
- Record the migration tool (e.g., Flyway, Liquibase, golang-migrate, Prisma Migrate, Alembic, Diesel) and ownership.

## Essential Decision Points to Record

In the Architecture Blueprint (Technology Decisions or Data Architecture):

1. Primary data store and version posture
2. Supporting stores (cache, search, specialised) and the concrete reason each is required
3. Consistency model and transaction boundaries
4. Multi-tenancy data isolation approach (link to patterns/multi-tenancy.md)
5. Migration strategy and tooling
6. Backup / point-in-time recovery expectations (high-level)
7. Any polyglot persistence and the residual complexity accepted

## Anti-Patterns

- Defaulting to a document store “for flexibility” without measuring the cost of lost constraints.
- Introducing a search cluster before query patterns and volume justify it.
- Using the cache as a durable store.
- Multiple competing ORMs or migration tools inside the same system.
- Ignoring index and query performance until production load reveals problems.
- Treating multi-tenancy as a pure application concern while storing all tenants in a shared schema without isolation analysis.

## Related Knowledge

- [patterns/multi-tenancy.md](../patterns/multi-tenancy.md)
- [patterns/caching.md](../patterns/caching.md)
- [patterns/search.md](../patterns/search.md)
- [backend.md](backend.md)
- [deployment.md](deployment.md)
- [observability.md](observability.md)
