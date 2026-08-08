# Requirements

**Version:** 0.2  
**Status:** draft  
**Upstream:** brief.md, intake.md  
**Assumptions:** none (all former open questions resolved by explicit human-directed engineering decisions recorded below)  
**Open questions:** see §5 (only residual content placeholders remain)  
**Last updated:** 2026-08-08  
**Authoring agent / human:** Analyst (with human direction to resolve open questions for production)

---

> **Producing agent:** Analyst  
> **Purpose:** Derive testable functional and non-functional requirements from the Project Brief and Intake. Every requirement is atomic, prioritised, and traceable. Open questions from v0.1 have been resolved by explicit engineering decisions (see §6) under human direction that this is a real production project. No requirements were invented; only previously open engineering choices were closed.

## 1. Functional Requirements

| ID    | Statement | Priority | Source | Testability note |
|-------|-----------|----------|--------|------------------|
| FR-01 | The site shall provide a single, professional website that presents selected work of the owner. | Must | brief.md §2; intake.md §1–2 | A visitor can locate and view the owner’s selected work from one primary site URL without needing external scattered links. |
| FR-02 | A visitor shall be able to understand what the owner does and see 4–6 selected projects within 60 seconds of arrival. | Must | brief.md §2, §5; intake.md §3 | Timed observation or moderated test: a first-time visitor can correctly state the owner’s role/offering and identify at least four projects within 60 s. |
| FR-03 | Every page of the site shall expose a contact form **and** a clear email/CTA. | Must | brief.md §2, §5; intake.md §3 + Decision Q6 | Manual inspection: on each page both a contact form and a clearly labelled email/CTA are present and reachable without leaving the page. |
| FR-04 | The owner shall be able to update project content by editing Markdown or structured content files (JSON/MDX front-matter) in the repository; a subsequent platform rebuild shall publish the change. No application source code changes are required. | Must | brief.md §2, §5; intake.md §3 + Decisions Q2, Q5 | Owner edits a content file, commits, and the live site reflects the change after the hosting platform rebuilds. No admin UI or CMS is required for v1. |
| FR-05 | The site shall include a home / landing view that presents a short introduction and selected work. | Must | intake.md §6 (In scope) | Home view renders an introduction and a set of selected projects. |
| FR-06 | The site shall include **separate project detail pages** for each of the 4–6 selected projects (case-study style). | Must | intake.md §6 (In scope) + Decision Q1 | Each featured project has its own URL and page that displays more detail than the summary card on the home view. |
| FR-07 | The site shall include an About section. | Must | intake.md §6 (In scope) | An About section or page is present and reachable from primary navigation or the home view. |
| FR-08 | The site shall provide a hybrid contact method: a lightweight form (powered by a zero-maintenance third-party service) **and** a clear visible email address / CTA, plus any additional social or professional links the owner supplies. | Must | intake.md §6 (In scope); brief.md §2 + Decision Q6 | Form submission works end-to-end via the chosen service; email is visible and clickable; no custom backend is required. |
| FR-09 | The site shall be responsive on mobile and desktop viewports. | Must | intake.md §6 (In scope) | Layout and content remain usable and readable at common mobile and desktop breakpoints without horizontal scrolling of primary content. |
| FR-10 | The site shall implement basic SEO consisting of a page title, meta description, and clean URLs. | Must | intake.md §6 (In scope) | Each page has a unique, descriptive `<title>` and meta description; URLs are human-readable and free of query-string or hash routing for primary content. |

## 2. Non-Functional Requirements

