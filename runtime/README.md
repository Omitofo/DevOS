# RUNTIME

**Purpose:** Executable definition of how DevOS thinks and moves.

The runtime is the control plane that turns the DevOS constitution and knowledge into a strict, auditable pipeline.

- **Workflow** declares the canonical stages, states, and legal transitions.
- **Agents** are pure Markdown contracts that own a single stage.
- **Orchestration** drives the pipeline, enforces transitions, and presents human gates.
- **Templates** supply the skeletal structure every agent must expand.

Workflow is a strict sequential pipeline. Stages cannot be skipped.  
State is derived from artifacts, never stored in a separate database.  
One stage per default invocation keeps context windows small and decisions auditable.
