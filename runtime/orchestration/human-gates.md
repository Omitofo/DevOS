# Human Gates

**Status:** Active  
**Authority:** stage-transitions.md + pipeline-driver.md + core/contract.md (“Human owns the vision”)  
**Last review:** 2026-08-05

## Purpose

Define every point at which the runtime must stop and obtain explicit human input before the pipeline may continue.  
Human gates protect the Contract principle that the human owns vision, constraints, taste, and ultimate judgment.

The runtime never auto-advances past a required gate.

## Gate Catalogue

| Gate ID | Trigger | When Presented | Required Human Response | Clearance Condition |
|---------|---------|----------------|--------------------------|---------------------|
| G0 | New project / missing intake | Before Planner can run | Supply or confirm `intake.md` | `intake.md` exists with usable content |
| G1 | Blocking open question | Any agent surfaces a question marked blocking | Answer the question or explicitly defer/accept | Answer recorded in the relevant artifact or a follow-up note |
| G2 | Transition blocked | Transition enforcer fails any pre-execution check | Resolve the failed checks (supply missing artifact, accept risk, fix status, etc.) | All failed checks now pass |
| G3 | Stage artifact ready for review | Agent finishes and sets status `in-review` (or human requests review) | Approve, request changes, or reject | Human statement of approval / change request / rejection |
| G4 | Auditor rejection | Security & Quality Auditor produces overall FAIL | Choose remediation path (which stage to return to, what to change) | Human decision recorded; project state re-derived after changes |
| G5 | Final approval (recommended) | Before Auditor writes `master-design-plan.md` status = approved | Explicit confirmation that the package is ready for final approval | Human confirmation received |
| G6 | Explicit batch request | Human asks to run multiple stages in one invocation | Name the exact sequence and accept responsibility for intermediate checks | Request is unambiguous and accepted by the driver |

Gates G0–G2 and G4 are mandatory.  
Gates G3 and G5 are strongly recommended; the runtime should surface them even if the human has previously indicated a preference for speed.

## Presentation Format

When a gate is open the runtime must present it in a clear, machine- and human-readable form:

```markdown
# Human Gate — <Gate ID>

**Project:** <name>
**Current derived state:** <state>
**Blocking reason:** <one-sentence summary>

## What is required from you
- Concrete list of decisions or artifacts needed.

## Context
- Links to the relevant artifact sections, open questions, or audit findings.

## How to clear this gate
- Exact statements or file changes that will allow the pipeline to continue.
```

The presentation must never bury the required action.  
The human should be able to respond with a short, unambiguous reply.

## Clearance Protocol

1. Human supplies the required response (answer, decision, status change, or new artifact content).
2. The response is written into the appropriate project artifact or recorded as a dated note.
3. On the next invocation the pipeline driver re-derives state and the transition enforcer re-evaluates preconditions.
4. If the gate is now clear, the next legal agent is loaded.
5. If the gate remains open, it is presented again with any new context.

Clearance is never implicit. Silence or “looks good” without an explicit decision does not clear a mandatory gate.

## Open-Question Handling

Agents are required to surface gaps as open questions.  
Questions are classified:

- **Blocking** — the current or a later stage cannot produce a trustworthy artifact until the question is answered. Triggers Gate G1.
- **Non-blocking** — the agent can proceed with a documented assumption or placeholder. The question remains visible for later resolution.

Only the human can re-classify a blocking question as accepted residual risk.  
The acceptance must be written; conversational acknowledgement alone is insufficient for the final Auditor.

## Rollback & Remediation Gates

When the Auditor rejects a package (Gate G4):

- The runtime presents the earliest stage that must be revisited and the concrete remediation items.
- The human decides the remediation path.
- After the human edits or re-runs the indicated stage(s), the driver re-derives state from the new artifact set.
- The pipeline resumes from the corrected point; later artifacts that are now inconsistent may need to be re-generated.

The runtime never auto-deletes earlier artifacts. The audit trail is preserved.

## Design Invariants

- Human gates are first-class citizens of the orchestration model, not afterthoughts.
- The runtime prefers an explicit stop over a silent assumption.
- Every gate presentation must be actionable: the human always knows exactly what is required to continue.
- Preference for speed never overrides a mandatory gate (G0, G1, G2, G4).
