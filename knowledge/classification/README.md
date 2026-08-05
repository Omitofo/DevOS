# knowledge/classification/

**Status:** Active  
**Authority:** core/contract.md · runtime/agents/architect.md · runtime/templates/architecture-blueprint.md  
**Last review:** 2026-08-05  
**Confidence:** High (decision criteria are explicit and testable)

Reusable classification knowledge that agents consult when categorising a project.  
Classification decisions must be recorded in the Architecture Blueprint (§1 Classification) with links back to the specific criteria used.

## Purpose

Give the Architect (and any upstream/downstream agent that needs orientation) a shared vocabulary and clear decision rules for:

- What kind of product / site this is
- How complex the effort is likely to be
- Which rendering / delivery strategy fits
- Which broad architecture family is appropriate

Classification is never an end in itself. It exists so that later choices (blueprint selection, technology choices, work-package sizing, risk flags) remain consistent and traceable.

## Files

| File | Responsibility |
|------|----------------|
| [website-types.md](website-types.md) | Primary product / site category |
| [project-complexity.md](project-complexity.md) | Complexity band and sizing signals |
| [rendering-strategies.md](rendering-strategies.md) | SSR / SSG / CSR / hybrid / edge decisions |
| [architecture-selection.md](architecture-selection.md) | Architecture family and high-level style |

## Usage Rules for Agents

1. Consult the relevant file(s) before writing the Classification section of `architecture.md`.
2. Record the chosen class **and** the concrete criteria that justified it.
3. When two classes remain plausible, list both, state the decision criteria applied, and surface residual ambiguity as an open question if it blocks downstream work.
4. Prefer links to these files over copying criteria into project artifacts.
5. Never invent a new top-level class without flagging it as a knowledge-gap / exception.

## Maintenance

- Every file declares Status, Confidence, and Last review.
- Conflicting guidance is resolved by the Security & Quality Auditor with explicit rationale.
- Placeholders are no longer present; all four classification dimensions are fully specified.
