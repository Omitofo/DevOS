# Authentication

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, Tech Lead, Security & Quality Auditor

Identity verification and session establishment.  
This pattern answers *who is the caller?* Authorization (what may they do?) is covered separately.

## Core Models

| Model | Description | Typical use |
|-------|-------------|-------------|
| **Session-based** | Server issues a session identifier stored in a cookie (or equivalent). Server holds session state. | Traditional web apps, admin panels, when server-side rendering dominates |
| **Token-based (Bearer)** | Client receives a signed token (usually JWT or opaque) and presents it on each request. | SPAs, mobile clients, API-first products |
| **Hybrid** | Short-lived access token + longer-lived refresh token; session cookie optional for browser contexts. | Modern full-stack apps that serve both browser and API clients |

## Preference Guidance

**Prefer session-based when**
- The primary client is a browser under the same origin.
- Server-side rendering or form posts are central.
- Revocation must be immediate and simple (delete the session record).
- Complexity band is S–M and operational simplicity is valued.

**Prefer token-based when**
- Multiple client types (web, mobile, third-party) consume the same API.
- The architecture is API-first or microservices-oriented.
- Stateless horizontal scaling of the application tier is a hard requirement.

**Prefer hybrid when**
- Both browser and non-browser clients must be supported well.
- Refresh rotation and short access-token lifetime are required for security posture.

## Essential Decision Points

1. **Identity provider**  
   - First-party (email/password, magic link, passkeys) vs. third-party IdP (OIDC/OAuth).  
   - Prefer passkeys / WebAuthn where the product audience and device support allow it.

2. **Credential storage**  
   - Passwords must be hashed with a modern adaptive algorithm (Argon2id preferred; bcrypt acceptable).  
   - Never store recoverable passwords.

3. **Session / token lifetime & rotation**  
   - Access tokens: short (minutes).  
   - Refresh tokens: longer, rotated on use, stored with high protection.  
   - Absolute session timeout and idle timeout must be defined.

4. **Transport security**  
   - HTTPS only.  
   - Secure, HttpOnly, SameSite cookies for session identifiers.  
   - CSRF protection required for cookie-based flows.

5. **Logout & revocation**  
   - Explicit logout must invalidate the server-side record or token family.  
   - For pure JWTs without a denylist, document the residual window and accept or mitigate it.

## Anti-Patterns

- Storing long-lived JWTs in localStorage without additional binding or short expiry.
- Rolling your own crypto or password-hashing scheme.
- Mixing session cookies and bearer tokens without a clear security model for each.
- Omitting rate limiting and lockout / progressive delay on authentication endpoints.
- Treating “remember me” as an infinite session without rotation or device binding.

## Recording Requirements

In the Architecture Blueprint (Technology Decisions or NFR Mapping):

- Chosen model (session / token / hybrid)
- Identity provider approach
- Token / session lifetime policy
- Revocation strategy
- Link back to this file and the concrete criteria used

## Related Patterns

- [authorization.md](authorization.md) — what the authenticated principal may do
- [multi-tenancy.md](multi-tenancy.md) — tenant context attachment after authentication
- knowledge/technologies/security-tooling.md — concrete libraries and services
