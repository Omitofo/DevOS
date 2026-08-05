# Orchestration

**Status:** Active  
**Purpose:** Executable definition of how the DevOS pipeline is driven, how stage transitions are enforced, and how human gates are presented and cleared.

Orchestration sits above the workflow definitions and the agent definitions.  
It is the runtime control plane: it decides *what* runs next, *whether* a transition is legal, and *when* the human must intervene.

The three workflow files (`pipeline.md`, `state-machine.md`, `stage-transitions.md`) declare the rules.  
The orchestration files declare how those rules are applied on every invocation.

## Files

| File | Responsibility |
|------|----------------|
| [pipeline-driver.md](pipeline-driver.md) | Main invocation loop, state detection, single-stage execution contract |
| [agent-loader.md](agent-loader.md) | Mapping from derived state → agent definition → execution |
| [transition-enforcer.md](transition-enforcer.md) | Precondition checks, blocking conditions, legal-move validation |
| [human-gates.md](human-gates.md) | Presentation of checkpoints, expected human responses, gate clearance |

## Design Notes (for maintainers & RAG)

- State is derived, never stored. The driver recomputes it from the project folder on every invocation.
- One stage per default invocation keeps context windows small, decisions auditable, and the human in the loop.
- Agents are pure Markdown contracts. The loader never invents behaviour; it only executes what the agent definition states.
- Human gates are first-class. The runtime never auto-advances past a required checkpoint.
- All four files are intentionally dense and cross-linked so an LLM can load the entire orchestration domain in a single retrieval pass.

## Authority

- core/contract.md (especially “never skip workflow stages”)
- runtime/workflow/* (pipeline, state machine, transitions)
- runtime/agents/* (the executable contracts that the loader runs)
- core/quality-gates.md (final enforcement via the Auditor)
