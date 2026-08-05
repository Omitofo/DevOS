# Workflow

**Status:** Active  
**Purpose:** Executable definition of the DevOS pipeline, states, and legal transitions.

The workflow is a strict sequential pipeline. Stages cannot be skipped.  
The active agent is determined solely by the current derived state.  
The runtime loads `runtime/agents/<agent>.md` and executes that agent’s responsibilities against the accumulated project artifacts.

## Files

| File | Responsibility |
|------|----------------|
| [pipeline.md](pipeline.md) | Canonical stage sequence, mandatory order, invocation contract |
| [state-machine.md](state-machine.md) | Formal states and how they are derived from artifacts |
| [stage-transitions.md](stage-transitions.md) | Preconditions, postconditions, blocking rules, human checkpoints |

## Design Notes (for maintainers & RAG)

- State is never a separate database; it is computed from the project folder.
- One stage per default invocation keeps context windows small and decisions auditable.
- All three files are intentionally dense and cross-linked so an LLM can load the entire workflow domain in a single retrieval pass.
