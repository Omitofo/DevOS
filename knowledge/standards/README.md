# knowledge/standards/

**Status:** Active  
**Authority:** core/contract.md · runtime/agents/tech-lead.md · runtime/agents/security-quality-auditor.md · runtime/templates/implementation-plan.md  
**Last review:** 2026-08-06  
**Confidence:** High (standards are normative conventions that agents and humans must follow)

Canonical conventions for naming, documentation, and testing.  
These files define the non-negotiable baseline that every project artifact, codebase, and agent output must satisfy unless an explicit, recorded exception is granted.

Standards are **not** optional style suggestions. They are the shared contract that keeps multi-agent and multi-human work coherent, reviewable, and maintainable over time.

## Purpose

Provide a single source of truth so that:

- Every project uses the same vocabulary for files, modules, APIs, tests, and documents.
- Documentation is discoverable, current, and structured the same way.
- Testing strategy, coverage expectations, and quality gates are predictable.
- The Security & Quality Auditor can enforce consistency without inventing rules per project.
- New agents or contributors can orient quickly by reading three files instead of reverse-engineering tribal knowledge.

## Files

| File | Responsibility |
|------|----------------|
| [naming.md](naming.md) | File, folder, code, API, database, environment, and commit naming conventions |
| [documentation.md](documentation.md) | Structure, ownership, freshness, and required artifacts for all documentation |
| [testing.md](testing.md) | Test pyramid, coverage expectations, naming, isolation, CI gates, and quality criteria |

## Usage Rules for Agents

1. Consult the relevant standard file(s) before producing or reviewing any artifact that involves names, documents, or tests.
2. Apply the conventions by default. Any deviation must be explicitly justified in the Architecture Blueprint, Implementation Plan, or a project-level ADR and flagged for the Security & Quality Auditor.
3. Prefer links to these files over copying rules into project folders.
4. When generating code, documentation, or test suites, enforce the naming and structural rules defined here.
5. Never invent project-specific naming schemes, documentation layouts, or testing philosophies without recording them as exceptions.
6. Standards describe *required* behaviour; they do not prescribe particular libraries or frameworks (those live in knowledge/technologies/).

## Relationship to Other Knowledge

- **patterns/** — patterns may reference naming or testing conventions when they affect implementation shape.
- **technologies/** — chosen stacks must be able to satisfy the testing and documentation standards.
- **design/** — design tokens, component names, and documentation of visual systems must follow naming and documentation rules.
- **classification/** — complexity band influences how rigorously certain testing layers are applied, but does not relax the baseline standards.
- **blueprints/** — starter structures already embed compliant naming and documentation skeletons.
- **runtime/templates/** — every template is expected to produce artifacts that already satisfy these standards.

## Maintenance

- Every file declares Status, Confidence, and Last review.
- Conflicting guidance is resolved by the Security & Quality Auditor with explicit rationale.
- Placeholders have been eliminated; the three core standards domains are fully specified.
- Revisit when team scale, tooling, or regulatory requirements materially change the cost of inconsistency.
