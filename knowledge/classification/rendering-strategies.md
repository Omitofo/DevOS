# Rendering Strategies

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, Tech Lead, UI

Chooses how HTML (and related assets) are produced and delivered to the client.  
The decision is driven by content freshness, SEO/performance needs, interactivity, and operational constraints.

## Strategy Catalogue

### 1. Static Site Generation (SSG)

**Definition:** Pages are pre-rendered at build time into static files served from a CDN or static host.

**Best when**
- Content changes on a known cadence (or is fully known at build)
- SEO and Time-to-First-Byte are critical
- Interactivity is limited or can be progressive-enhanced
- Operational simplicity and cost matter

**Avoid when**
- Content is highly dynamic per user or changes continuously
- Personalisation must be reflected in the initial HTML

**Typical stacks / patterns:** Next.js / Astro / Eleventy / Hugo with static export; pure HTML+CSS when extremely simple.

---

### 2. Server-Side Rendering (SSR)

**Definition:** HTML is generated on the server per request (or per navigation), usually with a Node or edge runtime.

**Best when**
- Content is dynamic and must be fresh on every request
- SEO still matters (crawlers receive full HTML)
- Some personalisation or auth-gated content appears in the initial payload

**Trade-offs**
- Higher origin/compute cost than pure static
- Caching strategy becomes important (CDN, stale-while-revalidate, etc.)

---

### 3. Client-Side Rendering (CSR)

**Definition:** A minimal HTML shell is delivered; the application boots in the browser and renders the UI from data fetched via APIs.

**Best when**
- The product is a highly interactive application (dashboard, SaaS workspace)
- SEO of application screens is secondary or handled by a separate marketing site
- Offline or rich client-side state is valuable

**Avoid as the sole strategy when**
- Public content pages require strong SEO
- First-contentful-paint on slow networks is a hard requirement

---

### 4. Hybrid / Selective Rendering

**Definition:** Different routes or components use different strategies (SSG for marketing, SSR for product pages, CSR for authenticated app shells).

**Best when**
- The product naturally contains both public content and authenticated application surfaces
- One strategy cannot optimally serve all routes

**Requires**
- Clear route-level ownership of rendering mode
- Consistent data-fetching and caching conventions across modes

---

### 5. Edge Rendering / Edge-Side Includes

**Definition:** Rendering or final assembly happens at the edge (CDN / edge workers) for low latency and geographic distribution.

**Best when**
- Global audience with strict latency targets
- Personalisation or A/B can be resolved at the edge
- Origin load must be minimised

**Note:** Often combined with SSG or SSR rather than used in isolation.

---

## Decision Criteria

| Criterion | Favours |
|-----------|---------|
| Content known at build time, infrequent updates | SSG |
| Content changes often, SEO still required | SSR (with caching) |
| Highly interactive authenticated app, SEO secondary | CSR |
| Mix of public marketing + app | Hybrid |
| Global low-latency + personalisation | Edge-assisted hybrid |
| Maximum operational simplicity, minimal dynamic data | SSG |
| Strong personalisation in first paint | SSR or Edge |

## Decision Process for Agents

1. Identify which pages/routes are public vs authenticated.
2. Identify SEO and first-load performance requirements from NFRs.
3. Identify content freshness and personalisation needs.
4. Select the simplest strategy that satisfies the hard constraints.
5. If multiple strategies are required, declare Hybrid and list the route groups.
6. Record the choice and the decisive criteria; link back to this file.

## Output Expectations for Agents

```markdown
- **Rendering strategy:** Hybrid
  - Marketing & docs routes → SSG
  - Authenticated app shell → CSR
  - Product landing with personalisation → SSR
  - Knowledge: knowledge/classification/rendering-strategies.md
```

Technology choices (framework, hosting) must be consistent with the chosen strategy and justified via `knowledge/technologies/`.
