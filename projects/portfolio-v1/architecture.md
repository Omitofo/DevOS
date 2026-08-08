# Architecture Blueprint

**Version:** 0.1  
**Status:** draft  
**Upstream:** requirements.md, brief.md  
**Assumptions:** none  
**Open questions:** see §7 (only residual content/brand placeholders)  
**Last updated:** 2026-08-08  
**Authoring agent / human:** Architect

---

> **Producing agent:** Architect  
> **Purpose:** Define system boundaries, components, data flow, and technology choices that satisfy the Requirements and Brief while remaining consistent with DevOS knowledge (classification, blueprints, technologies). No requirements are invented. Every decision is justified and traceable.

## 1. Classification

- **Primary type:** Portfolio / Brochure  
  - Criteria matched: showcase of selected work, case-study oriented, editorial content, CTA is contact / hire, visual presentation and storytelling are first-class, no authenticated application state or transactional flows.  
  - Knowledge: [knowledge/classification/website-types.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/classification/website-types.md)  
  - Blueprint: [knowledge/blueprints/portfolio.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/blueprints/portfolio.md)

- **Complexity:** S (Small)  
  - Drivers: single clear goal, limited surface area (Home + Project pages + About + Contact), mostly static content, solo developer + AI, 2–3 day first usable version, no integrations beyond a contact form service, no multi-user or multi-tenant concerns.  
  - Knowledge: [knowledge/classification/project-complexity.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/classification/project-complexity.md)

- **Rendering strategy:** Static Site Generation (SSG)  
  - Criteria: content known at build time and changes infrequently, SEO and Core Web Vitals are hard requirements, interactivity is limited, maximum operational simplicity required (no owner-maintained backend).  
  - Knowledge: [knowledge/classification/rendering-strategies.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/classification/rendering-strategies.md)

- **Architecture family:** Jamstack / Content-Centric  
  - Drivers: Portfolio type + Complexity S + SSG rendering + content in Markdown files + zero owner-maintained backend.  
  - Knowledge: [knowledge/classification/architecture-selection.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/classification/architecture-selection.md)

## 2. System Context

**System boundary**  
The system is a static website that presents the owner’s selected work and provides a frictionless contact path. All pages are pre-rendered at build time and served from a CDN / edge platform. No application server or database is owned or operated by the owner.

**External actors**
- **Visitor** (primary user): potential freelance clients, peers, future employers. Browses Home, Project pages, About; may submit the contact form or use the visible email.
- **Owner** (content author / decision maker): edits Markdown / structured content files, commits, and reviews the resulting preview or production site.

**External systems**
- **Hosting / CDN platform** (Vercel, Netlify, or equivalent): builds the site from the repository, serves static assets globally, provides preview deployments.
- **Contact-form service** (Formspree, Netlify Forms, Resend, or equivalent): receives form submissions, applies spam protection, and delivers messages to the owner’s email. Holds data transiently only.
- **Email provider** of the owner: receives contact messages.
- **Git repository** (source of truth for both code and content).

No other external systems are in scope for v1.

## 3. Component View

| Component | Responsibility | Interfaces | Key data |
|-----------|----------------|------------|----------|
| **Content layer** | Owns all editorial content (projects, about, home intro) as Markdown / MDX or structured files with front-matter. Single source of truth for project data. | Read by the static site generator at build time. Edited by the owner via the Git repository. | Project records (title, summary, description, media references, role, year, tags, outcomes); page-level text. |
| **Page templates / routes** | Renders the public pages: Home, individual Project detail pages, About, and any shared layout. | Consumes content layer at build time; outputs static HTML + assets. | Route → content mapping; SEO metadata per page. |
| **Shared UI primitives** | Layout, navigation, project card, typography, spacing, and any reusable presentational elements. Single responsibility: consistent visual structure. | Used by page templates. | Design tokens (to be refined in Visual Blueprint). |
| **Media pipeline** | Optimises and serves images (and any future light media) with correct formats, sizes, and priority loading. | Input: source images in the repository or public folder. Output: optimised assets referenced by pages. | Image metadata, aspect-ratio conventions, responsive breakpoints. |
| **Contact form surface** | Collects visitor message (name, email, message, optional fields) and posts it to the external form service. Also surfaces a clear visible email / CTA on every page. | Browser → third-party form endpoint. | Transient form payload only; no local storage of PII. |
| **Build & deploy pipeline** | Transforms content + templates into static assets and publishes them to the hosting platform. Provides preview environments for content changes. | Triggered by Git commits / PRs. | Build artefacts (HTML, CSS, JS, optimised media). |

