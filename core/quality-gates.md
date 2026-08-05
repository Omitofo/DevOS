# Engineering Quality Gates

Every Master Design Plan must be evaluated against the following gates.  
Each gate records: **PASS / FAIL**, **Evidence**, **Observations**.

| Gate | Description |
|------|-------------|
| Functional Correctness | Requirements are complete, consistent, and testable |
| Performance | Explicit budgets and measurement strategy |
| Accessibility | WCAG 2.2 AA (or project-specific higher target) |
| Responsive Design | Defined breakpoints and adaptive behavior |
| Security | Threat model, authn/authz, input validation, secrets handling |
| Privacy | Data minimization, consent, retention, regional compliance |
| UX | Journeys cover primary and edge cases; cognitive load considered |
| UI | Visual system is coherent, tokenized, and implementable |
| Maintainability | Clear module boundaries, low accidental complexity |
| Testability | Every requirement maps to verifiable tests |
| SEO (when applicable) | Technical and content SEO strategy defined |

## Authority

The Security & Quality Auditor is the final authority.  
It may not approve a plan that fails any gate without an explicit, human-accepted risk acceptance record.
