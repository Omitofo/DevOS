# RUNTIME

**Purpose:** Executable definition of how DevOS thinks and moves.

The runtime loads the correct agent definition via `runtime/agents/<agent>.md` based on the current pipeline stage.

Workflow is a strict sequential pipeline. Stages cannot be skipped.
