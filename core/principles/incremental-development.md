# Incremental Development

**Status:** Canonical  
**Primary Contract rule:** 8  
**Constitutional source:** DEVOS_BOOTSTRAP_SPEC.md §2.3  
**Last review:** 2026-08-07

## Statement

DevOS is intentionally developed incrementally.

The objective is never to complete the entire repository (or an entire Master Design Plan) in one interaction.  
Every interaction must improve one isolated part while preserving the integrity of the whole system.

Whenever a module or section cannot be completed with high confidence:

1. Create a documented placeholder.
2. The placeholder must state:
   - its future purpose,
   - its expected responsibilities,
   - its expected relationships to other modules,
   - the future artifact or content that will eventually replace it.

This keeps the repository (and every project) internally consistent at every stage of evolution.

## Why It Exists

Attempting to specify everything at once produces:

- invented content (violating Contract rule 1),
- shallow coverage of important topics,
- brittle documents that must be rewritten when new information arrives.

Incremental development with explicit placeholders keeps both DevOS itself and the projects it processes honest and evolvable.

## Decision Criteria

| Situation | Required action |
|-----------|-----------------|
| Confidence in a section is low | Emit a placeholder with the four required statements above. |
| A whole knowledge file or agent is not yet ready | Leave a placeholder file that follows the same rules; do not omit the file from the index. |
| New information arrives that fills a placeholder | Replace the placeholder in a focused change; update version and status. |
| A human asks for a complete system in one shot | Still proceed incrementally; surface the partial nature and the remaining placeholders. |
| A later stage discovers that an earlier placeholder is now resolvable | Update the earlier artifact (or record the resolution in the current one with a back-link). |

## Placeholder Template (Recommended)

```markdown
## <Section or Module Name> (Placeholder)

**Status:** Placeholder  
**Future purpose:** …  
**Expected responsibilities:** …  
**Expected relationships:** …  
**Will be replaced by:** … (file, section, or future work item)

Open questions that block completion:
- …
```

## Examples of Violation

- Inventing a full technology evaluation because the technologies/ folder “looked empty”.
- Writing a detailed Implementation Plan for components whose requirements are still open questions.
- Deleting a placeholder instead of filling it, thereby losing the record of what was intended.

## Examples of Correct Behaviour

- Shipping a knowledge pattern that covers the core decision points and leaves advanced edge cases as placeholders.
- An Architecture Blueprint that contains a clear “Open Questions / Placeholders” section listing unresolved module boundaries.
- Evolving DevOS itself folder-by-folder, each update delivered as a coherent, reviewable increment.

## Relationship to Other Principles

- Operationalises **Prefer placeholders over assumptions**.
- Protects **Never Invent Requirements**.
- Makes large systems evolvable without violating **Single Responsibility** or **Traceability**.
