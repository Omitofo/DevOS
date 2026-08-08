# Implementation Plan

**Version:** 0.2  
**Status:** draft  
**Upstream:** architecture.md, requirements.md, journeys.md, visual.md  
**Assumptions:** none  
**Open questions:** see §5 (none blocking)  
**Last updated:** 2026-08-08  
**Authoring agent / human:** Tech Lead (with human direction to close remaining open questions for production)

---

> **Producing agent:** Tech Lead  
> **Purpose:** Produce a work-breakdown, sequencing, risk register, and test strategy that is fully implementation-ready for a downstream coding session. The plan is traceable to all prior artifacts and contains no production code. Remaining open questions have been closed with explicit decisions under human direction.

## 1. Work Breakdown

| WP-ID | Description | Related components | Related requirements | Estimated complexity |
|-------|-------------|--------------------|----------------------|----------------------|
| **WP-01** | Project scaffolding: Next.js (App Router) + TypeScript (strict) + Inter via `next/font` + styling system (Tailwind or CSS modules + design tokens) + basic folder structure (`app/`, `content/`, `components/`, `public/`). Repository name: `devos-portfolio`. | Build & deploy pipeline, Shared UI primitives | NFR-04, NFR-05, NFR-06, Architecture tech decisions | S |
| **WP-02** | Content model: Markdown / MDX schema for projects and pages with front-matter (title, summary, date, role, tags, media paths, body). Content folder structure + the six placeholder projects and About/Home intro text defined in §6. | Content layer | FR-04, FR-05, FR-06, FR-07, NFR-05 | S |
| **WP-03** | Design tokens & global styles: Implement colour, typography (Inter), spacing, radius, and motion tokens from Visual Blueprint v0.2. Base layout (max-width, padding, responsive grid utilities). | Shared UI primitives | NFR-02, Visual §1, Visual §2 | S |
| **WP-04** | Shared shell: `SiteHeader` (text wordmark “Renatus Cartesius” or short title + nav), `SiteFooter`, root layout that injects contact affordances and applies tokens. Mobile navigation behaviour. | SiteHeader, SiteFooter, Shared shell | FR-03, FR-08, FR-09, J-01–J-04, Visual §3 | S |
| **WP-05** | Home page: `IntroBlock` + responsive project grid of `ProjectCard` components driven by content layer. 60-second scan optimised hierarchy. | IntroBlock, ProjectCard, Home template | FR-01, FR-02, FR-05, J-01, Visual §2–3 | M |
| **WP-06** | Project detail pages: Dynamic route for each project (`/work/[slug]` or equivalent). `ProjectDetail` layout with title, meta, narrative, media. Back / next navigation. | ProjectDetail, MediaImage | FR-06, FR-09, FR-10, J-02 | M |
| **WP-07** | About page: Simple content-driven page using the shared shell and the placeholder bio from §6. | About template | FR-07, J-04 | S |
| **WP-08** | Contact surface: `ContactForm` posting to **Formspree** (free tier) + persistent `EmailCTA` (`hello@renatuscartesius.com` as placeholder delivery address). Success / error states. | ContactForm, EmailCTA | FR-03, FR-08, NFR-03, J-03, E-01 | M |
| **WP-09** | Media pipeline: Image component with responsive sizes, modern formats (AVIF/WebP where supported), priority loading for LCP candidates, and correct `alt` handling. Placeholder images for the six projects. | MediaImage | NFR-01, Architecture media pipeline, Visual §3 | S |
| **WP-10** | SEO basics: Unique `<title>` and meta description per page, clean URLs, Open Graph defaults, `robots.txt` / basic sitemap if trivial. | Page templates | FR-10 | S |
| **WP-11** | Accessibility & responsive polish: Keyboard navigation, visible focus, form labels, reduced-motion support, final breakpoint checks, touch targets. | All UI components | FR-09, NFR-02, Visual §4–5, E-03 | S |
| **WP-12** | Deploy & preview: Connect `devos-portfolio` repository to **Vercel**. Confirm production and preview deploys work from content commits. | Build & deploy pipeline | NFR-04, NFR-06, Architecture hosting decision | S |

