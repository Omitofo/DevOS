# START.md — Entry Point

This is the canonical entry point for both humans and AI agents.

## First Actions for Any Session

1. Read this file completely.
2. Read [VISION.md](VISION.md) for the long-term intent.
3. Read [core/constitution.md](core/constitution.md) and [core/contract.md](core/contract.md).
4. Consult the domain `index.md` files to locate the information you need.
5. Never invent requirements. Prefer placeholders and explicit open questions.

## Routing Table

| Need | Go to |
|------|-------|
| Immutable rules & quality gates | [core/](core/index.md) |
| Reusable knowledge, patterns, blueprints | [knowledge/](knowledge/index.md) |
| Agents, pipeline, orchestration | [runtime/](runtime/index.md) |
| Live projects or project template | [projects/](projects/index.md) |

## Invoking the Pipeline on a New Project

1. Create a new folder under `projects/` by copying `projects/_template/`.
2. Place the human-provided Intake into `projects/<name>/intake.md`.
3. Begin at the **Planner** stage.
4. Load the corresponding agent definition from `runtime/agents/`.
5. Write each intermediate artifact into the project folder.
6. Proceed strictly through the pipeline defined in [runtime/workflow/pipeline.md](runtime/workflow/pipeline.md).
7. Final approval is performed by the Security & Quality Auditor.

## Persistence Rule

Git is the permanent memory.  
Every decision, assumption, and artifact must be written to a version-controlled Markdown file.  
Do not rely on conversational context across sessions.
