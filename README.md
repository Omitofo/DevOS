# DevOS

**Specification-driven engineering operating system**

DevOS transforms ideas into implementation-ready engineering specifications.

It does **not** generate code, deploy systems, or run CI/CD.  
Its sole product is the **Master Design Plan**.

## Quick Start

1. Read [START.md](START.md)
2. For a new project, prepare an Intake document (see external [INTAKE_TEMPLATE.md](https://github.com/Omitofo/devos-bootstrap/blob/master/INTAKE_TEMPLATE.md))
3. Invoke with the minimal prompt defined in the constitution

## Repository Structure

| Domain | Purpose |
|--------|---------|
| [core/](core/) | Immutable principles, contracts, quality gates |
| [knowledge/](knowledge/) | Reusable engineering knowledge, patterns, blueprints |
| [runtime/](runtime/) | Agents, workflow, orchestration |
| [projects/](projects/) | Live project workspaces |

## Constitution

The authoritative source of truth lives **outside** this repository:

→ **[DevOS Bootstrap Spec](https://github.com/Omitofo/devos-bootstrap)**  
(`DEVOS_BOOTSTRAP_SPEC.md` + `INTAKE_TEMPLATE.md`, versioned independently)

This repository is the living system generated from that constitution.

## Core Contract (Summary)

1. Never invent requirements  
2. Never skip workflow stages  
3. Never skip Engineering Quality Gates  
4. Never generate implementation before the Master Design Plan exists  
5. Every decision must be traceable  
6. Every module has exactly one responsibility  
7. Prefer links over duplication  
8. Prefer placeholders over assumptions  
9. Human owns the vision  
10. AI owns engineering reasoning  
11. Git owns memory  

See [core/contract.md](core/contract.md) for the full text.
