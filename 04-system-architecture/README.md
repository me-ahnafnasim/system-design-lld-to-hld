# 04 — System Architecture

> How is this specific software system structured?

Focus on **one system** (e.g. Admission Management System).

## Architectural styles
- Monolith, Modular Monolith, Microservices, Event-driven
- SOA, Serverless, Space-based, Client-server, P2P
- Layered / N-tier, Hexagonal / Clean, Onion, Pipe-and-filter, Blackboard, MVC / MVVM / MVP

## Quality attributes (design for these)
- Scalability (vertical / horizontal), availability, reliability
- Performance (latency, throughput), security, maintainability
- Extensibility, interoperability, testability, deployability

## Distributed systems
- CAP, BASE, ACID
- Consensus (Paxos, Raft), distributed transactions, 2PC, Saga

## Checklist
- [ ] Style chosen with reason (team + scale + domain)
- [ ] Quality attributes prioritized (not "all of them")
- [ ] Consistency choice explicit (per use-case, e.g. feed = AP, payments = CP)
- [ ] Failure modes considered

## Artifacts
- System context + container diagram (C4 L1/L2), ADRs
