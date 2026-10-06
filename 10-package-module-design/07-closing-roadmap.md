# Closing — Hierarchy, TypeScript Order, and the Recommended Structure

## The practical hierarchy (your index)

Don't memorize 20 styles independently. Navigate by this:

```text
PACKAGE / MODULE DESIGN
│
├── 1. ORGANIZATION
│   ├── By Type
│   ├── By Layer
│   ├── By Feature
│   ├── By Domain
│   ├── By Bounded Context
│   ├── By Capability
│   ├── By Component
│   ├── By Service
│   ├── By Workflow
│   └── Vertical Slice
│
├── 2. INTERNAL STRUCTURE
│   ├── Public API
│   ├── Internal implementation
│   ├── Shared Kernel
│   ├── Core
│   └── Plugins
│
├── 3. DISTRIBUTION
│   ├── Internal module
│   ├── Library
│   ├── npm/package
│   ├── Monorepo package
│   └── Independently deployable service
│
└── 4. DESIGN PRINCIPLES
    ├── High Cohesion
    ├── Low Coupling
    ├── REP
    ├── CCP
    ├── CRP
    ├── ADP
    ├── SDP
    └── SAP
```

## Learn them in this order (TypeScript)

```text
1. Package by Type
        ↓
2. Package by Layer
        ↓
3. Package by Feature
        ↓
4. Feature-first + Layer-inside
        ↓
5. Domain modules
        ↓
6. Public API / Internal implementation
        ↓
7. Shared vs Shared Kernel
        ↓
8. Vertical Slice
        ↓
9. Bounded Context modules
        ↓
10. Monorepo packages
        ↓
11. Package dependency graphs
        ↓
12. REP / CCP / CRP
        ↓
13. ADP / SDP / SAP
```

## The structure to prioritize

For the large TypeScript business applications in this repo:

```text
src/
├── modules/
│   ├── admission/
│   │   ├── api/
│   │   ├── application/
│   │   ├── domain/
│   │   ├── infrastructure/
│   │   └── index.ts
│   ├── finance/
│   ├── hr/
│   └── academic/
├── shared-kernel/
└── infrastructure/
```

> Domain/Feature first → layers inside → explicit public API → controlled dependencies → minimal shared code.

## Bridge to Level 11

Once module boundaries are correct, Level 11 decides how classes *inside* each module collaborate — Factory for creation, Strategy for rules, Adapter for integrations, Facade for orchestration, Singleton sparingly. Good modules make patterns simple; bad modules make patterns fight the structure.
