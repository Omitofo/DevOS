# Canonical Pipeline

**Status:** Active  
**Authority:** core/contract.md + core/quality-gates.md  
**Last review:** 2026-08-05

```
Idea
  → Project Intake          (human-owned)
  → Planner                 → Project Brief
  → Analyst                 → Requirements
  → Architect               → Architecture Blueprint
  → UX                      → User Journey
  → UI                      → Visual Blueprint
  → Tech Lead               → Implementation Plan
  → Security & Quality Auditor → Master Design Plan (or rejection)
```

## Stage Order (Mandatory)

| # | Stage                        | Agent                          | Primary Artifact              | Required Upstream Artifacts                  |
|---|------------------------------|--------------------------------|-------------------------------|----------------------------------------------|
| 0 | Intake                       | Human                          | intake.md                     | —                                            |
| 1 | Planner                      | runtime/agents/planner.md      | brief.md                      | intake.md                                    |
| 2 | Analyst                      | runtime/agents/analyst.md      | requirements.md               | brief.md, intake.md                          |
| 3 | Architect                    | runtime/agents/architect.md    | architecture.md               | requirements.md, brief.md                    |
| 4 | UX                           | runtime/agents/ux.md           | journeys.md                   | requirements.md, brief.md                    |
| 5 | UI                           | runtime/agents/ui.md           | visual.md                     | journeys.md, requirements.md                 |
| 6 | Tech Lead                    | runtime/agents/tech-lead.md    | implementation.md             | architecture.md, requirements.md, journeys.md, visual.md |
| 7 | Security & Quality Auditor   | runtime/agents/security-quality-auditor.md | audit.md + master-design-plan.md | All prior artifacts + quality gates |

## Inviolable Rules

- Stages execute strictly in order. No stage may be skipped or reordered.
- An agent may only write its own primary artifact (and any explicitly allowed secondary notes).
- No production code, repository scaffolding, or deployment configuration may be produced before the Master Design Plan is approved.
- Every transition must satisfy the preconditions defined in [stage-transitions.md](stage-transitions.md).
- Current state is derived exclusively from the presence and status of artifacts (see [state-machine.md](state-machine.md)).

## Invocation Contract

When DevOS is invoked on a project, the runtime:

1. Determines current state from existing artifacts under `projects/<name>/`.
2. Loads the single agent definition that owns the next legal stage.
3. Executes that agent against the accumulated artifacts.
4. Writes the new artifact(s) and updates status headers.
5. Stops. The human (or next invocation) decides whether to advance.

One stage per invocation is the default. Batching is permitted only when the human explicitly requests it and every intermediate quality check still passes.
