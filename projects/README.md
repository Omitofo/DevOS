# projects/

**Purpose:** Ephemeral, self-contained workspaces for live DevOS projects.

Each project is a folder that accumulates artifacts strictly in pipeline order.  
When the Master Design Plan is approved, the project may be archived, handed off, or left as a permanent record.

This directory is **not** a knowledge base and **not** a code repository.  
It holds only the living specification artifacts produced by the agents.

## Rules

1. **One project = one folder.**  
   Name the folder after the working title (kebab-case preferred).  
   Never reuse a folder for a different idea.

2. **Always start from the template.**  
   Copy `projects/_template/` in full.  
   Do not invent extra top-level files unless the Master Design Plan itself requires an appendix.

3. **Artifacts are written only by the authorised agent** (or the human for intake).  
   See the canonical sequence in [core/artifact-lifecycle.md](../core/artifact-lifecycle.md).

4. **Status and upstream fields are mandatory.**  
   Every artifact must carry the metadata header defined in the lifecycle document.

5. **No code, no generated application assets.**  
   DevOS produces specifications. Implementation happens outside this repository.

6. **Git is the permanent memory.**  
   Commit after every meaningful stage transition. Conversational context is not durable.

## Starting a New Project

```bash
cp -r projects/_template projects/<project-name>
# then place the human Intake into projects/<project-name>/intake.md
```

Follow the pipeline defined in [runtime/workflow/pipeline.md](../runtime/workflow/pipeline.md).

## Index

| Item | Purpose |
|------|---------|
| [index.md](index.md) | Navigation and quick reference |
| [_template/](_template/) | Exact scaffold for every new project |

Live project folders appear alongside `_template/` once created.
