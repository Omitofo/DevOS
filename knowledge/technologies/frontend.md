# Frontend Technologies

**Status:** Active  
**Authority:** core/contract.md · runtime/agents/architect.md · runtime/agents/tech-lead.md · knowledge/classification/rendering-strategies.md  
**Last review:** 2026-08-06  
**Confidence:** High

Decision framework for selecting frontend runtimes, frameworks, rendering models, state management, and supporting client tooling.

This file does **not** prescribe a single “best” stack. It provides criteria so that the same product class and complexity band tend to receive a coherent, justifiable frontend choice.

## Scope

- UI frameworks and meta-frameworks
- Rendering and delivery models (CSR, SSR, SSG, hybrid, edge)
- Client-side state and data-fetching approaches
- Styling systems and component primitives
- Build tooling and TypeScript posture

Out of scope: detailed visual design systems (see knowledge/design/), authentication flows (see knowledge/patterns/authentication.md), and deployment of the built assets (see deployment.md).

## Primary Decision Axes

1. **Rendering strategy** (must align with classification/rendering-strategies.md)
2. **Interactivity density** and expected client-side complexity
3. **Team skill and hiring surface**
4. **SEO / first-contentful-paint / Core Web Vitals requirements**
5. **Integration needs** with backend (API style, real-time, auth model)
6. **Operational simplicity** vs. framework power

## Recommended Families by Context

### 1. Content-heavy / marketing / documentation sites

**Prefer:** Static-site generators or SSG-first meta-frameworks (Astro, Next.js in SSG mode, Eleventy, Hugo where content volume is large and interactivity is low).

**Criteria:**
- Majority of pages are content or marketing.
- SEO and fast first paint are primary.
- Interactivity is limited to isolated islands or light client components.
- Complexity band S–M.

**Avoid when:** Heavy authenticated dashboards, complex real-time collaboration, or highly dynamic personalisation that cannot be pre-rendered or edge-cached.

### 2. Product / SaaS / authenticated applications

**Prefer:** React-based meta-frameworks with strong SSR/SSG + client hydration (Next.js App Router, Remix) or equivalent solid ecosystems (Nuxt for Vue, SvelteKit for Svelte).

**Criteria:**
- Significant authenticated surface area.
- Need for progressive enhancement or good default SEO on public pages.
- Team already productive in React/Vue/Svelte.
- Complexity band M–L.

**Trade-offs:**
- Next.js / Remix give excellent DX and ecosystem depth at the cost of framework lock-in and larger mental model.
- SvelteKit / SolidStart offer smaller client bundles and simpler reactivity models; ecosystem and hiring surface are smaller.

### 3. Highly interactive SPAs or internal tools

**Prefer:** Client-rendered React, Vue, or Svelte (Vite + framework) when the application is behind authentication, SEO is irrelevant, and the team values pure SPA ergonomics.

**Criteria:**
- No public SEO requirement.
- Dense client-side state and frequent interactions.
- Backend is a pure API (REST/GraphQL/tRPC).
- Complexity band S–M for internal tools; can scale to L with disciplined architecture.

**Caution:** Pure CSR is rarely the right default for public-facing product surfaces in 2026.

### 4. Edge-first / ultra-low latency experiences

**Prefer:** Frameworks with first-class edge runtime support (Next.js on edge, Remix on edge platforms, Astro with edge adapters, Cloudflare Workers + Hono/SvelteKit, etc.).

**Criteria:**
- Global audience with strict TTFB targets.
- Personalisation or A/B that must happen at the edge.
- Willingness to accept edge runtime constraints (limited Node APIs, cold-start characteristics).

## State Management Guidance

| Need | Prefer | Notes |
|------|--------|-------|
| Server-driven UI + light client state | Framework server components / loaders + URL state | Minimise client stores |
| Complex client workflows, optimistic UI | Zustand, Jotai, or framework-native stores | Avoid Redux unless the team already has deep Redux expertise and the domain is highly event-sourced |
| Shared server cache + mutations | TanStack Query / SWR / framework data layer | Prefer these over inventing a custom cache |
| Real-time collaborative state | CRDT libraries or specialised backends (see patterns/real-time.md) | Do not invent ad-hoc WebSocket state machines without a clear model |

**Anti-pattern:** Introducing a global client store “just in case” before the data-flow shape is understood.

## Styling & Component Systems

- Prefer CSS modules, Tailwind, or a well-scoped design-token system over runtime CSS-in-JS for most new projects (performance and simplicity).
- Component libraries (Radix, Headless UI, shadcn/ui patterns, etc.) are acceptable when accessibility primitives are required; record the choice and the accessibility baseline they provide.
- Design-system ownership remains with the design knowledge domain; technology choice here is about implementation vehicle only.

## TypeScript & Tooling Posture

- TypeScript is the default for any non-trivial frontend.
- Vite is the preferred bundler/dev-server foundation unless the chosen meta-framework already owns the toolchain.
- Enforce strict mode and avoid `any` leakage into domain types.
- Testing: prefer component tests (Testing Library) + a small number of critical path e2e (Playwright) over exhaustive snapshot suites.

## Essential Decision Points to Record

In the Architecture Blueprint (Technology Decisions):

1. Primary framework / meta-framework and version posture
2. Rendering model (CSR / SSR / SSG / hybrid / edge) and justification linked to classification
3. State and data-fetching approach
4. Styling system
5. TypeScript and testing baseline
6. Any deliberate deviation from the recommendations above and the residual risk accepted

## Anti-Patterns

- Choosing a framework solely because it is “modern” without matching rendering needs.
- Mixing multiple competing state libraries without clear boundaries.
- Shipping a pure CSR public site when SEO or first-load performance is a stated requirement.
- Adopting a large component library without an accessibility and theming strategy.
- Ignoring Core Web Vitals budgets until late in the project.

## Related Knowledge

- [classification/rendering-strategies.md](../classification/rendering-strategies.md)
- [patterns/forms.md](../patterns/forms.md)
- [patterns/real-time.md](../patterns/real-time.md)
- [patterns/authentication.md](../patterns/authentication.md)
- [deployment.md](deployment.md)
- knowledge/design/ (visual and interaction constraints)