Every component has exactly one responsibility. No component owns both content authoring and runtime form processing; the latter is externalised.

## 4. Data Flow

**Primary content flow (read path)**  
1. Owner writes or updates Markdown / structured content files in the repository.  
2. Commit / push triggers the hosting platform build.  
3. Static site generator reads the content layer, applies page templates and media pipeline, and emits static HTML + assets.  
4. CDN / edge serves the pre-rendered pages to visitors.  
5. Consistency model: eventual consistency via rebuild. Content is consistent with the last successful build.

**Contact flow (write path)**  
1. Visitor fills the contact form on any page.  
2. Browser posts the payload directly to the chosen third-party form service (no origin server involvement).  
3. Form service applies spam protection and forwards the message to the owner’s email.  
4. No PII is stored by the portfolio system itself. Retention is governed solely by the form service’s policy (must be transient / contact-purpose only).

**Ownership**  
- Content: owned by the repository (owner-controlled).  
- Form data: owned transiently by the external form service.  
- Static assets: owned by the hosting platform after build.

No application database, session store, or server-side state exists inside the system boundary.

## 5. Technology Decisions

| Decision | Choice | Justification | Knowledge link |
|----------|--------|---------------|----------------|
| Frontend / meta-framework | **Next.js (App Router) with static generation** (or Astro as equally valid alternative) | Owner is comfortable with React/Next.js; Next.js provides excellent SSG support, image optimisation, and file-based routing while remaining compatible with a pure static export or Vercel/Netlify deployment. Astro is an equally strong pure-SSG alternative if maximum lean output is preferred later. | [knowledge/technologies/frontend.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/technologies/frontend.md) · [knowledge/blueprints/portfolio.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/blueprints/portfolio.md) |
| Rendering model | **SSG (Static Site Generation)** | Content is known at build time, changes infrequently, SEO and Core Web Vitals are mandatory, operational simplicity is a hard constraint. | [knowledge/classification/rendering-strategies.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/classification/rendering-strategies.md) |
| Content model | **Markdown / MDX (or JSON) files with front-matter, stored in the repository** | Satisfies FR-04 and NFR-05 exactly: owner edits files, commits, platform rebuilds. No CMS, no admin UI, no backend. | Requirements FR-04, NFR-05 · [knowledge/blueprints/portfolio.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/blueprints/portfolio.md) |
| Styling approach | **Utility-first (Tailwind) or CSS modules + design tokens** | Supports calm, minimal visual system; keeps CSS surface small; design tokens will be refined in the Visual Blueprint stage. Final choice left open for UI stage within this family. | [knowledge/technologies/frontend.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/technologies/frontend.md) |
| Image / media optimisation | **Framework-native image component + modern formats (AVIF/WebP) + responsive sizes** | Required to meet NFR-01 (Core Web Vitals Good, especially LCP) given that portfolios are image-heavy. | NFR-01 · [knowledge/blueprints/portfolio.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/blueprints/portfolio.md) |
| Contact form backend | **Zero-maintenance third-party service** (Formspree, Netlify Forms, Resend, or equivalent) | Satisfies FR-03, FR-08, NFR-03, NFR-04. No owner-operated server or database. Data remains transient. | Requirements FR-03, FR-08, NFR-03, NFR-04 |
| Hosting / deployment | **Vercel, Netlify, or equivalent static / edge platform** | Simple hosting required by constraints; preview deployments support the content-update workflow; global CDN supports performance targets. | Constraints · [knowledge/technologies/deployment.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/technologies/deployment.md) |
| TypeScript | **TypeScript (strict)** | Default for any non-trivial frontend; improves maintainability for a solo + AI workflow. | [knowledge/technologies/frontend.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/technologies/frontend.md) |
| Backend / database | **None** | Explicit non-goal and hard constraint. All dynamic behaviour is either build-time or externalised to the form service. | NFR-04, Out of Scope, Constraints |

Technology choices remain inside the families recommended by the Portfolio blueprint and the technologies knowledge base. No novel stacks were introduced.

## 6. NFR Mapping

