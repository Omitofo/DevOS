# Composition

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** UI, UX, Architect

Layout hierarchy, visual weight, and spatial organisation.  
Composition answers: what is primary, what is secondary, how elements relate, and how the eye and the hand move through the interface.

## Core Models

| Model | Description | Typical use |
|-------|-------------|-------------|
| **Single-column progressive** | Vertical stack with clear reading order; primary action near the top or after key content | Forms, articles, mobile-first flows, onboarding |
| **Grid / modular** | Repeated units on a consistent grid; hierarchy via size and placement within the grid | Dashboards, card collections, media libraries, admin tables |
| **Split / asymmetric** | Two (or more) regions with different roles (content vs. controls, list vs. detail) | Master-detail, settings with navigation, comparison views |
| **Spatial / freeform** | Absolute or relatively free positioning; relationships expressed by proximity and connection lines | Canvases, diagram editors, creative tools, some marketing hero sections |

## Preference Guidance

**Prefer single-column progressive when**
- The dominant interaction is sequential or form-filling.
- Viewport width is constrained or mobile is primary.
- Cognitive load must be minimised and reading order must be unambiguous.

**Prefer grid / modular when**
- Content is a collection of similar items.
- Scanning and comparison are primary tasks.
- Responsive reflow across breakpoints is required without rewriting hierarchy.

**Prefer split / asymmetric when**
- Two complementary contexts must be visible simultaneously (list + detail, navigation + content).
- Users frequently switch focus between the regions.
- Complexity band is M–L and the information architecture is already stable.

**Prefer spatial / freeform when**
- The product is a creative or diagramming surface.
- Relationships between objects are as important as the objects themselves.
- Users expect direct manipulation and infinite or large canvases.

## Essential Decision Points

1. **Visual hierarchy levels**  
   - Define at most 3–4 distinct hierarchy levels (primary, secondary, tertiary, ambient).  
   - Each level must be distinguishable by size, weight, colour, or position — not by a single cue alone.

2. **Primary action placement**  
   - Primary actions should be reachable without scrolling on the most common viewport, or be sticky / persistently available when the flow is long.  
   - Avoid competing primary actions of equal weight.

3. **Grouping and proximity**  
   - Related elements must be closer to each other than to unrelated elements (Gestalt proximity).  
   - Use consistent spacing scales (see spacing.md) so proximity is readable.

4. **Responsive behaviour**  
   - Decide how hierarchy collapses or reflows at each major breakpoint.  
   - Never let a secondary element become primary (or vice versa) solely because of viewport size unless intentional.

5. **Density vs. clarity**  
   - High-density layouts are acceptable for expert / frequent users when scanning speed matters.  
   - Prefer lower density for first-time, occasional, or high-stakes flows.

## Anti-Patterns

- Equal visual weight given to three or more competing actions or messages.
- Relying solely on colour to express hierarchy (fails for colour-vision deficiency and non-visual users).
- Breaking the grid inconsistently without purpose.
- Nesting interactive elements so deeply that hit targets become ambiguous.
- Using spatial / freeform composition for task-oriented, form-heavy products where predictability is required.
- Ignoring the fold or safe areas so that primary actions are hidden on common devices.

## Recording Requirements

In the UI Specification (Layout System or Screen Specifications):

- Chosen composition model for major surface types
- Hierarchy levels and the cues that distinguish them
- Primary-action placement rules
- Responsive collapse / reflow strategy
- Link back to this file and the concrete criteria used

## Related Design Knowledge

- [cinematography.md](cinematography.md) — framing and depth that composition realises
- [spacing.md](spacing.md) — the spacing scale that makes proximity and hierarchy legible
- [typography.md](typography.md) — type hierarchy that reinforces layout hierarchy
- [storytelling.md](storytelling.md) — narrative order that composition should support
