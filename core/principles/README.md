# Core Principles

**Status:** Canonical  
**Authority:** DEVOS_BOOTSTRAP_SPEC.md + core/contract.md  
**Last review:** 2026-08-07

These principles are derived directly from the DevOS Contract and the foundational philosophy of the constitution.

They are immutable under normal evolution. Changes require constitutional amendment.

## Purpose

Principles translate the abstract Contract rules into concrete guidance that agents and humans can apply while writing artifacts, designing modules, and evolving the DevOS repository itself.

## Contents

| File | Principle | Primary Contract Rule |
|------|-----------|-----------------------|
| [never-invent.md](never-invent.md) | Never invent requirements | 1, 8 |
| [single-responsibility.md](single-responsibility.md) | Every module has exactly one responsibility | 6 |
| [traceability.md](traceability.md) | Every decision must be traceable | 5 |
| [incremental-development.md](incremental-development.md) | Prefer placeholders; develop incrementally | 8, 2.3 of constitution |

## Usage

- Agents should load the relevant principle files early in their process steps.
- When a decision feels borderline, the principle documents supply the decision criteria.
- Principles never override the Contract; they illuminate it.
