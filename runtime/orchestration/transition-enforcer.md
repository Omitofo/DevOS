# Transition Enforcer

**Status:** Active  
**Authority:** runtime/workflow/stage-transitions.md + state-machine.md + core/contract.md  
**Last review:** 2026-08-05

## Purpose

Validate every attempted stage transition before the agent is allowed to write.  
Enforce preconditions, detect blocking conditions, and reject illegal moves.  
The enforcer is the runtime realisation of the rules declared in stage-transitions.md.

No agent may proceed past a failed enforcement check.

## Enforcement Points

The enforcer runs at two moments:

1. **Pre-execution** — immediately after the agent is loaded and before it writes any file.
2. **Post-execution** — after the agent claims to have finished, to confirm the expected postcondition holds.

## Pre-Execution Checklist (must all pass)

| # | Check | Source of Truth |
|---|-------|-----------------|
| 1 | All required upstream artifacts exist | stage-transitions.md transition table |
| 2 | Every required upstream has status ≥ `draft` | state-machine.md derivation rules |
| 3 | No required upstream has status `rejected` | stage-transitions.md blocking conditions |
| 4 | No open *blocking* open questions remain unresolved (unless human has explicitly accepted them) | artifact Open Questions sections + human-gates.md |
| 5 | No prior Contract violation has been recorded against the project | core/contract.md + prior audit notes |
| 6 | The target stage is the immediate next legal stage (no skipping) | pipeline.md stage order |
| 7 | The agent being loaded is the owner of the target stage | agent-loader.md mapping |

If any check fails the enforcer:

- Halts the agent.
- Emits a structured blocking report (see format below).
- Returns control to the pipeline driver, which presents the block as a human gate.

## Post-Execution Checklist

| # | Check | Action on Failure |
|---|-------|-------------------|
| 1 | The agent’s primary artifact now exists | Treat the stage as incomplete; do not advance state |
| 2 | The artifact carries a valid status header | Reject the write for state derivation |
| 3 | The artifact’s Upstream field correctly lists required predecessors | Flag as traceability defect; may be escalated by Auditor |
| 4 | No orphan later-stage artifacts were created | Ignore them for state derivation; surface as anomaly |

## Blocking Report Format

When a transition is blocked the enforcer must produce a report that contains:

```markdown
# Transition Blocked

**Attempted transition:** <from-state> → <to-state>
**Agent:** <agent-name>
**Timestamp:** <ISO-8601 or session marker>

## Failed Checks
| Check | Detail |
|-------|--------|
| …     | …      |

## Required Actions
- Concrete steps the human (or a prior agent) must take before the transition can be retried.

## Open Questions (blocking)
- Any unresolved blocking questions that contributed to the failure.
```

The report is written into the conversation (and may be appended to a project-level notes file if one exists). It is never silently discarded.

## Special Cases

### Orphan Artifacts
An artifact that appears without its required upstreams is treated as invalid.  
The enforcer ignores it for both pre- and post-condition evaluation and surfaces an anomaly.

### Rollback
Rollback is a human action. The human changes the status of an artifact (or deletes it).  
On the next invocation the driver re-derives state; the enforcer simply evaluates the new derived state against the normal rules. The enforcer never deletes or rewrites earlier artifacts.

### Risk Acceptance
A human may accept residual risk on a quality gate.  
Such acceptance must be recorded in writing (normally inside the audit report or a dedicated risk-acceptance note).  
Only an explicit written acceptance allows the Auditor to PASS a previously failing gate. The enforcer treats the written acceptance as clearing the corresponding blocking condition.

### Contract Violations
Any detected violation of core/contract.md (especially “never invent requirements” or “never skip workflow stages”) is an immediate hard block.  
The Security & Quality Auditor is the final authority and may reject an entire package for transition or Contract violations.

## Relationship to Agents

Every agent definition in `runtime/agents/` restates the preconditions that apply to its own stage.  
The enforcer is the external, authoritative check; the agent’s own restatement is a convenience for the executing LLM.  
In case of conflict the enforcer (and ultimately the Auditor) wins.
