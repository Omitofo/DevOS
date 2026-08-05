# Architecture Selection

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, Tech Lead

High-level architecture family and style.  
This is not a technology choice; it is the structural approach that later technology and pattern decisions must respect.

## Architecture Families

### 1. Simple / Monolithic Web App

**Definition:** Single deployable unit that serves UI and API (or UI + thin API). Clear module boundaries inside one codebase are still required.

**Prefer when**
- Complexity band S–M
- Small team or single developer
- Limited integration surface
- Speed of delivery outweighs long-term independent scaling of parts

**Constraints to respect**
- Modules still follow single-responsibility
- Clear separation of domain, application, and interface layers inside the monolith

---

### 2. Modular Monolith

**Definition:** One deployable unit with explicit, enforced module boundaries (packages, bounded contexts, or internal APIs). Can be split later if needed.

**Prefer when**
- Complexity band M–L
- Domain has clear sub-domains that should not leak into each other
- Team wants evolutionary path toward services without paying distributed-systems cost yet

**Key practices**
- Explicit public interfaces between modules
- No circular dependencies
- Independent testability of modules

---

### 3. Service-Oriented / Multi-Service

**Definition:** Multiple independently deployable services collaborating via well-defined APIs or events.

**Prefer when**
- Complexity band L–XL
- Independent scaling, release cadence, or team ownership is required
- Different parts have materially different technology or reliability needs

**Requires justification**
- Operational overhead (observability, deployment, consistency) must be accepted
- Distributed-systems concerns (latency, partial failure, data ownership) must be addressed in the Architecture Blueprint

---

### 4. Jamstack / Content-Centric

**Definition:** Pre-rendered or edge-rendered front-end consuming headless CMS, APIs, and serverless functions. Emphasis on static assets + API composition.

**Prefer when**
- Primary type is Landing, Portfolio, Documentation, or content-heavy marketing
- Rendering strategy is SSG or Hybrid with strong static bias
- Backend logic is limited to forms, search, or light personalisation

---

### 5. Event-Driven / Asynchronous

**Definition:** Core interactions are mediated by events or messages rather than synchronous request/response only.

**Prefer when**
- Workflows span multiple systems or long-running processes
- High throughput or decoupling of producers and consumers is required
- Complexity band L–XL with clear event boundaries

**Usually combined with** one of the families above rather than used alone.

---

### 6. Mobile / Client-Heavy

**Definition:** Primary logic and state live in a native or rich client; backend is primarily an API and sync layer.

**Prefer when**
- Primary type is Mobile / Native-First
- Offline capability, device sensors, or rich client UX dominate

---

## Selection Process

1. Start from the already-decided **website type**, **complexity band**, and **rendering strategy**.
2. Apply the preference tables above.
3. Choose the simplest family that satisfies the hard constraints of the project.
4. If a more distributed style is chosen, explicitly list the drivers that made a simpler family insufficient.
5. Record the family, the decisive drivers, and any evolutionary intent (e.g. “modular monolith with clear seams for later extraction”).

## Decision Table (quick reference)

| Primary type + Complexity | Default family to consider first |
|---------------------------|----------------------------------|
| Landing / Portfolio / Docs + S–M | Jamstack / Content-Centric or Simple Monolith |
| Dashboard / Internal Tool + S–M | Simple or Modular Monolith |
| SaaS + M | Modular Monolith |
| SaaS + L–XL | Modular Monolith → multi-service if independent scaling/ownership required |
| E-commerce + M–L | Modular Monolith (or service-oriented for catalogue / checkout / fulfilment split) |
| Mobile-first | Mobile / Client-Heavy + API backend |
| Any + strong async workflows | Add Event-Driven characteristics |

## Output Expectations for Agents

```markdown
- **Architecture family:** Modular Monolith
  - Drivers: Complexity M–L, clear sub-domains, single team for now, evolutionary path desired
  - Rendering alignment: Hybrid (SSG marketing + CSR app shell)
  - Knowledge: knowledge/classification/architecture-selection.md
```

All subsequent component boundaries, technology choices, and pattern selections must be consistent with the chosen family. Deviations require explicit justification and are subject to Auditor review.
