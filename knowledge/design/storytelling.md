# Storytelling

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** UX, UI, Architect

Narrative structure, emotional arc, and information sequencing for digital products.  
Storytelling here is not marketing copy; it is the deliberate ordering of information and emotion so that users understand where they are, what matters, and what to do next.

## Core Models

| Model | Description | Typical use |
|-------|-------------|-------------|
| **Linear progressive** | Single path that reveals information in a fixed, escalating sequence | Onboarding, tutorials, multi-step wizards, linear content experiences |
| **Goal-oriented** | Narrative organised around a clear user goal; supporting information is subordinate | Task-focused product surfaces, dashboards with primary CTAs |
| **Exploratory / spatial** | Multiple entry points and self-directed paths; narrative is emergent | Content platforms, knowledge bases, complex tools with many features |
| **Conversational** | Turn-based or prompt-driven sequencing that adapts to user input | Chat interfaces, guided assistants, progressive disclosure forms |

## Preference Guidance

**Prefer linear progressive when**
- The user must complete a sequence to unlock value (onboarding, compliance flows).
- Cognitive load must be tightly controlled.
- Complexity band is S–M and the product has a single dominant first-use path.

**Prefer goal-oriented when**
- Users arrive with a known intent (create X, analyse Y, complete Z).
- Secondary information can be deferred or revealed on demand.
- Success metrics are task completion and time-to-value.

**Prefer exploratory when**
- The product is a platform or content system with many equally valid paths.
- Users are expected to return and discover over time.
- Search, navigation, and personalisation carry the narrative load.

**Prefer conversational when**
- The domain is complex and users benefit from adaptive questioning.
- Natural-language interaction is a core differentiator.
- Fallback to structured UI is always available.

## Essential Decision Points

1. **Primary narrative arc**  
   - What is the single most important transformation the user experiences?  
   - Document the beginning state, the tension/conflict (problem), and the resolution (value delivered).

2. **Information sequencing**  
   - What must be known before the next action is safe or meaningful?  
   - Apply progressive disclosure: reveal complexity only when the user has demonstrated readiness or need.

3. **Emotional register**  
   - Confidence, urgency, calm, delight, seriousness — choose deliberately and keep it consistent within a flow.  
   - Avoid abrupt tonal shifts that undermine trust.

4. **Entry and re-entry points**  
   - First-time, returning, and interrupted sessions must each have a coherent narrative restart.  
   - Never assume the user remembers prior context unless it is explicitly restored.

5. **Success and failure narratives**  
   - Completion states should reinforce progress and next steps.  
   - Error and empty states must still advance understanding rather than dead-end the user.

## Anti-Patterns

- Front-loading every feature or benefit on the first screen (“feature dump”).
- Mixing competing primary narratives on the same surface (e.g., sell + educate + convert simultaneously without hierarchy).
- Assuming users will read long explanatory text before acting.
- Treating empty states as decorative rather than narrative opportunities.
- Changing the emotional register mid-flow without signalling (e.g., playful onboarding → clinical error page).
- Forcing a linear story on an inherently exploratory product (or vice versa).

## Recording Requirements

In the User Journey and/or UI Specification:

- Chosen narrative model (linear / goal-oriented / exploratory / conversational)
- Primary arc statement (beginning → tension → resolution)
- Key progressive-disclosure points
- Link back to this file and the concrete criteria used

## Related Design Knowledge

- [composition.md](composition.md) — how visual hierarchy supports the narrative
- [cinematography.md](cinematography.md) — framing and focus that reinforce story beats
- [motion.md](motion.md) — transitions that signal narrative progression
- [accessibility-patterns.md](accessibility-patterns.md) — ensuring the story is perceivable and operable by all users
