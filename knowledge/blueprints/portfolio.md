# Portfolio Blueprint

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** Architect, Planner, Tech Lead, Content / Design agents

Opinionated starting architecture and decision framework for portfolio, personal-brand, agency, or brochure-style showcase sites.  
Use this blueprint when the primary product type is **Portfolio / Brochure** (see [knowledge/classification/website-types.md](../classification/website-types.md)).

This file does **not** prescribe a single stack. It supplies a coherent default shape, the critical decision points that must be resolved early, and the knowledge links agents are expected to follow.

## When to Apply

**Strong signals that this blueprint is appropriate**
- Primary goal is to showcase work, people, case studies, or offerings with strong visual storytelling.
- Content is editorial and relatively stable (projects, bios, process, selected writing).
- Call-to-action is contact, inquiry, hire, or “view more” rather than conversion funnels or persistent accounts.
- Visual hierarchy, imagery, motion, and typography are first-class product concerns.
- SEO and shareability matter, but the site is not a high-volume content hub or transactional catalogue.

**Counter-signals (consider a different blueprint)**
- Primary success metric is lead-gen conversion or multi-step marketing funnels → [landing-page.md](landing-page.md)
- Organised, searchable, versioned knowledge is the core value → [documentation-site.md](documentation-site.md)
- Persistent authenticated product experience or multi-user workspaces → [saas.md](saas.md)
- Transactional catalogue + cart + fulfilment dominates → [ecommerce.md](ecommerce.md)
- Internal operational interface for known users → [dashboard.md](dashboard.md)

If the portfolio is the public face of a larger product (e.g. agency site + client portal), treat the **showcase surface** under this blueprint and record the authenticated surface under the appropriate application blueprint.

## Recommended Starting Shape

A typical modern portfolio / brochure architecture optimised for visual quality, editorial control, performance, and easy content updates:

| Layer | Default posture | Notes |
|-------|-----------------|-------|
| **Rendering** | Static-site generation (SSG) or hybrid (SSG + selective SSR/ISR) | Prefer fully static or edge-rendered; avoid full CSR for primary content |
| **Content** | Markdown / MDX or lightweight headless CMS | Case studies and project pages should be easy to add without code changes once the model stabilises |
| **Media** | Optimised images + optional short video / motion | Image pipeline and aspect-ratio discipline are non-negotiable |
| **Design system** | Shared tokens + carefully curated component set | Strong visual identity; avoid sprawling component libraries early |
| **Interaction** | Light, purposeful motion and micro-interactions | Prefer CSS / library-based motion over heavy JavaScript frameworks |
| **Contact / inquiry** | Simple form or mailto + optional CRM integration | Keep friction low; privacy and spam protection still required |
| **Performance** | Aggressive image optimisation, font subsetting, minimal JS | Core Web Vitals and perceived speed are part of the brand experience |
| **Deployment** | Edge / CDN-first static hosting with preview environments | Non-developers should be able to review content and design changes |

**Default complexity target:** Small. The shape above stays lean. Introduce a CMS, multi-language support, or complex filtering only when content volume or team workflow justifies it.

## Core Decision Points (must be resolved in Architecture Blueprint)

These decisions are high-leverage and expensive to reverse. Record the choice **and** the decisive criteria.

### 1. Rendering & delivery strategy
- Pure SSG vs. hybrid (SSG + SSR/ISR for dynamic sections such as latest work or availability).
- Edge rendering vs. origin rendering.
- Must align with [classification/rendering-strategies.md](../classification/rendering-strategies.md).
- Link: [technologies/frontend.md](../technologies/frontend.md)

### 2. Content model & authoring
- File-based (Markdown/MDX with frontmatter) vs. headless CMS vs. hybrid.
- Structure of a “project / case study” (title, summary, role, timeline, outcomes, media, process narrative).
- Who edits content (designer/developer only, client, both) and how often.
- Internationalisation / localisation needs (if any).

### 3. Visual & storytelling system
- Grid, typography scale, spacing rhythm, and colour system that support the brand narrative.
- Image treatment conventions (aspect ratios, crop rules, caption style, lightbox behaviour).
- Motion language (entrance, hover, page transitions) and reduced-motion fallbacks.
- Links: [design/storytelling.md](../design/storytelling.md), [design/composition.md](../design/composition.md), [design/typography.md](../design/typography.md), [design/spacing.md](../design/spacing.md), [design/color-systems.md](../design/color-systems.md), [design/motion.md](../design/motion.md), [design/accessibility-patterns.md](../design/accessibility-patterns.md)

### 4. Media pipeline
- Source formats, optimisation targets (AVIF/WebP, responsive sizes, priority loading).
- Video strategy (if used): host, autoplay policy, poster frames, accessibility captions.
- Asset organisation and naming conventions so future projects can be added cleanly.

### 5. Contact, inquiry & conversion surface
- Primary CTA (contact form, calendar link, email, social).
- Form fields, validation, spam protection, and destination (email, CRM, Notion, etc.).
- Privacy, consent, and retention expectations.
- Link: [patterns/forms.md](../patterns/forms.md)

### 6. Navigation & information architecture
- Flat vs. hierarchical navigation; how projects are filtered or grouped (type, year, role, industry).
- Whether a blog / notes section exists and how it relates to the core portfolio.
- Deep-linking and shareability of individual case studies.

