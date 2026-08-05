# State Machine

**Status:** Active  
**Authority:** pipeline.md + stage-transitions.md  
**Last review:** 2026-08-05

## Purpose

Formal definition of project lifecycle states.  
State is never stored in a separate file; it is derived from the existence, completeness, and status headers of the artifacts themselves.

## States

| State                  | Meaning                                                                 | Detected by presence of                          | Terminal? |
|------------------------|-------------------------------------------------------------------------|--------------------------------------------------|-----------|
| `intake`               | Human has supplied raw intent                                           | intake.md (any status)                           | No        |
| `briefed`              | Problem scoped; goals, constraints, success metrics captured            | brief.md with status ≥ draft                     | No        |
| `requirements-complete`| Functional & non-functional requirements exist and are traceable        | requirements.md with status ≥ draft              | No        |
| `architected`          | System boundaries, components, data flow, and tech choices defined      | architecture.md with status ≥ draft              | No        |
| `journeyed`            | Personas, primary flows, edge cases, and success criteria defined       | journeys.md with status ≥ draft                  | No        |
| `visualized`           | Layout system, component hierarchy, design tokens specified             | visual.md with status ≥ draft                    | No        |
| `planned`              | Work breakdown, sequencing, test strategy, residual risks documented    | implementation.md with status ≥ draft            | No        |
| `audited`              | All quality gates evaluated; residual risks recorded                    | audit.md                                         | No        |
| `approved`             | Master Design Plan issued and marked approved                           | master-design-plan.md with status = approved     | Yes       |
| `rejected`             | Auditor refused approval; remediation required                          | audit.md with overall result = FAIL              | No*       |

\* `rejected` returns the project to the earliest failing stage after human review.

## Derivation Rules

- A project is always in exactly one primary state (the highest completed stage).
- An artifact with status `draft` or `in-review` counts as present for state detection.
- An artifact with status `approved` is preferred but not required until the final Master Design Plan.
- Missing upstream artifacts automatically keep the project in the earlier state even if a later file exists (orphan artifacts are invalid).

## Legal Transitions

See [stage-transitions.md](stage-transitions.md) for the complete set of allowed moves, preconditions, and postconditions.

## State Header Convention

Every project artifact must carry:

```
Status: draft | in-review | approved | rejected
```

The Security & Quality Auditor is the only agent authorised to set the final `approved` status on master-design-plan.md.
