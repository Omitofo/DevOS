# Typography

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** UI, UX

Type scale, hierarchy, readability, and voice.  
Typography is the primary carrier of meaning in most interfaces. Decisions here affect comprehension, scanning speed, emotional tone, and accessibility.

## Core Models

| Model | Description | Typical use |
|-------|-------------|-------------|
| **Tight product scale** | Limited set of sizes (typically 6–8) optimised for UI density and consistency | SaaS products, dashboards, admin tools, forms |
| **Editorial / content scale** | Wider range with strong display sizes and generous body leading | Marketing sites, documentation, long-form content, blogs |
| **Hybrid** | Product scale for application chrome + editorial scale for content regions | Product marketing pages, hybrid content + app experiences |
| **Monospace / technical** | Fixed-width or code-oriented faces used intentionally for data, code, or status | Developer tools, logs, data-dense tables, terminals |

## Preference Guidance

**Prefer tight product scale when**
- The majority of surfaces are interactive UI rather than long reading.
- Consistency and density are valued over expressive display type.
- Complexity band is S–L and the product will grow many screens.

**Prefer editorial / content scale when**
- Long-form reading or strong brand expression is central.
- Hero statements and article-like layouts dominate.
- First-contentful-paint and reading comfort are primary metrics.

**Prefer hybrid when**
- The product mixes application surfaces with marketing or help content.
- A single type family can serve both roles with different optical sizes or weights.

**Prefer monospace / technical when**
- The audience expects code or tabular precision.
- Alignment of characters or columns is functionally important.
- Never use monospace for general UI body text.

## Essential Decision Points

1. **Type family selection**  
   - Prefer families with good screen rendering, multiple weights, and open metrics.  
   - Limit to one primary family + one secondary (or mono) unless brand requirements justify more.  
   - Variable fonts are preferred when they reduce payload without sacrificing quality.

2. **Modular scale**  
   - Choose a ratio (commonly 1.2–1.333) and generate sizes from a base (usually 16 px / 1 rem).  
   - Document the scale explicitly; do not invent ad-hoc sizes.

3. **Hierarchy roles**  
   - Map roles (display, heading 1–3, body, caption, label, overline) to concrete size/weight/line-height combinations.  
   - Each role must be visually distinct under normal and high-contrast conditions.

4. **Readability constraints**  
   - Body text: target 45–75 characters per line for long reading; shorter is acceptable for UI.  
   - Line height: typically 1.4–1.6 for body; tighter for headings.  
   - Avoid pure black on pure white for large text blocks when softer contrast improves comfort.

5. **Voice and tone**  
   - Typography contributes to voice (serious, playful, technical, warm).  
   - Weight, tracking, and case (title vs. sentence) must remain consistent with the chosen voice.

## Anti-Patterns

- More than two or three type families without strong justification.
- Using display sizes for body text or body sizes for primary headings.
- Insufficient contrast between text and background (fails WCAG).
- All-caps body text or excessive letter-spacing that harms readability.
- Ignoring font loading strategy (FOIT/FOUT) so that layout shifts or invisible text occur.
- Relying on italic or light weights for critical information (low legibility at small sizes).

## Recording Requirements

In the UI Specification (Typography System):

- Chosen model (tight product / editorial / hybrid / technical)
- Type families and fallback stack
- Modular scale and role-to-size mapping
- Key readability rules (line length, line height, contrast)
- Link back to this file and the concrete criteria used

## Related Design Knowledge

- [composition.md](composition.md) — layout hierarchy that type must reinforce
- [spacing.md](spacing.md) — vertical rhythm between text blocks
- [color-systems.md](color-systems.md) — text colour and contrast
- [accessibility-patterns.md](accessibility-patterns.md) — minimum sizes, contrast, and reflow requirements
