# knowledge/blueprints/

**Status:** Active (incremental)  
**Last review:** 2026-08-06

Reusable, opinionated starting architectures for common product categories.  
Agents consult these files by link; content is never copied into project folders.

## Purpose

Provide a coherent default shape, the high-leverage decision points that must be resolved early, technology-family guidance, required patterns, anti-patterns, and exact recording requirements for the Architecture Blueprint.

A blueprint is selected after classification (see knowledge/classification/website-types.md). It constrains and accelerates the Architect; it does not replace judgement.

## Current Coverage

| Blueprint | Status | Primary use |
|-----------|--------|-------------|
| [saas.md](saas.md) | Active | Multi-user products with accounts, tenancy, and usually billing |
| [landing-page.md](landing-page.md) | Active | Conversion / marketing focused sites |
| [ecommerce.md](ecommerce.md) | Active | Catalogue + cart + checkout + fulfilment |
| [dashboard.md](dashboard.md) | Active | Authenticated internal / operational tools |
| [documentation-site.md](documentation-site.md) | Active | Docs / knowledge bases / content sites |
| portfolio.md | Placeholder | Showcase / brochure |
| mobile-app.md | Placeholder | Native / hybrid mobile-first |

## Expected Relationships

- Referenced by the Architect agent when producing `architecture.md`.
- Consumed by Planner (orientation) and Tech Lead (implementation constraints).
- Cross-links into patterns/, technologies/, classification/, and standards/.

## Maintenance Rules

- Every active blueprint declares **Status**, **Confidence**, **Last review**, and **Primary consumers**.
- Recommendations are defaults with explicit decision points and anti-patterns; deviations must be justified in the project Architecture Blueprint.
- When a specialised recurring shape appears (e.g. multi-sided marketplace, heavy real-time collaboration), promote it to its own blueprint rather than overloading an existing one.
