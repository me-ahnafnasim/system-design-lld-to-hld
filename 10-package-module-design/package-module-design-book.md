# Package / Module Design — Complete Book (Chapters 1–21 + Closing)

> There isn't one universally accepted list called "all types of Package / Module Design." Learn it in **three parts**: **1. Ways to organize, 2. Principles for boundaries, 3. Dependency rules.** This single file contains all of it.

## Contents

- [Part I — Organization by Technical Shape](#part-i--organization-by-technical-shape) — Ch 1 By Type, Ch 2 By Layer
- [Part I — Organization by Business Shape](#part-i--organization-by-business-shape) — Ch 3 Feature, Ch 4 Feature-first, Ch 5 Domain, Ch 6 Bounded Context, Ch 10 Capability, Ch 11 Subdomain
- [Part I — Organization by Flow and Unit](#part-i--organization-by-flow-and-unit) — Ch 7 Vertical Slice, Ch 12 Workflow, Ch 13 Role, Ch 8 Component, Ch 9 Service
- [Part II — Internal Structure](#part-ii--internal-structure) — Ch 14 Shared, Ch 15 Shared Kernel, Ch 16 Core, Ch 17 Plugin, Ch 18 Public API + Internal
- [Part II — Distribution](#part-ii--distribution) — Ch 19 Library, Ch 20 Monorepo
- [Part III — Dependency Design and Principles](#part-iii--dependency-design-and-package-principles) — Ch 21 + REP/CCP/CRP/ADP/SDP/SAP
- [Closing — Hierarchy, TypeScript Order, Recommended Structure](#closing--hierarchy-typescript-order-and-the-recommended-structure)

---


Chapters 1–2. Group by *what kind of code it is*, not by *what business capability it serves*. Simplest to start with, first to outgrow.

---

## Chapter 1 — Package by Type

Group files by technical type.

```text
src/
├── controllers/
├── services/
├── repositories/
├── models/
└── validators/
```

**Use when:** small application, simple CRUD, small team, framework conventions dominate.

**The problem it creates as you grow:**

```text
One feature touches:
controllers/
services/
repositories/
models/
```

One business feature gets scattered across the whole codebase. A single admission change means editing four distant folders, with merge conflicts and unclear ownership.

**Verdict:** good starting point, poor scaling story. Move to feature/domain organization once features start changing independently.

---

## Chapter 2 — Package by Layer

Group by architectural layer.

```text
src/
├── presentation/
├── application/
├── domain/
└── infrastructure/
```

Typical dependency direction:

```text
Presentation
     ↓
Application
     ↓
Domain
     ↑
Infrastructure implements interfaces
```

Concrete example:

```text
presentation/
  UserController.ts

application/
  CreateUserUseCase.ts

domain/
  User.ts

infrastructure/
  PostgresUserRepository.ts
```

**Use when:** Layered Architecture, Clean Architecture, Hexagonal-style applications, or when strong separation of concerns matters more than feature locality.

**Strength:** every file's technical responsibility is obvious; domain stays free of framework code.

**Limitation:** a large feature is still distributed across several folders — you know *where a layer lives*, but not *where a feature lives*. That is why larger codebases nest layers *inside* features (Chapter 4).

**Takeaway of Part I (technical shape):** By Type and By Layer answer "what kind of code is this?" They teach separation of concerns, but they scatter business change. The next chapters answer the more scaling-relevant question: "what business capability does this serve?"

---


Chapters 3–6, 10–11. Group by *what the business does*. This is where cohesion starts working for you instead of against you.

---

## Chapter 3 — Package by Feature

Group everything belonging to one business feature together.

```text
src/
├── users/
│   ├── UserController.ts
│   ├── UserService.ts
│   ├── UserRepository.ts
│   └── User.ts
├── orders/
└── payments/
```

**Use when:** medium/large application, business features change independently, you want strong feature ownership, modular monolith, or future extraction into services is possible.

**Why it wins:** high cohesion — files that change together stay together. An admission-policy change touches `admission/`, not four technical folders.

---

## Chapter 4 — Feature First, Layer Second

One of the strongest practical structures for larger applications.

```text
src/
├── admission/
│   ├── presentation/
│   ├── application/
│   ├── domain/
│   └── infrastructure/
├── finance/
│   ├── presentation/
│   ├── application/
│   ├── domain/
│   └── infrastructure/
└── hr/
```

So:

```text
Feature
   ↓
Layers inside feature
```

Concrete example:

```text
admission/
├── api/
│   └── AdmissionController.ts
├── application/
│   └── AdmitStudent.ts
├── domain/
│   ├── Student.ts
│   └── AdmissionRepository.ts
└── infrastructure/
    └── PostgresAdmissionRepository.ts
```

This combines business cohesion *and* architectural separation. Each feature remains independently understandable, while Clean/Hexagonal rules still hold inside it.

---

## Chapter 5 — Package by Domain

Very close to package-by-feature, but the boundary comes from the **business domain** rather than a UI feature.

```text
src/
├── admission/
├── academic/
├── finance/
├── hr/
└── payroll/
```

**Use when:** Domain-Driven Design, complex business software, ERP, CRM, financial systems, large modular monolith.

The difference that matters:

```text
Feature:
Reset Password
Student Search
Generate Report

Domain:
Identity
Admission
Finance
Academic
```

A domain is normally a larger and more stable business capability than a single feature. Features come and go; domains persist.

---

## Chapter 6 — Package by Bounded Context

The DDD version of domain modularization. Each context owns its language and model.

```text
src/
├── admission-context/
├── finance-context/
├── academic-context/
└── hr-context/
```

The same word means different things per context:

```text
Admission context:  Student = applicant/prospect
Academic context:   Student = enrolled learner
Finance context:    Student = account holder
```

**Use when:** the business model is complex, different departments use different rules, or you need strong domain boundaries that survive team growth.

---

## Chapter 10 — Package by Capability

Organize around business capabilities — "what can the business do?" rather than "what technical type is this file?"

```text
capabilities/
├── student-recruitment/
├── admissions/
├── billing/
├── scheduling/
└── employee-management/
```

Especially useful at enterprise / modular-monolith scale, where capabilities are the stable planning unit across teams and releases.

---

## Chapter 11 — Package by Subdomain

DDD divides domains into core, supporting, and generic subdomains, and packages can follow those boundaries.

```text
University
├── Admission    ← Core
├── Academic     ← Core
├── Reporting    ← Supporting
└── Authentication ← Generic
```

Invest your best modeling in core subdomains; keep supporting lean and buy/borrow generic ones where possible. Package boundaries make that investment visible.

**Takeaway:** business-shaped organization (feature → domain → context → capability) trades a little technical uniformity for a lot of change locality. If two files change for the same business reason, they belong in the same module.

---


Chapters 7–9, 12–13. Group by *how work flows* or *what deployable unit it belongs to*.

---

## Chapter 7 — Vertical Slice Design

Organize around a **use case**, not broad technical layers. One use case = one slice.

Instead of:

```text
controllers/
services/
repositories/
```

you get:

```text
features/
├── create-user/
│   ├── command.ts
│   ├── handler.ts
│   ├── validator.ts
│   └── controller.ts
├── update-user/
└── delete-user/
```

Request flow inside a slice:

```text
HTTP Request
    ↓
CreateUser slice
    ↓
Validation
    ↓
Handler
    ↓
Persistence
```

**Use when:** the application has many independent use cases, CQRS-style design, or you want changes isolated by use case. Like feature organization, it keeps feature-shaped changes localized.

---

## Chapter 12 — Package by Workflow / Process

Useful for process-heavy applications where the long-running flow matters more than entities.

```text
workflows/
├── admission-processing/
├── refund-processing/
├── employee-onboarding/
└── graduation-processing/
```

Example flow:

```text
Admission Workflow
    ↓
Application
    ↓
Document Check
    ↓
Payment
    ↓
Approval
    ↓
Enrollment
```

**Use when:** approvals, onboarding, refunds, or graduation-style pipelines dominate the domain.

---

## Chapter 13 — Package by Role / Responsibility

Group modules by responsibility — good for reusable infrastructure or cross-cutting modules.

```text
src/
├── identity/
├── authorization/
├── auditing/
├── notifications/
├── reporting/
└── persistence/
```

Keep each role's public surface narrow; otherwise it degrades into a second `shared/` folder.

---

## Chapter 8 — Package by Component

Organize around larger reusable application components, each with a public interface.

```text
src/
├── authentication/
├── notification/
├── reporting/
├── file-storage/
└── billing/
```

Conceptually:

```text
Billing Component
├── Public API
└── Internal implementation
```

Other modules should know only:

```ts
billing.charge()
```

not:

```text
billing/internal/stripe/
billing/internal/database/
billing/internal/calculator/
```

**Use when** you want explicit component boundaries with information hiding, without yet paying for separate deployments.

---

## Chapter 9 — Package by Service

Common in service-oriented or microservice-style codebases.

```text
services/
├── admission-service/
├── payment-service/
├── notification-service/
└── reporting-service/
```

Inside each service, layers reappear:

```text
payment-service/
├── api/
├── application/
├── domain/
└── infrastructure/
```

Important:

> A package/module is not automatically a microservice.

A service becomes a microservice only when it has an independent operational/deployment boundary. Until then, it is a well-isolated module — which is a good thing.

**Takeaway:** slices, workflows, components, and services are all answers to "what changes and deploys together?" Pick the unit that matches your real independence — use-case, process, component, or service — not the one that sounds most impressive.

---


Chapters 14–18. Once the outer grouping is right, design what is *public*, what is *shared*, and what stays *hidden*.

---

## Chapter 14 — Shared / Common Module

Typical shape:

```text
src/
├── admission/
├── finance/
├── hr/
└── shared/
```

But this is dangerous. `shared/` easily becomes:

```text
shared/
├── everything.ts
├── helpers.ts
├── utils.ts
├── common.ts
└── randomStuff.ts
```

Prefer named, cohesive shared modules:

```text
shared/
├── money/
├── date/
├── errors/
└── pagination/
```

Only truly shared concepts go here. Otherwise every module couples to everything.

---

## Chapter 15 — Shared Kernel

The disciplined DDD version of sharing. Only very stable concepts shared across domains belong here.

```text
shared-kernel/
├── Money.ts
├── Address.ts
├── EntityId.ts
└── DomainEvent.ts
```

Example:

```text
Admission ──┐
            ├── Money
Finance ────┘
```

Keep it small — every change affects multiple domains.

---

## Chapter 16 — Core / Infrastructure Module

Technical infrastructure reused across features:

```text
src/
├── core/
│   ├── logging/
│   ├── config/
│   ├── database/
│   └── messaging/
└── features/
```

Use for logging, config, database, and messaging plumbing. Avoid mixing business logic into `core`; it should stay boring and stable.

---

## Chapter 17 — Plugin / Extension Module Design

The core provides extension points; plugins implement them.

```text
Core
 ├── Plugin A
 ├── Plugin B
 └── Plugin C
```

Example:

```text
Payment Plugin Interface
        │
 ┌──────┼───────┐
Stripe bKash PayPal
```

**Use when:** the product needs extensibility, third parties add functionality, or integrations change frequently.

---

## Chapter 18 — Public API + Internal Module

One of the most important modularity ideas: information hiding at the folder level.

```text
payment/
├── index.ts        ← public API
├── PaymentService.ts
└── internal/
    ├── StripeAdapter.ts
    ├── Mapper.ts
    └── PaymentEntity.ts
```

Consumers import:

```ts
import { PaymentService } from "./payment";
```

never:

```ts
import { StripeAdapter } from "./payment/internal/StripeAdapter";
```

Every module gets an `index.ts` (or equivalent barrel) that is its contract. Everything else is an implementation detail you can refactor freely.

**Takeaway:** sharing is a design decision, not a folder. Share little, share stable things, and hide the rest behind a public API.

---


Chapters 19–20. When a module must live beyond one application.

---

## Chapter 19 — Library / Reusable Package

```text
packages/
├── validation/
├── logger/
├── auth-client/
└── ui-components/
```

These may ship as npm packages, Java JARs, .NET packages, or Python packages.

**Use when** functionality needs reuse across applications. At this point release and versioning concerns (SemVer, changelogs, compatibility) become part of module design — REP starts to matter as much as clean code.

---

## Chapter 20 — Monorepo Package Design

Example TypeScript monorepo:

```text
repo/
├── apps/
│   ├── web/
│   ├── admin/
│   └── api/
└── packages/
    ├── auth/
    ├── database/
    ├── contracts/
    └── ui/
```

Typical tooling: pnpm workspaces, Nx, Turborepo, npm workspaces.

The central design question becomes:

```text
Can package A depend on package B?
```

Answer it with explicit dependency rules (Part III), or the monorepo quietly becomes a distributed ball of mud.

**Takeaway:** distribution upgrades a folder into a product. Version it, document its public API, and enforce who may depend on it.

---


Chapter 21. From *how to group code* to *how packages depend on each other*.

---

## Chapter 21 — Package Dependency Design

Ideal — a directed acyclic graph:

```text
A → B → C
```

Avoid — cycles, especially direct ones:

```text
A → B
↑   ↓
└── C
```

```text
A → B
B → A
```

Circular dependencies are a warning sign of poor boundaries: two modules that cannot live without each other are usually one module.

---

## The 6 classic package design principles

Robert C. Martin's package/component principles: three for **cohesion**, three for **coupling**.

### Cohesion — REP: Reuse/Release Equivalence

> Things reused together should generally be released/versioned together.

```text
payment-sdk
├── PaymentClient
├── PaymentRequest
└── PaymentResponse
```

Don't mix unrelated reusable things into one package just because they are small.

### Cohesion — CCP: Common Closure Principle

Classes that change for the **same reason** stay together. Package-level SRP.

```text
admission/
├── Lead.ts
├── Counselling.ts
└── Visit.ts
```

If admission policy changes, these change together — so they belong together.

### Cohesion — CRP: Common Reuse Principle

Classes used together stay together. If a client needs one class but must depend on 30 unrelated ones, the package is poorly designed.

### Coupling — ADP: Acyclic Dependencies Principle

No cyclic dependency graphs. `A → B → C` is fine; `A → B → C → A` is not. Break cycles with interfaces, events, or by merging the modules.

### Coupling — SDP: Stable Dependencies Principle

Less-stable modules depend on more-stable modules, not the reverse.

```text
UI
 ↓
Application
 ↓
Domain
```

Stable business rules must not depend on rapidly changing UI/framework code.

### Coupling — SAP: Stable Abstractions Principle

Very stable packages should contain abstractions, so they evolve through implementations.

```ts
interface PaymentGateway {
  charge(): Promise<void>;
}
```

```text
StripeGateway
BkashGateway
BankGateway
```

Consumers depend on the stable interface; new gateways arrive without forcing rewrites.

**Takeaway:** REP/CCP/CRP decide *what belongs together*; ADP/SDP/SAP decide *which direction dependencies may point*. Enforce both in code review, not just in diagrams.

---


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
