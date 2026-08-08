# User Journey

**Version:** 0.1  
**Status:** draft  
**Upstream:** requirements.md, brief.md  
**Assumptions:** none  
**Open questions:** see §5 (only residual content/brand placeholders)  
**Last updated:** 2026-08-08  
**Authoring agent / human:** UX

---

> **Producing agent:** UX  
> **Purpose:** Define personas, primary and edge-case flows, pain points, and success criteria grounded exclusively in the Requirements and Project Brief. Cognitive load is considered. No user needs are invented.

## 1. Personas

| Persona | Goals | Key characteristics | Source |
|---------|-------|---------------------|--------|
| **P1 — Potential Client** | Quickly understand what the owner does, evaluate fit through selected work, and contact the owner with low friction. | Freelance buyer (startup, small agency, or product team). Time-poor. Values clarity, professionalism, and evidence of craft. Primarily English-speaking. Arrives from referral, LinkedIn, or search. | brief.md §6; intake.md §5; requirements FR-01, FR-02, FR-03, FR-08 |
| **P2 — Peer / Collaborator / Future Employer** | Get a fast, trustworthy sense of the owner’s skills, taste, and working style. | Designer, developer, or hiring manager scanning portfolios. Looks for signal of quality and personality without marketing noise. | brief.md §6; intake.md §5 |
| **P3 — Owner (Content Author)** | Keep project content up to date with very little friction so the site remains accurate and professional. | Solo practitioner comfortable with modern web tooling and Git. Does not want a complex CMS or backend to maintain. | brief.md §4, §6; requirements FR-04, NFR-05 |

## 2. Primary Journeys

### Journey J-01 — First impression & project overview (60-second scan)

- **Trigger:** Visitor lands on the Home / landing view (direct URL, search, or shared link).
- **Steps:**
  1. Page loads quickly on mobile or desktop.
  2. Visitor immediately sees a short, clear introduction that states what the owner does.
  3. Visitor sees a set of 4–6 selected project cards / summaries without scrolling excessively or hunting.
  4. Within 60 seconds the visitor can correctly state the owner’s offering and name at least four projects.
  5. Clear navigation or cues exist to go deeper into any project, the About section, or contact.
- **Success criteria:** A first-time visitor understands the offering and can identify 4–6 projects within 60 seconds (FR-02 / Brief success metric). Site feels calm and professional (NFR-02).
- **Related requirements:** FR-01, FR-02, FR-05, FR-09, FR-10, NFR-01, NFR-02

### Journey J-02 — Explore a project case study

- **Trigger:** Visitor clicks a project card / link from the Home view (or arrives via deep link).
- **Steps:**
  1. Visitor is taken to a dedicated project detail page (separate URL).
  2. Page presents more depth than the summary: context, role, process or outcomes, and media as available.
  3. Visitor can return to the project list / Home or move to another project without friction.
  4. Contact form and clear email/CTA remain visible and reachable on this page.
- **Success criteria:** Visitor can examine a single project in greater detail and still reach contact without leaving the site flow. Page remains fast and readable on mobile.
- **Related requirements:** FR-06, FR-03, FR-08, FR-09, FR-10, NFR-01

### Journey J-03 — Contact the owner

- **Trigger:** Visitor decides to get in touch (from any page).
- **Steps:**
  1. On the current page the visitor sees both a contact form and a clear, visible email / CTA (no hunting required).
  2. Visitor either:
     - a) Fills the lightweight form (name, email, message, optional fields) and submits, or
     - b) Clicks / copies the visible email address.
  3. If form is used: confirmation of successful submission is shown; message is delivered via the third-party service to the owner.
  4. Visitor can also follow any additional professional links the owner has supplied.
- **Success criteria:** Contact is possible from every page with minimal steps. Both form and email paths work. No custom backend is required.
- **Related requirements:** FR-03, FR-08, NFR-03, NFR-04

### Journey J-04 — Learn about the owner

- **Trigger:** Visitor wants context beyond the projects (background, approach, personality).
- **Steps:**
  1. Visitor navigates to the About section / page from primary navigation or Home.
  2. Visitor reads a concise About presentation.
  3. Contact form and email/CTA remain available on this surface.
- **Success criteria:** About content is reachable and supports the overall impression of calm professionalism. Contact remains one step away.
- **Related requirements:** FR-07, FR-03, FR-08

### Journey J-05 — Owner updates project content

- **Trigger:** Owner needs to add, edit, or retire a project (or update About / intro text).
- **Steps:**
  1. Owner opens the repository and edits the relevant Markdown / structured content file(s).
  2. Owner commits and pushes the change.
  3. Hosting platform rebuilds the site.
  4. Owner verifies the change on a preview or production URL.
