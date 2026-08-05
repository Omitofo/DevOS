# Agent Loader

**How agents are loaded**

1. Determine the current pipeline stage from the project’s accumulated artifacts (or explicit state).
2. Map the stage to the agent name (see [pipeline.md](../workflow/pipeline.md)).
3. Load `runtime/agents/<agent>.md`.
4. Execute the agent’s responsibilities against the current set of project artifacts.
5. Write the agent’s primary output into the project folder.
6. Advance only when the stage’s exit criteria are met.

Agents are pure Markdown definitions. The loader treats them as the executable contract for that stage.
