# Agents

**Status:** Active  
**Authority:** runtime/workflow/pipeline.md + core/contract.md  
**Last review:** 2026-08-05

Each agent lives in its own file and is loaded by the runtime according to the current pipeline stage (see [workflow/state-machine.md](../workflow/state-machine.md)).

## Design Principles for Agent Definitions

Every agent definition must contain:

1. **Precise process steps** — ordered, executable instructions the agent follows.
2. **Decision criteria** — explicit rules for when to proceed, when to surface an open question, and when to block.
3. **Exact output structure expectations** — the sections and fields the produced artifact must contain, aligned with the corresponding template under `runtime/templates/`.
4. **Explicit workflow references** — links to the relevant preconditions, postconditions, and stage-transition rules.
5. **Template reference** — the skeletal template the agent must start from and expand.

Agents have:

- Clearly defined inputs  
- Clearly defined primary output  
- Explicit responsibilities and constraints  
- Obligation to respect the DevOS Contract  
- Obligation to restate the preconditions of their own stage (enforcement rule from stage-transitions.md)

## Loading Contract

When the runtime selects an agent:

1. Determine current state from artifacts (state-machine.md).
2. Load exactly one agent definition: `runtime/agents/<agent>.md`.
3. The agent may only write its own primary artifact (and any explicitly allowed secondary notes).
4. One stage per default invocation.

## Files

| File | Agent | Primary Artifact |
|------|-------|------------------|
| [planner.md](planner.md) | Planner | brief.md |
| [analyst.md](analyst.md) | Analyst | requirements.md |
| [architect.md](architect.md) | Architect | architecture.md |
| [ux.md](ux.md) | UX | journeys.md |
| [ui.md](ui.md) | UI | visual.md |
| [tech-lead.md](tech-lead.md) | Tech Lead | implementation.md |
| [security-quality-auditor.md](security-quality-auditor.md) | Security & Quality Auditor | audit.md + master-design-plan.md |
