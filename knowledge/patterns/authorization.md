# Authorization

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, Tech Lead, Security & Quality Auditor

Access-control decisions after authentication.  
Answers *what may this principal do?* under the current context (tenant, resource, environment).

## Core Models

| Model | Description | Best fit |
|-------|-------------|----------|
| **RBAC** (Role-Based) | Permissions are assigned to roles; principals receive roles. | Most business applications with stable role sets |
| **ABAC** (Attribute-Based) | Decisions evaluated from attributes of principal, resource, action, and environment. | Fine-grained or dynamic policy needs |
| **ReBAC** (Relationship-Based) | Permissions derived from relationships in a graph (e.g. “owner of”, “member of”). | Collaboration, social, document-sharing products |
| **ACL** | Per-resource lists of principals and allowed actions. | Simple resources with few sharers; often combined with RBAC |

## Preference Guidance

**Prefer RBAC when**
- Roles are relatively stable and map cleanly to organisational or product concepts (Admin, Editor, Viewer, Billing).
- The majority of decisions can be expressed as “role X may perform action Y on resource type Z”.
- Complexity band is S–L and auditability of role assignments is important.

**Prefer ABAC when**
- Policies depend on dynamic attributes (time of day, device posture, data classification, geographic region).
- Fine-grained or contextual rules would explode the number of roles.
- A policy engine (OPA, Cedar, custom) is already in the technology landscape or is justified.

**Prefer ReBAC when**
- The product is fundamentally about relationships between users and objects (folders, documents, teams, projects).
- “Who can see this?” is answered by walking a graph rather than by role membership alone.

**Hybrid approaches** are common and acceptable: RBAC for coarse gates + ABAC/ReBAC for resource-level decisions. Document the layering explicitly.

## Essential Decision Points

1. **Where the decision is evaluated**  
   - Centralised policy service vs. application-embedded checks vs. data-layer filters (row-level security).  
   - Prefer a single authoritative evaluation path; avoid duplicated logic that can drift.

2. **Policy storage & change control**  
   - Who may change roles / policies?  
   - Audit trail of permission grants and policy updates is required for L–XL and any regulated context.

3. **Default deny**  
   - Absence of an explicit allow must be deny.  
   - Fail-closed behaviour under policy-engine unavailability must be defined.

4. **Tenant isolation**  
   - Authorization must compose with the multi-tenancy model (see multi-tenancy.md).  
   - Cross-tenant access must be impossible by construction, not merely by policy.

5. **Testing & verification**  
   - Critical permission combinations should be expressed as executable tests or policy unit tests.

## Anti-Patterns

- Hard-coding role checks deep inside business logic without a clear permission abstraction.
- Granting “admin” as a shortcut that bypasses all checks.
- Evaluating authorization only on the write path while leaving read paths unprotected.
- Storing permissions in the client and trusting the client to enforce them.
- Creating an explosion of roles that no one can reason about.

## Recording Requirements

In the Architecture Blueprint:

- Chosen primary model (and any secondary / hybrid layer)
- Evaluation location (service, middleware, data layer)
- How tenant context is bound into the decision
- Default-deny and fail-closed posture
- Link to this file and the decisive criteria

## Related Patterns

- [authentication.md](authentication.md) — establishing the principal
- [multi-tenancy.md](multi-tenancy.md) — tenant boundary that authorization must respect
- knowledge/technologies/security-tooling.md — policy engines and libraries
