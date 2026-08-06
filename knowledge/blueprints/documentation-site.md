# Documentation Site Blueprint

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** Architect, Planner, Tech Lead, Content / Design agents

Opinionated starting architecture and decision framework for documentation, knowledge-base, and content-heavy sites.  
Use this blueprint when the primary product type is **Documentation / Content Site** (see [knowledge/classification/website-types.md](../classification/website-types.md)).

This file does **not** prescribe a single stack. It supplies a coherent default shape, the critical decision points that must be resolved early, and the knowledge links agents are expected to follow.

## When to Apply

**Strong signals that this blueprint is appropriate**
- Primary value is organised, searchable, versioned content (API docs, product docs, knowledge base, learning materials, editorial site).
- Hierarchical or tag-based information architecture is central.
- Search, navigation, and content discoverability are first-class engineering concerns.
- Content lifecycle (authoring, review, publish, versioning) matters.
- Multi-language, versioned, or multi-product documentation is common or planned.
- Readers are mostly anonymous or lightly authenticated; the core experience is consumption, not transactional application state.

**Counter-signals (consider a different blueprint)**
- Primary goal is lead capture or single-conversion marketing → [landing-page.md](landing-page.md)
- Showcase of personal or agency work with strong visual storytelling → [portfolio.md](portfolio.md)
- Multi-user product with accounts, tenancy, and ongoing feature use → [saas.md](saas.md)
- Authenticated operational interface for known internal users → [dashboard.md](dashboard.md)
- Transactional catalogue + cart + fulfilment dominates → [ecommerce.md](ecommerce.md)

If the product is a hybrid (public docs + authenticated product), treat the **documentation surface** as primary when content organisation and search dominate risk; otherwise apply the product blueprint and treat docs as a secondary surface. Record the dual nature explicitly.

## Recommended Starting Shape

A typical modern documentation / knowledge-base architecture optimised for content velocity, search quality, and long-term maintainability:

| Layer | Default posture | Notes |
|-------|-----------------|-------|
| **Rendering** | Static-site generation (SSG) or hybrid (SSG + selective SSR / ISR) | Prefer fully static or edge-rendered pages for public docs; avoid pure CSR for primary content |
| **Content source** | Markdown / MDX in-repo or headless CMS with structured front-matter | Authors should be able to change content without a full application redeploy once volume grows |
| **Navigation & IA** | Hierarchical sidebar + breadcrumbs + on-page TOC | Generated from content structure; support version and product selectors |
| **Search** | Full-text search with ranking, filters, and (optionally) AI-assisted answers | Client-side for small corpora; dedicated search service or index for larger / multi-version sets |
| **Versioning** | Path or subdomain based version selection | Current + previous major versions kept live; archive older versions |
| **API / interactive** | Optional embedded API explorers, code runners, or live examples | Isolate interactive islands; keep static shell fast |
| **Auth (if needed)** | Optional gated sections or private docs | Prefer lightweight auth for private knowledge bases; full identity only when required |

Complexity band guidance: S–M for pure public docs; M–L when multi-version, multi-product, i18n, or heavy interactive examples are required.

## Core Decision Points

Resolve these early and record the answers in the Architecture Blueprint.

### 1. Content model & ownership
- Source of truth: Git (Markdown/MDX) vs. headless CMS vs. hybrid.
- Front-matter schema (title, description, sidebar order, tags, version, product, status).
- Who authors, reviews, and publishes (engineers, technical writers, product, community).
- Draft → review → publish workflow and preview environments.
- Link: [standards/documentation.md](../standards/documentation.md)

### 2. Information architecture & navigation
- Hierarchy depth, version selectors, product / product-line selectors.
- Sidebar generation strategy (auto from folder structure vs. explicit config).
- Cross-linking, related-content, and “next / previous” conventions.
- Landing pages, section overviews, and “getting started” paths.

### 3. Search strategy
- Scope (single version, all versions, multi-product).
- Ranking signals (title, headings, recency, popularity).
- Client-side index vs. server / managed search service.
- Faceting, filters, and (if used) semantic or AI-assisted retrieval.
- Link: [patterns/search.md](../patterns/search.md)

### 4. Versioning & deprecation
- How versions are identified (semver, date, named releases).
- URL strategy (path prefix, subdomain, query).
- How long previous versions stay live and how redirects / banners are handled.
- Deprecation notices and migration guides.

### 5. Rendering & delivery
- Must align with [classification/rendering-strategies.md](../classification/rendering-strategies.md).
- Typical choice: SSG or hybrid for public docs; selective SSR for search results or authenticated private sections.
- Link: [technologies/frontend.md](../technologies/frontend.md)

### 6. Interactive & embedded content
- Code samples, API explorers, runnable examples, diagrams, videos.
- How interactive islands are isolated so they do not degrade the static shell.
- Sandboxing and security boundaries for user-supplied or third-party embeds.

### 7. Internationalisation & accessibility
- Multi-language content strategy (separate trees, parallel files, or CMS locales).
- Accessibility baseline (heading structure, landmarks, focus, reduced motion, contrast).
- Links: [design/accessibility-patterns.md](../design/accessibility-patterns.md), [design/typography.md](../design/typography.md)

### 8. Analytics & feedback
- Which events matter (page views, search queries, feedback votes, “was this helpful”).
- Feedback loops back to authors (issue links, edit suggestions, rating aggregation).
- Privacy posture for analytics on documentation sites.

## Technology Family Guidance

Prefer coherence, content velocity, and minimal operational overhead. Record the concrete choices and the criteria that selected them.