Complexity legend: **S** = small (hours), **M** = medium (half-day to one day). Overall fit remains inside the 2–3 day first-usable-version target (NFR-06).

## 2. Sequencing

Recommended order that maximises vertical slices and respects dependencies:

1. **WP-01** — Scaffolding (foundation for everything)
2. **WP-03** — Design tokens & global styles (needed by all UI)
3. **WP-02** — Content model + the six placeholder projects (needed by pages)
4. **WP-04** — Shared shell (Header / Footer / layout)
5. **WP-05** — Home page (first visible vertical slice + 60 s scan)
6. **WP-09** — Media pipeline (supports Home cards and Project detail)
7. **WP-06** — Project detail pages
8. **WP-07** — About page
9. **WP-08** — Contact form (Formspree) + Email CTA
10. **WP-10** — SEO basics
11. **WP-11** — Accessibility & responsive polish
12. **WP-12** — Deploy to Vercel + preview environments

**Definition of “first usable version”:**  
WP-01 → WP-08 complete and deployed on Vercel (WP-12). Visitor can scan projects, open a case study, read About, and contact the owner.

## 3. Test Strategy

| Requirement ID | Verification method | Notes |
|----------------|---------------------|-------|
| FR-01 | Manual + visual | Single URL presents selected work |
| FR-02 | Moderated timed test or stopwatch | First-time visitor states offering and identifies ≥ 4 projects within 60 s |
| FR-03 | Manual inspection on every page | Contact form **and** clear email/CTA present |
| FR-04 | Manual process test | Edit content file → commit → Vercel rebuild → change visible |
| FR-05 | Manual + visual | Home shows intro + selected work |
| FR-06 | Manual + URL check | Each project has its own detail page |
| FR-07 | Manual navigation | About reachable from primary nav or Home |
| FR-08 | End-to-end form test + mailto check | Formspree receives submission; email CTA works |
| FR-09 | Manual viewport + device check | No horizontal scroll of primary content |
| FR-10 | Source inspection + Lighthouse SEO | Unique title & meta; clean URLs |
| NFR-01 | Lighthouse (mobile mid-tier / throttled) | LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1 |
| NFR-02 | Subjective review against Visual Blueprint v0.2 | Calm, modern, trustworthy; matches locked tokens |
| NFR-03 | Process check | Form data handled only by Formspree; no local PII storage |
| NFR-04 | Architecture / deploy check | No owner-operated server or database |
| NFR-05 | Dependency + content audit | No heavy animation libraries; content is file-based |
| NFR-06 | Calendar | First usable version within 2–3 days of implementation start |
| NFR-07 | Content review | English, international, clean tone |

## 4. Risk Register

| Risk | Impact | Likelihood | Mitigation | Residual |
|------|--------|------------|------------|----------|
| Image-heavy LCP exceeds budget | H | M | Strict MediaImage pipeline, priority on LCP candidates, modern formats (WP-09) | Low if discipline kept |
| Formspree free-tier limit or misconfiguration | M | L | Visible EmailCTA as permanent fallback on every page; test form early (WP-08) | Low |
| Content still placeholder at first deploy | M | H | Structure fully functional; placeholders clearly temporary; easy to replace later via content files | Accepted for v1 |
| Scope creep beyond 2–3 day target | H | M | Stick to sequenced WP list; first usable = WP-01–08 + WP-12 | Low if sequence followed |
| Accessibility gaps discovered late | M | L | WP-11 dedicated; tokens/components designed with AA contrast and keyboard use from the start | Low |

## 5. Open Questions & Blockers

| ID  | Question / Blocker | Blocking? | Status |
|-----|--------------------|-----------|--------|
| — | None blocking | — | All previous open questions closed in §6 |

