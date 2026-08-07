# The DevOS Contract (Inviolable Rules)

**Status:** Canonical  
**Authority:** DEVOS_BOOTSTRAP_SPEC.md §2.7  
**Last review:** 2026-08-07

These eleven rules are inviolable.  
Violation of any rule invalidates the resulting Master Design Plan.  
No agent, human collaborator, or downstream process may waive a rule without an explicit, recorded constitutional amendment.

---

## 1. Never invent requirements

Every functional, non-functional, and constraint statement must be traceable to either:

- explicit user input (Intake or later human clarification), or  
- a documented, justified inference that is clearly marked as such and presented for human confirmation.

**Implications**
- Placeholders and open questions are preferred over assumptions.
- An agent that silently fills a gap with a “reasonable” default has violated the Contract.
- Inferences must be listed separately and never promoted to requirements until the human confirms them.

**Common violations**
- Inventing acceptance criteria that were never stated.
- Assuming a particular authentication method because it is “common”.
- Filling missing non-functional budgets with industry averages without marking them as inferences.

---

## 2. Never skip workflow stages

The pipeline defined in `runtime/workflow/pipeline.md` is sequential and mandatory.

**Implications**
- No stage may be omitted, reordered, or short-circuited.
- An agent may produce only the artifact(s) that belong to its own stage.
- Batching multiple stages in one invocation is permitted only when the human explicitly requests it **and** every intermediate gate still passes.

**Common violations**
- Jumping from Brief directly to Architecture.
- Writing implementation details inside the Requirements artifact.
- Treating the Auditor stage as optional.

---

## 3. Never skip Engineering Quality Gates

Every Master Design Plan must be evaluated against **all** gates defined in `core/quality-gates.md`.

**Implications**
- The Security & Quality Auditor is the final authority.
- A plan that fails any gate cannot be approved unless an explicit, human-accepted risk-acceptance record exists.
- Upstream agents must design with the gates in mind; they are not a late surprise.

**Common violations**
- Declaring a gate “not applicable” without recorded justification and human concurrence.
- Approving a plan that has residual high-severity findings without risk acceptance.

---

## 4. Never generate implementation before the Master Design Plan exists

Code generation, repository scaffolding, CI configuration, and deployment scripts live **downstream** of DevOS.

**Implications**
- No production code, infrastructure-as-code, or runnable prototype may be emitted by any DevOS agent.
- The sole product of a DevOS run is the approved Master Design Plan.
- Implementation work begins only after the Auditor has signed off (or after explicit risk acceptance).

**Common violations**
- Emitting sample code “for illustration” that is actually production-ready.
- Creating a GitHub repository or project structure inside a DevOS stage.

---

## 5. Every decision must be traceable

Artifacts must contain links to upstream sources, explicit assumptions (if any), and open questions (if any).

**Implications**
- A requirement, architectural choice, or design decision that cannot be traced back to an upstream artifact or a documented human confirmation is invalid.
- Traceability is recorded inside the artifact itself (see `artifact-lifecycle.md`).

**Common violations**
- Stating “we chose Postgres because it is popular” with no link to a requirement or constraint.
- Omitting the Upstream / Assumptions / Open questions header block.

---

## 6. Every module has exactly one responsibility

Single-responsibility principle is mandatory for:

- folders and files inside the DevOS repository itself,
- components described in Architecture Blueprints,
- agents in the runtime.

**Implications**
- If a unit of work starts to serve two distinct purposes, it must be split.
- Agents must not absorb responsibilities that belong to another agent.

**Common violations**
- An Architecture document that also contains detailed UI component specs.
- A single agent definition that both plans and audits.

---

## 7. Prefer links over duplication

Knowledge is never copied; it is referenced via Markdown links.

**Implications**
- Project artifacts link to `knowledge/` entries; they do not embed full copies.
- CORE rules are referenced, not restated in every agent prompt.
- Duplication creates divergence and is therefore forbidden.

**Common violations**
- Copy-pasting a pattern into a project folder “for convenience”.
- Re-defining a Quality Gate inside an agent file.

---

## 8. Prefer placeholders over assumptions

Documented placeholders are first-class citizens.

**Implications**
- When confidence is low or information is missing, create an explicit placeholder that states:
  - future purpose,
  - expected responsibilities,
  - expected relationships,
  - the future artifact that will replace it.
- A placeholder is always preferable to a silent assumption.

**Common violations**
- Inventing a technology choice because a placeholder felt incomplete.
- Leaving a section blank without declaring it a placeholder.

---

## 9. Human owns the vision

The human supplies the problem, constraints, taste, and ultimate judgment.

**Implications**
- Agents never invent product vision, brand personality, or strategic priorities.
- When taste or preference is required and missing, the agent surfaces an open question.
- Final approval of the Master Design Plan rests with the human (or a human-delegated risk-acceptance process).

**Common violations**
- An agent deciding the product should target “enterprise” when the Intake never said so.
- Overriding a human-stated non-goal because the agent believes it is short-sighted.

---

## 10. AI owns engineering reasoning

The AI supplies decomposition, trade-off analysis, consistency checking, and specification completeness.

**Implications**
- Agents are expected to perform rigorous engineering work inside the bounds of the Contract.
- “I don’t know” is acceptable only when turned into a documented open question; vague hand-waving is not.
- The human is not required to supply technical solutions; that is the AI’s responsibility.

**Common violations**
- Asking the human to invent the architecture.
- Producing an Architecture Blueprint that merely restates the requirements without analysis.

---

## 11. Git owns memory

Language models are stateless.  
Every engineering decision lives in version-controlled Markdown.

**Implications**
- Conversational context is ephemeral and must never be treated as authoritative.
- Every decision, assumption, open question, and approval must be written into a file under the project folder (or into CORE / knowledge when appropriate).
- A later session that cannot reconstruct the full rationale from Git has lost information and must treat the gap as an open question.

**Common violations**
- Relying on “as we discussed earlier” without a corresponding artifact update.
- Leaving critical trade-off reasoning only in the chat transcript.

---

## Enforcement

- The Security & Quality Auditor is required to verify Contract compliance as part of the final audit.
- Any detected violation must be recorded in the Audit Report and blocks approval unless an explicit risk-acceptance record (signed by the human) is present.
- Runtime orchestration must refuse to advance a stage whose preconditions or Contract rules are violated.
