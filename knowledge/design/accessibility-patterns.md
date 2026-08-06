# Accessibility Patterns

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** UX, UI, Security & Quality Auditor

Inclusive design patterns, WCAG alignment, and cognitive considerations.  
Accessibility is not a separate layer added at the end; it is a set of constraints and opportunities that shape every design decision. This file focuses on patterns that agents and designers must apply consistently.

## Core Models

| Model | Description | Typical use |
|-------|-------------|-------------|
| **Perceivable-first** | Ensure information and UI components are presentable to users in ways they can perceive (vision, hearing, touch) | All products; baseline for any public or regulated surface |
| **Operable-first** | Ensure all functionality is available from keyboard, assistive technology, and alternative inputs | Forms, applications, interactive tools |
| **Understandable-first** | Ensure information and operation are clear, predictable, and tolerant of error | Complex flows, onboarding, high-stakes transactions |
| **Robust / future-proof** | Ensure compatibility with current and future assistive technologies through semantic structure | Long-lived products, public-sector, enterprise |

Most products require all four; the models help prioritise when trade-offs appear.

## Preference Guidance

**Always apply**
- Semantic HTML (or equivalent accessibility tree) as the foundation.
- Keyboard operability for every interactive element.
- Visible focus indicators that meet contrast requirements.
- Text alternatives for non-text content.
- Sufficient colour contrast (see color-systems.md).
- Support for `prefers-reduced-motion` and system colour schemes.

**Raise the bar when**
- The product is used in regulated domains (finance, health, government, education).
- Users include people with disabilities as a primary or significant audience.
- The complexity band is L–XL and error cost is high.
- Public-sector or large-enterprise procurement requires formal WCAG conformance claims.

## Essential Decision Points

1. **Conformance target**  
   - Default recommendation: WCAG 2.2 Level AA.  
   - Document any intentional Level A-only or AAA targets and the rationale.  
   - Conformance is a claim about the delivered product; design must make that claim achievable.

2. **Keyboard and focus**  
   - Tab order must match visual order.  
   - Focus must never be trapped (except intentional modal focus traps with escape).  
   - Custom components must implement the appropriate ARIA patterns and keyboard interactions.

3. **Forms and errors**  
   - Labels must be programmatically associated.  
   - Errors must be identified, described in text, and linked to the field.  
   - Instructions and required indicators must be clear before submission.

4. **Non-text content and media**  
   - Images that convey meaning need text alternatives.  
   - Decorative images must be marked so they are ignored by assistive technology.  
   - Video/audio require captions, transcripts, or audio description as appropriate.

5. **Cognitive and situational support**  
   - Avoid time limits that cannot be extended or disabled.  
   - Provide clear, consistent navigation and feedback.  
   - Minimise cognitive load through progressive disclosure and plain language (see storytelling.md).  
   - Support users who may be interrupted, distracted, or using the product in stressful contexts.

6. **Testing posture**  
   - Automated checks catch only a subset of issues.  
   - Design and specification must enable manual and assistive-technology testing.  
   - Critical user journeys should be verified with keyboard-only and at least one screen-reader path.

## Anti-Patterns

- Relying on colour alone to convey meaning or state.
- Placeholder text used as the only label for form fields.
- Custom interactive elements without keyboard support or correct roles.
- Focus indicators removed or made invisible for aesthetic reasons.
- Animations or carousels that cannot be paused or that convey essential information only through motion.
- Captchas or interactions that are hostile to assistive technology without accessible alternatives.
- Declaring “AA compliant” without a testing and remediation plan.

## Recording Requirements

In the User Journey, UI Specification, and/or Architecture Blueprint (NFR / Quality Attributes):

- Target WCAG level and any scoped exemptions
- Keyboard and focus strategy for custom components
- Form labelling and error pattern
- Media alternative requirements
- Reduced-motion and theme support commitments
- Link back to this file and the concrete criteria used

Security & Quality Auditor must verify that accessibility commitments are testable and have not been silently dropped.

## Related Design Knowledge

- [color-systems.md](color-systems.md) — contrast and non-colour indicators
- [typography.md](typography.md) — readable text sizing and reflow
- [motion.md](motion.md) — reduced-motion and non-motion feedback
- [spacing.md](spacing.md) — touch target sizes and spacing
- [composition.md](composition.md) — logical reading and focus order
- [storytelling.md](storytelling.md) — clear narrative and progressive disclosure that reduce cognitive load

## Related Patterns & Technologies

- knowledge/patterns/forms.md — accessible form handling
- knowledge/technologies/frontend.md — framework and component choices that affect the accessibility tree
