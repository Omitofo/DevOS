# Engineering Quality Gates

**Status:** Canonical  
**Authority:** DEVOS_BOOTSTRAP_SPEC.md §7  
**Last review:** 2026-08-07  
**Primary consumer:** Security & Quality Auditor  
**Secondary consumers:** All upstream agents (must design with these gates in mind)

Every Master Design Plan must be evaluated against **all** gates listed below.  
Each gate records:

| Field | Meaning |
|-------|---------|
| **Result** | `PASS` / `FAIL` / `N/A` (with justification) |
| **Evidence** | Concrete references to sections of project artifacts that demonstrate compliance |
| **Observations** | Findings, residual risks, or clarifying notes |
| **Risk acceptance** | Required only when Result = FAIL and the human has explicitly accepted the residual risk |

The Security & Quality Auditor is the final authority.  
It may not approve a plan that fails any gate without an explicit, human-accepted risk-acceptance record stored in the project.

---

## Gate Evaluation Process

1. Load the complete set of project artifacts up to and including the Implementation Plan.
2. For each gate, locate the required evidence (see per-gate criteria).
3. Record Result, Evidence, and Observations in the Audit Report.
4. If any gate is FAIL and no risk-acceptance record exists, the Master Design Plan cannot be approved.
5. `N/A` is permitted only when the gate is demonstrably irrelevant to the project type **and** the justification is recorded. The Auditor may still override an unjustified `N/A`.

---

## 1. Functional Correctness

**Intent:** Requirements are complete, consistent, and testable.

**Pass criteria**
- Every stated goal and constraint from the Brief appears in the Requirements (or is explicitly deferred with rationale).
- No contradictory requirements exist.
- Every functional requirement is written so that a future test can confirm its satisfaction.
- Edge cases and error paths identified in Journeys or Requirements are covered.

**Required evidence**
- Requirements document with clear IDs or anchors.
- Traceability from Brief → Requirements.
- Absence of open conflicts marked as unresolved.

**Common failure modes**
- Ambiguous verbs (“handle”, “support”, “manage”) without measurable criteria.
- Missing error or empty-state behaviour that the Journeys imply.
- Requirements that cannot be tested without further invention.

---

## 2. Performance

**Intent:** Explicit budgets and a measurement strategy exist.

**Pass criteria**
- Key user-facing and system-level performance budgets are stated (latency, throughput, payload size, time-to-interactive, etc.).
- Budgets are justified by reference to constraints or inferred needs (marked as such).
- A measurement approach is defined (what will be measured, where, and with what tooling class).
- Degradation behaviour under load is considered if the project is expected to face variable traffic.

**Required evidence**
- Explicit performance section in Requirements or Architecture.
- Linkage to any non-functional constraints in the Intake/Brief.

**Common failure modes**
- “The system should be fast” with no numbers.
- Budgets that are impossible given the chosen architecture without any acknowledgement.
- No plan for how performance will be verified.

---

## 3. Accessibility

**Intent:** WCAG 2.2 Level AA (or a project-specific higher target) is achievable from the specification.

**Pass criteria**
- Target standard is declared (default: WCAG 2.2 AA).
- Visual Blueprint and Journeys account for keyboard navigation, focus management, colour contrast, text alternatives, and screen-reader semantics.
- Any intentional deviation is recorded and justified.
- Forms, interactive components, and media have accessibility considerations.

**Required evidence**
- Accessibility target statement.
- Notes or acceptance criteria in Visual Blueprint / Journeys covering the major WCAG success criteria relevant to the UI.

**Common failure modes**
- Colour-only status indicators with no non-colour equivalent.
- Custom components described without keyboard or ARIA considerations.
- Declaring AA while the design system tokens make contrast impossible.

---

## 4. Responsive Design

**Intent:** Defined breakpoints and adaptive behaviour exist.

**Pass criteria**
- Breakpoints (or container queries) are explicitly listed.
- Layout, navigation, and key components have defined behaviour at each major viewport class.
- Touch targets and input methods are considered for smaller viewports.
- Content priority / progressive disclosure strategy is present when space is constrained.

**Required evidence**
- Breakpoint table or equivalent in Visual Blueprint or Architecture.
- Adaptive notes for primary screens/journeys.

**Common failure modes**
- Desktop-only wireframe language with no mobile treatment.
- Fixed-pixel layouts that cannot reflow.
- Ignoring landscape / foldable / large-desktop extremes when the product context requires them.

---

## 5. Security

**Intent:** Threat model, authentication, authorization, input validation, and secrets handling are addressed.

**Pass criteria**
- A threat model (even lightweight) identifies assets, attackers, and key threats.
- Authentication and authorization approach is stated and linked to a knowledge pattern where applicable.
- Input validation and output encoding responsibilities are assigned.
- Secrets, keys, and credentials have a handling strategy (never in client bundles, rotation, etc.).
- Common web/API vulnerabilities relevant to the architecture are considered (injection, CSRF, SSRF, insecure direct object references, etc.).