| Concern | Preferred starting families (2026) | Avoid early unless justified |
|---------|------------------------------------|------------------------------|
| Frontend / rendering | Documentation-oriented frameworks (Astro, Next.js with contentlayer / MDX, Docusaurus, VitePress, Nextra, or equivalent Vue/Svelte static-first tools) | Pure CSR SPA for the primary reading experience; heavy client-side state for static pages |
| Content | MDX / Markdown in-repo with structured front-matter, or a simple headless CMS | Full traditional CMS with complex workflows before authoring volume warrants it |
| Search | Lightweight client-side (Pagefind, FlexSearch) for small corpora; managed or self-hosted full-text (Algolia, Typesense, Meilisearch, Elasticsearch) for larger / multi-version sets | Ad-hoc grepping of Markdown or database `LIKE` queries at scale |
| Styling | Constrained design-token system that maps to the design knowledge; typography and spacing first | Unconstrained global CSS that fights readability |
| Hosting | Edge / CDN static or hybrid platforms with preview deploys and instant cache invalidation | Origin-heavy servers for purely static documentation |
| Interactive examples | Isolated client islands or dedicated sandboxes (StackBlitz, CodeSandbox embeds, or self-hosted runners) | Embedding full application runtimes inside every page |
| Versioning | Framework-native versioning or simple path-based routing + redirects | Multiple independent sites that diverge without a shared content model |

Deeper criteria live in [knowledge/technologies/](../technologies/index.md).

## Essential Patterns to Apply

Every Documentation-Site Architecture Blueprint should explicitly reference (and decide against where appropriate) at least:

- [search.md](../patterns/search.md)
- [api-design.md](../patterns/api-design.md) — when interactive API explorers or reference generation are present
- [caching.md](../patterns/caching.md) — especially for search indexes and versioned assets

Optional but frequent:
- [authentication.md](../patterns/authentication.md) and [authorization.md](../patterns/authorization.md) — for private or gated knowledge bases
- [forms.md](../patterns/forms.md) — feedback widgets, contribution forms
- [file-uploads.md](../patterns/file-uploads.md) — media or attachment handling in CMS flows

## Recommended Project Skeleton (high-level)

```
/
├── apps/ or packages/
│   ├── docs/                # documentation site (SSG / hybrid)
│   └── (optional) api/      # search, feedback, or private-doc endpoints
├── content/ or docs/        # Markdown / MDX source (or CMS sync)
│   ├── current/             # or versioned directories
│   └── ...
├── packages/                # shared UI, design tokens, search config
├── infrastructure/          # IaC, environments, CDN, search indexes
└── ...
```

Exact layout is language- and monorepo-tool dependent; the important invariant is a clear content root, generated navigation/search artifacts, and separation of the static reading shell from any interactive or authenticated surfaces.

## Anti-Patterns

- Treating documentation as an afterthought bolted onto the product application with no independent content model or search.
- Using pure client-side rendering for the primary reading experience, harming SEO, performance, and offline usability.
- Building a custom full-text search engine before evaluating mature client-side or managed options.
- Letting navigation and sidebar structure drift from the actual content hierarchy.
- Shipping multiple major versions without clear version selectors, deprecation banners, or redirect strategy.
- Embedding heavy interactive demos that block or slow the static shell on every page.
- Ignoring accessibility (heading order, landmarks, keyboard navigation, contrast) because “docs are simple”.
- Coupling content publishing to full application deploys when authors are non-engineers.
- Omitting feedback or “was this helpful” signals, so content quality cannot be measured.
- Over-engineering i18n or multi-product selectors before the content volume justifies the complexity.

## Recording Requirements

In the project’s Architecture Blueprint the Architect must record at minimum:

```markdown
## Classification
- Primary type: Documentation / Content Site
- Knowledge: knowledge/classification/website-types.md
- Blueprint: knowledge/blueprints/documentation-site.md

## Content Model
- Source of truth: <Git MDX | headless CMS | hybrid>
- Front-matter / schema conventions: …
- Authoring & publish workflow: …

## Information Architecture
- Hierarchy & sidebar strategy: …
- Versioning approach: <path | subdomain | …>
- Multi-product / multi-language (if any): …

## Search
- Strategy: <client-side | managed service | hybrid>
- Scope & ranking signals: …

## Key Technology Decisions
- Frontend family + rendering strategy
- Content pipeline
- Search implementation
- Hosting / CDN approach
- Links to the decisive knowledge files and criteria
```

Deviations from this blueprint’s recommendations are allowed; they must be explicit and justified.

## Related Knowledge

- [classification/website-types.md](../classification/website-types.md) — type definition
- [classification/project-complexity.md](../classification/project-complexity.md) — sizing
- [classification/rendering-strategies.md](../classification/rendering-strategies.md)
- [classification/architecture-selection.md](../classification/architecture-selection.md)
- [patterns/](../patterns/index.md) — search, api-design, caching, authentication, forms, …
- [technologies/](../technologies/index.md) — concrete stack criteria
- [design/](../design/index.md) — typography, spacing, accessibility, composition
- [standards/](../standards/index.md) — documentation, naming, testing conventions that apply to the resulting codebase

## Evolution Notes

This blueprint targets the common case of public or semi-public product documentation, API references, and knowledge bases.  
Specialised variants (heavy interactive learning platforms, multi-tenant private knowledge bases, community-edited wikis) should extend or override sections rather than fork the entire file. When a recurring specialised shape appears, promote it to its own blueprint.
