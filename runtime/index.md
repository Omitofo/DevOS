# runtime/ — Index

Executable definition of how DevOS thinks and moves.

| Sub-domain | Purpose | Status |
|------------|---------|--------|
| [agents/](agents/index.md) | Agent definitions (one per pipeline stage) | Active |
| [workflow/](workflow/index.md) | Pipeline, state machine, stage transitions | Active |
| [orchestration/](orchestration/index.md) | Pipeline driver, agent loader, transition enforcer, human gates | Active |
| [templates/](templates/index.md) | Artifact templates used by agents | Active |

**Recommended load order for a new session:**  
workflow/ → orchestration/ → agents/ (only the active stage) → templates/ (matching template).
