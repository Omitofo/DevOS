# Naming

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** Architect, Tech Lead, all implementers, Security & Quality Auditor

Canonical conventions for naming files, folders, code symbols, APIs, data stores, environment variables, commits, and related artifacts.  
Consistent naming is the primary mechanism that keeps multi-agent and multi-human collaboration legible.

## Core Principles

1. **Intention-revealing** — a name should communicate purpose without requiring the reader to open the file or inspect the implementation.
2. **Consistent within domain** — the same concept uses the same linguistic form everywhere (e.g., always `userId`, never mixing `user_id`, `UserID`, `uid`).
3. **Pronounceable and searchable** — avoid cryptic abbreviations and character sequences that are hard to type or grep.
4. **Scoped length** — short where frequency is high and context is local; longer and more descriptive where the name travels across boundaries.
5. **No encoding of type or scope in the name** unless the language or platform convention requires it (Hungarian notation is forbidden).

## File & Folder Naming

| Context | Convention | Examples | Notes |
|---------|------------|----------|-------|
| Source directories | kebab-case or lowercase-with-hyphens | `user-service/`, `auth-handlers/` | Prefer flat, purpose-named folders over deep nesting |
| Source files | kebab-case (most languages) or language-idiomatic | `create-user.ts`, `UserService.java` | Match the dominant ecosystem convention; stay consistent inside a project |
| Test files | mirror the unit under test + `.test` / `.spec` / `_test` suffix | `create-user.test.ts`, `UserService_test.go` | Co-locate or mirror path; never invent a parallel tree without reason |
| Documentation | kebab-case Markdown | `architecture-blueprint.md`, `adr-003-auth-model.md` | Always `.md` for human-readable docs inside the repo |
| Configuration | kebab-case or conventional names | `docker-compose.yml`, `.env.example` | Follow tool expectations first |
| Generated / build output | never commit; ignore via standard patterns | `dist/`, `build/`, `.next/` | — |

**Anti-patterns**
- Mixing camelCase, snake_case, and kebab-case for the same class of files.
- Using spaces, special characters, or non-ASCII in paths that travel through tooling.
- Encoding version or status in the filename (`final-v2-final.md`).

## Code Symbol Naming

### General

| Element | Preferred style | Example |
|---------|-----------------|---------|
| Variables & parameters | camelCase (JS/TS/Java/…) or snake_case (Python/Go/Rust where idiomatic) | `userId`, `order_total` |
| Functions / methods | verb or verb-phrase, same case as variables | `createUser`, `calculate_total` |
| Classes / types / interfaces | PascalCase | `UserProfile`, `PaymentIntent` |
| Constants | SCREAMING_SNAKE_CASE or language-idiomatic | `MAX_RETRY_COUNT`, `DEFAULT_TIMEOUT_MS` |
| Enums / enum members | PascalCase type + PascalCase or SCREAMING members | `OrderStatus.Pending` or `OrderStatus.PENDING` |
| Boolean variables | prefix with `is`, `has`, `can`, `should` | `isActive`, `hasPermission` |
| Private / internal | language convention (`_`, `#`, or package-private) | `_cache`, `#privateField` |

### Domain language

- Prefer the ubiquitous language of the product over technical synonyms (`Order` not `PurchaseRecord` if the domain says Order).
- Avoid generic names (`data`, `info`, `manager`, `helper`, `util`) unless the scope is truly generic and the name is immediately qualified.

## API & HTTP Naming

| Element | Convention | Example |
|---------|------------|---------|
| URL path segments | kebab-case, plural nouns for collections | `/api/v1/user-profiles`, `/orders/{orderId}/items` |
| Query parameters | camelCase or snake_case (pick one project-wide) | `?pageSize=20&sortBy=createdAt` |
| JSON request/response fields | camelCase (most public APIs) or snake_case when the consumer ecosystem expects it | `{ "userId": "...", "createdAt": "..." }` |
| HTTP headers (custom) | Prefix with product or `X-` sparingly; prefer standard headers | `X-Request-Id` only when necessary |
| Error codes / problem titles | stable, machine-readable, kebab or snake | `invalid-credentials`, `resource_not_found` |

**Versioning**  
Prefer URI versioning (`/v1/`) or explicit content negotiation; record the chosen scheme in the Architecture Blueprint. Never put versions inside resource names.

## Database & Persistence Naming

| Element | Convention | Example |
|---------|------------|---------|
| Tables / collections | snake_case, plural | `users`, `order_items` |
| Columns | snake_case | `created_at`, `user_id` |
| Primary keys | `id` or `<table>_id` | `id`, `order_id` |
| Foreign keys | `<referenced_table>_id` | `user_id` |
| Indexes | `idx_<table>_<columns>` | `idx_orders_user_id_created_at` |
| Constraints | `chk_`, `uq_`, `fk_` prefixes | `uq_users_email` |

Stay consistent with the chosen ORM/query layer; do not invent a second naming scheme on top of it.

## Environment Variables & Secrets

- SCREAMING_SNAKE_CASE.
- Prefix with a short product or service identifier when the variable is not global (`APP_`, `PAYMENTS_`, etc.).
- Never put secrets in variable *names*; only in values.
- Document every required variable in `.env.example` (or equivalent) with a short comment.

## Git & Commit Naming

- Branch names: `type/short-description` using kebab-case  
  Examples: `feat/user-onboarding`, `fix/payment-webhook-retry`, `chore/upgrade-deps`
- Commit subjects: imperative mood, ≤ 72 characters, no trailing period  
  Examples: `Add rate limiting to auth endpoints`, `Fix null dereference in order calculator`
- Prefer Conventional Commits when the project has adopted them; otherwise stay consistent with the existing history.

## Recording Requirements

In the Architecture Blueprint or project-level coding standards note:

- Confirmed adherence to this file (or explicit list of approved deviations).
- Any project-specific glossary of domain terms that extend or specialise the conventions above.
- Chosen case style for JSON and query parameters if the default is overridden.

## Related Standards & Knowledge

- [documentation.md](documentation.md) — how named artifacts are described and kept current
- [testing.md](testing.md) — naming of test files, suites, and cases
- knowledge/patterns/api-design.md — deeper API shape decisions that interact with naming
- knowledge/technologies/* — language- and framework-specific idioms that must still respect these rules
