# Part I — Organization by Flow and Unit

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
