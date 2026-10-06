# Cross-Cutting Concerns

Vertical concerns — apply at **all** levels. Don't treat as "one level below" anything.

## Security
- AuthN/Z, identity, secrets, encryption, network boundaries, threat modeling, audit

## Reliability
- Retry, timeout, circuit breaker, bulkhead, redundancy

## Observability
- Logs, metrics, traces, alerts; dashboards + runbooks

## Performance
- Cache, CDN, concurrency, scaling; SLOs (latency/throughput)

## Integration contracts
- API / event / messaging versioning, idempotency

## Data governance
- Ownership, storage, movement, retention, compliance (GDPR etc.)

## General engineering
- Logging, error handling, config management, i18n/l10n, transactions, audit trails, caching strategies

Read alongside 01 → 12. Each level README should link here when a decision has security/reliability impact.
