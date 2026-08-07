# CORE

**Status:** Canonical  
**Authority:** DEVOS_BOOTSTRAP_SPEC.md (external constitution)  
**Change policy:** Extremely conservative — requires explicit constitutional amendment and a migration note

## Purpose

Immutable foundation of DevOS.  
Everything else in the repository (knowledge, runtime, projects) is subordinate to the rules defined here.

CORE exists so that:

- Every agent and every human session shares the same inviolable rules.
- Quality gates cannot be silently weakened.
- The artifact lifecycle remains stable across versions of DevOS.
- Engineering principles are explicit, not tribal knowledge.

## Contains

| File / Folder | Responsibility |
|---------------|----------------|
| [constitution.md](constitution.md) | Pointer to the external authoritative bootstrap specification |
| [contract.md](contract.md) | The 11 inviolable rules of DevOS |
| [quality-gates.md](quality-gates.md) | Full definitions, evidence requirements, and evaluation criteria for every Engineering Quality Gate |
| [artifact-lifecycle.md](artifact-lifecycle.md) | Birth, versioning, status transitions, and retirement of every project artifact |
| [principles/](principles/index.md) | Core engineering principles derived from the Contract |

## Does NOT Contain

- Project-specific content
- Technology recommendations or stack opinions
- Agent definitions or workflow logic
- Knowledge that evolves with industry practice
- Any file that an ordinary knowledge or runtime maintainer may edit

## Ownership & Change Rules

- **Owner:** The constitution itself.
- **Who may propose a change:** Only the human owner of the bootstrap specification.
- **How a change is made:**
  1. Amend the external `DEVOS_BOOTSTRAP_SPEC.md` and increment its version.
  2. Record a migration note describing the exact impact on existing projects and agents.
  3. Update the corresponding CORE files in a single, atomic commit.
  4. Never edit CORE files in isolation from the bootstrap specification.

Any change that weakens a Contract rule, removes a Quality Gate, or short-circuits the artifact lifecycle is invalid by definition.

## Relationship to Other Domains

- **runtime/** agents load and obey the Contract and Quality Gates.
- **knowledge/** may be referenced by agents but never overrides CORE.
- **projects/** artifacts must satisfy the lifecycle and the gates; they never redefine them.