### 7. Performance, SEO & accessibility budgets
- Explicit Core Web Vitals targets (especially LCP given heavy imagery).
- Metadata, Open Graph / social cards, structured data for creative work.
- Accessibility baseline (contrast, focus management, keyboard navigation, alt text discipline, reduced motion).

## Technology Family Guidance

Prefer coherence, visual control, and minimal moving parts. Record the concrete choices and the criteria that selected them.

| Concern | Preferred starting families (2026) | Avoid early unless justified |
|---------|------------------------------------|------------------------------|
| Frontend / rendering | Astro, Next.js (App Router with strong SSG), Remix, or equivalent Vue/Svelte static-first tools | Pure CSR SPA for the primary content surface; heavy client-side state frameworks |
| Content | MDX / Markdown in-repo or a simple headless CMS with good media support | Full traditional CMS with complex editorial workflows before content volume warrants it |
| Styling & design system | Utility-first or constrained design-token system tightly mapped to the visual identity | Unconstrained global CSS or design-system bloat that slows iteration |
| Media | Built-in image optimisation (framework or CDN) + disciplined source assets | Shipping unoptimised originals or relying solely on browser resizing |
| Forms / contact | Framework server actions or lightweight serverless + established form service | Building a custom form backend or storing PII without a retention policy |
| Hosting | Edge / CDN static or hybrid platforms with instant preview deploys | Origin-heavy servers for primarily static portfolio content |
| Analytics | Privacy-respecting first-party or carefully audited third-party | Unbounded script injection that harms Core Web Vitals and brand perception |

Deeper criteria live in [knowledge/technologies/](../technologies/index.md) and [knowledge/design/](../design/index.md).

## Essential Patterns to Apply

Every Portfolio Architecture Blueprint should explicitly reference (and decide against where appropriate) at least:

- [design/storytelling.md](../design/storytelling.md) and related design files — narrative structure, visual hierarchy, motion language
- [design/accessibility-patterns.md](../design/accessibility-patterns.md)
- [classification/rendering-strategies.md](../classification/rendering-strategies.md)
- [patterns/forms.md](../patterns/forms.md) — contact / inquiry handling

Optional but frequent: file-uploads (for richer inquiry forms), caching (for CMS-backed media), search (if the portfolio grows large enough to need filtering beyond simple tags).

## Recommended Project Skeleton (high-level)

```
/
├── apps/ or packages/
│   └── web/                 # portfolio site (SSG / hybrid)
├── content/ or src/content/ # Markdown / MDX / CMS sync (projects, pages, notes)
├── public/                  # static assets, favicons, robots, og defaults
├── packages/                # shared UI, design tokens, config (if multi-package)
└── ...
```

Exact layout is language- and tooling-dependent. The important invariants are:

- Clear separation of content (case studies, bios, process) from presentation code.
- A disciplined media pipeline so new projects can be added without performance regressions.
- Design tokens and components that express a coherent visual identity.
- Preview / staging environments that non-developers (or the client) can use for content and design review.

## Anti-Patterns

- Shipping a full client-side SPA for a primarily static showcase site.
- Treating image optimisation and Core Web Vitals as “nice to have” after launch.
- Hard-coding every project page so that adding a new case study requires a developer and a full redesign pass.
- Using heavy, unoptimised hero videos or carousels that destroy LCP and battery life.
- Designing elaborate motion without a reduced-motion path or keyboard-accessible alternatives.
- Collecting personal data on contact forms without a documented retention and consent path.
- Creating a complex filtering / taxonomy system before the number of projects justifies it.
- Ignoring mobile viewport, touch targets, and reading experience while optimising only for large desktop screenshots.
- Letting the visual system drift so that each new project page feels like a different site.

## Recording Requirements

In the project’s Architecture Blueprint the Architect must record at minimum:

```markdown
## Classification
- Primary type: Portfolio / Brochure
- Knowledge: knowledge/classification/website-types.md
- Blueprint: knowledge/blueprints/portfolio.md

## Rendering & Delivery
- Strategy: <SSG | hybrid | …>
- Hosting / edge approach: …

## Content Model
- Source: <Markdown/MDX | headless CMS | hybrid>
- Project / case-study structure: …
- Authoring workflow: …

## Visual & Media System
- Design-token / component approach
- Image & video pipeline and optimisation targets
- Motion language and reduced-motion handling

## Contact / Inquiry
- Primary CTA and form handling
- Privacy / consent notes

## Key Technology Decisions
- Frontend family + rendering strategy
- Content approach
- Media / optimisation approach
- Links to the decisive knowledge files and criteria

## Performance, SEO & Accessibility Budgets
- Core Web Vitals targets (especially LCP)
- Metadata / social-card strategy
- Accessibility baseline commitments
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

This blueprint targets the common case of personal, studio, agency, and product-showcase portfolio sites.

When the portfolio grows into a substantial content hub with search, versioning, and editorial workflows, promote the content concerns toward the [documentation-site.md](documentation-site.md) blueprint.  
When the same codebase also hosts an authenticated product or client portal, keep the showcase surface under this blueprint (or a thin extension) and let the product surface follow [saas.md](saas.md), [dashboard.md](dashboard.md), or the appropriate application blueprint.  
Specialised high-volume creative platforms or multi-artist marketplaces should extend rather than fork this file until a recurring specialised shape warrants its own blueprint.
