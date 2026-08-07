# Single Responsibility

**Status:** Canonical  
**Primary Contract rule:** 6  
**Last review:** 2026-08-07

## Statement

Every module has exactly one responsibility.

This applies to:

- folders and files inside the DevOS repository itself,
- components, services, and bounded contexts described in Architecture Blueprints,
- agents defined under `runtime/agents/`.

If a unit of work starts to serve two distinct purposes, it must be split.

## Why It Exists

Mixed responsibilities produce:

- artifacts that are hard to review and hard to evolve,
- agents that step on each other’s outputs,
- architectures that become untestable and unmaintainable.

The principle keeps both the DevOS system and the products it specifies coherent.

## Decision Criteria

| Situation | Required action |
|-----------|-----------------|
| A file or folder has two reasons to change | Split it. |
| An agent definition begins to produce outputs that belong to another stage | Move the responsibility to the correct agent; keep only cross-references. |
| An architectural module both stores data and renders UI | Separate the concerns into distinct modules with an explicit interface. |
| A knowledge document tries to be both a pattern and a technology evaluation | Split into the appropriate knowledge sub-domains. |
| A temporary convenience seems to justify mixing concerns | Resist. Record the temptation as a note if useful, but do not mix. |

## Examples of Violation

- An Architecture Blueprint that also contains full visual component specifications (belongs in Visual Blueprint).
- A single agent file that both creates the Project Brief and performs the final audit.
- A `core/` file that begins to contain technology recommendations.

## Examples of Correct Behaviour

- `runtime/agents/planner.md` owns only the Project Brief.
- `knowledge/patterns/authentication.md` answers “how do we establish identity?” and links to authorization for “what may they do?”.
- Project folders contain one artifact per lifecycle stage.

## Relationship to Other Principles

- Supports **Traceability** (clear ownership makes links meaningful).
- Supports **Maintainability** Quality Gate.
- Reinforced by the domain separation of CORE / KNOWLEDGE / RUNTIME / PROJECTS.
