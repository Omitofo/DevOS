# runtime/templates/ — Index

**Status:** Active  
**Purpose:** Canonical skeletons that agents expand into project artifacts.  
Every template encodes the exact header conventions, required sections, tables, and traceability blocks declared by the corresponding agent.

| Template | Artifact Filename | Used by | Agent File |
|----------|-------------------|---------|------------|
| [project-brief.md](project-brief.md) | brief.md | Planner | [planner.md](../agents/planner.md) |
| [requirements.md](requirements.md) | requirements.md | Analyst | [analyst.md](../agents/analyst.md) |
| [architecture-blueprint.md](architecture-blueprint.md) | architecture.md | Architect | [architect.md](../agents/architect.md) |
| [user-journey.md](user-journey.md) | journeys.md | UX | [ux.md](../agents/ux.md) |
| [visual-blueprint.md](visual-blueprint.md) | visual.md | UI | [ui.md](../agents/ui.md) |
| [implementation-plan.md](implementation-plan.md) | implementation.md | Tech Lead | [tech-lead.md](../agents/tech-lead.md) |
| [audit-report.md](audit-report.md) | audit.md | Security & Quality Auditor | [security-quality-auditor.md](../agents/security-quality-auditor.md) |
| [master-design-plan.md](master-design-plan.md) | master-design-plan.md | Security & Quality Auditor (on approval only) | [security-quality-auditor.md](../agents/security-quality-auditor.md) |

## Common Conventions

- **Header fields (required on every artifact):** Version, Status, Upstream, Assumptions, Open questions.
- **Status values:** `draft` → `in-review` → `approved` (or `complete` for audit reports). Master Design Plan starts at `approved`.
- **Versioning:** Start at `0.1`. Increment on substantive revision. Master Design Plan uses `1.0` on first approval.
- **Traceability block:** Mandatory. Every substantive statement must link to an upstream source or be explicitly marked as an inference pending confirmation.
- **Tables:** Prefer the column layouts shown in each template; they match the agent output-structure expectations exactly.

## Workflow Cross-References

- Pipeline & stage order: [../workflow/pipeline.md](../workflow/pipeline.md)
- Agent definitions: [../agents/index.md](../agents/index.md)
- Quality gates: [../../core/quality-gates.md](../../core/quality-gates.md)
