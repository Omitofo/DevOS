# Cinematography

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** UI, UX

Visual framing, depth, focus, and scene-composition principles applied to interfaces.  
Cinematography here is an analogy layer: the same concerns that guide a camera (what is in frame, what is sharp, what moves, where the eye is led) apply to screens, cards, and transitions.

## Core Models

| Model | Description | Typical use |
|-------|-------------|-------------|
| **Static frame** | Fixed composition; hierarchy is established by size, contrast, and position | Dashboards, settings, dense data tables, most form surfaces |
| **Guided frame** | Composition that deliberately leads the eye along a path (Z-pattern, F-pattern, focal point) | Marketing pages, onboarding screens, feature highlights |
| **Layered / depth** | Explicit foreground, mid-ground, background with controlled occlusion and elevation | Modals, drawers, overlapping cards, spatial navigation |
| **Dynamic frame** | Composition that changes over time (camera-like pans, zooms, or reveal) | Story-driven experiences, product tours, high-emotion moments |

## Preference Guidance

**Prefer static frame when**
- The surface is task-oriented and users return frequently.
- Information density is high and predictability is valued.
- Motion would add cognitive cost without proportional benefit.

**Prefer guided frame when**
- The primary goal is to communicate a single idea or conversion path.
- First-time or occasional users need clear visual direction.
- The page has a strong narrative beat (see storytelling.md).

**Prefer layered / depth when**
- Multiple contexts must coexist (e.g., main content + overlay).
- Elevation and shadow systems are already part of the design language.
- Focus management (what is interactive vs. decorative) must be unambiguous.

**Prefer dynamic frame when**
- The experience is intentionally cinematic or story-led.
- Transition moments carry meaning (e.g., “entering” a new domain).
- Performance budget and reduced-motion support are explicitly planned.

## Essential Decision Points

1. **Primary focal point**  
   - Every significant screen should have one dominant focal element.  
   - Secondary elements must not compete for attention at the same visual weight.

2. **Depth and elevation**  
   - Establish a limited elevation scale (typically 3–5 levels).  
   - Higher elevation implies higher interactivity or temporary focus; never use elevation purely decoratively.

3. **Edge and crop**  
   - Decide what is allowed to bleed or be cropped.  
   - Avoid accidental cropping of interactive elements or critical text.

4. **Transition as camera move**  
   - When screens change, decide whether the transition is a cut, a dissolve, a push, or a shared-element morph.  
   - Each carries different narrative weight; default to the least dramatic that still communicates continuity.

5. **Safe areas and framing**  
   - Respect device safe areas, notches, and dynamic islands.  
   - Important actions and status indicators must remain inside the safe frame on all target viewports.

## Anti-Patterns

- Multiple competing focal points of equal visual weight.
- Using deep elevation / heavy shadows for non-interactive decoration.
- Animating layout shifts that move the user’s target mid-interaction.
- Framing critical content so that it is cut off on common device sizes.
- “Ken Burns” style motion on static imagery without purpose.
- Ignoring reduced-motion preferences while using dynamic framing.

## Recording Requirements

In the UI Specification (Visual System or Screen Specifications):

- Chosen framing model for key surfaces
- Elevation scale and rules for its use
- Transition philosophy (cut / dissolve / shared-element / none)
- Link back to this file and the concrete criteria used

## Related Design Knowledge

- [composition.md](composition.md) — layout hierarchy that implements the frame
- [storytelling.md](storytelling.md) — narrative beats that cinematography supports
- [motion.md](motion.md) — timing and easing of camera-like moves
- [spacing.md](spacing.md) — margins and padding that define the frame edges
