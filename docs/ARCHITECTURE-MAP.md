# Software Architecture & Design Map — From Business to Function

Single canonical map merging:
- **A:** 13-level scope hierarchy (Business → Function/Algorithm)
- **B:** 6-level terminology hierarchy (Enterprise → Class/Object) + patterns, principles, metrics

Terminology varies by org. TOGAF distinguishes **Business, Data, Application, and Technology Architecture**; other frameworks separate Enterprise / Solution / System scope. This repo uses the 13 levels below as a learning map, not a mandated standard.

## The 13 levels

| # | Level | Main question |
|---:|---|---|
| 1 | **Business Architecture** | What business capabilities / processes are we supporting? |
| 2 | **Enterprise Architecture** | How do all business, application, data, technology systems fit together? |
| 3 | **Solution Architecture** | How will we solve this particular business problem across systems? |
| 4 | **System Architecture** | How is this specific software system structured? |
| 5 | **Application Architecture** | How is one application internally organized? |
| 6 | **Integration Architecture** | How do applications / services communicate? |
| 7 | **Data Architecture** | How is data modeled, stored, governed, and moved? |
| 8 | **Infrastructure / Deployment Architecture** | Where and how does the software run? |
| 9 | **Component / Service Architecture** | What major components / services exist inside the application / system? |
| 10 | **Package / Module Design** | How is source code divided into modules / packages? |
| 11 | **OOP Foundations** | What are the language/design atoms (class, object, pillars, relationships)? |
| 12 | **Class & Object Design** | How do classes / objects collaborate internally? |
| 13 | **Function / Algorithm Design** | How does an individual behavior actually work? |

```text
1. Business Architecture
          ↓
2. Enterprise Architecture
          ↓
3. Solution Architecture
          ↓
4. System Architecture
          ↓
5. Application Architecture
          ↓
6. Integration Architecture
          ↓
7. Data Architecture
          ↓
8. Infrastructure / Deployment Architecture
          ↓
9. Component / Service Architecture
          ↓
10. Package / Module Design
          ↓
11. OOP Foundations
          ↓
12. Class / Object Design
          ↓
13. Function / Algorithm Design
```

## Strict hierarchy vs parallel concerns

Several levels are **parallel concerns, not strict parent → child**.

Inside one system:

```text
System Architecture
│
├── Application Architecture
├── Data Architecture
├── Integration Architecture
├── Security Architecture
└── Infrastructure Architecture
```

Similarly at code level, component → package → foundations → class → function is roughly top-down, but data, integration, and infra cut across.

Think of these as **vertical / cross-cutting concerns**:

```text
                     SYSTEM LEVEL
                          │
Security ─────────────────┤
Observability ────────────┤
Performance ──────────────┤
Reliability ──────────────┤
Compliance ───────────────┤
Data Governance ──────────┤
                          │
                     CODE LEVEL
```

See `cross-cutting-concerns/` for checklists.

## How the old 6 levels map to the new 13

| Old 6-level name | Maps to new levels |
|---|---|
| 1. Enterprise & System Architecture | 1 Business + 2 Enterprise + part of 4 System |
| 2. Solution / System Architecture | 3 Solution + 4 System (styles, quality attributes, distributed systems) |
| 3. Component & Module Architecture | 9 Component/Service + 6 Integration + 8 Infrastructure/Deployment |
| 4. Application Architecture | 5 Application (layers, DDD, Clean/Hexagonal, app patterns) |
| 5. Package & Module Design | 10 Package/Module (cohesion, coupling, package principles) |
| 6. Class & Object Design | 11 OOP Foundations + 12 Class/Object + 13 Function/Algorithm |

No content was dropped — it was redistributed to its exact level (see folder READMEs).

## Level summaries

### 1. Business Architecture — above software
What does the org do? Capabilities, processes, actors, boundaries. No Singleton/Microservices here.
Example (university): Recruitment, Admission, Academic Management, Finance, HR, Examination.

### 2. Enterprise Architecture — whole organization
Application portfolio, technology standards, shared platforms, data ownership, integration standards, governance, roadmap.
Frameworks: TOGAF, Zachman, FEAF. Concerns: capability mapping, portfolio management, IT governance.

