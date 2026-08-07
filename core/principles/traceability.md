# Traceability

**Status:** Canonical  
**Primary Contract rule:** 5  
**Last review:** 2026-08-07

## Statement

Every decision must be traceable.

Artifacts must contain:

- links to upstream artifacts,
- explicit assumptions (if any),
- open questions (if any).

A requirement, architectural choice, or design decision that cannot be traced back to an upstream source or a documented human confirmation is invalid.

## Why It Exists

Without traceability:

- later sessions cannot reconstruct why a decision was made,
- the Auditor cannot verify Contract compliance,
- the human cannot audit the reasoning that led to the Master Design Plan,
- Git ceases to be a reliable memory.

## Required Practices

1. **Upstream field** in the metadata header lists every artifact that this document depends on.
2. **Inline citations** or section-level references point to specific requirements, constraints, or prior decisions.
3. **Assumptions** section enumerates every inference that is not yet confirmed.
4. **Open questions** section enumerates every gap; blocking questions are marked as such.
5. When a human supplies a clarification in chat, the responsible agent updates the relevant artifact and cites the clarification (date + summary) so that Git captures it.

## Decision Criteria

| Situation | Required action |
|-----------|-----------------|
| A statement is copied or derived from an upstream artifact | Link to it. Prefer precise anchors when possible. |
| A statement is an inference | Place it under Assumptions and mark it pending confirmation. |
| A statement has no upstream source and is not a pure engineering deduction from already-traced premises | It is an invention → convert to an open question or remove. |
| Two upstream sources conflict | Surface both, mark the conflict, and do not silently choose. |
| An earlier decision is being reversed | Record the reversal, the reason, and the new upstream authority. |

## Examples of Violation

- “We selected event-sourced architecture” with no link to a requirement or constraint that justifies the complexity.
- Omitting the Assumptions / Open questions blocks because “there were none” (still declare “none”).
- Relying on conversational memory (“as we agreed last week”) without writing the agreement into an artifact.

## Examples of Correct Behaviour

- “Requirement R-17 (from requirements.md#r-17) drives the need for an audit log.”
- “Assumption A-3: Multi-region active-active is required. Source: inference from ‘global user base’ in Intake; pending confirmation.”
- “Open question Q-4 (blocking): What is the maximum acceptable RPO?”

## Relationship to Other Principles

- Directly enforces Contract rule 5.
- Makes **Never Invent** auditable.
- Makes **Git owns memory** operational.
