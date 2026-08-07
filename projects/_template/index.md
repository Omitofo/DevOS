# Project Template — Index

**Status:** Canonical scaffold  
**Authority:** core/artifact-lifecycle.md · runtime/workflow/pipeline.md

Copy this entire `_template` folder to start a new project.  
Rename the copy to the project working title (kebab-case recommended).

## Artifact Map

| Artifact | File | Stage / Agent | Upstream |
|----------|------|---------------|----------|
| Intake | [intake.md](intake.md) | Human | — |
| Project Brief | [brief.md](brief.md) | Planner | intake.md |
| Requirements | [requirements.md](requirements.md) | Analyst | brief.md, intake.md |
| Architecture Blueprint | [architecture.md](architecture.md) | Architect | requirements.md, brief.md |
| User Journey | [journeys.md](journeys.md) | UX | requirements.md, brief.md |
| Visual Blueprint | [visual.md](visual.md) | UI | journeys.md, requirements.md |
| Implementation Plan | [implementation.md](implementation.md) | Tech Lead | architecture.md, requirements.md, journeys.md, visual.md |
| Audit Report | [audit.md](audit.md) | Security & Quality Auditor | All prior |
| Master Design Plan | [master-design-plan.md](master-design-plan.md) | Security & Quality Auditor | All prior + successful audit |

## Usage Rules

1. Fill files **only** in pipeline order. Never invent a later artifact before its upstreams exist.
2. Every file must begin with the required metadata header (see each template).
3. Status values: `draft` → `in-review` → `approved` (or `rejected` / `superseded`).  
   Only the Security & Quality Auditor may set `approved` on the Master Design Plan.
4. Preserve the Traceability section in every artifact. Empty tables are acceptable while the document is still draft; they must be complete before `in-review`.
5. Do not delete or rename these files. Appendices, if required, may be added as additional Markdown files and referenced from the Master Design Plan.

## Starting Checklist

- [ ] Folder copied and renamed
- [ ] Human Intake placed in `intake.md`
- [ ] Planner invoked → `brief.md`
- [ ] Analyst → `requirements.md`
- [ ] Architect → `architecture.md`
- [ ] UX → `journeys.md`
- [ ] UI → `visual.md`
- [ ] Tech Lead → `implementation.md`
- [ ] Security & Quality Auditor → `audit.md` + `master-design-plan.md`
