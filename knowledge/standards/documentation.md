# Documentation

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** Tech Lead, Architect, Security & Quality Auditor, all agents that produce or consume artifacts

Canonical requirements for the structure, ownership, freshness, and discoverability of documentation across every DevOS project.

Documentation is a first-class deliverable, not an afterthought. Agents and humans are expected to keep it accurate; stale or missing documentation is treated as a quality defect.

## Core Principles

1. **Single source of truth** — each fact lives in exactly one place; everything else links to it.
2. **Audience-aware** — distinguish documents written for agents, for implementers, for operators, and for external stakeholders.
3. **Living but dated** — every document declares its last-review date and owner; undated documents are considered untrusted.
4. **Progressive disclosure** — high-level overviews first; deep detail linked, never forced on the reader.
5. **Executable where possible** — prefer diagrams, tables, and examples that can be validated over pure prose.

## Required Project-Level Documents

Every non-trivial project must maintain the following (names may be adjusted only with recorded justification):

| Document | Purpose | Typical location | Owner |
|----------|---------|------------------|-------|
| **README** | Entry point: what the project is, how to run it, where to find everything else | repository root | Tech Lead |
| **Architecture Blueprint** | System context, major decisions, technology choices, NFR mapping | `docs/` or runtime template output | Architect |
| **Implementation Plan** | Phased delivery, task breakdown, dependencies | `docs/` or runtime template output | Tech Lead |
| **ADRs (Architecture Decision Records)** | Significant decisions with context, options, and consequences | `docs/adr/` or equivalent | Architect / Tech Lead |
| **API / Contract documentation** | Public and internal interfaces | OpenAPI/AsyncAPI files or equivalent + short overview | Tech Lead |
| **Runbook / Operations notes** | How to deploy, monitor, recover, and escalate | `docs/ops/` or equivalent | Tech Lead + operators |
| **Changelog** | User- and operator-visible changes | `CHANGELOG.md` or release notes | Tech Lead |

Smaller projects (complexity band S) may collapse some of these into the README, but the *information* must still exist and be current.

## Document Structure Conventions

### Markdown files

- Start with a clear H1 title.
- Immediately after the title, include a metadata block (Status, Owner or Primary consumers, Last review, Confidence when applicable).
- Use hierarchical headings; do not skip levels.
- Prefer tables for structured comparisons and decision matrices.
- Keep paragraphs short; use bullet lists for steps or criteria.
- Link liberally to other knowledge files and project artifacts; never duplicate content.
- End with a short “Related” or “See also” section when the document participates in a larger web.

### Architecture Decision Records (ADRs)

Use a lightweight, consistent template:

```markdown
# ADR-NNN: Title in sentence case

**Status:** Proposed | Accepted | Deprecated | Superseded by ADR-XXX  
**Date:** YYYY-MM-DD  
**Deciders:** …

## Context
## Decision
## Consequences
## Alternatives considered
```

Number ADRs sequentially. Never rewrite history; supersede instead.

### README minimum contents

1. One-paragraph purpose statement.
2. Quick-start (prerequisites + commands to run locally or in the primary environment).
3. Link map to the other required documents.
4. Status / maturity badge or short note if the project is not production-ready.
5. Contact or ownership information.

## Freshness & Ownership Rules

- Every document must declare a **Last review** date (ISO format).
- Documents older than the project’s defined review cadence (default: 90 days for active projects) are considered stale and must be re-validated or marked deprecated.
- Ownership is explicit: either a named role (Architect, Tech Lead) or a concrete person.
- When an agent updates a related artifact, it must also update (or explicitly leave unchanged with rationale) the linked documentation.
- Deleted or superseded documents are moved to an archive location or clearly marked; silent deletion is forbidden.

## What Must Never Be Documented in the Repo

- Secrets, credentials, or private keys.
- Personal data that would violate privacy regulations.
- Temporary debugging notes that have no lasting value (use issues or PR comments instead).

## Agent-Specific Expectations

- When producing any runtime template artifact (Architecture Blueprint, Implementation Plan, UI Specification, etc.), ensure the output already satisfies the structural and metadata conventions above.
- Prefer linking to knowledge/standards/, knowledge/patterns/, and knowledge/technologies/ over restating rules.
- If a required document is missing, the agent must either create a compliant stub or raise the gap to the human gate / Security & Quality Auditor.

## Anti-Patterns

- “Documentation will be written later.”
- Long prose without headings, tables, or links.
- Multiple conflicting sources of truth for the same decision.
- READMEs that only contain a project title and a license.
- ADRs that record a decision without context or consequences.
- Copy-pasting large sections of knowledge files into project docs.

## Recording Requirements

In the Implementation Plan or project kickoff checklist:

- Confirmation that the required document set will be produced and kept current.
- Chosen location and naming for ADRs and ops docs.
- Review cadence if different from the default.

## Related Standards & Knowledge

- [naming.md](naming.md) — how documents and their internal identifiers are named
- [testing.md](testing.md) — documentation of test strategy and coverage expectations
- runtime/templates/* — the concrete templates that must emit compliant documentation
- knowledge/design/* and knowledge/patterns/* — domain knowledge that documentation frequently references
