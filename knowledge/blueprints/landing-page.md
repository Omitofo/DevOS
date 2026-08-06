# Landing Page Blueprint

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** Architect, Planner, Tech Lead, Content / Design agents

Opinionated starting architecture and decision framework for conversion-focused or marketing-oriented sites.  
Use this blueprint when the primary product type is **Landing / Marketing Page** (see [knowledge/classification/website-types.md](../classification/website-types.md)).

This file does **not** prescribe a single stack. It supplies a coherent default shape, the critical decision points that must be resolved early, and the knowledge links agents are expected to follow.

## When to Apply

**Strong signals that this blueprint is appropriate**
- Primary success metric is conversion (sign-up, demo request, purchase, wait-list, download).
- Content is largely static or CMS-driven with infrequent structural change.
- Forms are lead-generation, contact, or simple multi-step funnels (no persistent user accounts or complex application state).
- SEO, page speed, and visual storytelling dominate engineering concerns.
- The site is often a single page or a short set of marketing pages.

**Counter-signals (consider a different blueprint)**
- Persistent authenticated product experience or multi-user workspaces → [saas.md](saas.md)
- Transactional catalogue + cart + fulfilment dominates → [ecommerce.md](ecommerce.md)
- Showcase of work / case studies with editorial focus → [portfolio.md](portfolio.md)
- Organised, searchable, versioned knowledge is the core value → [documentation-site.md](documentation-site.md)
- Internal operational interface for known users → [dashboard.md](dashboard.md)

If the product is a clear hybrid (public marketing site + authenticated SaaS), treat the **SaaS surface** as the primary architectural driver and note the marketing surface as secondary (see [saas.md](saas.md)).

## Recommended Starting Shape

A typical modern landing / marketing architecture optimised for speed of delivery, conversion, and SEO:

| Layer | Default posture | Notes |
|-------|-----------------|-------|
| **Rendering** | Static-site generation (SSG) or hybrid (SSG + selective SSR) | Prefer fully static or ISR/edge-rendered for public pages; avoid full CSR for primary content |
| **Content** | Markdown / MDX or headless CMS | Content authors should not require a deploy for copy/image changes when volume grows |
| **Forms & lead capture** | Server actions or lightweight API + email / CRM integration | Never store PII without clear retention and consent handling |
| **Analytics & conversion tracking** | Privacy-aware analytics + conversion events | First-party preferred; document third-party scripts carefully |
| **Design system** | Shared tokens + component library (even if small) | Consistency across hero, sections, and future pages |
| **Performance** | Aggressive image optimisation, font subsetting, minimal JS | Core Web Vitals are a product requirement, not an afterthought |
| **Deployment** | Edge / CDN-first static hosting | Preview environments for content and design changes |

**Default complexity target:** Small to Medium. The shape above stays lean; introduce a CMS or multi-page structure only when justified by content volume or team size.

## Core Decision Points (must be resolved in Architecture Blueprint)

These decisions are high-leverage and expensive to reverse. Record the choice **and** the decisive criteria.

### 1. Rendering & delivery strategy
- Pure SSG vs. hybrid (SSG + SSR/ISR for personalised or frequently changing sections).
- Edge rendering vs. origin rendering.
- Must align with [classification/rendering-strategies.md](../classification/rendering-strategies.md).
- Link: [technologies/frontend.md](../technologies/frontend.md)

### 2. Content model & authoring
- File-based (Markdown/MDX) vs. headless CMS vs. hybrid.
- Who edits content (developers only, marketing, both) and how often.
- Structured sections (hero, features, testimonials, pricing, FAQ) vs. free-form pages.
- Internationalisation / localisation needs.

### 3. Conversion & form handling
- Primary conversion actions and success metrics.
- Form validation, spam protection, and submission destination (email, CRM, wait-list service, database).
- Multi-step funnels vs. single form.
- Consent, privacy policy, and data-retention requirements.
- Link: [patterns/forms.md](../patterns/forms.md)

### 4. Analytics, tracking & experimentation
- Which events matter (page views, scroll depth, CTA clicks, form starts/completions).
- First-party vs. third-party scripts and their performance/privacy cost.
- A/B or multivariate testing approach (if any) and how variants are delivered.

### 5. Design & storytelling system
- Visual hierarchy, motion, and component vocabulary that support the conversion narrative.
- Accessibility baseline (contrast, focus, reduced-motion).
- Links: [design/storytelling.md](../design/storytelling.md), [design/composition.md](../design/composition.md), [design/motion.md](../design/motion.md), [design/accessibility-patterns.md](../design/accessibility-patterns.md)

### 6. Performance & Core Web Vitals budget
- Explicit targets for LCP, INP, CLS.
- Image strategy (formats, responsive sizes, priority loading).
- Font and third-party script budget.

### 7. SEO & discoverability
- Metadata, structured data, sitemap, and canonical strategy.
- Open Graph / social sharing cards.
- Indexing expectations (public vs. gated wait-list pages).

## Technology Family Guidance

