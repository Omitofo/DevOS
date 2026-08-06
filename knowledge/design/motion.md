# Motion

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** UI, UX

Timing, easing, purpose of animation, and reduced-motion support.  
Motion is a communication tool. It should clarify spatial relationships, provide feedback, and reinforce narrative — never decorate for its own sake or interfere with task completion.

## Core Models

| Model | Description | Typical use |
|-------|-------------|-------------|
| **Functional only** | Motion limited to feedback (press, success, error) and orientation (page transitions, expanding panels) | Most product UIs, data tools, forms, enterprise applications |
| **Narrative / expressive** | Motion that carries story beats, brand personality, or delight at key moments | Marketing sites, onboarding, consumer products with strong brand |
| **Spatial continuity** | Shared-element transitions and continuous surfaces that preserve object identity across views | Master-detail, galleries, navigation between related contexts |
| **Physics-inspired** | Spring, inertia, or momentum-based motion for direct manipulation | Creative tools, maps, canvases, highly interactive mobile experiences |

## Preference Guidance

**Prefer functional only when**
- Users perform frequent, time-sensitive tasks.
- Cognitive load and performance budgets are tight.
- The product is used for extended sessions where novelty fades and predictability matters.

**Prefer narrative / expressive when**
- First-time or occasional emotional impact is a product goal.
- Brand differentiation through motion is intentional and resourced.
- Motion is confined to non-critical paths or celebration moments.

**Prefer spatial continuity when**
- Users move between tightly related views and object identity must be preserved.
- The information architecture is hierarchical or spatial.
- Implementation cost of shared-element transitions is accepted.

**Prefer physics-inspired when**
- Direct manipulation is core (drag, flick, pinch).
- The interface metaphor is physical or spatial.
- Performance on target devices can sustain the required frame rate.

## Essential Decision Points

1. **Purpose test**  
   - Every animation must answer: what does the user understand better because of this motion?  
   - If the answer is only “it looks nicer,” default to no motion or a simpler alternative.

2. **Duration and easing**  
   - Short feedback: 100–200 ms.  
   - Transitions between views: 200–400 ms.  
   - Complex choreography: rarely exceed 500–700 ms.  
   - Prefer ease-out for entrances, ease-in for exits, and standard curves over custom unless brand requires it.

3. **Reduced motion**  
   - Honour `prefers-reduced-motion: reduce` by providing equivalent non-animated or minimally animated alternatives.  
   - Critical feedback (success, error, loading) must remain perceivable without motion.  
   - Never convey essential information solely through animation.

4. **Performance budget**  
   - Animations must not cause layout thrashing or sustained main-thread work.  
   - Prefer compositor-friendly properties (transform, opacity) over layout-affecting properties.  
   - Test on mid-tier and low-end devices, not only flagship hardware.

5. **Interruptibility**  
   - Users must be able to interrupt or skip long sequences.  
   - Loading and progress indicators should remain accurate and cancellable where the operation is cancellable.

## Anti-Patterns

- Animating everything “because the library makes it easy.”
- Long entrance animations that delay task start.
- Motion that moves the user’s target or changes layout during interaction.
- Ignoring `prefers-reduced-motion`.
- Using motion as the only indicator of state change.
- Physics simulations that feel sluggish or unpredictable on real devices.
- Large parallax or scroll-linked effects that cause accessibility or performance problems.

## Recording Requirements

In the UI Specification (Motion / Interaction System):

- Chosen model (functional / narrative / spatial / physics)
- Duration and easing defaults
- Reduced-motion policy and fallback behaviour
- Performance constraints
- Link back to this file and the concrete criteria used

## Related Design Knowledge

- [cinematography.md](cinematography.md) — framing and camera-like transitions
- [storytelling.md](storytelling.md) — narrative beats that motion can reinforce
- [composition.md](composition.md) — spatial relationships that motion makes continuous
- [accessibility-patterns.md](accessibility-patterns.md) — reduced-motion, vestibular considerations, and non-motion feedback
