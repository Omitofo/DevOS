# Workflow

The DevOS workflow is a strict sequential pipeline.  
Stages cannot be skipped.

The active agent is determined solely by the current stage.  
The runtime loads `runtime/agents/<agent>.md` and executes that agent’s responsibilities against the accumulated project artifacts.