Prefer coherence and minimal moving parts. Record the concrete choices and the criteria that selected them.

| Concern | Preferred starting families (2026) | Avoid early unless justified |
|---------|------------------------------------|------------------------------|
| Frontend / rendering | React meta-framework with strong SSG/ISR (Next.js App Router, Astro, Remix) or equivalent Vue/Svelte static-first tools | Pure CSR SPA for the primary conversion pages; heavy client-side state |
| Content | MDX / Markdown in-repo or a simple headless CMS | Full traditional CMS with complex workflows before content volume warrants it |
| Forms | Framework server actions or lightweight serverless endpoints + established form/CRM service | Building a custom form backend or storing PII without retention policy |
| Styling | Utility-first or constrained design-token system that maps to the design knowledge | Unconstrained global CSS that fights the design system |
| Hosting | Edge / CDN static or hybrid platforms with preview deploys | Origin-heavy servers for purely static marketing content |
| Analytics | Privacy-respecting first-party or carefully audited third-party | Unbounded script injection that harms Core Web Vitals |

Deeper criteria live in [knowledge/technologies/](../technologies/index.md) and [knowledge/design/](../design/index.md).

## Essential Patterns to Apply

Every Landing Page Architecture Blueprint should explicitly reference (and decide against where appropriate) at least:

- [forms.md](../patterns/forms.md) — lead capture, validation, spam protection
- [design/storytelling.md](../design/storytelling.md) and related design files — narrative structure, visual hierarchy
- [design/accessibility-patterns.md](../design/accessibility-patterns.md)
- [classification/rendering-strategies.md](../classification/rendering-strategies.md)

Optional but frequent: file-uploads (for richer forms), caching (for CMS-backed pages), search (if the site grows into a content hub).

## Recommended Project Skeleton (high-level)

```
/
├── apps/ or packages/
│   └── web/                 # marketing site (SSG / hybrid)
├── content/ or src/content/ # Markdown / MDX / CMS sync
├── packages/                # shared UI, design tokens, config
├── public/                  # static assets, favicons, robots
└── ...
```

Exact layout is language- and tooling-dependent. The important invariants are:
- Clear separation of content from presentation code.
- Design tokens and components that can later be shared with a product application if the landing page becomes the public face of a SaaS.
- Preview / staging environments that non-developers can use for content review.

## Anti-Patterns

- Shipping a full client-side SPA for a primarily static conversion page.
- Treating performance and Core Web Vitals as “nice to have” after launch.
- Collecting personal data without a documented retention and consent path.
- Hard-coding all copy and images so that every marketing change requires a developer deploy.
- Loading multiple heavy analytics and A/B testing scripts without measuring their impact on LCP/INP.
- Designing the visual system in isolation from the conversion narrative and accessibility requirements.
- Ignoring mobile viewport and touch targets while optimising only for desktop hero screenshots.
- Building a complex multi-page information architecture before validating the primary conversion path.

## Recording Requirements

In the project’s Architecture Blueprint the Architect must record at minimum:

```markdown
## Classification
- Primary type: Landing / Marketing Page
- Knowledge: knowledge/classification/website-types.md
- Blueprint: knowledge/blueprints/landing-page.md

## Rendering & Delivery
- Strategy: <SSG | hybrid | …>
- Hosting / edge approach: …

## Content Model
- Source: <Markdown/MDX | headless CMS | hybrid>
- Authoring workflow: …

## Conversion & Forms
- Primary actions: …
- Form handling & destination: …
- Privacy / consent notes: …

## Key Technology Decisions
- Frontend family + rendering strategy
- Content approach
- Analytics / tracking approach
- Links to the decisive knowledge files and criteria

## Performance & SEO Budgets
- Core Web Vitals targets
- Image / font / script strategy
```

Deviations from this blueprint’s recommendations are allowed; they must be explicit and justified.

## Related Knowledge

- [classification/website-types.md](../classification/website-types.md) — type definition
- [classification/project-complexity.md](../classification/project-complexity.md) — sizing
- [classification/rendering-strategies.md](../classification/rendering-strategies.md)
- [classification/architecture-selection.md](../classification/architecture-selection.md)
- [patterns/forms.md](../patterns/forms.md)
- [design/](../design/index.md) — storytelling, composition, typography, spacing, color, motion, accessibility
- [technologies/frontend.md](../technologies/frontend.md) and [technologies/deployment.md](../technologies/deployment.md)
- [standards/](../standards/index.md) — naming, documentation, testing conventions that apply to the resulting codebase

## Evolution Notes

This blueprint targets the common case of conversion-focused marketing sites and simple multi-page marketing properties.  

When the landing page grows into a substantial content hub, promote the content concerns toward the [documentation-site.md](documentation-site.md) blueprint.  
When the same codebase also hosts an authenticated product, keep the marketing surface under this blueprint (or a thin extension) and let the product surface follow [saas.md](saas.md) or the appropriate application blueprint.  
Specialised high-conversion or multi-variant experimentation platforms should extend rather than fork this file until a recurring specialised shape warrants its own blueprint.
