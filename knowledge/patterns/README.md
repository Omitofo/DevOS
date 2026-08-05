# knowledge/patterns/

**Status:** Active  
**Authority:** core/contract.md · runtime/agents/architect.md · runtime/agents/tech-lead.md · runtime/templates/architecture-blueprint.md  
**Last review:** 2026-08-05  
**Confidence:** High (patterns are decision-oriented and explicitly scoped)

Reusable solution patterns for recurring engineering problems.  
Agents consult these files when selecting mechanisms for authentication, data access, real-time behaviour, payments, multi-tenancy, and related concerns.

Patterns are **not** implementation code. They are decision frameworks: when to choose an approach, what trade-offs it carries, which anti-patterns to avoid, and how the choice must be recorded in the Architecture Blueprint.

## Purpose

Give Architect, Tech Lead, and Security & Quality Auditor a shared, versioned vocabulary for common technical decisions so that:

- The same problem is solved the same way across projects unless an explicit exception is justified.
- Trade-offs are visible and traceable.
- Novel or high-risk approaches are flagged rather than silently invented.

## Files

| File | Responsibility |
|------|----------------|
| [authentication.md](authentication.md) | Identity verification approaches and session models |
| [authorization.md](authorization.md) | Access-control models (RBAC, ABAC, ReBAC, etc.) |
| [api-design.md](api-design.md) | API style, versioning, error contracts, and evolution |
| [caching.md](caching.md) | Caching layers, invalidation, and consistency |
| [forms.md](forms.md) | Form handling, validation, and progressive enhancement |
| [file-uploads.md](file-uploads.md) | Upload flows, storage, scanning, and size limits |
| [real-time.md](real-time.md) | WebSockets, SSE, polling, and event-driven UI |
| [search.md](search.md) | Full-text, faceted, and semantic search patterns |
| [payments.md](payments.md) | Payment orchestration, webhooks, and reconciliation |
| [multi-tenancy.md](multi-tenancy.md) | Tenant isolation models and data partitioning |

## Usage Rules for Agents

1. Consult the relevant pattern file(s) before recording a technical decision in the Architecture Blueprint or Implementation Plan.
2. Record the chosen approach **and** the concrete criteria that justified it (link back to the pattern section).
3. When two approaches remain plausible, list both, state the decision criteria applied, and surface residual risk if the choice is irreversible or high-cost.
4. Prefer links to these files over copying guidance into project artifacts.
5. Never invent a novel pattern without flagging it as a knowledge-gap / exception and routing it through the Security & Quality Auditor.
6. Patterns describe *what* and *why*; they do not prescribe exact library versions or code.

## Maintenance

- Every file declares Status, Confidence, and Last review.
- Conflicting guidance is resolved by the Security & Quality Auditor with explicit rationale.
- Placeholders have been eliminated; all ten core patterns are fully specified.
