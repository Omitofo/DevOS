# Pipeline Driver

**Status:** Active  
**Authority:** runtime/workflow/pipeline.md + state-machine.md + stage-transitions.md + core/contract.md  
**Last review:** 2026-08-05

## Purpose

The single entry-point protocol that drives every DevOS invocation on a project.  
It answers three questions in order:

1. What is the current derived state of this project?
2. Which agent (if any) is allowed to run next?
3. Has a human gate been reached that must stop further progress?

The driver never invents stages, never skips stages, and never writes artifacts itself.  
It only selects, loads, and constrains the correct agent.

## Invocation Loop (Canonical)

```
1. Locate project folder          → projects/<name>/
2. Derive current state           → see state-machine.md
3. Identify next legal stage      → see stage-transitions.md
4. Check for open human gates     → see human-gates.md
5. If gate is open                → STOP and present the gate
6. Load the owning agent          → see agent-loader.md
7. Enforce transition preconditions → see transition-enforcer.md
8. Execute the agent              → agent writes its primary artifact(s)
9. Re-derive state                → confirm the expected postcondition
10. STOP                          → one stage per default invocation
```

## State Detection Protocol

On every invocation the driver must:

1. List every artifact present under `projects/<name>/`.
2. Read the status header of each artifact (`draft | in-review | approved | rejected | complete`).
3. Apply the derivation rules in [state-machine.md](../workflow/state-machine.md):
   - Highest completed stage wins.
   - Orphan artifacts (later files without required upstreams) are ignored.
   - Status `rejected` on any artifact forces the project back to the earliest failing stage after human review.
4. Emit the single primary state string (e.g. `briefed`, `architected`, `planned`).

State is never cached across sessions. Git is the only memory.

## Single-Stage Contract

Default behaviour is strictly one stage per invocation.

| Rule | Rationale |
|------|-----------|
| One agent runs | Keeps context window focused and decisions auditable |
| Agent writes only its primary artifact | Prevents scope creep and preserves single-responsibility |
| Driver stops after the agent finishes | Returns control to the human |
| Batching is opt-in only | Human must explicitly request multi-stage execution and every intermediate quality check must still pass |

## Explicit Human Override Paths

The human may, in a single message, request:

- **Re-run current stage** — agent is re-loaded; previous artifact is treated as draft for rework.
- **Advance after review** — human confirms the current artifact; driver proceeds to the next legal stage.
- **Rollback** — human changes status of an artifact (or deletes it); driver re-derives state from the earlier point.
- **Batch N stages** — only when the human names the exact sequence and accepts responsibility for intermediate checks.

The driver never initiates these overrides itself.

## Terminal States

| Derived State | Driver Action |
|---------------|---------------|
| `approved` | STOP. Master Design Plan is final. Downstream code generation is out of scope for DevOS. |
| `rejected` | STOP. Present the Auditor’s remediation requirements and wait for human decision on the remediation path. |
| Any other state with an open human gate | STOP and present the gate (see human-gates.md). |
| Any other state with no open gate | Load and execute the next legal agent. |

## Failure Modes the Driver Must Surface

- Missing project folder or missing `intake.md` on a brand-new project.
- Orphan artifacts that prevent clean state derivation.
- Contract violation detected by any agent (especially “never invent requirements”).
- Transition blocked by unresolved blocking open questions.
- Attempt to load an agent whose preconditions are not met.

In every failure mode the driver stops, reports the precise reason, and never silently continues.

## Relationship to Other Orchestration Files

- **agent-loader.md** — performs the actual load and execution once the driver has selected the stage.
- **transition-enforcer.md** — validates preconditions immediately before the agent is allowed to write.
- **human-gates.md** — defines the format and clearance protocol for every stop that requires human input.
