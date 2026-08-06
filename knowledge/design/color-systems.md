# Color Systems

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** UI, UX, Architect

Palette construction, semantic colour, contrast, and theming.  
Colour is both a brand expression tool and a critical accessibility and information channel. Decisions here must satisfy both aesthetic and functional requirements.

## Core Models

| Model | Description | Typical use |
|-------|-------------|-------------|
| **Semantic token system** | Colours named by role (primary, danger, success, surface, on-surface) rather than by hue | Product UIs, design systems, multi-brand or multi-theme products |
| **Palette-first** | A curated set of hue families with tints/shades; roles are mapped later | Brand-heavy marketing, single-product experiences with strong visual identity |
| **Neutral-dominant** | Large neutral scale + limited accent colours | Data-dense tools, professional / enterprise surfaces, accessibility-first products |
| **Dual / multi-theme** | Explicit light and dark (and optionally high-contrast) themes built from the same semantic tokens | Modern applications expected to respect system preference or user choice |

## Preference Guidance

**Prefer semantic token system when**
- A design system will be shared across multiple surfaces or products.
- Themes (light/dark) or brand variants are required.
- Developers and designers need stable contracts that survive palette changes.

**Prefer palette-first when**
- Brand expression is a primary differentiator and the product surface is relatively small.
- The palette is owned by brand guidelines that pre-date the product system.

**Prefer neutral-dominant when**
- Information density is high and colour must not compete with data.
- Accessibility and long-session comfort are prioritised.
- Accents are reserved strictly for status, links, and primary actions.

**Prefer dual / multi-theme when**
- Users expect light/dark support (or the product runs in mixed environments).
- System colour-scheme preference must be honoured.
- High-contrast or forced-colors modes are in scope for accessibility compliance.

## Essential Decision Points

1. **Semantic roles**  
   - At minimum define: background / surface, on-surface (text/icon), primary, secondary, success, warning, danger, info, border/divider.  
   - Each role must have a clear usage rule; do not overload a single colour with multiple meanings.

2. **Contrast requirements**  
   - Text and interactive elements must meet WCAG 2.2 AA contrast (4.5:1 normal text, 3:1 large text and UI components) as a baseline.  
   - Prefer AAA (7:1) for body text when feasible.  
   - Test both light and dark themes independently.

3. **Theme architecture**  
   - Prefer semantic tokens that resolve to different raw values per theme over hard-coded hex values in components.  
   - Document how system preference, user preference, and forced-colors interact.

4. **Status and feedback colours**  
   - Success, warning, and danger must remain distinguishable under colour-vision deficiency (do not rely on hue alone).  
   - Pair colour with icon, text, or pattern where the meaning is critical.

5. **Decorative vs. functional colour**  
   - Decorative colour may be more expressive; functional colour (status, links, focus) must remain stable and high-contrast.  
   - Never use pure decorative colour for interactive or status meaning.

## Anti-Patterns

- Hard-coding hex or rgb values inside components instead of tokens.
- Using colour as the sole indicator of state or meaning.
- Insufficient contrast between text and background (or between adjacent interactive elements).
- Inverting an entire light palette for “dark mode” without re-checking contrast and elevation.
- Too many accent colours that dilute hierarchy and brand recognition.
- Ignoring forced-colors / high-contrast system modes.

## Recording Requirements

In the UI Specification (Color System / Theme):

- Chosen model (semantic / palette-first / neutral-dominant / multi-theme)
- Semantic role list and usage rules
- Contrast targets (AA / AAA) and verification method
- Theme strategy (light / dark / system / high-contrast)
- Link back to this file and the concrete criteria used

## Related Design Knowledge

- [typography.md](typography.md) — text colour and contrast against backgrounds
- [composition.md](composition.md) — how colour weight contributes to hierarchy
- [accessibility-patterns.md](accessibility-patterns.md) — contrast, non-colour indicators, and forced-colors
- [motion.md](motion.md) — colour transitions and focus indicators
