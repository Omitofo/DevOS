# Deployment Technologies

**Status:** Active  
**Authority:** core/contract.md · runtime/agents/architect.md · runtime/agents/tech-lead.md  
**Last review:** 2026-08-06  
**Confidence:** High

Decision framework for selecting hosting models, packaging, platforms, and release strategies.

Deployment choices are constrained by complexity band, team operational maturity, availability targets, and cost sensitivity.

## Scope

- Hosting models (PaaS, containers, serverless, VMs, edge)
- Packaging (containers, language-native artefacts, static assets)
- Orchestration and platforms
- CI/CD and release strategies (rolling, blue/green, canary)
- Environment topology (local, preview, staging, production)

Out of scope: detailed infrastructure-as-code module design, secret distribution mechanics (see security-tooling.md), and observability agent installation (see observability.md).

## Primary Decision Axes

1. **Operational maturity** of the team (can they run Kubernetes? do they want to?)
2. **Traffic shape** (steady, spiky, global)
3. **Availability and recovery targets**
4. **Cost model** (predictable vs pay-per-use)
5. **Language / runtime constraints** (cold starts, binary size, native dependencies)
6. **Complexity band** and expected growth

## Recommended Models by Context

### 1. Complexity S – small products, internal tools, MVPs

**Prefer managed PaaS or simple container hosts:**
- Vercel / Netlify / Cloudflare Pages for frontend + serverless functions
- Railway, Render, Fly.io, or equivalent for full-stack or backend services
- Managed database offerings from the same provider or a specialist (Neon, Supabase, PlanetScale, RDS)

**Rationale:** Minimise undifferentiated operational work. Focus engineering time on product.

### 2. Complexity M – growing products with moderate ops capacity

**Prefer container-based deployment on a managed platform:**
- Single-region or multi-AZ container services (ECS, Cloud Run, App Service, Fly.io machines, etc.)
- Managed Kubernetes only if the team already has (or is committed to building) the necessary expertise
- Managed relational database + Redis

**Rationale:** Containers give reproducibility without forcing full orchestration complexity.

### 3. Complexity L or multi-team / regulated

**Prefer explicit platforms with strong isolation and policy:**
- Kubernetes (EKS, GKE, AKS, or on-prem) when multiple services, custom networking, or compliance controls require it
- Or a mature internal platform built on top of containers
- Clear separation of environments, network policies, and secret management

**Rationale:** The coordination and compliance cost is already present; the platform must support it.

### 4. Edge and global low-latency

**Prefer edge platforms** (Cloudflare Workers/Pages, Deno Deploy, Vercel Edge, Fastly Compute, etc.) when:
- TTFB and geographic distribution dominate
- The workload fits edge runtime constraints
- Data access patterns can tolerate eventual consistency or regional data

Combine with origin services for durable writes and complex transactions.

## Packaging

| Artefact type | Prefer when |
|---------------|-------------|
| Static assets / SSG output | Content sites, marketing, documentation |
| Language-native binary or serverless package | Simple services, functions, CLI tools |
| OCI container | Most backend services and full-stack apps that need reproducible environments |
| Wasm / edge module | Ultra-light edge logic |

**Default for services:** build a minimal OCI image (distroless or slim base where possible), pin base image digests, and scan in CI.

## Release Strategies

- **Rolling** — default for most web services when backward-compatible changes are enforced.
- **Blue/green** — when instant rollback and full environment parity are required.
- **Canary / progressive** — when risk is high or user impact must be limited; requires good observability and automated analysis.
- **Feature flags** — complementary; prefer for gradual product exposure rather than as a substitute for safe deployment mechanics.

Record the chosen strategy and the conditions that would trigger a rollback.

## Environment Topology

Minimum expected set for anything beyond a pure prototype:

1. Local development (documented, scriptable)
2. Preview / PR environments (ephemeral when feasible)
3. Staging (production-like data shape and configuration)
4. Production

Configuration must be externalised (environment variables, secret stores); never bake environment-specific secrets into images.

## Essential Decision Points to Record

In the Architecture Blueprint (Technology Decisions or Deployment View):

1. Hosting model and primary platform
2. Packaging format (container, serverless, static, etc.)
3. Database and cache hosting choices
4. Release strategy and rollback approach
5. Environment topology
6. CI/CD high-level flow (build → test → scan → deploy)
7. Any self-managed infrastructure and the operational load accepted

## Anti-Patterns

- Running Kubernetes “because it is industry standard” with a team that has never operated it.
- Deploying straight from developer laptops to production.
- Mixing multiple unrelated PaaS providers without a clear boundary or cost-control strategy.
- Ignoring image scanning and base-image currency.
- Treating staging as optional once the product has real users.
- Baking secrets or environment-specific config into build artefacts.

## Related Knowledge

- [frontend.md](frontend.md)
- [backend.md](backend.md)
- [databases.md](databases.md)
- [observability.md](observability.md)
- [security-tooling.md](security-tooling.md)
- knowledge/patterns/ (for runtime behaviour that affects deployment, e.g. real-time, file-uploads)
