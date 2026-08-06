# Spacing

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** UI, UX

Rhythm, density, and whitespace systems.  
Spacing is the invisible structure that makes hierarchy, grouping, and breathing room legible. Inconsistent spacing is one of the fastest ways to make an interface feel unfinished or stressful.

## Core Models

| Model | Description | Typical use |
|-------|-------------|-------------|
| **Fixed modular scale** | Discrete steps (e.g., 4 / 8 / 12 / 16 / 24 / 32 / 48 / 64) used for all margins, padding, and gaps | Most product UIs, design systems, component libraries |
| **Proportional / relative** | Spacing expressed as fractions of a base or as percentages of container | Fluid marketing layouts, some editorial designs |
| **Density variants** | Same scale with “comfortable / default / compact” multipliers or alternate token sets | Data-dense tools that also support less experienced users |
| **Optical adjustment** | Base scale plus small manual corrections for visual balance (icons, text, borders) | High-polish brand experiences, icon-heavy interfaces |

## Preference Guidance

**Prefer fixed modular scale when**
- A design system or component library will be maintained over time.
- Multiple contributors must produce consistent results.
- Predictability and implementation simplicity are valued.

**Prefer proportional / relative when**
- Layouts must fluidly adapt across a very wide range of viewports.
- The visual language is intentionally editorial or magazine-like.
- Absolute pixel control is less important than overall proportion.

**Prefer density variants when**
- The same product serves both expert power users and occasional users.
- Tables, lists, or toolbars benefit from compact modes without rewriting components.
- Complexity band is M–L.

**Prefer optical adjustment when**
- Brand expression and pixel-level craft are differentiators.
- Icons, avatars, or mixed content require fine-tuning beyond the base scale.
- A design system still provides the base tokens; optical tweaks are documented exceptions.

## Essential Decision Points

1. **Base unit**  
   - 4 px or 8 px base is conventional and aligns well with most platform grids.  
   - All spacing tokens should be multiples of the base unit.

2. **Token set**  
   - Name tokens by role or by size (space-xs, space-sm, space-md … or space-1, space-2 …).  
   - Prefer semantic role names when the same value is used for different purposes (e.g., stack-gap vs. inline-gap).

3. **Vertical rhythm**  
   - Text blocks, form fields, and cards should share a consistent vertical cadence.  
   - Avoid arbitrary gaps that break the rhythm between related groups.

4. **Internal vs. external spacing**  
   - Padding (internal) and margin / gap (external) must be distinguished.  
   - Components should own their internal padding; layout should own the gaps between components.

5. **Responsive spacing**  
   - Decide whether spacing scales down on small viewports or stays constant.  
   - Large hero spacing often reduces on mobile; component-internal spacing usually stays stable.

## Anti-Patterns

- Arbitrary pixel values that do not belong to the scale.
- Mixing margin and padding inconsistently so that component boundaries become unclear.
- Zero or near-zero spacing between interactive elements (hit-target and cognitive problems).
- Extremely large empty regions that force excessive scrolling without narrative purpose.
- Different spacing scales for “marketing” vs. “app” without a documented bridge.
- Ignoring touch-target minimums when compact density is chosen.

## Recording Requirements

In the UI Specification (Spacing / Layout System):

- Chosen model (fixed modular / proportional / density variants / optical)
- Base unit and full token list
- Rules for internal vs. external spacing
- Density or responsive adjustment policy if any
- Link back to this file and the concrete criteria used

## Related Design Knowledge

- [composition.md](composition.md) — spatial organisation that spacing realises
- [typography.md](typography.md) — vertical rhythm between text elements
- [cinematography.md](cinematography.md) — frame margins and safe areas
- [accessibility-patterns.md](accessibility-patterns.md) — minimum touch targets and spacing for usability