- **Success criteria:** Content change is live without any modification to application source code. Friction is limited to editing files + commit (FR-04 / Decision Q5).
- **Related requirements:** FR-04, NFR-05, NFR-04, NFR-06

## 3. Edge Cases & Error Paths

### Edge E-01 — Contact form submission fails or is blocked

- **Trigger / condition:** Network error, third-party service outage, or spam filter rejection.
- **Steps / recovery:**
  1. Form provides clear feedback that submission did not succeed.
  2. Visible email / CTA remains available as the reliable fallback on the same page.
  3. Visitor can complete the contact intent via email without leaving the site.
- **Related requirements:** FR-03, FR-08 (hybrid method deliberately provides redundancy)

### Edge E-02 — Content still contains placeholders

- **Trigger / condition:** Exact projects (Q3) or brand assets (Q7) have not yet been supplied; site is viewed with placeholder text/images.
- **Steps / recovery:**
  1. Structure and navigation still function.
  2. Placeholders are recognisable as temporary so they do not mislead about final quality.
  3. Contact paths remain fully operational.
- **Related requirements:** Residual open questions Q3, Q7; FR-01–FR-03 still satisfied at structural level

### Edge E-03 — Mobile / small viewport or slow network

- **Trigger / condition:** Visitor is on a mid-tier mobile device or constrained connection.
- **Steps / recovery:**
  1. Layout remains usable and readable without horizontal scrolling of primary content (FR-09).
  2. Performance targets (Core Web Vitals Good) keep the 60-second scan feasible (NFR-01).
  3. Contact form and email remain reachable without excessive zooming or hunting.
- **Related requirements:** FR-09, NFR-01, FR-02, FR-03

### Edge E-04 — Visitor wants to leave or is unsure

- **Trigger / condition:** Visitor is not the right fit or is only browsing.
- **Steps / recovery:**
  1. Site does not trap the visitor with aggressive modals or forced flows.
  2. Navigation and content hierarchy remain calm and scannable so the visitor can exit with a clear impression rather than confusion.
- **Related requirements:** NFR-02 (calm, trustworthy character); supports overall brand goal

## 4. Pain Points & Cognitive Load Notes

- **60-second constraint is strict.** Home view must prioritise clarity of offering + project visibility over decorative density. Cognitive load rises if the visitor must hunt for “what does this person do?” or scroll past large empty or ambiguous regions.
- **Project depth vs. scan speed.** Separate project pages (already decided) allow depth without forcing every visitor through long case studies on first arrival. Cards on Home must still communicate enough signal to decide “worth clicking”.
- **Contact friction.** Dual path (form + visible email) reduces the risk that a single failed form or missing email blocks the primary conversion. Form should ask only for essential fields.
- **Owner update path.** Markdown + Git is low-friction for the stated owner profile, but still requires basic Git comfort. This is accepted by prior decisions; no CMS is introduced.
- **Placeholder period.** Until real projects and brand assets arrive, the site must not feel broken. Clear structural hierarchy is more important than finished visual polish during that window.
- **No authentication or multi-step funnels.** Cognitive load stays low because the site never asks the visitor to create an account, remember a password, or complete a multi-page wizard.

## 5. Open Questions

| ID  | Question | Blocking? | Related journey |
|-----|----------|-----------|-----------------|
| Q3  | Exact 4–6 projects to feature (titles, descriptions, media) | no | J-01, J-02, E-02 |
| Q7  | Domain, logo, typography, colour, photography, bio text, final project write-ups | no | J-01, J-04, E-02 |

No new open questions were introduced by this stage. The residual items remain content/brand placeholders owned by the human.

---

## Traceability

Every journey and edge maps to confirmed requirements.

| Journey / Edge | Requirement ID(s) | Notes |
|----------------|-------------------|-------|
| J-01 First impression & 60 s scan | FR-01, FR-02, FR-05, FR-09, FR-10, NFR-01, NFR-02 | Core success metric from Brief |
| J-02 Project case study | FR-06, FR-03, FR-08, FR-09, FR-10, NFR-01 | Separate pages decision already locked |
| J-03 Contact the owner | FR-03, FR-08, NFR-03, NFR-04 | Hybrid form + email |
| J-04 About | FR-07, FR-03, FR-08 | Supporting context |
| J-05 Owner content update | FR-04, NFR-05, NFR-04, NFR-06 | File-based workflow |
| E-01 Form failure | FR-03, FR-08 | Redundancy via visible email |
| E-02 Placeholders | Q3, Q7 (residual) | Structural integrity preserved |
| E-03 Mobile / slow network | FR-09, NFR-01, FR-02, FR-03 | Performance + responsive |
| E-04 Unsure / leave | NFR-02 | Calm character, no traps |

**Source of truth:** `requirements.md` (v0.2) and `brief.md`. Personas and flows are derived only from stated goals, stakeholders, and functional requirements. No user needs were invented.