**Required evidence**
- Security section in Architecture or a dedicated subsection.
- References to `knowledge/patterns/authentication.md`, `authorization.md`, etc., where used.
- Explicit statements on secrets and validation.

**Common failure modes**
- “We will use JWT” with no further threat analysis.
- Missing authorization boundaries between tenants or roles.
- Client-side only validation presented as sufficient.

---

## 6. Privacy

**Intent:** Data minimization, consent, retention, and regional compliance are addressed.

**Pass criteria**
- Categories of personal data collected or processed are listed.
- Purpose and legal basis (or equivalent) for each category are stated or marked as open questions.
- Retention periods or deletion triggers are defined.
- User rights (access, deletion, export) have a high-level fulfilment path.
- Cross-border transfer or regional requirements (GDPR, CCPA, etc.) are acknowledged when the audience implies them.

**Required evidence**
- Data / privacy section in Requirements or Architecture.
- Explicit open questions where legal advice is still required.

**Common failure modes**
- Collecting data “just in case” with no purpose.
- No retention or deletion story.
- Ignoring that the product will have EU users when the Intake mentioned a global audience.

---

## 7. UX

**Intent:** Journeys cover primary and edge cases; cognitive load is considered.

**Pass criteria**
- Primary user journeys are fully described end-to-end.
- Critical edge cases, error states, empty states, and permission-denied paths appear.
- Cognitive load, decision points, and potential confusion are acknowledged.
- Success and failure criteria for each journey are clear.

**Required evidence**
- User Journey artifact with primary + secondary / edge paths.
- Consistency with Requirements (no journey invents functionality).

**Common failure modes**
- Happy-path-only journeys.
- Journeys that assume knowledge the user cannot have.
- Missing recovery paths after errors.

---

## 8. UI

**Intent:** Visual system is coherent, tokenized, and implementable.

**Pass criteria**
- Design tokens (colour, type, space, radius, elevation, motion) are defined or referenced.
- Component inventory or key component behaviours are specified at a level sufficient for implementation.
- Visual hierarchy and information architecture support the journeys.
- States (default, hover, focus, active, disabled, error, loading) are considered for interactive elements.
- The specification is free of pure decoration that has no corresponding token or rationale.

**Required evidence**
- Visual Blueprint with token definitions or links to a design system.
- Component-level notes sufficient for a developer to implement without invention.

**Common failure modes**
- Screenshot-only “design” with no tokens or states.
- Inconsistent spacing / type scales.
- Interactive elements described only in their default state.

---

## 9. Maintainability

**Intent:** Clear module boundaries and low accidental complexity.

**Pass criteria**
- Architecture shows modules / bounded contexts with single responsibilities.
- Dependencies between modules are explicit and justified.
- Accidental complexity (over-abstraction, premature generalization, tight coupling) is avoided or called out.
- Naming and structural conventions are consistent with `knowledge/standards/`.

**Required evidence**
- Architecture Blueprint with module diagram or equivalent description.
- Responsibility statements per major module.

**Common failure modes**
- Monolithic “god” services or components.
- Circular dependencies.
- Abstractions introduced “for future flexibility” with no current requirement.

---

## 10. Testability

**Intent:** Every requirement maps to verifiable tests.

**Pass criteria**
- Each functional requirement can be traced to one or more test ideas (unit, integration, end-to-end, or manual).
- Non-functional requirements have a verification approach.
- Testability is not blocked by architectural choices (e.g., irreplaceable third-party calls without seams).
- Critical paths have higher test emphasis acknowledged.

**Required evidence**
- Requirements written in testable language.
- Architecture notes on test seams where relevant.
- Optional but recommended: high-level test strategy section in Implementation Plan.

**Common failure modes**
- Requirements that can only be verified by “looking at it”.
- Architecture that embeds third-party SDKs with no abstraction, making tests brittle or impossible.

---

## 11. SEO (when applicable)

**Intent:** Technical and content SEO strategy is defined for public, indexable surfaces.

**Applicability**
- Required when the product has public web pages intended to be discovered via search engines.
- `N/A` is acceptable for pure internal tools, authenticated-only apps, or native mobile apps with no web presence — justification must be recorded.

**Pass criteria**
- Indexable vs. non-indexable routes are identified.
- Title, meta, canonical, and structured-data strategy is outlined.
- Performance and Core Web Vitals implications are acknowledged (links to Performance gate).
- Content hierarchy supports crawlability.

**Required evidence**
- SEO section in Requirements, Architecture, or Visual Blueprint.
- Explicit `N/A` justification when the gate is skipped.

**Common failure modes**
- Public marketing pages with no meta or heading strategy.
- Client-side-only rendering of critical content without a fallback plan.
- Declaring SEO “not needed” for a public content site.

---

## Residual Risk & Approval

- Any FAIL result that is not covered by a human-signed risk-acceptance record blocks approval of the Master Design Plan.
- Risk-acceptance records must state: the gate, the residual risk, the mitigation (if any), the human approver, and the date.
- The Auditor may still recommend against approval even when risk acceptance exists; the human makes the final call.
