# Visual Blueprint

**Version:** 0.2  
**Status:** draft  
**Upstream:** journeys.md, requirements.md  
**Assumptions:** none (brand visual system locked by explicit decisions below; content assets remain placeholders)  
**Open questions:** see §6 (only residual content placeholders)  
**Last updated:** 2026-08-08  
**Authoring agent / human:** UI (with human direction to close visual open questions for production)

---

> **Producing agent:** UI  
> **Purpose:** Define the layout system, component hierarchy, design tokens, accessibility behaviour, and responsive rules required to realise the confirmed journeys. The system is tokenized and implementable. Visual direction remains “minimal, elegant, calm”. Concrete type and colour choices have been locked under human direction for a real production trajectory. Content (projects, bio, photography, domain, logo mark) remains placeholder until supplied.

## 1. Design Tokens

### Color (locked)

Neutral monochrome base + single restrained accent. High legibility, low visual noise, calm and professional.

| Token | Value | Usage |
|-------|-------|-------|
| `background` | `#FAFAFA` | Page background |
| `surface` | `#FFFFFF` | Cards, elevated surfaces |
| `foreground` / `text-primary` | `#0A0A0A` | Primary text |
| `text-secondary` | `#525252` | Supporting / meta text |
| `accent` | `#1E3A5F` | Links, primary CTA, sparse emphasis (deep slate-blue) |
| `accent-hover` | `#152A45` | Hover / active state of accent |
| `border` / `divider` | `#E5E5E5` | Borders, hairlines |
| `success` | `#166534` | Form success feedback |
| `error` | `#991B1B` | Form error feedback |

- **Contrast:** All combinations meet or exceed WCAG 2.2 AA.
- **Direction:** Monochrome + one accent only. No multi-accent or high-saturation schemes.
- Knowledge: [knowledge/design/color-systems.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/design/color-systems.md)

### Typography (locked)

Single high-quality neutral sans-serif family for the entire interface. Clean, modern, excellent readability, widely available, and consistent with the calm professional character.

| Role | Font | Weight | Notes |
|------|------|--------|-------|
| `display` / `h1` | Inter | 600–700 | Page / section titles |
| `h2` / `h3` | Inter | 600 | Project titles, section headings |
| `body` | Inter | 400 | Primary reading text |
| `small` / `meta` | Inter | 400–500 | Captions, dates, labels |

- **Source:** Inter (Google Fonts / `next/font/google` or equivalent). Variable font preferred.
- **Fallbacks:** `ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`
- **Hierarchy:** Clear size steps (e.g. 2.25 rem / 1.5 rem / 1.125 rem / 0.875 rem) with comfortable line-height (1.5–1.6 for body).
- **Line length:** ≈ 60–75 characters for body text.
- Knowledge: [knowledge/design/typography.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/design/typography.md)

### Spacing
- **Base unit:** 4 px modular scale.
- **Token set:** `space-1` (4 px) … `space-8` (32 px) and larger steps as needed for section rhythm.
- **Whitespace:** Generous but purposeful; supports calm character without large empty regions that hurt the 60-second scan (J-01).
- Knowledge: [knowledge/design/spacing.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/design/spacing.md)

### Radius
- Small, consistent: `4 px` (controls, inputs) to `8 px` (cards).
- Avoid large pills or highly rounded shapes.

### Elevation
- Minimal. Prefer 1 px borders (`border` token) or very subtle surface shifts over drop shadows.
- At most one soft shadow level if needed for card separation on `background`.

### Motion
- **Principle:** Purposeful and restrained. No heavy animation libraries (NFR-05).
- **Allowed:** Short CSS transitions (150–250 ms) on hover, focus, and simple page-enter.
- **Required:** Honour `prefers-reduced-motion: reduce` — disable non-essential motion.
- Knowledge: [knowledge/design/motion.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/design/motion.md)

## 2. Layout System

### Grid & structure
- Single-column reading layout for body content with constrained max-width for comfortable line length.
- Project cards on Home arranged in a simple responsive grid (1 column mobile → 2 columns tablet → 2–3 columns desktop).
- Consistent horizontal page padding that scales with viewport.
- Knowledge: [knowledge/design/composition.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/design/composition.md)

### Breakpoints
| Token | Approx. width | Intent |
|-------|---------------|--------|
| `sm` | ≥ 640 px | Larger phones / small tablets |
| `md` | ≥ 768 px | Tablets |
| `lg` | ≥ 1024 px | Desktop |
| `xl` | ≥ 1280 px | Wide desktop |

### Page templates (required by journeys)
| Template | Supports journeys | Key regions |
|----------|-------------------|-------------|
| **Home** | J-01, J-03 | Intro / offering statement · Project grid (4–6 cards) · Persistent contact (form + email/CTA) · Primary nav |
| **Project detail** | J-02, J-03 | Project title & meta · Narrative / media blocks · Back / next navigation · Persistent contact |
| **About** | J-04, J-03 | About content · Persistent contact |
| **Shared shell** | All | Header (identity + nav) · Footer (minimal) · Global contact affordances |

All templates keep the contact form and clear email/CTA reachable without leaving the page (FR-03, FR-08, J-03).

## 3. Component Hierarchy