| NFR ID | Architectural mechanism |
|--------|-------------------------|
| NFR-01 (Performance – Core Web Vitals Good) | SSG + CDN delivery, framework image optimisation (AVIF/WebP, priority loading, correct sizes), minimal client JavaScript, font subsetting / preloading discipline. LCP, INP and CLS targets are design constraints for later UX/UI and Tech Lead stages. |
| NFR-02 (Visual – calm, modern, trustworthy) | Architecture leaves visual system fully open to UX/UI stages; component set is intentionally small and token-driven so the “minimal / elegant / calm” direction can be realised without structural friction. |
| NFR-03 (Privacy – transient contact data) | Form posts go directly to a third-party service; no application-level storage of PII; retention policy is that of the chosen service (must be contact-purpose only). |
| NFR-04 (No owner-maintained backend) | Pure static output + external form service + managed hosting platform. No servers, containers, or databases under owner operation. |
| NFR-05 (Simplicity – Markdown content, no heavy animation libraries, no complex CMS) | Content layer is file-based; architecture forbids heavy animation libraries and any CMS. Motion, if any, must be light and CSS-first (to be constrained further in Visual Blueprint). |
| NFR-06 (Timeline 2–3 days) | Small complexity band, SSG, file-based content, and a narrow component set keep the implementation surface compatible with a 2–3 day first usable version. |
| NFR-07 (English, international, clean) | Content and design direction are left to later stages; architecture imposes no locale or culture-specific technical constraints. |

## 7. Risks & Open Questions

**Residual open questions (non-blocking for this stage)**  
Carried forward from Requirements:

| ID | Question | Blocking? | Notes |
|----|----------|-----------|-------|
| Q3 | Exact 4–6 projects to feature | no | Structure supports 4–6 project slots. Placeholders will be used until content is supplied. |
| Q7 | Domain, logo, typography, colour palette, photography, bio text, final project descriptions | no | All treated as placeholders. Visual system will be defined in UX / UI stages; content supplied by owner. |

**Architectural risks (accepted or mitigated)**  
- **Image-heavy LCP risk** — Portfolios are media-rich. Mitigation: mandatory image optimisation pipeline and explicit Core Web Vitals budget already recorded in NFR-01. Residual risk is implementation discipline (Tech Lead / implementation stage).  
- **Form-service dependency** — Contact path depends on a third-party. Mitigation: choose a mature, well-documented service; keep a visible email/CTA as fallback on every page (FR-03 / FR-08).  
- **Content authoring friction** — Markdown editing assumes basic Git comfort. Accepted under the explicit “very little friction = edit files + commit” decision already recorded in Requirements.  

No blocking architectural open questions remain. Downstream stages (UX, UI, Tech Lead) can proceed.

---

## Traceability

Every decision maps to a requirement, constraint, or knowledge source.

| Decision / Element | Source (requirement / knowledge) | Notes |
|--------------------|----------------------------------|-------|
| Primary type Portfolio / Brochure | requirements.md (FR-01, FR-02, FR-05, FR-06, FR-07); brief.md §1–2; knowledge/classification/website-types.md | Direct match to showcase + contact goals |
| Complexity S | brief.md constraints (solo, 2–3 days); requirements NFR-06; knowledge/classification/project-complexity.md | Limited surface, static content |
| Rendering SSG | NFR-01, NFR-04, NFR-05; knowledge/classification/rendering-strategies.md; knowledge/blueprints/portfolio.md | Content known at build, performance + simplicity |
| Architecture family Jamstack / Content-Centric | Same drivers as above; knowledge/classification/architecture-selection.md | Consistent with type + complexity + rendering |
| Content = Markdown / MDX in-repo | FR-04, NFR-05; Decision Q2/Q5 in requirements.md | Explicitly closed in Analyst stage |
| Separate project pages | FR-06; Decision Q1 in requirements.md | Already decided |
| Hybrid contact (email + form service) | FR-03, FR-08, NFR-03; Decision Q6 | Already decided |
| Next.js (or Astro) + SSG | Owner preference (brief.md §4); knowledge/technologies/frontend.md; knowledge/blueprints/portfolio.md | Within recommended families |
| Vercel / Netlify (or equivalent) | Constraints (simple hosting); knowledge/technologies/deployment.md | Managed static/edge platforms |
| No backend / no database | NFR-04, Out of Scope, Constraints | Hard constraint |
| Core Web Vitals Good targets | NFR-01 | Mapped to SSG + image pipeline + minimal JS |
| Residual Q3, Q7 | requirements.md §5 | Content and brand assets remain human-owned |

**Source of truth:** `requirements.md` (v0.2) and `brief.md`. This Architecture Blueprint introduces no new product requirements; it only structures the system that satisfies the already-accepted requirements and constraints.
