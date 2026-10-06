# Part III — Dependency Design and Package Principles

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
