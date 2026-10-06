# 06 — Integration Architecture

> How do different systems communicate?

## Mechanisms
- REST, GraphQL, gRPC, SOAP
- Webhooks, WebSockets
- Message brokers: Kafka, RabbitMQ, ActiveMQ; ESB (legacy)

## Example — Event-driven admission
```text
Admission ──StudentAdmitted event──▶ Event Bus
                                       ├── Finance
                                       ├── Email
                                       └── Analytics
```

## Design notes
- Sync (request/response) vs async (events/queues) — pick per coupling/latency needs
- Contracts versioned; idempotency for retries; dead-letter queues
- BFF pattern when frontends need aggregated payloads

## Checklist
- [ ] Sync vs async justified per flow
- [ ] Contracts + versioning defined
- [ ] Retry / idempotency / DLQ considered
- [ ] Auth between services (not just edge)

## Artifacts
- Integration sequence diagram, event catalog