### 3. Solution Architecture — one business problem
Which systems together solve this problem? Example: digital admission platform = Website + CRM + Admission ERP + Payment Gateway + SMS/WhatsApp + Analytics.

### 4. System Architecture — one system
Monolith vs Modular Monolith vs Microservices vs Event-driven vs Serverless vs SOA. Quality attributes: scalability, availability, reliability, performance, security, maintainability, extensibility, interoperability, testability, deployability. Distributed: CAP, BASE, ACID, Paxos/Raft, 2PC, Saga.

### 5. Application Architecture — inside one app
Layered / MVC / MVVM / MVP / Clean / Hexagonal / Onion / Vertical Slice. Layers: presentation → application/business → domain → infrastructure/persistence. DDD: bounded context, ubiquitous language, context mapping, subdomains. Ports/adapters, DI/IoC, Repository, Unit of Work.

### 6. Integration Architecture — how systems talk
REST, GraphQL, gRPC, SOAP, Webhooks, WebSockets, Kafka/RabbitMQ, ESB. Example: `StudentAdmitted` event → Event Bus → Finance, Email, Analytics.

### 7. Data Architecture — how data lives and moves
SQL vs NoSQL, schemas, ownership, warehouse/lake, replication, partitioning/sharding, indexing, caching, event sourcing, governance. Example: Operational DB → ETL → Warehouse → BI.

### 8. Infrastructure / Deployment — where it runs
Cloud, Docker, Kubernetes, service mesh, LB, networking, CDN, storage, regions/AZ, DR. Strategies: blue-green, canary, feature flags, IaC, autoscaling. Roughly TOGAF Technology Architecture.

### 9. Component / Service — inside app/system
Admission App: Lead, Counselling, Visit, Admission, Reporting, Notifications. Or microservices: Admission/Payment/Notification/Analytics. Patterns: API Gateway, BFF, Strangler, Sidecar, Ambassador, Anti-corruption, Circuit Breaker, Bulkhead, Retry, Cache-Aside, CQRS, Event Sourcing, Pub/Sub. C4 component level lives here.

### 10. Package / Module — code organization
High cohesion, low coupling, info hiding, public API, dependency direction. Cohesion/coupling types. Package principles: REP, CCP, CRP, ADP, SDP, SAP. Namespaces, visibility, SemVer. Example `src/admission/{domain,application,infrastructure,presentation}`.

### 11. OOP Foundations — atoms before judgment
Class, object, property/field, method, constructor, access modifiers, encapsulation, abstraction, inheritance, polymorphism, interface vs abstract class, association, aggregation, composition, dependency, static vs instance, `this`/`super`, coupling, cohesion, composition-over-inheritance. See `11-object-oriented-programming-foundations/`.

### 12. Class & Object — GoF lives here
SOLID + DRY/KISS/YAGNI/Tell-Don't-Ask/Law of Demeter/Hollywood/program-to-interface. GRASP: Expert, Creator, Low Coupling, High Cohesion, Controller, Polymorphism, Pure Fabrication, Indirection, Protected Variations. Creational/Structural/Behavioral patterns. Anti-patterns (God Object, spaghetti, etc.) and metrics (cyclomatic, DIT, CBO, LCOM, etc.). Assumes Level 11 atoms.

### 13. Function / Algorithm — single behavior
`calculateScholarship()`: algorithm, control flow, time/space complexity, validation, error handling, pure functions, data structures. Linear vs binary vs hash lookup.

## Cross-cutting concerns (all levels)

| Concern | Study |
|---|---|
| Security | AuthN/Z, identity, secrets, encryption, network boundaries, threat modeling, audit |
| Reliability | Retry, timeout, circuit breaker, redundancy, bulkhead |
| Observability | Logs, metrics, traces, alerts |
| Performance | Cache, CDN, concurrency, scaling |
| Integration | API/event/messaging contracts |
| Data governance | Ownership, storage, movement, compliance |
| General | Logging, error handling, config, i18n/l10n, transactions, audit trails |

## References

- TOGAF 9.2 — Business, Data, Application, Technology Architecture
- C4 model — context / container / component levels
- Solution architecture spanning multiple systems/applications

## Folder map

Each level has an exact folder. Start here, then go in order 01 → 13, with `cross-cutting-concerns/` read alongside, and `examples/worldconnect/` as the end-to-end trace.
