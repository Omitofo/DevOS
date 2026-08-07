# Never Invent Requirements

**Status:** Canonical  
**Primary Contract rules:** 1, 8  
**Last review:** 2026-08-07

## Statement

Every functional, non-functional, and constraint statement must be traceable to either:

- explicit user input (Intake or later human clarification), or  
- a documented, justified inference that is clearly marked as such and presented for human confirmation.

Placeholders and open questions are preferred over assumptions.

## Why It Exists

Language models are trained to be helpful and complete.  
That helpfulness becomes a liability when the model silently fills gaps with plausible but unauthorised requirements.  
The resulting Master Design Plan then encodes decisions the human never made and may not even notice until implementation.

## Decision Criteria

| Situation | Required action |
|-----------|-----------------|
| Information is present in the Intake or a confirmed human message | Use it. Cite the source. |
| Information is a reasonable inference from explicit statements | Record it under **Assumptions** or **Inferences pending confirmation**. Never promote it to a requirement. |
| Information is missing and required for a complete artifact | Record an **Open question**. Prefer a blocking open question over invention. |
| A common industry practice seems applicable | Link to a knowledge pattern if relevant; still treat the *application* of that pattern to this project as an inference unless the human confirmed it. |
| The human later confirms an inference | Move it from Assumptions to Requirements (or the appropriate section) and cite the confirmation. |

## Examples of Violation

- Adding “Users must be able to reset their password via email” when the Intake never mentioned authentication recovery.
- Setting a performance budget of “< 200 ms p95” because “that is standard” without recording the number as an inference.
- Choosing a particular database engine inside the Architecture because the agent “knows it is a good fit”.

## Examples of Correct Behaviour

- “Open question (blocking): What authentication methods must be supported?”
- “Assumption (pending confirmation): Because the Intake describes a multi-tenant SaaS, we infer that tenant isolation is required. Confirmation needed before this becomes a requirement.”
- Leaving a section titled “Non-functional budgets” as a placeholder that lists the missing dimensions.

## Relationship to Other Principles

- Supports **Traceability** (every statement must point somewhere).
- Reinforced by **Incremental Development** (placeholders are the sanctioned response to incomplete information).
