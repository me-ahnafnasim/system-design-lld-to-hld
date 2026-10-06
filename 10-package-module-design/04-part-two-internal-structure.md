# Part II — Internal Structure

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