| ID     | Category       | Statement | Priority | Source | Measurement |
|--------|----------------|-----------|----------|--------|-------------|
| NFR-01 | Performance    | The site shall meet Core Web Vitals “Good” thresholds: LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1 on mid-tier mobile devices. | Must | brief.md §2, §5; intake.md §3 + Decision Q4 | Measured with Lighthouse or equivalent on a representative mid-tier mobile device / throttled 4G profile. |
| NFR-02 | Visual / Brand | The site shall feel calm, modern, and trustworthy — not too flashy nor template-looking. Direction: minimal, elegant, calm. | Must | brief.md §2, §5; intake.md §3, §7 | Subjective evaluation against the stated character; exact visual system is defined in later UX/UI stages. |
| NFR-03 | Privacy        | Contact-form data shall be handled by a zero-maintenance third-party service; data is transient and used only for the contact purpose. No long-term owner-side storage. | Must | brief.md §4; intake.md §4 + Decision Q6 | Chosen service’s privacy policy and retention behaviour satisfy basic privacy expectations; no custom database is introduced. |
| NFR-04 | Maintainability| The solution shall not require a backend server that the owner must maintain. | Must | brief.md §3, §4; intake.md §4 | Deployment target is a static or managed platform (Vercel, Netlify or equivalent) that places no ongoing server-administration burden on the owner. |
| NFR-05 | Simplicity     | The solution shall use Markdown / structured content files for project data and shall not introduce heavy animation libraries or a complex CMS. | Must | brief.md §3, §4; intake.md §4 + Decisions Q2, Q5 | Content lives in the repository; no heavy animation libraries are present in the dependency graph. |
| NFR-06 | Timeline       | A first usable version shall be deliverable within 2–3 days of implementation start. | Must | brief.md §2, §4, §5; intake.md §4 | Calendar check against implementation start date. |
| NFR-07 | Audience       | Primary content and design shall target an English-speaking audience and feel international and clean. | Should | brief.md §4, §6; intake.md §5 | Content is written in English; visual language avoids strongly culture-specific cues that would feel local rather than international. |

## 3. Constraints (inherited)

These constraints act as hard requirements and are restated from upstream for downstream traceability:

- **Timeline:** First usable version within 2–3 days of starting implementation. (intake.md §4; brief.md §4)
- **Team / Budget:** Solo (owner + AI assistance). No paid team. (intake.md §4; brief.md §4)
- **Technology preferences (not final decisions):** Modern web technologies; owner comfortable with React/Next.js or similar; simple hosting (Vercel, Netlify or equivalent). Technology selection belongs to later stages. (intake.md §4; brief.md §4)
- **Must-avoid:** Heavy animation libraries; complex CMS; any solution requiring a backend server the owner must maintain. (intake.md §4; brief.md §3–4)
- **Compliance:** Basic privacy for contact-form data. No special certifications. (intake.md §4; brief.md §4)
- **Integrations:** None. Greenfield site. (intake.md §4; brief.md §4)
- **Audience language / geo:** Primarily English-speaking; design should feel international and clean. (intake.md §5; brief.md §4, §6)

## 4. Out of Scope

Confirmed non-goals carried forward from Intake and Brief (must not appear as requirements):

- Blog or articles
- Client login / dashboard
- E-commerce or payments
- Multi-language support (for v1)
- Dark/light mode toggle (nice-to-have later, not required now)
- Complex animations or interactive 3D elements
- Heavy animation libraries
- Complex CMS
- Any solution that requires a backend server the owner must maintain

(Sources: intake.md §4, §6; brief.md §3)

## 5. Open Questions (residual)

| ID  | Question | Blocking? | Related requirement | Notes |
|-----|----------|-----------|---------------------|-------|
| Q3  | What exact 4–6 projects should be featured? | no | FR-02, FR-05, FR-06 | Structure supports 4–6 slots. Exact titles, descriptions, images and write-ups remain human-supplied content. Placeholders will be used until content is provided. |
| Q7  | Domain, logo, typography, colour palette, photography, bio text, and final project descriptions. | no | FR-01, NFR-02 | All treated as placeholders. Visual system and content will be defined in UX / UI stages and supplied by the owner. |

All other former open questions (Q1, Q2, Q4, Q5, Q6) have been resolved by the decisions recorded in §6 and are no longer open.

## 6. Resolved Decisions (v0.1 → v0.2)

These decisions were made under explicit human direction that portfolio-v1 is a real production project and that engineering choices should be closed where the information is sufficient. Each decision is traceable to upstream constraints and the project’s stated preferences for simplicity, calm aesthetics, and zero-maintenance hosting.

