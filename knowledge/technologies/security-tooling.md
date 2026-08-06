# Security Tooling

**Status:** Active  
**Authority:** core/contract.md · runtime/agents/architect.md · runtime/agents/tech-lead.md · runtime/agents/security-quality-auditor.md  
**Last review:** 2026-08-06  
**Confidence:** High

Decision framework for selecting authentication/authorization libraries, secret management, scanning, and hardening tooling.

Security tooling supports the patterns defined in knowledge/patterns/ (especially authentication, authorization, and related concerns). It does not replace those decision frameworks.

## Scope

- Authentication and session/token libraries
- Authorization engines and policy tools
- Secret and configuration management
- Static / dynamic / dependency scanning
- Container and infrastructure hardening helpers
- Security headers and browser security utilities

Out of scope: the security model itself (see patterns), compliance evidence collection, and physical or organisational security.

## Primary Decision Axes

1. **Threat model and data sensitivity**
2. **Authentication and authorization patterns already chosen**
3. **Runtime and language ecosystem**
4. **Team capacity** to operate additional security infrastructure
5. **Regulatory or contractual requirements** (if any)

## Authentication & Session Tooling

Align library choice with the model selected in patterns/authentication.md.

| Model | Typical tooling posture |
|-------|-------------------------|
| Session-based | Server-side session store (Redis or DB) + secure cookie helpers; CSRF protection library |
| Token-based (JWT / opaque) | Battle-tested JWT library with explicit algorithm allow-list; refresh-token rotation support; denylist or short expiry strategy |
| External IdP | Mature OIDC/OAuth client (Auth.js / NextAuth, Passport, oauth4webapi, Spring Security, etc.) |
| Passkeys / WebAuthn | Dedicated WebAuthn library; fallback path still required for many audiences |

**Rules of thumb:**
- Prefer libraries that are actively maintained and widely reviewed.
- Never implement crypto primitives or JWT parsing from scratch.
- Record token lifetime, rotation, and revocation strategy alongside the library choice.

## Authorization Tooling

Align with patterns/authorization.md (RBAC, ABAC, ReBAC, etc.).

- Simple RBAC can often live in application code or a lightweight policy table.
- When policies become complex or must be updated without deploys, consider a policy engine (OPA/Gatekeeper, Cedar, Casbin, or equivalent).
- Record where the source of truth for roles/permissions lives and how it is audited.

## Secret Management

**Minimum expectation for any non-prototype system:**

- Secrets are never committed to source control.
- Production secrets come from a managed secret store or the platform’s native secret mechanism (AWS Secrets Manager, GCP Secret Manager, Azure Key Vault, Doppler, Infisical, platform env + encryption, etc.).
- Local development uses a clearly documented, non-production secret path (direnv, 1Password, etc.).

**Prefer:**
- Short-lived credentials and automatic rotation where the platform supports it.
- Least-privilege IAM roles for services that read secrets.

## Scanning & Supply Chain

Integrate into CI as quality gates appropriate to the complexity band:

| Gate | Typical tools / approach |
|------|--------------------------|
| Dependency vulnerabilities | Dependabot, Renovate, npm audit, pip-audit, govulncheck, Trivy, Snyk, etc. |
| Static analysis / SAST | Language-appropriate linters + security plugins (CodeQL, Semgrep, Bandit, etc.) |
| Container image scanning | Trivy, Grype, or platform-native scanners on every image |
| Secret scanning | gitleaks, trufflehog, or platform secret scanning on push / PR |
| License compliance | When redistribution or policy requires it |

**Complexity S:** dependency + secret scanning is the practical minimum.  
**Complexity M–L:** add SAST and container scanning; treat high/critical findings as release blockers unless explicitly risk-accepted.

## Browser & Transport Hardening

- Enforce HTTPS and HSTS.
- Apply a sensible Content-Security-Policy; start strict and relax only with justification.
- Use secure, HttpOnly, SameSite cookie attributes for session identifiers.
- Prefer framework or well-known middleware for security headers rather than ad-hoc implementations.

## Essential Decision Points to Record

In the Architecture Blueprint (Security / Technology Decisions):

1. Authentication library / IdP integration and how it maps to the chosen pattern
2. Authorization approach and any policy engine
3. Secret management solution and rotation posture
4. Scanning gates that are part of the definition of done
5. Security headers and browser hardening baseline
6. Any accepted risks (e.g., delayed patching windows, prototype exceptions) with owner and review date

## Anti-Patterns

- Rolling custom crypto or JWT handling.
- Storing long-lived secrets in environment variables that are visible in logs or crash dumps.
- Skipping dependency scanning because “we only use popular packages”.
- Treating security headers as optional for authenticated product surfaces.
- Running production with debug modes or overly permissive CORS.
- Accepting critical vulnerabilities without a documented exception and expiry.

## Related Knowledge

- [patterns/authentication.md](../patterns/authentication.md)
- [patterns/authorization.md](../patterns/authorization.md)
- [patterns/file-uploads.md](../patterns/file-uploads.md) (scanning and size limits)
- [patterns/payments.md](../patterns/payments.md) (PCI-adjacent tooling implications)
- [deployment.md](deployment.md)
- [observability.md](observability.md) (security event visibility)
- [backend.md](backend.md)
- [frontend.md](frontend.md)
