# Forms

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, UX, UI, Tech Lead

Form design, validation, submission, and progressive enhancement.

## Core Concerns

| Concern | Description |
|---------|-------------|
| **Validation** | Client-side for UX; server-side is authoritative |
| **Submission model** | Classic POST, AJAX / fetch, or progressive enhancement |
| **Error presentation** | Field-level and form-level; accessible and clear |
| **State & persistence** | Drafts, multi-step wizards, recovery after failure |
| **Security** | CSRF, rate limiting, input sanitisation, file handling |

## Preference Guidance

**Prefer progressive enhancement when**
- The form must work without JavaScript (accessibility, resilience, SEO contexts).
- Complexity band is S–M and the team values simplicity.

**Prefer client-heavy (SPA-style) forms when**
- Multi-step flows, rich live validation, or complex conditional fields dominate.
- The rest of the product is already a rich client.
- Offline or optimistic UI is a requirement.

**Always enforce server-side validation** regardless of client approach. Client validation is a UX optimisation, never a security boundary.

## Essential Decision Points

1. **Validation ownership**  
   - Shared schema (JSON Schema, Zod, etc.) that can run on both client and server is preferred when the same rules apply.  
   - Business-rule validation that depends on current data state must live on the server.

2. **Submission & feedback**  
   - Success and failure paths must be explicit.  
   - For long-running processing, return quickly and provide a status / polling or push mechanism (see real-time.md).

3. **Multi-step / wizard flows**  
   - Decide whether intermediate state is stored server-side, client-side, or both.  
   - Back-navigation and partial completion must be designed, not accidental.

4. **Accessibility**  
   - Labels, error association (`aria-describedby`), focus management, and keyboard operability are non-negotiable.  
   - See knowledge/design/accessibility-patterns.md.

5. **CSRF & origin protection**  
   - Cookie-based session forms require CSRF tokens or SameSite + careful origin checks.  
   - Token-based APIs rely on the existing authentication scheme.

6. **Rate limiting & abuse**  
   - Public forms (contact, signup, password reset) need rate limiting and, where appropriate, CAPTCHA or similar friction.

## Anti-Patterns

- Relying solely on client-side validation.
- Silent failure or generic “something went wrong” without field-level guidance.
- Losing user input on validation failure.
- Multi-step forms that cannot be resumed or that lose data on browser back.
- Accessible labels omitted or only present as placeholder text.

## Recording Requirements

In the Architecture Blueprint or Visual / Implementation artifacts:

- Submission model (progressive vs. client-driven)
- Validation strategy (shared schema vs. separate)
- Multi-step state handling if applicable
- Security measures (CSRF, rate limits)
- Link to this file

## Related Patterns

- [file-uploads.md](file-uploads.md) — when forms contain files
- [authentication.md](authentication.md) — login / signup forms
- knowledge/design/accessibility-patterns.md
- knowledge/design/forms-related design notes if present
