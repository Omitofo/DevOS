# knowledge/design/

**Status:** Active  
**Authority:** core/contract.md · runtime/agents/ux.md · runtime/agents/ui.md · runtime/agents/architect.md · runtime/templates/user-journey.md · runtime/templates/ui-specification.md  
**Last review:** 2026-08-06  
**Confidence:** High (principles are decision-oriented, scoped, and explicitly linked to agent responsibilities)

Reusable visual, narrative, and interaction principles.  
Agents consult these files when shaping user experience, visual systems, motion language, and accessibility posture.

Design knowledge is **not** a style guide for a single brand. It is a decision framework: when a principle applies, what trade-offs it carries, which anti-patterns to avoid, and how the choice must be recorded in journey, UI, or architecture artifacts.

## Purpose

Give UX, UI, Architect, and Security & Quality Auditor a shared, versioned vocabulary for design decisions so that:

- Narrative structure, visual hierarchy, and interaction quality remain coherent across projects.
- Accessibility and cognitive-load considerations are treated as first-class constraints rather than afterthoughts.
- Motion, typography, colour, and spacing choices are justified and traceable.
- Downstream UI specifications and implementation plans inherit consistent principles instead of inventing local rules.

## Files

| File | Responsibility |
|------|----------------|
| [storytelling.md](storytelling.md) | Narrative structure, emotional arc, and information sequencing |
| [cinematography.md](cinematography.md) | Visual framing, depth, focus, and scene composition analogies |
| [composition.md](composition.md) | Layout hierarchy, visual weight, and spatial organisation |
| [typography.md](typography.md) | Type scale, hierarchy, readability, and voice |
| [spacing.md](spacing.md) | Rhythm, density, and whitespace systems |
| [color-systems.md](color-systems.md) | Palette construction, semantic colour, contrast, and theming |
| [motion.md](motion.md) | Timing, easing, purpose of animation, and reduced-motion |
| [accessibility-patterns.md](accessibility-patterns.md) | Inclusive design patterns, WCAG alignment, and cognitive considerations |

## Usage Rules for Agents

1. Consult the relevant design file(s) before recording visual, narrative, or interaction decisions in the User Journey, UI Specification, or Architecture Blueprint.
2. Record the chosen approach **and** the concrete criteria that justified it (link back to the relevant section).
3. When two approaches remain plausible, list both, state the decision criteria applied, and surface residual risk if the choice affects accessibility or cognitive load.
4. Prefer links to these files over copying guidance into project artifacts.
5. Never invent a novel design system without flagging it as a knowledge-gap / exception and routing significant deviations through the Security & Quality Auditor when they touch accessibility or safety.
6. Design knowledge describes *what* and *why*; it does not prescribe exact component libraries, brand tokens, or pixel values unless those are required for a principle to be actionable.

## Relationship to Other Knowledge

- **classification/** — product type and complexity band influence how aggressively storytelling and motion are applied.
- **patterns/** — forms, real-time, and search patterns often constrain interaction design.
- **technologies/** — frontend choices (rendering model, component system) must remain compatible with the design principles selected here.
- **standards/** — naming and documentation conventions that design tokens and component APIs must satisfy.
- **blueprints/** — opinionated starting points that already embed compatible design defaults.

## Maintenance

- Every file declares Status, Confidence, and Last review.
- Conflicting guidance is resolved by the Security & Quality Auditor with explicit rationale (especially for accessibility).
- Placeholders have been eliminated; all eight design dimensions are fully specified.
- Revisit when major platform shifts occur (new accessibility standards, significant changes in device form factors, or material evolution of motion/reduced-motion expectations).
