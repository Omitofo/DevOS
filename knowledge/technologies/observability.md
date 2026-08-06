# Observability Technologies

**Status:** Active  
**Authority:** core/contract.md · runtime/agents/architect.md · runtime/agents/tech-lead.md · runtime/agents/security-quality-auditor.md  
**Last review:** 2026-08-06  
**Confidence:** High

Decision framework for selecting logging, metrics, tracing, and alerting approaches.

Observability is not an afterthought. The Architecture Blueprint must declare the minimum viable observability posture for the complexity band and availability targets.

## Scope

- Structured logging
- Metrics (RED/USE or equivalent)
- Distributed tracing
- Alerting and on-call signals
- Dashboards and continuous profiling (when justified)

Out of scope: detailed runbook content, specific alert thresholds (those belong in project operational docs), and security event management (see security-tooling.md).

## Primary Decision Axes

1. **Complexity band** and number of services
2. **Availability / latency SLOs** (if any)
3. **Team size** and on-call capacity
4. **Budget** for commercial observability platforms vs open-source + self-host
5. **Runtime and deployment model** (containers, serverless, edge)

## Minimum Viable Observability by Complexity

### Complexity S

- Structured logs (JSON) to a centralised log sink or platform log service
- Basic health and readiness endpoints
- Error tracking (Sentry or equivalent) for exceptions
- Uptime monitoring on critical external URLs

### Complexity M

Everything in S, plus:

- Standard metrics: request rate, error rate, duration (RED) for key services
- Distributed tracing on the critical request path
- Centralised dashboards for the golden signals
- Alerting on sustained error-rate or latency budget burn

### Complexity L

Everything in M, plus:

- Service-level objectives with error-budget policy
- Continuous profiling or equivalent for performance regressions
- Cross-service trace sampling strategy that remains useful under load
- Synthetic monitoring for critical user journeys
- Clear ownership of dashboards and alert routes

## Technology Families

### Logging

**Prefer structured, levelled logs** (JSON or equivalent) with a consistent correlation / request ID.

| Approach | Prefer when |
|----------|-------------|
| Platform-native logging (CloudWatch, GCP Logging, etc.) | Team wants minimal extra infrastructure |
| Open-source stack (Loki, Elasticsearch/OpenSearch, Vector/Fluent Bit) | Multi-cloud or self-hosted preference, cost control at volume |
| Commercial (Datadog, New Relic, Axiom, etc.) | Team values integrated UX and can accept the cost |

**Anti-pattern:** Unstructured printf-style logs in production services.

### Metrics

**Prefer OpenTelemetry metrics or Prometheus-compatible exposition** as the interchange format.

- RED method (Rate, Errors, Duration) for request-driven services
- USE method (Utilisation, Saturation, Errors) for resources
- Business / domain metrics only when they drive decisions or alerts

Avoid high-cardinality labels that explode series count.

### Tracing

**Prefer OpenTelemetry** as the instrumentation standard.

- Propagate context across process and service boundaries
- Sample intelligently (head-based or tail-based according to volume and need)
- Ensure the critical user journeys are represented

Tracing is high leverage once more than one service or a non-trivial async boundary exists.

### Alerting

- Alert on symptoms (user-visible error rate, latency SLO burn) before causes.
- Prefer fewer, higher-quality alerts that wake humans only when action is required.
- Every alert must have an owner and a path to diagnosis (dashboard or runbook link).

## Tooling Selection Guidance

| Need | Lean option | Integrated commercial |
|------|-------------|------------------------|
| Logs + metrics + traces | Grafana stack (Loki, Mimir/Prometheus, Tempo) + OpenTelemetry | Datadog, New Relic, Elastic Observability |
| Error tracking | Sentry (or open-source equivalent) | Often included in commercial platforms |
| Uptime / synthetics | Checkly, Better Uptime, or cloud-native | Usually available in commercial suites |

Record the choice and whether the team is accepting self-host operational load or vendor cost/lock-in.

## Essential Decision Points to Record

In the Architecture Blueprint (NFR / Observability section):

1. Logging approach and sink
2. Metrics system and primary golden signals
3. Tracing standard and sampling posture
4. Error tracking
5. Alerting philosophy and ownership model
6. Any commercial platform vs self-hosted decision and residual risk
7. How observability is validated in staging / pre-production

## Anti-Patterns

- Treating logs as the only signal once the system has more than one service.
- Alerting on every error or every metric spike (alert fatigue).
- High-cardinality metrics that make the metrics system unusable.
- Instrumenting only after the first production incident.
- Dashboards that no one owns or that drift from the actual architecture.
- Ignoring cold-start and edge runtime visibility when those platforms are used.

## Related Knowledge

- [deployment.md](deployment.md)
- [backend.md](backend.md)
- [security-tooling.md](security-tooling.md)
- knowledge/patterns/ (especially real-time, payments, multi-tenancy — each has observability implications)
