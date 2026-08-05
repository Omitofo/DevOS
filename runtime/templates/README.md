# runtime/templates/

Canonical skeletons for every artifact type produced by the DevOS pipeline.

Agents **must** start from the matching template and expand it according to the Output Structure Expectations declared in their agent definition. The templates already contain:

- Required header fields (Version, Status, Upstream, Assumptions, Open questions)
- Canonical section headings and tables
- Traceability blocks
- Status and versioning conventions

Do not invent additional top-level sections unless the human explicitly requests them. Prefer filling the existing structure and adding rows to the provided tables.

## Contents

See [index.md](index.md) for the full mapping of templates → artifacts → agents.

## Status Conventions

| Status       | Meaning                                      | Typical producer          |
|--------------|----------------------------------------------|---------------------------|
| draft        | First complete version from the agent        | Any stage agent           |
| in-review    | Ready for human or auditor scrutiny          | Any stage agent           |
| approved     | Human or auditor has accepted the artifact   | Final gate / human        |
| complete     | Audit report finished (PASS or FAIL)         | Security & Quality Auditor |
| rejected     | Failed quality gates; return to earlier stage| Security & Quality Auditor |

## Authority

These templates implement the artifact requirements defined in:

- core/contract.md
- core/artifact-lifecycle.md
- core/quality-gates.md
- The individual agent Output Structure Expectations

They are living documents. When an agent’s expected structure changes, the corresponding template must be updated in the same change set.
