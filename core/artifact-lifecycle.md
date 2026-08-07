# Artifact Lifecycle

**Status:** Canonical  
**Authority:** DEVOS_BOOTSTRAP_SPEC.md §6  
**Last review:** 2026-08-07  
**Primary consumers:** All agents that write artifacts, runtime/orchestration

This document defines how every project artifact is born, versioned, transitioned, and retired.  
It is binding on every agent and on the orchestration layer.

---

## Canonical Artifact Sequence

| # | Artifact | Filename (under projects/<name>/) | Producing Stage / Agent | Required Upstream |
|---|----------|-----------------------------------|-------------------------|-------------------|
| 0 | Intake | `intake.md` | Human | — |
| 1 | Project Brief | `brief.md` | Planner | intake.md |
| 2 | Requirements | `requirements.md` | Analyst | brief.md, intake.md |
| 3 | Architecture Blueprint | `architecture.md` | Architect | requirements.md, brief.md |
| 4 | User Journey | `journeys.md` | UX | requirements.md, brief.md |
| 5 | Visual Blueprint | `visual.md` | UI | journeys.md, requirements.md |
| 6 | Implementation Plan | `implementation.md` | Tech Lead | architecture.md, requirements.md, journeys.md, visual.md |
| 7 | Audit Report | `audit.md` | Security & Quality Auditor | All prior + quality gates |
| 8 | Master Design Plan | `master-design-plan.md` | Security & Quality Auditor | All prior + successful (or risk-accepted) audit |

The Master Design Plan is the sole approved, implementation-ready package.  
It may package or reference the preceding artifacts; it never replaces the need for the audit.

---

## Required Metadata Header (Every Artifact)

Every artifact **must** begin with a metadata block of the following form (or a strict superset):

```markdown
# <Artifact Title>

**Version:** <semver or sequential, e.g. 0.1>  
**Status:** draft | in-review | approved | superseded | rejected  
**Upstream:** <comma-separated list of source artifacts or “none”>  
**Assumptions:** <list or “none”>  
**Open questions:** <list; mark blocking ones explicitly>  
**Last updated:** <ISO date>  
**Authoring agent / human:** <identifier>
```

Additional fields (Confidence, Reviewers, Risk-acceptance links, etc.) may be added by specific templates but must not remove the required fields.

---

## Status Definitions

| Status | Meaning | Who may set it |
|--------|---------|----------------|
| `draft` | Work in progress; not yet ready for human or downstream review | Producing agent |
| `in-review` | Ready for human inspection or for the next agent to consume | Producing agent or human |
| `approved` | Accepted as input for downstream stages or (for Master Design Plan) as final | Human, or Auditor for the final plan |
| `superseded` | Replaced by a later version; kept for audit trail | Any agent or human that creates the replacement |
| `rejected` | Explicitly rejected; must not be used as upstream | Human or Auditor |

Only the Security & Quality Auditor (or the human) may set the Master Design Plan to `approved`.  
Upstream artifacts should normally reach at least `in-review` before the next stage begins; orchestration may enforce this.

---

## Versioning Rules

- Artifacts are versioned via Git. The `Version` field inside the file is informational and must be updated on every meaningful change.
- When a later stage invalidates or substantially revises an earlier artifact, the earlier artifact is **not** deleted. Its status is set to `superseded` and a pointer to the replacement is added.
- Minor clarifications may keep the same major version; structural or requirement-level changes increment the version and are recorded in the project history.
- The Audit Report and Master Design Plan must reference the exact versions (or Git commits) of the artifacts they evaluated.

---

## Birth Rules

1. An artifact may be created only by its owning agent (or by the human for Intake).
2. The agent must load and respect the corresponding template under `runtime/templates/`.
3. The agent must populate the required metadata header before writing any body content.
4. The agent must not invent content that violates the Contract (especially “Never invent requirements”).
5. If required upstream information is missing, the agent records blocking open questions and leaves the dependent sections as explicit placeholders.

---

## Transition Rules

- A stage may begin only when its required upstream artifacts exist and are not in `rejected` status.
- Orchestration (see `runtime/orchestration/`) is responsible for enforcing legal transitions.
- Human intervention may insert additional review cycles; these do not alter the canonical sequence but may add intermediate status changes.
- After the Master Design Plan is approved, the project is considered complete from DevOS’s perspective. Further changes require a new Intake or an explicit amendment process.

---

## Retirement & Audit Trail

- Artifacts are never silently deleted.
- Superseded and rejected artifacts remain in the project folder (or are moved to an `archive/` subfolder if the human prefers) so that the full decision history is reconstructible from Git.
- When a project is abandoned, the entire folder is retained; only the human may decide to remove it from the working tree.
- The Audit Report must list every artifact version that was examined.

---

## Cross-Cutting Requirements for Every Artifact

Regardless of stage, every artifact must satisfy:

1. **Traceability** — Links to upstream sources (Contract rule 5).
2. **Explicit assumptions** — Any inference is marked and listed.
3. **Open questions** — Missing information becomes a question, never a silent default.
4. **No implementation leakage** — No production code, infrastructure scripts, or runnable prototypes (Contract rule 4).
5. **Single responsibility** — The artifact stays within the scope of its stage (Contract rule 6).

Templates under `runtime/templates/` encode these requirements as concrete section structures. Agents must follow the templates.
