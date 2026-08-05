# Stage Transitions

**Status:** Active  
**Authority:** pipeline.md + state-machine.md + core/contract.md  
**Last review:** 2026-08-05

## Purpose

Explicit rules that govern when a stage may complete and the next stage may begin.  
These rules are enforced by the runtime and by every agent.

## General Preconditions (apply to every transition)

1. All upstream artifacts required by the target stage exist and have status ≥ `draft`.
2. No open questions marked “blocking” remain unresolved unless the human has explicitly accepted them.
3. The DevOS Contract has not been violated in any prior artifact.
4. The agent for the current stage has finished writing its primary artifact and updated its status header.

## Transition Table

| From State             | To State                 | Trigger Agent                  | Required Artifacts Before Transition                  | Postcondition                                      |
|------------------------|--------------------------|--------------------------------|-------------------------------------------------------|----------------------------------------------------|
| intake                 | briefed                  | Planner                        | intake.md                                             | brief.md created                                   |
| briefed                | requirements-complete    | Analyst                        | brief.md, intake.md                                   | requirements.md created                            |
| requirements-complete  | architected              | Architect                      | requirements.md, brief.md                             | architecture.md created                            |
| architected            | journeyed                | UX                             | requirements.md, brief.md                             | journeys.md created                                |
| journeyed              | visualized               | UI                             | journeys.md, requirements.md                          | visual.md created                                  |
| visualized             | planned                  | Tech Lead                      | architecture.md, requirements.md, journeys.md, visual.md | implementation.md created                       |
| planned                | audited                  | Security & Quality Auditor     | All prior artifacts                                   | audit.md created                                   |
| audited                | approved                 | Security & Quality Auditor     | audit.md with all gates PASS (or accepted residual risk) | master-design-plan.md status = approved         |
| audited                | rejected                 | Security & Quality Auditor     | audit.md with any gate FAIL and no risk acceptance    | Project returns to earliest failing stage          |

## Blocking Conditions

A transition is blocked (and must be reported as an open question) when:

- Any required upstream artifact is missing.
- Any required upstream artifact has status `rejected`.
- A quality gate that is mandatory for the current stage has previously failed without human risk acceptance.
- The agent detects a Contract violation (especially “never invent requirements”).

## Human Checkpoints

The following transitions require explicit human confirmation before the next agent may run:

- After Auditor produces a `rejected` result (human decides remediation path).
- Any time an agent surfaces a blocking open question that cannot be resolved from existing artifacts.
- Before the final `approved` status is written to master-design-plan.md (optional but recommended).

## Orphan & Rollback Rules

- An artifact that appears without its required upstreams is treated as invalid and must be ignored for state derivation.
- Rollback is performed by the human: change the status of the offending artifact and re-invoke from the correct earlier stage.
- The runtime never auto-deletes or auto-rewrites earlier artifacts; the audit trail is preserved.

## Enforcement

Every agent definition in `runtime/agents/` must restate the relevant preconditions for its own stage.  
The Security & Quality Auditor is the final enforcer and may reject an entire package for transition violations.