## 6. Resolved Decisions (v0.1 → v0.2)

### TL-01 — Form service & delivery email
- **Form service:** **Formspree** (free tier).  
  Rationale: Dead-simple, works with any host, free tier is more than enough for a personal portfolio, requires almost no configuration (just a form endpoint). Sends the form fields as an email.
- **Delivery address (placeholder):** `hello@renatuscartesius.com`  
  Owner can replace this with a real address later; Formspree allows updating the notification email without code changes.

### TL-02 — Repository & hosting
- **Repository name:** `devos-portfolio`
- **Hosting:** **Vercel**  
  Rationale: Best-in-class Next.js support, free hobby plan, instant preview deployments on every push, zero server maintenance. Formspree works equally well on Vercel (no dependency on Netlify Forms).

### Q3 — Placeholder projects (6)
Creative but clearly marked placeholder case studies that fit a calm, professional designer/developer portfolio:

| # | Title | One-line summary (placeholder) |
|---|-------|--------------------------------|
| 1 | **Atlas Design System** | A minimal component library and token set built for internal product teams. |
| 2 | **Northwind Notes** | A calm, keyboard-first note-taking experiment focused on speed and clarity. |
| 3 | **Signal Dashboard** | Lightweight operational overview for small teams — clarity over decoration. |
| 4 | **Harbor Brand Identity** | Visual identity and website for a fictional independent studio. |
| 5 | **Cascade CLI** | Open-source command-line tool that turns structured content into static sites. |
| 6 | **Quiet Portfolio (meta)** | The design and build process of this very site — documented as a case study. |

Media: simple geometric or abstract placeholder images (or solid colour blocks with labels) until real photography is supplied.

### Q7-content — Remaining content placeholders
- **Domain:** `renatuscartesius.com` (placeholder — owner can point a real domain later).
- **Logo mark:** None for v1. Identity is the text wordmark “Renatus Cartesius”.
- **Bio (About placeholder):**  
  > Renatus Cartesius is a designer and developer focused on calm, precise digital products. He works with startups and small teams that value clarity over noise.
- **Photography:** None required for v1. Project cards and detail pages use abstract/geometric placeholders or carefully chosen open-licence stills if desired later.
- **Project write-ups:** Short placeholder body text will be generated alongside the front-matter in WP-02 so every page is readable.

All of the above are explicitly temporary and can be replaced by editing content files only (FR-04).

---

## Traceability

| WP-ID | Architecture Component(s) | Requirement ID(s) | Notes |
|-------|---------------------------|-------------------|-------|
| WP-01 | Build & deploy pipeline, Shared UI | NFR-04, NFR-05, NFR-06 | Repo = `devos-portfolio` |
| WP-02 | Content layer | FR-04, FR-05, FR-06, FR-07, NFR-05 | Six placeholder projects locked |
| WP-03 | Shared UI primitives | NFR-02, Visual tokens | |
| WP-04 | SiteHeader, SiteFooter, Shared shell | FR-03, FR-08, FR-09 | Text wordmark |
| WP-05 | IntroBlock, ProjectCard, Home | FR-01, FR-02, FR-05, J-01 | |
| WP-06 | ProjectDetail, MediaImage | FR-06, FR-09, FR-10, J-02 | |
| WP-07 | About template | FR-07, J-04 | Placeholder bio |
| WP-08 | ContactForm, EmailCTA | FR-03, FR-08, NFR-03, J-03, E-01 | Formspree + placeholder email |
| WP-09 | MediaImage | NFR-01 | |
| WP-10 | Page templates | FR-10 | |
| WP-11 | All UI components | FR-09, Visual §4–5, E-03 | |
| WP-12 | Build & deploy pipeline | NFR-04, NFR-06 | Vercel |

**Source of truth:** `architecture.md`, `requirements.md`, `journeys.md`, and `visual.md` (v0.2).  
This plan introduces no new product requirements and contains no production code. It is ready for a downstream implementation session.