| Former ID | Decision | Rationale summary | Effect on requirements |
|-----------|----------|-------------------|------------------------|
| Q1 | Project case studies use **separate pages**. | Cleaner URLs, better SEO, more professional depth, still fully static. Expand-in-place adds unnecessary interaction complexity for a calm portfolio. | FR-06 updated |
| Q2 | **Markdown / structured content files** (front-matter + MD or JSON) are the accepted content model for v1. No CMS. | Matches “lean toward simplicity”, “no complex CMS”, and the static-hosting constraint. | FR-04, NFR-05 updated |
| Q4 | “Loads quickly” is defined as **Core Web Vitals Good** thresholds (LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1). | Concrete, measurable, and realistic for a modern static site. | NFR-01 updated |
| Q5 | “Very little friction” means **edit content files in the repo + platform auto-deploy**. No admin UI required for v1. | Lowest-friction path that still satisfies the no-owner-maintained-backend rule. | FR-04, NFR-05 updated |
| Q6 | **Hybrid contact**: visible email/CTA on every page **plus** a lightweight form via a zero-maintenance third-party service. Form data is transient. | Trust + convenience; zero custom backend; basic privacy by design. | FR-03, FR-08, NFR-03 updated |
| Q7 (partial) | No brand assets currently exist. All visual and content assets remain **placeholders** until supplied by the owner or defined in later stages. | Contract-compliant; prevents invention of brand identity. | Residual open question retained |

## Traceability Matrix

Every FR/NFR maps to an upstream source or to an explicit decision recorded in §6. No requirement was invented.

| Requirement ID | Upstream Source(s) | Notes |
|----------------|--------------------|-------|
| FR-01 | brief.md §2; intake.md §1–2 | Direct derivation of the core pitch and problem statement |
| FR-02 | brief.md §2, §5; intake.md §3 | Success definition; 60-second criterion preserved |
| FR-03 | brief.md §2, §5; intake.md §3 + Decision Q6 | Contact availability on every page; hybrid form + email |
| FR-04 | brief.md §2, §5; intake.md §3 + Decisions Q2, Q5 | Content-update goal closed with concrete low-friction method |
| FR-05 | intake.md §6 (In scope) | Home / landing view |
| FR-06 | intake.md §6 (In scope) + Decision Q1 | Project detail pages (separate pages) |
| FR-07 | intake.md §6 (In scope) | About section |
| FR-08 | intake.md §6 (In scope); brief.md §2 + Decision Q6 | Hybrid contact method |
| FR-09 | intake.md §6 (In scope) | Responsive design |
| FR-10 | intake.md §6 (In scope) | Basic SEO |
| NFR-01 | brief.md §2, §5; intake.md §3 + Decision Q4 | Performance now has concrete Core Web Vitals targets |
| NFR-02 | brief.md §2, §5; intake.md §3, §7 | Visual character; direction remains minimal / elegant / calm |
| NFR-03 | brief.md §4; intake.md §4 + Decision Q6 | Basic privacy via third-party service, transient data |
| NFR-04 | brief.md §3, §4; intake.md §4 | No owner-maintained backend |
| NFR-05 | brief.md §3, §4; intake.md §4 + Decisions Q2, Q5 | Avoid heavy animation libraries and complex CMS; Markdown content |
| NFR-06 | brief.md §2, §4, §5; intake.md §4 | Timeline constraint elevated to NFR |
| NFR-07 | brief.md §4, §6; intake.md §5 | Audience / language / cultural feel |
| Constraints (all) | intake.md §4–5; brief.md §4 | Restated, not newly invented |
| Out of Scope (all) | intake.md §4, §6; brief.md §3 | Confirmed non-goals |
| Decisions Q1,Q2,Q4,Q5,Q6 | Human direction + engineering judgment against existing constraints | Recorded in §6; former open questions closed |
| Residual Q3, Q7 | intake.md §7; brief.md §7 | Content and brand assets remain human-owned placeholders |

**Source of truth:** `intake.md` (human-owned) and `brief.md` (Planner derivative). Version 0.2 incorporates only the engineering closures explicitly requested by the human for a real production trajectory. No architecture, implementation code, or invented product vision has been introduced.
