# Backend Technologies

**Status:** Active  
**Authority:** core/contract.md · runtime/agents/architect.md · runtime/agents/tech-lead.md · knowledge/classification/architecture-selection.md  
**Last review:** 2026-08-06  
**Confidence:** High

Decision framework for selecting application runtimes, API frameworks, service styles, and supporting backend libraries.

This file focuses on the application tier. Data stores are covered in databases.md; deployment topology in deployment.md; security libraries in security-tooling.md.

## Scope

- Language / runtime families
- Web / API frameworks
- Service architecture style (modular monolith, service-oriented, event-driven)
- Background job and queue processing
- Validation, serialisation, and error-contract libraries

Out of scope: database engines, container orchestration, and observability agents (see sibling files).

## Primary Decision Axes

1. **Team skill and existing codebase language**
2. **Latency, throughput, and concurrency model required**
3. **Operational complexity the team can sustain**
4. **Integration style** (REST, GraphQL, tRPC, gRPC, event-driven)
5. **Complexity band** and expected evolution path
6. **Security and compliance posture** (memory safety, ecosystem maturity)

## Recommended Families by Context

### 1. TypeScript / Node.js ecosystem

**Prefer when:**
- Frontend is already TypeScript and shared types or tRPC-style end-to-end safety is valuable.
- Team is strongest in JavaScript/TypeScript.
- Complexity band S–M, or M–L with disciplined modular boundaries.
- Real-time (WebSockets/SSE) or serverless functions are part of the design.

**Typical stacks:** Node.js + Fastify / Hono / NestJS / Express (legacy), or Bun where compatibility is verified.

**Trade-offs:** Single-threaded event loop requires care for CPU-bound work; ecosystem is vast but quality varies. Prefer well-maintained, typed libraries.

### 2. Python ecosystem

**Prefer when:**
- Data science, ML inference, or heavy numeric work lives close to the API.
- Team is strongest in Python.
- Rapid prototyping of business logic is prioritised over raw request throughput.
- Complexity band S–M; can scale with careful async (FastAPI, Starlette) or separate worker tiers.

**Typical stacks:** FastAPI, Django (when batteries-included admin and ORM are desired), Flask for minimal services.

**Trade-offs:** GIL and deployment packaging (especially with native extensions) need explicit attention. Prefer async frameworks for I/O-bound APIs.

### 3. Go

**Prefer when:**
- High concurrency, modest memory footprint, and simple deployment binaries matter.
- Network services, proxies, CLIs, or performance-sensitive microservices are central.
- Team can sustain Go’s explicit error handling and smaller standard web ecosystem.
- Complexity band M–L for service-oriented architectures.

**Typical stacks:** net/http + chi/gorilla, or Gin/Echo; gRPC is first-class.

**Trade-offs:** Generics and ecosystem maturity are good in 2026; still less “batteries-included” than Django/Rails for CRUD-heavy products.

### 4. JVM (Java / Kotlin)

**Prefer when:**
- Strong typing, mature enterprise tooling, and long-term maintainability are required.
- Existing organisation standards mandate JVM.
- High-throughput transactional systems or complex domain models.
- Complexity band L or regulated environments.

**Typical stacks:** Spring Boot, Quarkus, Micronaut, or Ktor (Kotlin).

**Trade-offs:** Heavier memory and longer cold starts unless tuned (GraalVM native, Quarkus). Excellent observability and library ecosystem.

### 5. Other runtimes (Rust, Elixir, .NET, etc.)

**Use only with explicit justification:**
- Rust: systems-level safety, performance-critical components, or WebAssembly edge logic.
- Elixir/Phoenix: massive concurrency and soft-real-time (chat, presence) with high developer leverage.
- .NET: organisation already invested; excellent performance and tooling on Windows/Linux.

Record residual risk around hiring, library coverage, and operational familiarity.

## Service Style Guidance

| Style | Prefer when | Caution |
|-------|-------------|---------|
| Modular monolith | Complexity S–M, single team, rapid iteration | Keep module boundaries enforceable (packages, lint rules, ownership) |
| Service-oriented / microservices | Multiple teams, independent scaling, clear bounded contexts | Distributed systems cost is real; start modular and extract |
| Event-driven | High decoupling, audit trails, eventual consistency acceptable | Requires solid messaging infrastructure and idempotency discipline |
| Serverless functions | Spiky traffic, minimal ops, event-driven triggers | Cold starts, vendor limits, local dev parity, observability gaps |

**Default recommendation for most new products in complexity S–M:** modular monolith with clear internal packages, designed so that extraction remains possible later.

## API Style

- **REST + OpenAPI** — default for public or multi-client APIs; excellent tooling and cacheability.
- **tRPC / similar** — excellent when frontend and backend share a TypeScript monorepo and the API is not public.
- **GraphQL** — when clients need flexible querying and over-fetching is a demonstrated problem; accept the complexity of schema design, N+1 protection, and caching.
- **gRPC** — internal service-to-service, high performance, strong contracts; less ideal for browser clients without a gateway.

See also knowledge/patterns/api-design.md.

## Background Work & Queues

- Prefer a dedicated job runner (BullMQ, Sidekiq, Celery, River, etc.) over ad-hoc in-process work for anything that must survive process restarts or be retried.
- Record at-least-once vs exactly-once expectations and idempotency strategy.
- Keep the queue technology aligned with the primary runtime where possible to reduce operational surface.

## Essential Decision Points to Record

In the Architecture Blueprint (Technology Decisions):

1. Primary language / runtime and version posture
2. Web / API framework
3. Service style (modular monolith vs services vs serverless) and justification
4. API style and contract approach
5. Background job / queue technology
6. Validation and error-contract libraries
7. Any deliberate deviation and residual risk

## Anti-Patterns

- Choosing a language solely for personal preference without regard to team skill or hiring.
- Starting with microservices before bounded contexts and operational maturity exist.
- Mixing multiple API styles without a clear gateway or BFF strategy.
- Performing long-running or unreliable work inside request handlers.
- Ignoring structured error contracts and observability from day one.

## Related Knowledge

- [classification/architecture-selection.md](../classification/architecture-selection.md)
- [patterns/api-design.md](../patterns/api-design.md)
- [patterns/authentication.md](../patterns/authentication.md)
- [patterns/authorization.md](../patterns/authorization.md)
- [databases.md](databases.md)
- [deployment.md](deployment.md)
- [observability.md](observability.md)
