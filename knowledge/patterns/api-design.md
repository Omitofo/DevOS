# API Design

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, Tech Lead

Style, contracts, versioning, and evolution rules for interfaces exposed to clients or other services.

## Style Families

| Style | Characteristics | Prefer when |
|-------|-----------------|-------------|
| **REST / Resource-oriented** | Resources identified by URIs; standard HTTP methods; hypermedia optional | Broad client ecosystems, CRUD-heavy domains, public APIs |
| **RPC-style (JSON-RPC, gRPC, tRPC, etc.)** | Procedure-centric; often strongly typed | Internal service-to-service, high-performance needs, strongly typed clients |
| **GraphQL** | Client-specified queries; single endpoint | Complex, nested data needs; multiple client types with divergent data requirements |
| **Event / Async API** | Messages or events rather than request/response | Decoupled producers/consumers, long-running processes, high fan-out |

Hybrid surfaces (REST for public + gRPC for internal) are acceptable when the boundary is explicit.

## Preference Guidance

**Prefer REST when**
- The API is public or will be consumed by unknown third parties.
- Caching, intermediary proxies, and standard HTTP tooling are valuable.
- The domain maps naturally to resources and collections.

**Prefer GraphQL when**
- Clients have significantly different data shapes and over-fetching is a real cost.
- A single flexible query surface reduces coordination overhead between frontend and backend teams.
- Complexity band is M–L and the team can own the GraphQL operational concerns (N+1, complexity limits, caching).

**Prefer RPC / gRPC when**
- Latency and payload size matter (internal services, mobile under constrained networks).
- Strong contracts and code generation are desired.
- Streaming is a first-class need.

## Essential Decision Points

1. **Contract first**  
   - Define the interface (OpenAPI, Protobuf, GraphQL schema, AsyncAPI) before implementation.  
   - Contracts are the source of truth for generated clients and documentation.

2. **Versioning strategy**  
   - URI versioning (`/v1/`), header versioning, or content negotiation.  
   - Breaking changes require a new major version; additive changes may stay in the same version.  
   - Document the deprecation and sunset policy.

3. **Error model**  
   - Consistent error shape (code, message, details, correlation id).  
   - Map domain errors to appropriate HTTP status classes (or equivalent).  
   - Never leak internal stack traces or sensitive identifiers.

4. **Pagination, filtering, sorting**  
   - Cursor-based pagination preferred for large or volatile collections; offset acceptable for small, stable sets.  
   - Filtering and sorting must be explicit and documented; avoid unbounded queries.

5. **Idempotency**  
   - Unsafe methods that may be retried (payments, resource creation) must support idempotency keys or natural idempotent design.

6. **Security surface**  
   - Authentication and authorization are mandatory on every non-public endpoint.  
   - Rate limiting and abuse protection are part of the API contract, not an afterthought.

## Anti-Patterns

- “REST” that is actually RPC over HTTP with verbs in the path and no resource model.
- Returning 200 for application-level errors.
- Breaking changes without a version bump or migration path.
- Unbounded list endpoints with no pagination or filtering limits.
- Embedding authorization decisions only in the UI while the API remains open.

## Recording Requirements

In the Architecture Blueprint (Technology Decisions or Component interfaces):

- Primary API style(s) and the boundary between them
- Versioning and deprecation policy
- Error contract summary
- Link to this file and the criteria used

## Related Patterns

- [authentication.md](authentication.md) / [authorization.md](authorization.md)
- [caching.md](caching.md) — cacheability of responses
- knowledge/technologies/backend.md — concrete frameworks and tooling
