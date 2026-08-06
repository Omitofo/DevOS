# Testing

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** Tech Lead, Security & Quality Auditor, implementers, CI systems

Canonical expectations for test strategy, structure, naming, isolation, coverage, and quality gates.  
Testing is treated as a permanent part of the delivery pipeline, not an optional phase.

## Core Principles

1. **Fast feedback first** — the majority of tests must run in seconds so that agents and developers can iterate safely.
2. **Pyramid, not ice-cream** — many narrow unit tests, fewer integration tests, very few end-to-end tests.
3. **Behaviour over implementation** — tests describe observable outcomes; they do not mirror internal structure.
4. **Deterministic** — a test that passes or fails randomly is a defect in the test suite.
5. **Owned** — every test has a clear purpose and is maintained with the same care as production code.

## Test Pyramid Guidance

| Layer | Purpose | Typical tools / style | Volume guidance | Speed target |
|-------|---------|-----------------------|-----------------|--------------|
| **Unit** | Verify a single unit (function, class, component) in isolation | Language-native runners, Testing Library for UI units | Largest share | Milliseconds–low seconds |
| **Integration** | Verify collaboration between a few real modules or with a real dependency (DB, queue, external API contract) | Testcontainers, in-memory or local doubles, contract tests | Medium | Seconds |
| **End-to-end / System** | Verify critical user or system journeys through the real (or near-real) stack | Playwright, Cypress, or language equivalents; API-level journey tests | Smallest share | Tens of seconds–minutes |
| **Manual / Exploratory** | Edge cases, usability, visual polish, and risk areas that automation cannot cheaply cover | Human or agent-assisted sessions | As needed | — |

Complexity band influences absolute numbers, not the relative shape of the pyramid. Even band-S projects keep the pyramid orientation.

## What Must Be Tested

- Core domain logic and business rules.
- Authentication, authorization, and multi-tenancy boundaries.
- Input validation and error paths that affect security or data integrity.
- Public API contracts (request/response shapes, status codes, error formats).
- Critical user journeys identified in the User Journey or Implementation Plan.
- Regression coverage for every previously fixed production defect of severity Medium or higher.

## What May Be Deferred or Omitted (with justification)

- Purely presentational UI that has no behaviour (still prefer a minimal smoke test).
- Generated code that is covered by the generator’s own tests.
- Third-party library internals (test the integration surface instead).
- Exhaustive combinatorial matrix of every configuration option (prefer property-based or targeted sampling).

## Naming & Structure

- Test files mirror the unit under test and use a consistent suffix (`.test.`, `.spec.`, `_test.`, etc.).
- Test names (or `it`/`test` descriptions) read as plain-language specifications:  
  `createUser rejects duplicate email`  
  not `createUser_duplicateEmail`.
- Arrange–Act–Assert (or Given–When–Then) structure is preferred inside each test.
- Shared fixtures and factories live in clearly named helpers; avoid hidden global state.

## Isolation & Determinism Rules

- Unit tests must not depend on real networks, real clocks, or shared mutable databases.
- Time, randomness, and external services are injected or replaced by test doubles.
- Integration tests that require a database or broker use ephemeral, isolated instances (containers, transactions that roll back, or dedicated test schemas).
- Parallel test execution must be safe; tests that cannot run in parallel are explicitly marked and kept to a minimum.
- Flaky tests are treated as defects: quarantined immediately and fixed or deleted within the same iteration.

## Coverage Expectations

Coverage numbers are a signal, not a goal. The following baseline applies unless the Architecture Blueprint records a different risk-based target:

| Complexity band | Line / statement coverage (critical packages) | Branch coverage focus |
|-----------------|-----------------------------------------------|------------------------|
| S | ≥ 70 % on domain and API layers | Security and validation paths |
| M | ≥ 80 % on domain, API, and core services | All authz, payment, and data-mutation paths |
| L / XL | ≥ 85 % on domain + explicit risk-based targets for high-severity modules | Same + chaos / resilience scenarios |

100 % coverage is never required and is often harmful. Uncovered code must be intentional and reviewed.

## Continuous Integration Gates

Every project pipeline must, at minimum:

1. Run the full unit + fast integration suite on every pull request / change set.
2. Fail the build on test failure or on coverage falling below the agreed threshold for the changed packages.
3. Run the critical-path end-to-end suite on mainline / release branches (or on a schedule if the suite is expensive).
4. Surface test results and coverage reports in a form readable by both humans and agents.
5. Block merge when new flaky tests are introduced.

## Anti-Patterns

- Testing implementation details (private methods, internal state) instead of observable behaviour.
- Enormous end-to-end suites that take tens of minutes and are the only safety net.
- Shared mutable fixtures that cause order-dependent failures.
- Commenting out or `@Ignore`-ing failing tests without a tracked issue.
- Measuring coverage on generated or third-party code and treating the number as meaningful.
- Writing tests only after production incidents (“test-after-bug”).

## Recording Requirements

In the Architecture Blueprint or Implementation Plan:

- Chosen test pyramid emphasis and any deliberate deviation.
- Coverage targets per band or per high-risk module.
- Tooling choices (runners, assertion libraries, e2e framework) and rationale.
- Policy for flaky tests and for updating tests when behaviour intentionally changes.
- Link back to this file.

## Related Standards & Knowledge

- [naming.md](naming.md) — consistent naming of test files and cases
- [documentation.md](documentation.md) — how the test strategy itself is documented
- knowledge/patterns/* — many patterns contain testing implications (auth, payments, real-time, etc.)
- knowledge/technologies/* — concrete tooling recommendations that must still satisfy these rules
- runtime/agents/security-quality-auditor.md — enforcement of these gates
