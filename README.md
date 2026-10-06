# System Design: LLD to HLD — Full Architecture Map (13 Levels)

From **Business Architecture** down to **Function / Algorithm Design**, with LLD + HLD in their exact places.

> Business → Enterprise → Solution → System → Application → Integration → Data → Infrastructure → Component/Service → Package/Module → OOP Foundations → Class/Object → Function/Algorithm
>
> Plus cross-cutting: Security, Reliability, Observability, Performance, Governance.

Start with [`docs/ARCHITECTURE-MAP.md`](docs/ARCHITECTURE-MAP.md) — the single merged map — then work 01 → 13, and finish with `examples/worldconnect/`.

## Structure

```text
.
├── 01-business-architecture/          # Capabilities, processes, actors
├── 02-enterprise-architecture/        # Portfolio, standards, governance, roadmap
├── 03-solution-architecture/          # One business problem across systems
├── 04-system-architecture/            # Monolith / microservices, quality attrs, CAP/BASE/ACID
├── 05-application-architecture/       # Layered / Clean / Hexagonal, DDD
├── 06-integration-architecture/       # REST / GraphQL / gRPC / events / queues
├── 07-data-architecture/              # DBs, ownership, warehouse, replication, sharding
├── 08-infrastructure-deployment-architecture/  # Cloud / K8s / LB / CDN / DR / deploys
├── 09-component-service-architecture/ # Components / services, Gateway / BFF / CQRS / breakers
├── 10-package-module-design/          # Cohesion / coupling, REP/CCP/CRP/ADP/SDP/SAP
├── 11-object-oriented-programming-foundations/ # Class, object, pillars, relationships, coupling/cohesion
├── 12-class-object-design/            # SOLID, GRASP, GoF, anti-patterns, metrics
├── 13-function-algorithm-design/      # Complexity, validation, pure functions
├── cross-cutting-concerns/            # Security / reliability / observability (all levels)
├── examples/
│   └── worldconnect/                  # End-to-end React + microservices trace (13 levels)
├── interview-prep/                    # 45-min HLD + 35-min LLD frameworks, checklists
├── assets/diagrams/
└── docs/
    ├── ARCHITECTURE-MAP.md            # Canonical merged hierarchy (read first)
    ├── ROADMAP.md
    ├── GLOSSARY.md
    └── RESOURCES.md
```

Each numbered folder has a `README.md` with question, scope, checklist, and artifacts. Old `01-low-level-design` / `02-high-level-design` stubs were merged into their exact levels (09–12 for LLD, 04/06/07/08 for HLD) — see `docs/ARCHITECTURE-MAP.md` for the mapping.

## Getting started

1. Read `docs/ARCHITECTURE-MAP.md`.
2. Go 01 → 13 in order; read `cross-cutting-concerns/` alongside.
3. Trace `examples/worldconnect/` top-down without looking.
4. Use `interview-prep/` for timed mocks.

No build step — docs + examples. Code in Java (backend) / TypeScript + React (frontend) where relevant.

## Conventions

- Folders: `kebab-case`, numbered by scope (`01-` → `13-`)
- One concept per file; diagrams in `assets/diagrams/`
- Every pattern / case study: `README.md` (problem → design → diagram → trade-offs) + code

See `CONTRIBUTING.md`.

## License

MIT — see `LICENSE`.
