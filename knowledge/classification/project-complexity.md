# Project Complexity

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, Tech Lead, Planner (sizing signals)

Complexity is a **sizing and risk** signal, not a quality judgement.  
It influences work-package granularity, review intensity, and whether certain architectural patterns are justified.

## Complexity Bands

| Band | Label | Typical characteristics | Implications for agents |
|------|-------|-------------------------|-------------------------|
| **S** | Small | Single clear goal, few stakeholders, limited surface area, mostly static or simple CRUD | Lightweight architecture, fewer work packages, lower audit intensity |
| **M** | Medium | Multiple user roles or workflows, some integrations, moderate data model | Standard architecture process, explicit NFR mapping, normal work-package sizing |
| **L** | Large | Multi-tenant or multi-product, significant integrations, complex domain rules, higher compliance | Explicit risk register, stronger modularity, careful technology justification |
| **XL** | Extra-large | Platform-scale, high concurrency, regulatory weight, many teams or long-lived evolution | Architecture must emphasise boundaries, observability, and evolutionary paths; human gates expected |

## Complexity Drivers (evaluate each)

Agents should score the project against these drivers. A project lands in a band when a majority of drivers point there, or when any single driver is extreme.

### 1. Scope & Surface Area

| Signal | Points toward |
|--------|---------------|
| One primary user journey, few pages/screens | S |
| Several distinct workflows or personas | M |
| Many modules, admin + public + partner surfaces | L–XL |
| Open-ended platform with third-party extension | XL |

### 2. Data & Domain Complexity

| Signal | Points toward |
|--------|---------------|
| Simple entities, little relationship complexity | S |
| Moderate domain model, some invariants | M |
| Rich domain rules, eventual consistency, multi-aggregate transactions | L |
| Distributed data ownership, complex consistency requirements | XL |

### 3. Integration Surface

| Signal | Points toward |
|--------|---------------|
| None or 1–2 simple external services | S |
| Several third-party APIs with auth and error handling | M |
| Payment, identity, messaging, analytics, and partner APIs | L |
| Event-driven integration with many systems of record | XL |

### 4. Non-Functional Load

| Signal | Points toward |
|--------|---------------|
| Best-effort performance, basic availability | S |
| Explicit latency, throughput, or uptime targets | M |
| Strict SLAs, multi-region, or high concurrency | L–XL |
| Regulated data (PII, financial, health) with audit requirements | L–XL |

### 5. Team & Longevity Signals

| Signal | Points toward |
|--------|---------------|
| Single developer or short-lived project | S–M |
| Small team, expected evolution over 6–18 months | M–L |
| Multiple teams or long-lived product with continuous delivery | L–XL |

## Decision Rules

1. Start from the Brief and Requirements. Do not invent drivers that are not evidenced.
2. List the active drivers and the band each one suggests.
3. Choose the highest band that is clearly justified by at least two drivers, or by one extreme driver.
4. If evidence is insufficient, default to **M** and surface an open question: “Complexity drivers X and Y are under-specified; confirmation needed for sizing.”
5. Record both the band and the decisive drivers in the Architecture Blueprint.

## Output Expectations for Agents

```markdown
- **Complexity:** M (Medium)
  - Drivers: multiple personas + payment integration + explicit performance NFR
  - Knowledge: knowledge/classification/project-complexity.md
```

Tech Lead may later refine work-package estimates using the same band as a starting point; the Architect’s classification remains the authoritative high-level signal.
