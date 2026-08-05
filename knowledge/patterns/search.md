# Search

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, Tech Lead

Full-text, faceted, filtered, and semantic search patterns.

## Approach Families

| Approach | Description | Prefer when |
|----------|-------------|-------------|
| **Database native** | LIKE / full-text indexes of the primary store (Postgres tsvector, MySQL FULLTEXT, etc.) | Small-to-medium corpora, simple ranking needs, strong consistency with primary data |
| **Dedicated search engine** | Elasticsearch, OpenSearch, Typesense, Meilisearch, etc. | Larger corpora, relevance tuning, facets, typo tolerance, high query volume |
| **Managed search service** | Algolia, Typesense Cloud, Elasticsearch Service, etc. | Team wants to avoid operating the search cluster |
| **Vector / semantic** | Embeddings + vector store (or hybrid with keyword) | Natural-language queries, “find similar”, RAG-style retrieval |
| **Hybrid** | Keyword + vector, or primary DB + search engine | Best relevance across exact and conceptual matches |

## Preference Guidance

**Prefer database-native when**
- Document count and query volume are modest.
- Freshness must match the primary database transactionally (or near-transactionally).
- Operational simplicity is valued and relevance requirements are basic.

**Prefer a dedicated search engine when**
- Relevance, faceting, highlighting, or typo tolerance are product features.
- The primary database is not optimised for the expected query patterns.
- Indexing can be asynchronous with an accepted lag window.

**Prefer vector / semantic when**
- Users express intent in natural language rather than keywords.
- “Similar items” or conceptual retrieval is core to the experience.
- The team can own embedding generation, model choice, and evaluation.

## Essential Decision Points

1. **Source of truth & indexing**  
   - Primary data remains in the system of record.  
   - Search index is a derived view; define the lag tolerance and the indexing trigger (sync, async, CDC, batch).

2. **Schema & mapping**  
   - Which fields are searchable, filterable, sortable, facetable.  
   - Analysers, tokenisers, and language support must be explicit.

3. **Relevance & ranking**  
   - Default ranking (BM25, custom signals, learning-to-rank).  
   - Business rules that boost or bury results must be documented and testable.

4. **Security & multi-tenancy**  
   - Search results must respect the same authorization model as the rest of the product.  
   - Tenant isolation must be enforced at query time (filters) or by index partitioning.

5. **Observability & feedback**  
   - Query latency, zero-result rate, and click-through or conversion signals should be measurable.  
   - Zero-result and low-relevance queries are product opportunities.

6. **Fallbacks**  
   - Behaviour when the search service is unavailable (degrade to DB, show cached, or clear error).

## Anti-Patterns

- Running expensive LIKE '%term%' queries on large unindexed tables at request time.
- Indexing sensitive fields that should never appear in search results.
- Ignoring tenant or permission filters in search queries.
- Treating the search index as a writable source of truth.
- Launching semantic search without an evaluation set or quality metric.

## Recording Requirements

In the Architecture Blueprint:

- Chosen approach (DB-native, dedicated engine, vector, hybrid)
- Indexing strategy and accepted lag
- Authorization / tenant filter approach
- Link to this file and the criteria used

## Related Patterns

- [multi-tenancy.md](multi-tenancy.md) — tenant-scoped search
- [authorization.md](authorization.md) — result filtering
- [caching.md](caching.md) — caching of popular queries or facets
- knowledge/technologies/ — concrete search products
