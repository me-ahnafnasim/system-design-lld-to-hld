# 09 — Component / Service Architecture

> What major components / services exist inside the application / system?

C4 component level lives here.

## Example — Admission Application
```text
├── Lead Management
├── Counselling
├── Visit Management
├── Admission
├── Reporting
└── Notifications
```

Or microservices: Admission / Payment / Notification / Analytics.

## Patterns
- API Gateway, BFF, Strangler Fig, Sidecar, Ambassador, Anti-corruption layer
- Circuit Breaker, Bulkhead, Retry, Cache-Aside, Read-/Write-through
- CQRS, Event Sourcing, Pub/Sub, Message Queue, Broker

## Checklist
- [ ] Responsibilities per component (no god service)
- [ ] Dependencies acyclic
- [ ] Resilience pattern per remote call
- [ ] Read vs write paths separated where hot

## Artifacts
- Component diagram, responsibility table