| Component | Purpose | Variants | Accessibility notes |
|-----------|---------|----------|---------------------|
| **SiteHeader** | Identity (text wordmark for v1) + primary navigation (Home, Work, About, Contact) | Compact on mobile | Landmark `banner`; keyboard-reachable; visible focus |
| **SiteFooter** | Minimal secondary links / copyright / email | Optional | Landmark `contentinfo` |
| **IntroBlock** | Short offering statement on Home | — | Correct heading hierarchy; sufficient contrast |
| **ProjectCard** | Summary of one project (title, short description, optional thumbnail) linking to detail page | With / without image | Focusable link; alt text; clear hit target |
| **ProjectDetail** | Full case-study content for one project | — | Proper heading structure; media with alt / captions |
| **ContactForm** | Lightweight form (name, email, message) posting to third-party service | — | Labelled inputs, error announcements, success feedback, keyboard operable |
| **EmailCTA** | Clear, visible email address / mailto link | Inline or button-styled | Sufficient contrast; not the sole contact method |
| **Button / Link** | Primary and secondary actions | Primary (accent), secondary, text | Focus ring; disabled state; minimum touch target ≈ 44×44 px |
| **MediaImage** | Optimised project or decorative image | Responsive sizes | Meaningful `alt`; decorative images marked appropriately |
| **Section** | Vertical spacing and optional heading wrapper | — | Consistent rhythm |

Identity for v1 is a **text wordmark** (owner name or short title). No custom logo mark is required until supplied.

## 4. Responsive Behaviour

| Breakpoint | Adaptations |
|------------|-------------|
| **Default (< sm)** | Single-column. Project cards stack. Navigation may collapse. Contact form full-width. Touch targets ≥ 44×44 px. |
| **sm – md** | Project grid → 2 columns where space allows. |
| **lg+** | Project grid 2–3 columns. Max-width container centres content. |
| **All** | No horizontal scrolling of primary content (FR-09). Images scale responsively. Typography remains readable. |

Image elements must use the media pipeline from Architecture so LCP stays within Core Web Vitals Good targets (NFR-01).

## 5. Accessibility Summary

- **Target standard:** WCAG 2.2 Level AA.
- **Keyboard model:** All interactive elements reachable and operable via keyboard. Logical tab order follows visual order.
- **Focus management:** Visible focus indicators on all interactive components; no focus traps.
- **Forms:** Programmatic labels, clear error messages, success confirmation.
- **Media:** Informative images have descriptive `alt`; decorative images are ignored by assistive technology.
- **Motion:** Non-essential animation disabled under `prefers-reduced-motion`.
- **Colour:** Contrast ratios meet AA; colour is never the sole means of conveying information.
- Knowledge: [knowledge/design/accessibility-patterns.md](https://github.com/Omitofo/DevOS/blob/master/knowledge/design/accessibility-patterns.md)

## 6. Open Questions (residual)

| ID  | Question | Blocking? | Related component / journey |
|-----|----------|-----------|-----------------------------|
| Q3  | Exact 4–6 projects (titles, descriptions, media) | no | ProjectCard, ProjectDetail, J-01, J-02 |
| Q7-content | Domain, photography, bio text, final project write-ups, optional logo mark | no | IntroBlock, About, media, SiteHeader |

**Closed in this version:**
- V-01 → Inter (single family) locked.
- V-02 → Neutral monochrome + single deep slate-blue accent locked.
- Q7 visual tokens (typefaces + colour palette) → locked above. Remaining Q7 items are content/assets only.

## 7. Resolved Visual Decisions (v0.1 → v0.2)

| Former ID | Decision | Rationale |
|-----------|----------|-----------|
| V-01 | **Inter** as the sole typeface family (weights 400–700) | Clean, highly legible, professional, excellent variable-font support, widely available via `next/font` or Google Fonts, zero decorative noise. Matches “minimal, elegant, calm”. |
| V-02 | **Neutral monochrome + single accent** (`#1E3A5F` deep slate-blue) | High contrast, calm, trustworthy. Accent is sparse and purposeful (links + primary CTA only). Avoids template-looking multi-colour schemes. |
| Q7 (visual part) | Concrete palette and type locked as above. Identity = text wordmark for v1. | Enables implementation while still leaving photography, bio, domain, and project content as human-owned placeholders. |

---

## Traceability

| Decision / Element | Source | Notes |
|--------------------|--------|-------|
| Minimal / calm / elegant direction | NFR-02; intake; journeys | Core visual character |
| Inter typeface | V-01 resolution; knowledge/design/typography.md | Locked |
| Monochrome + `#1E3A5F` accent | V-02 resolution; knowledge/design/color-systems.md | Locked |
| Token structure + spacing / radius / motion | knowledge/design/*; UI agent | Unchanged |
| Home / Project detail / About / Shared shell | J-01–J-04; FR-05–FR-07 | |
| ProjectCard + ProjectDetail | J-01, J-02; FR-06 | |
| ContactForm + EmailCTA on every page | J-03; FR-03, FR-08 | |
| Responsive + no horizontal scroll | FR-09; E-03 | |
| WCAG 2.2 AA, keyboard, focus, reduced-motion | knowledge/design/accessibility-patterns.md | |
| Light motion only | NFR-05; knowledge/design/motion.md | |
| Residual Q3 + content part of Q7 | requirements.md / prior stages | Content still human-owned |

**Source of truth:** `journeys.md` and `requirements.md`. Visual tokens are now concrete so the Tech Lead stage can proceed without ambiguity. Content assets (projects, bio, photography, domain) remain placeholders.
