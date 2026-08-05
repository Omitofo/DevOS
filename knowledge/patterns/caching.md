# Caching

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, Tech Lead

Caching layers, invalidation strategies, and consistency implications.

## Cache Layers (common stack)

| Layer | Location | Typical lifetime | Invalidation difficulty |
|-------|----------|------------------|-------------------------|
| **Browser / CDN** | Edge or client | Seconds to days | Easy for static; hard for personalized |
| **Application / reverse-proxy** | In-process or shared (Redis, Memcached, Varnish) | Seconds to hours | Moderate |
| **Database query / result** | DB or ORM level | Seconds to minutes | Can be subtle |
| **Computed / derived** | Application or dedicated store | Minutes to hours | Must track source dependencies |

Not every project needs every layer. Start from the simplest that meets the NFR.

## Preference Guidance

**Prefer CDN / edge caching when**
- Content is public or can be varied by a small set of headers (language, device class).
- Latency to end users is a primary NFR.
- Static or semi-static assets dominate.

**Prefer shared application cache (Redis etc.) when**
- Multiple application instances must see the same cached values.
- Cache is used for session data, rate-limit counters, or frequently read domain objects.
- Explicit TTLs and key design can be owned by the team.

**Prefer in-process cache when**
- The data is process-local and eventual consistency across instances is acceptable.
- Extremely high read volume on a small working set.

**Avoid deep caching when**
- Data changes frequently and stale reads are costly or incorrect.
- Consistency requirements are strong (financial balances, inventory under contention).

## Essential Decision Points

1. **What is cached**  
   - Identify the hot read paths and the cost of a cache miss.  
   - Prefer caching pure, derived, or slowly changing data.

2. **Key design**  
   - Keys must encode the full identity of the cached value (tenant, user, locale, version, etc.).  
   - Collision or missing dimensions produce silent incorrectness.

3. **TTL vs. explicit invalidation**  
   - TTL alone is simple but can serve stale data.  
   - Explicit invalidation (or event-driven) is more precise but requires reliable propagation.  
   - Hybrid (short TTL + invalidation on write) is often the practical sweet spot.

4. **Consistency model**  
   - Document whether readers may see stale data and for how long.  
   - For strong consistency needs, caching may be inappropriate or must be bypassed on the critical path.

5. **Stampede / thundering herd protection**  
   - On popular keys, coordinate population (locks, single-flight, probabilistic early expiry).

6. **Observability**  
   - Hit rate, eviction rate, and latency contribution of the cache layer must be measurable.

## Anti-Patterns

- Caching user-specific or permission-sensitive data without including those dimensions in the key.
- Infinite or extremely long TTLs with no invalidation path.
- Treating the cache as a source of truth rather than a performance optimisation.
- Nested caches with opaque invalidation chains that no one can reason about.
- Ignoring cache behaviour under partial failure (cache down → cascade to origin).

## Recording Requirements

In the Architecture Blueprint (NFR Mapping or Technology Decisions):

- Which layers are introduced and for which data classes
- TTL / invalidation strategy per class
- Accepted staleness window
- Link to this file and the decisive criteria

## Related Patterns

- [api-design.md](api-design.md) — cacheability of HTTP responses
- [real-time.md](real-time.md) — when push is preferable to cache + poll
- knowledge/technologies/ — concrete cache products
