# Level 10 — Package / Module Design: The Complete Mini-Book

> **Level 10 question:** Which code belongs together? What is public? What dependencies are allowed?

| | |
|---|---|
| **Level** | 10 of 12 — Package / Module Design |
| **Read after** | Level 09 Component/Service Architecture |
| **Read before** | Level 11 Class / Object Design |
| **Time** | ~45 min cover-to-cover, ~10 min via Closing index |
| **Stack** | TypeScript-first, language-agnostic principles |

There isn't one universally accepted list called "all types of Package / Module Design." Learn it in **three parts**:

1. **Ways to organize** packages/modules (Ch 1–13)
2. **Internal structure + distribution** — public API, sharing, libraries, monorepo (Ch 14–20)
3. **Dependency rules + principles** — graphs, REP/CCP/CRP/ADP/SDP/SAP (Ch 21)

## How to use this file

- **First read:** Chapters 1 → 21 in order. Each chapter follows the same template: *Idea → Structure → Use when → Watch out*.
- **Later:** jump via the [Decision guide](#decision-guide--which-organization-when) or the [Practical hierarchy](#practical-hierarchy--your-index).
- **Practice:** after each Part, apply its checklist to one real folder in your codebase before moving on.

## Contents

- [Part I — Organization (Ch 1–13)](#part-i--organization-ch-113)
- [Part II — Internal Structure + Distribution (Ch 14–20)](#part-ii--internal-structure--distribution-ch-1420)
- [Part III — Dependencies + Principles (Ch 21)](#part-iii--dependencies--principles-ch-21)
- [Decision guide](#decision-guide--which-organization-when)
- [Practical hierarchy + TypeScript order](#practical-hierarchy--your-index)
- [Recommended structure + bridge to Level 11](#recommended-structure-for-large-typescript-apps)
- [Checklist](#final-checklist)

---

# Part I — Organization (Ch 1–13)

> Grouping rule: if two files change for the same business reason, they belong in the same module.

## Ch 1 — Package by Type

**Idea.** Group files by technical type.

```text
src/
├── controllers/
├── services/
├── repositories/
├── models/
└── validators/
```

**Use when:** small app, simple CRUD, small team, framework conventions dominate.

**Watch out:** one feature scatters across the codebase:

```text
One feature touches: controllers/ + services/ + repositories/ + models/
```

An admission change means four distant edits, merge conflicts, unclear ownership. **Verdict:** good start, poor scaling story.

## Ch 2 — Package by Layer

**Idea.** Group by architectural layer, with strict dependency direction.

```text
src/
├── presentation/
├── application/
├── domain/
└── infrastructure/
```

```text
Presentation → Application → Domain ← Infrastructure (implements interfaces)
```

```text
presentation/UserController.ts
application/CreateUserUseCase.ts
domain/User.ts
infrastructure/PostgresUserRepository.ts
```

**Use when:** Layered / Clean / Hexagonal apps where separation of concerns matters most.

**Watch out:** you know *where a layer lives* but not *where a feature lives*. Large features still span four folders. Fix: nest layers *inside* features (Ch 4).

## Ch 3 — Package by Feature

**Idea.** Group everything one business feature needs.

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

**Use when:** medium/large app, features change independently, feature ownership, modular monolith, possible future service extraction.

**Why it wins:** high cohesion — files that change together stay together.

## Ch 4 — Feature First, Layer Second

**Idea.** The strongest default for larger apps: feature outside, layers inside.

```text
src/
├── admission/
│   ├── presentation/
│   ├── application/
│   ├── domain/
│   └── infrastructure/
├── finance/
└── hr/
```

```text
admission/
├── api/AdmissionController.ts
├── application/AdmitStudent.ts
├── domain/{Student.ts, AdmissionRepository.ts}
└── infrastructure/PostgresAdmissionRepository.ts
```

Combines business cohesion *and* Clean/Hexagonal discipline. Each feature stays independently understandable.

## Ch 5 — Package by Domain

**Idea.** Like feature, but boundaries come from the **business domain**, not a UI feature.

```text
src/
├── admission/
├── academic/
├── finance/
├── hr/
└── payroll/
```

```text
Feature: Reset Password, Student Search, Generate Report
Domain:  Identity, Admission, Finance, Academic
```

**Use when:** DDD, ERP/CRM/finance, large modular monolith. Features come and go; domains persist.

## Ch 6 — Package by Bounded Context

**Idea.** DDD-grade domains: each context owns its language and model.

```text
src/
├── admission-context/
├── finance-context/
├── academic-context/
└── hr-context/
```

```text
Admission: Student = applicant/prospect
Academic:  Student = enrolled learner
Finance:   Student = account holder
```

**Use when:** complex model, departments use different rules, boundaries must survive team growth.

## Ch 7 — Vertical Slice Design

**Idea.** Organize around a **use case**. One slice = one end-to-end flow.

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

```text
HTTP → CreateUser slice → Validation → Handler → Persistence
```

**Use when:** many independent use cases, CQRS-style design, change isolation by use case.

## Ch 8 — Package by Component

**Idea.** Larger reusable components with an explicit public interface.

```text
src/
├── authentication/
├── notification/
├── reporting/
├── file-storage/
└── billing/
```

```text
Billing Component
├── Public API: billing.charge()
└── Internal: stripe/, database/, calculator/ (hidden)
```

**Use when** you want information hiding and explicit boundaries without separate deployments.

## Ch 9 — Package by Service

**Idea.** Service-shaped modules; layers reappear inside each one.

```text
services/
├── admission-service/
├── payment-service/
├── notification-service/
└── reporting-service/
```

```text
payment-service/
├── api/
├── application/
├── domain/
└── infrastructure/
```

> A package is not automatically a microservice. It becomes one only with an independent deployment boundary. Until then it is a well-isolated module — which is good.

## Ch 10 — Package by Capability

**Idea.** Organize around *"what can the business do?"* instead of *"what technical type is this?"*

```text
capabilities/
├── student-recruitment/
├── admissions/
├── billing/
├── scheduling/
└── employee-management/
```

Best at enterprise / modular-monolith scale, where capabilities are the stable planning unit.

## Ch 11 — Package by Subdomain

**Idea.** Let DDD subdomains draw the lines; invest accordingly.

```text
University
├── Admission       ← Core (best modeling here)
├── Academic        ← Core
├── Reporting       ← Supporting (keep lean)
└── Authentication  ← Generic (buy/borrow)
```

## Ch 12 — Package by Workflow / Process

**Idea.** For process-heavy apps, the flow is the module.

```text
workflows/
├── admission-processing/
├── refund-processing/
├── employee-onboarding/
└── graduation-processing/
```

```text
Admission: Application → Document Check → Payment → Approval → Enrollment
```

**Use when** approvals, onboarding, refunds, or graduation pipelines dominate.

## Ch 13 — Package by Role / Responsibility

**Idea.** Group by responsibility; ideal for reusable infrastructure.

```text
src/
├── identity/
├── authorization/
├── auditing/
├── notifications/
├── reporting/
└── persistence/
```

Keep each role's public surface narrow or it rots into a second `shared/` folder.

---

# Part II — Internal Structure + Distribution (Ch 14–20)

> Outer grouping answers "what belongs together." This part answers "what is public, what is shared, what ships."

## Ch 14 — Shared / Common Module

```text
src/
├── admission/
├── finance/
├── hr/
└── shared/
```

Danger: `shared/` becomes `everything.ts / helpers.ts / utils.ts / common.ts / randomStuff.ts`. Prefer named, cohesive modules:

```text
shared/
├── money/
├── date/
├── errors/
└── pagination/
```

Only truly shared concepts. Otherwise every module couples to everything.

## Ch 15 — Shared Kernel

Disciplined DDD sharing — only very stable cross-domain concepts:

```text
shared-kernel/
├── Money.ts
├── Address.ts
├── EntityId.ts
└── DomainEvent.ts
```

```text
Admission ──┐
            ├── Money
Finance ────┘
```

Keep it small. Every change ripples across domains.

## Ch 16 — Core / Infrastructure Module

Boring, stable technical plumbing reused across features:

```text
src/
├── core/
│   ├── logging/
│   ├── config/
│   ├── database/
│   └── messaging/
└── features/
```

Never mix business logic into `core`.

## Ch 17 — Plugin / Extension Modules

Core defines extension points; plugins implement them.

```text
Core
 ├── Plugin A
 ├── Plugin B
 └── Plugin C
```

```text
Payment Plugin Interface → Stripe | bKash | PayPal
```

**Use when** third parties extend the product or integrations churn frequently.

## Ch 18 — Public API + Internal Module

The most important modularity habit: hide implementation behind a barrel.

```text
payment/
├── index.ts              ← public API (the contract)
├── PaymentService.ts
└── internal/
    ├── StripeAdapter.ts
    ├── Mapper.ts
    └── PaymentEntity.ts
```

```ts
import { PaymentService } from "./payment";            // ✅
import { StripeAdapter } from "./payment/internal/StripeAdapter"; // ❌
```

Everything outside `index.ts` is refactorable without breaking consumers.

## Ch 19 — Library / Reusable Package

```text
packages/
├── validation/
├── logger/
├── auth-client/
└── ui-components/
```

Ships as npm / JAR / .NET / PyPI. From here, SemVer, changelogs, and compatibility are part of module design.

## Ch 20 — Monorepo Package Design

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

Tooling: pnpm workspaces, Nx, Turborepo, npm workspaces. Central question: *"Can package A depend on package B?"* Answer with Part III rules or the monorepo becomes a distributed ball of mud.

> Distribution upgrades a folder into a product: version it, document its API, enforce its dependents.

---

# Part III — Dependencies + Principles (Ch 21)

## Ch 21 — Package Dependency Design

Ideal — a directed acyclic graph:

```text
A → B → C
```

Avoid — cycles:

```text
A → B → C → A   (or A ⇄ B)
```

Two modules that cannot live without each other are usually one module. Break cycles with interfaces, events, or a merge.

## The 6 classic principles (Robert C. Martin)

| Group | Principle | One-line rule |
|---|---|---|
| Cohesion | **REP** — Reuse/Release Equivalence | Things reused together ship together (`payment-sdk`: Client + Request + Response) |
| Cohesion | **CCP** — Common Closure | Things changing for the same reason stay together (`admission/`: Lead, Counselling, Visit) |
| Cohesion | **CRP** — Common Reuse | Things used together stay together; don't force 30 deps for 1 class |
| Coupling | **ADP** — Acyclic Dependencies | No cycles, ever |
| Coupling | **SDP** — Stable Dependencies | Volatile → stable (`UI → Application → Domain`), never reverse |
| Coupling | **SAP** — Stable Abstractions | Stable packages expose interfaces (`PaymentGateway`), implementations vary (`Stripe/Bkash/BankGateway`) |

```ts
interface PaymentGateway {
  charge(): Promise<void>;
}
```

REP/CCP/CRP decide **what belongs together**; ADP/SDP/SAP decide **which way dependencies point**. Enforce both in review, not just diagrams.

---

# Decision guide — which organization, when

| Situation | Start with |
|---|---|
| Small CRUD, small team | Ch 1 By Type → Ch 2 By Layer |
| Features change independently | Ch 3 By Feature |
| Large app needing both cohesion + layers | Ch 4 Feature-first, Layer-inside |
| Complex business, DDD/ERP | Ch 5 Domain → Ch 6 Bounded Context |
| Many independent use cases / CQRS | Ch 7 Vertical Slice |
| Explicit boundaries, no extra deploys | Ch 8 Component |
| Service/deployment boundary real | Ch 9 Service |
| Enterprise planning unit | Ch 10 Capability / Ch 11 Subdomain |
| Process-heavy flows | Ch 12 Workflow |
| Shared infra plumbing | Ch 13 Role / Ch 16 Core |

---

# Practical hierarchy — your index

```text
PACKAGE / MODULE DESIGN
├── 1. ORGANIZATION
│   ├── By Type · By Layer · By Feature · By Domain
│   ├── By Bounded Context · By Capability · By Component
│   ├── By Service · By Workflow · Vertical Slice
├── 2. INTERNAL STRUCTURE
│   ├── Public API · Internal impl · Shared Kernel · Core · Plugins
├── 3. DISTRIBUTION
│   ├── Internal module · Library · npm/package · Monorepo · Deployable service
└── 4. DESIGN PRINCIPLES
    ├── High Cohesion · Low Coupling
    ├── REP · CCP · CRP · ADP · SDP · SAP
```

## Learn in this TypeScript order

```text
1. By Type → 2. By Layer → 3. By Feature → 4. Feature-first + Layer-inside
→ 5. Domain → 6. Public API/Internal → 7. Shared vs Shared Kernel
→ 8. Vertical Slice → 9. Bounded Context → 10. Monorepo
→ 11. Dependency graphs → 12. REP/CCP/CRP → 13. ADP/SDP/SAP
```

---

# Recommended structure for large TypeScript apps

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

> **Domain/Feature first → layers inside → explicit public API → controlled dependencies → minimal shared code.**

## Bridge to Level 11

Correct module boundaries make class design easy: Factory for creation, Strategy for rules, Adapter for integrations, Facade for orchestration inside each module. Bad modules make patterns fight the structure.

---

# Final checklist

- [ ] Organization fits size and domain (not copied from a blog)
- [ ] Public API minimal (`index.ts` contract); internals hidden
- [ ] No cycles (ADP); dependencies point toward stability (SDP/SAP)
- [ ] Shared code minimal, named, and versioned — never a `utils.ts` dump
- [ ] One module = one reason to change (CCP); consumers don't over-depend (CRP/REP)

See also: `REFERENCES.md` for sources, `../11-class-object-design/` for what lives *inside* each module.
