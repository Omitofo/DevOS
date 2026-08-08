# Project Brief

**Version:** 0.2  
**Status:** draft  
**Upstream:** intake.md  
**Assumptions:** none  
**Open questions:** see §7; none currently blocking  
**Last updated:** 2026-08-08  
**Authoring agent / human:** Planner

---

> **Producing agent:** Planner  
> **Purpose:** Define the problem, goals, constraints, success criteria, stakeholders, and unresolved questions for downstream DevOS stages.  
> **Rules:** Requirements are extracted from `intake.md`; any inference is explicitly marked. No architecture, implementation, or invented requirements are introduced.

## 1. Problem Statement

The owner currently shares their work through scattered links (GitHub, LinkedIn, Google Drive). Potential clients often find it difficult to get a quick overview of what the owner does and how to hire them. The project therefore needs a single, professional place that presents selected work and makes contacting the owner frictionless.

**One-sentence pitch:** “A clean, elegant, fast personal website that showcases my work and makes it easy for potential clients to contact me.”

**Source:** `intake.md` §1–2.

## 2. Goals

- Provide a single, professional website that showcases selected work and makes contacting the owner frictionless. (`intake.md` §1–2)
- Enable a visitor to understand what the owner does and see 4–6 selected projects within 60 seconds. (`intake.md` §3)
- Make a contact form or clear email/CTA available on every page. (`intake.md` §3)
- Make the site load quickly on both mobile and desktop. (`intake.md` §3)
- Allow the owner to update project content without touching code, or with very little friction. (`intake.md` §3)
- Make the site feel calm, modern, and trustworthy — not too flashy or template-looking. (`intake.md` §3)
- Deliver a first usable version within 2–3 days of implementation starting. (`intake.md` §4)

## 3. Non-Goals / Out of Scope

- Blog or articles. (`intake.md` §6)
- Client login or dashboard. (`intake.md` §6)
- E-commerce or payments. (`intake.md` §6)
- Multi-language support. (`intake.md` §6)
- Dark/light mode toggle; this may be considered later. (`intake.md` §6)
- Complex animations or interactive 3D elements. (`intake.md` §6)
- Heavy animation libraries. (`intake.md` §4)
- Complex CMS. (`intake.md` §4)
- Any solution requiring a backend server that the owner must maintain. (`intake.md` §4)

## 4. Constraints

### Timeline
- First usable version within 2–3 days of implementation start. (`intake.md` §4)

### Team / Budget
- Solo effort: owner plus AI assistance.
- No paid team. (`intake.md` §4)

### Technology / Platform Preferences
- Prefer modern web technologies.
- Owner is comfortable with React/Next.js or similar.
- Hosting should be simple, such as Vercel, Netlify, or equivalent. (`intake.md` §4)

These are preferences, not final implementation decisions. Technology selection belongs to downstream DevOS stages.

### Technology / Approach Exclusions
- Avoid heavy animation libraries.
- Avoid a complex CMS.
- Avoid anything requiring a backend server maintained by the owner. (`intake.md` §4)

### Compliance
- Basic privacy handling is required for contact-form data.
- No special certifications are required. (`intake.md` §4)

### Existing Systems
- None. This is a greenfield project. (`intake.md` §4)

### Audience / Language
- Primarily English-speaking audience.
- Design should feel international and clean. (`intake.md` §5)

## 5. Success Metrics

A v1 is successful when:

1. A visitor can understand what the owner does and see 4–6 selected projects within 60 seconds. (`intake.md` §3)
2. Every page provides either a contact form or a clear email/CTA. (`intake.md` §3)
3. The site loads quickly on mobile and desktop. (`intake.md` §3)
4. The owner can update project content without touching code, or with very little friction. (`intake.md` §3)
5. The resulting experience feels calm, modern, and trustworthy rather than flashy or template-like. (`intake.md` §3)
6. A first usable version is available within 2–3 days of implementation starting. (`intake.md` §4)

**Measurement gaps:** The Intake does not define a numeric performance budget or a formal measurement method for the qualitative visual/brand goal. These remain open questions rather than invented thresholds.

## 6. Stakeholders & Personas (high-level)

### Primary decision maker
- **Renatus Cartesius**, project owner. (`intake.md` §1)

### Primary users
- Potential freelance clients, including startups, small agencies, and product teams, looking for a designer/developer. (`intake.md` §5)

### Secondary users
- Peers, future employers, or collaborators who want a quick sense of the owner's work. (`intake.md` §5)

### Business context
- Personal brand / freelance practice. (`intake.md` §5)

### Geographic / cultural context
- Primarily English-speaking audience.
- The experience should feel international and clean. (`intake.md` §5)

## 7. Open Questions

| ID | Question | Blocking? | Source |
|---|---|---|---|
| Q1 | Should project case studies be separate pages or expand in place? | no | `intake.md` §7 — preference: whichever is cleaner |
| Q2 | Is Markdown/JSON-based project editing acceptable for v1, or is a lightweight CMS required? | no | `intake.md` §7 — preference leans toward simplicity |
| Q3 | What exact 4–6 projects should be featured? | no | `intake.md` §7 — to be decided during content work |
| Q4 | What concrete performance target should define “loads quickly”? | no | Gap in `intake.md` §3; no numeric target was provided |
| Q5 | What exactly counts as “very little friction” for project-content updates? | no | Gap in `intake.md` §3; no editing workflow threshold was provided |
| Q6 | Which contact mechanism should v1 use: a contact form or a clear email/CTA, and what privacy/spam-handling expectations apply? | no | `intake.md` §3–4; both options are permitted but none is selected |
| Q7 | Are existing brand assets or content available (domain, logo, typography, colours, photography, bio, project descriptions), or should these remain placeholders for later stages? | no | Gap; not specified in `intake.md` |

**Human direction already established:** The visual direction leans minimal, elegant, and calm; this is not an unresolved requirement category, although the exact visual system remains for downstream UX/UI work. (`intake.md` §7, Q4)

## 8. Planning Boundaries

This brief intentionally does **not** decide:

- page architecture or routing;
- information architecture;
- interaction or UX patterns;
- visual design system;
- framework/library selection;
- content model or CMS implementation;
- contact-service implementation;
- hosting/deployment architecture;
- detailed accessibility, SEO, security, or performance budgets.

Those decisions should be derived by the appropriate downstream DevOS stages from this brief, the Intake, project knowledge, and documented human clarifications.

## Traceability

| Statement / Section | Source | Notes |
|---|---|---|
| §1 Problem Statement | `intake.md` §1–2 | Direct extraction, lightly edited for brief format |
| §2 Goals | `intake.md` §1–4 | Direct extraction; timeline retained |
| §3 Non-Goals | `intake.md` §4, §6 | Direct extraction |
| §4 Constraints | `intake.md` §4–5 | Direct extraction, grouped for downstream use |
| §5 Success Metrics | `intake.md` §3–4 | Direct extraction; measurement gaps explicitly preserved |
| §6 Stakeholders & Personas | `intake.md` §1, §5 | Direct extraction |
| §7 Q1–Q3 | `intake.md` §7 | Carried forward with human notes |
| §7 Q4–Q7 | Planner gap analysis | Explicitly marked as gaps/inferences; not promoted to requirements |
| §8 Planning Boundaries | DevOS Planner-stage discipline | Clarifies what this artifact intentionally does not decide; not a product requirement |

**Source of truth:** `intake.md` is the human-owned source of project intent. This brief is its structured planning derivative. No statement above should be treated as a confirmed requirement when it is explicitly marked as a gap or open question.
