# Agent Loader

**Status:** Active  
**Authority:** pipeline-driver.md + runtime/workflow/pipeline.md + runtime/agents/*  
**Last review:** 2026-08-05

## Purpose

Map a derived pipeline state to the single agent definition that owns the next legal stage, load that definition, and treat it as the executable contract for the current invocation.

Agents are pure Markdown. The loader never adds behaviour that is not written in the agent file.

## Load Sequence

1. **Receive target stage** from the pipeline driver (or from an explicit human override).
2. **Map stage → agent file** using the canonical table below.
3. **Resolve path** → `runtime/agents/<agent>.md`.
4. **Validate the agent file exists** and carries `Status: Active`.
5. **Load the complete agent definition** into the execution context.
6. **Hand control to the agent** under the constraints of the transition enforcer.
7. **After the agent finishes**, return control to the pipeline driver for state re-derivation and stop.

## Canonical Stage → Agent Mapping

| Derived State (current) | Next Stage Agent | Agent File | Primary Artifact Written |
|-------------------------|------------------|------------|--------------------------|
| `intake` | Planner | `runtime/agents/planner.md` | `brief.md` |
| `briefed` | Analyst | `runtime/agents/analyst.md` | `requirements.md` |
| `requirements-complete` | Architect | `runtime/agents/architect.md` | `architecture.md` |
| `architected` | UX | `runtime/agents/ux.md` | `journeys.md` |
| `journeyed` | UI | `runtime/agents/ui.md` | `visual.md` |
| `visualized` | Tech Lead | `runtime/agents/tech-lead.md` | `implementation.md` |
| `planned` | Security & Quality Auditor | `runtime/agents/security-quality-auditor.md` | `audit.md` (+ `master-design-plan.md` on approval) |
| `audited` (PASS path) | (Auditor continues) | same | `master-design-plan.md` status = approved |
| `audited` (FAIL path) | (Auditor continues) | same | rejection + remediation |
| `approved` | — | none | terminal |
| `rejected` | — | none | terminal until human remediation |

The mapping is authoritative. No other agent may write an artifact that belongs to a different stage.

## Execution Contract Imposed on Every Agent

When the loader hands control to an agent it enforces:

1. The agent may read any upstream artifact and any file under `core/` or `knowledge/`.
2. The agent may write **only** its primary artifact (and any secondary notes explicitly permitted in its own definition).
3. The agent must begin from the matching template in `runtime/templates/`.
4. The agent must respect every precondition listed in its own definition and in [transition-enforcer.md](transition-enforcer.md).
5. The agent must stop after writing its artifact; it never chains to the next stage.
6. The agent must never set `Status: approved` on the Master Design Plan except the Security & Quality Auditor.

## Loader Failure Modes

| Condition | Loader Action |
|-----------|---------------|
| Target agent file missing or `Status` ≠ Active | STOP. Report missing agent definition. |
| Derived state has no mapping (should be impossible) | STOP. Report internal inconsistency. |
| Human has requested a stage whose preconditions are unmet | Refuse load; hand the blocking conditions to the transition enforcer. |
| Agent attempts to write an artifact outside its ownership | The violation is recorded; the write is invalid for state derivation. |

## Design Invariants

- Agents are stateless. All memory lives in the project folder and in Git.
- The loader never modifies agent files.
- The loader never invents process steps; it only executes what the agent Markdown already contains.
- Cross-references inside agent files (to templates, workflow files, knowledge) are resolved by the executing LLM, not by the loader itself.
