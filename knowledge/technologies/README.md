# knowledge/technologies/

**Status:** Active  
**Authority:** core/contract.md · runtime/agents/architect.md · runtime/agents/tech-lead.md · runtime/templates/architecture-blueprint.md  
**Last review:** 2026-08-06  
**Confidence:** High (selection criteria are explicit, decision-oriented, and scoped to common engineering contexts)

Living evaluations of technology stacks and selection criteria.  
Agents consult these files when choosing languages, frameworks, data stores, deployment targets, observability stacks, and security tooling.

Technologies knowledge is **not** a catalogue of every available tool. It is a decision framework: when a family of technologies is appropriate, what trade-offs it carries, which anti-patterns to avoid, and how the choice must be recorded in the Architecture Blueprint.

## Purpose

Give Architect, Tech Lead, and Security & Quality Auditor a shared, versioned vocabulary for technology decisions so that:

- The same problem class tends to receive the same technology family unless an explicit exception is justified.
- Trade-offs (operational complexity, team skill, latency, cost, lock-in) are visible and traceable.
- Novel or high-risk technology choices are flagged rather than silently invented.
- Downstream work packages and implementation plans remain consistent with the chosen stack.

## Files

| File | Responsibility |
|------|----------------|
| [frontend.md](frontend.md) | UI frameworks, rendering models, state, and client tooling |
| [backend.md](backend.md) | Application runtimes, API frameworks, and service styles |
| [databases.md](databases.md) | Relational, document, key-value, search, and specialised stores |
| [deployment.md](deployment.md) | Hosting models, containers, platforms, and release strategies |
| [observability.md](observability.md) | Logging, metrics, tracing, and alerting stacks |
| [security-tooling.md](security-tooling.md) | Auth libraries, secret management, scanning, and hardening tools |

## Usage Rules for Agents

1. Consult the relevant technology file(s) before recording a Technology Decision in the Architecture Blueprint or Implementation Plan.
2. Record the chosen family or concrete stack **and** the concrete criteria that justified it (link back to the relevant section).
3. When two approaches remain plausible, list both, state the decision criteria applied, and surface residual ambiguity as an open question if it blocks downstream work.
4. Prefer links to these files over copying criteria into project artifacts.
5. Never invent a new top-level technology family without flagging it as a knowledge-gap / exception.
6. Technology choices must remain consistent with classification results (complexity band, rendering strategy, architecture family) and with selected patterns.

## Relationship to Other Knowledge

- **classification/** — constrains which technology families are even in scope.
- **patterns/** — once a technology family is chosen, patterns guide how it is applied (auth, caching, multi-tenancy, etc.).
- **design/** — visual and interaction constraints that may influence frontend choices.
- **standards/** — naming, documentation, and testing conventions that the chosen stack must satisfy.
- **blueprints/** — opinionated starting points that already embed compatible technology defaults.

## Maintenance

- Every file declares Status, Confidence, and Last review.
- Conflicting guidance is resolved by the Security & Quality Auditor with explicit rationale.
- Placeholders are no longer present; all six technology dimensions are fully specified.
- Revisit when major platform shifts occur (new mainstream runtimes, significant deprecations, or material changes in operational cost models).
