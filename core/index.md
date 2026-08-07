# core/ — Index

**Status:** Canonical  
**Authority:** DEVOS_BOOTSTRAP_SPEC.md

Immutable foundation of DevOS.  
Agents and humans must treat every rule, gate, and lifecycle stage defined here as non-negotiable unless the external constitution is amended.

| File | Purpose | Primary Consumers |
|------|---------|-------------------|
| [README.md](README.md) | Human-oriented overview of the domain | Humans, onboarding agents |
| [constitution.md](constitution.md) | Authoritative pointer to the external bootstrap specification | All agents (load first) |
| [contract.md](contract.md) | The 11 inviolable rules | Every agent, every stage |
| [quality-gates.md](quality-gates.md) | Full definition and evaluation criteria for all Engineering Quality Gates | Security & Quality Auditor (primary), all upstream agents (awareness) |
| [artifact-lifecycle.md](artifact-lifecycle.md) | Birth, metadata, versioning, status transitions, and retirement of artifacts | All agents that write artifacts, orchestration |
| [principles/](principles/index.md) | Core engineering principles derived from the Contract | All agents, especially Planner, Analyst, Architect |

## Change Policy

Extremely conservative.  
Any modification requires:

1. An explicit amendment to the external `DEVOS_BOOTSTRAP_SPEC.md`.
2. A version bump of the bootstrap specification.
3. A migration note describing impact on existing projects.
4. An atomic update of the corresponding CORE files.

Silent or unilateral changes to CORE are constitutionally invalid.
