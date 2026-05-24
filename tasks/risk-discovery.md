# Risk Discovery Task Layer

Use when inspecting a codebase, scaffold, design, or implementation to identify non-functional limitations, risks, constraints, and follow-up actions.

Do not implement fixes unless explicitly requested.

## Scope Control

- Assess only the current repository/workspace unless the user provides external systems, docs, or architecture context.
- Do not infer risks from systems that are not present in the repo or described by the user.
- Mark missing context as `Unknown` rather than assuming.
- Evidence must come from repository files, configuration, observed behavior, or explicit user-provided context.
- If a risk depends on an external service or production setup that is not visible, list it as an assumption or open question.

## Discovery Areas

- Performance, latency, scalability, and concurrency
- Reliability, resilience, and failure modes
- Designing for failure: timeouts, retries, backoff, idempotency, circuit breakers, graceful degradation, recovery paths
- Data consistency, integrity, availability, migrations, and backup/restore
- Security, authentication, authorization, secrets, and dependency risk
- Observability: logs, metrics, traces, dashboards, alerts, and runbooks
- CI/CD, deployment, rollback, environment parity, and release safety
- Local development, configuration, and operational complexity
- Test coverage, quality gates, and validation gaps
- Accessibility, browser support, and client-side rendering risks
- Cost, vendor lock-in, quotas, and external service limits

## Required Behavior

- Inspect relevant repo structure and docs; do not read everything.
- Identify evidence for each finding: file path, config, code pattern, missing artifact, or stated assumption.
- Distinguish confirmed risks from assumptions and unknowns.
- Explain data consistency and availability implications where relevant.
- Propose mitigation strategies, not only findings.
- Prioritize the highest-risk limitations first.
- Do not make code changes unless the user explicitly asks.

## Risk Register

Produce a table with:

| Area | Finding | Impact | Likelihood | Severity | Evidence | Mitigation Strategy | Recommended Action | Follow-up |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |

Use `low`, `medium`, or `high` for likelihood and severity.

Evidence must reference a file, config, observed behavior, or explicit user-provided context.

## Mitigation Strategy Examples

- Design for failure: retries with backoff, timeouts, idempotency keys, circuit breakers, bulkheads, degraded modes
- Data consistency: transactions, optimistic locking, idempotent writes, reconciliation jobs, outbox pattern, migration rollback
- Availability: health checks, readiness checks, load shedding, queue backpressure, failover, backup/restore, rollback strategy
- Observability: structured logs, metrics, traces, dashboards, alerts, runbooks
- Delivery safety: feature flags, staged rollout, smoke tests, CI gates, deployment rollback

## Output Expectations

- Executive summary of highest risks
- Risk register
- Assumptions and unknowns
- Quick wins
- Follow-up tasks ordered by priority
- Suggested validation or monitoring to confirm mitigation success
