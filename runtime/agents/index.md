# runtime/agents/ — Index

**Status:** Active  
**Load order:** README.md → individual agent file matching the next legal stage.

| Agent | Stage # | Primary Output | Artifact Filename | Template | File |
|-------|---------|----------------|-------------------|----------|------|
| Planner | 1 | Project Brief | brief.md | [project-brief.md](../templates/project-brief.md) | [planner.md](planner.md) |
| Analyst | 2 | Requirements | requirements.md | [requirements.md](../templates/requirements.md) | [analyst.md](analyst.md) |
| Architect | 3 | Architecture Blueprint | architecture.md | [architecture-blueprint.md](../templates/architecture-blueprint.md) | [architect.md](architect.md) |
| UX | 4 | User Journey | journeys.md | [user-journey.md](../templates/user-journey.md) | [ux.md](ux.md) |
| UI | 5 | Visual Blueprint | visual.md | [visual-blueprint.md](../templates/visual-blueprint.md) | [ui.md](ui.md) |
| Tech Lead | 6 | Implementation Plan | implementation.md | [implementation-plan.md](../templates/implementation-plan.md) | [tech-lead.md](tech-lead.md) |
| Security & Quality Auditor | 7 | Master Design Plan (approved) or rejection | audit.md + master-design-plan.md | [master-design-plan.md](../templates/master-design-plan.md) | [security-quality-auditor.md](security-quality-auditor.md) |

## Workflow Cross-References

- Pipeline & stage order: [../workflow/pipeline.md](../workflow/pipeline.md)
- State derivation: [../workflow/state-machine.md](../workflow/state-machine.md)
- Preconditions / postconditions / blocking rules: [../workflow/stage-transitions.md](../workflow/stage-transitions.md)
