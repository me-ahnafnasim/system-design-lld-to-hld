# 10 — Package / Module Design

> Which code belongs together? What is public? What dependencies are allowed?

Closest to source code, above classes. If this level is wrong, even good classes become hard to change.

## The book in one paragraph

There isn't one universally accepted list called "all types of Package / Module Design." In practice, learn it in **three parts**: **1. Ways to organize packages/modules, 2. Principles for deciding boundaries, 3. Dependency rules between modules.** That is the complete picture.

## How to read this mini-book

| Part | Chapters | File |
|---|---|---|
| I — Organization: technical shape | 1. By Type · 2. By Layer | `01-part-one-organization-by-technical-shape.md` |
| I — Organization: business shape | 3. By Feature · 4. Feature-first + Layer-inside · 5. By Domain · 6. By Bounded Context · 10. By Capability · 11. By Subdomain | `02-part-one-organization-by-business-shape.md` |
| I — Organization: flow and unit | 7. Vertical Slice · 12. By Workflow · 13. By Role · 8. By Component · 9. By Service | `03-part-one-organization-by-flow-and-unit.md` |
| II — Internal structure | 14. Shared/Common · 15. Shared Kernel · 16. Core/Infrastructure · 17. Plugin · 18. Public API + Internal | `04-part-two-internal-structure.md` |
| II — Distribution | 19. Library · 20. Monorepo | `05-part-two-distribution.md` |
| III — Dependencies + principles | 21. Dependency graphs · REP/CCP/CRP/ADP/SDP/SAP | `06-part-three-dependencies-and-principles.md` |
| Closing | Hierarchy + TypeScript order + recommended structure | `07-closing-roadmap.md` |

Start at Chapter 1 and read in order the first time. Later, use Chapter 7's hierarchy diagram as your index.

## The destination (spoiler)

For a large TypeScript business application, you are heading here:

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

In one line: **Domain/Feature first → layers inside → explicit public API → controlled dependencies → minimal shared code.**

That combination connects directly to Level 11 (Class & Object Design): once module boundaries are correct, you decide how classes inside each module use Factory, Strategy, Adapter, Facade, and the other patterns.

## Checklist (use after the book)

- [ ] Organization chosen for the right reason (size, DDD, use-case independence)
- [ ] Public API minimal; internals hidden
- [ ] No cycles (ADP); stable modules don't depend on volatile ones (SDP)
- [ ] Shared code minimal and disciplined (shared-kernel, not `utils.ts` dumping ground)
