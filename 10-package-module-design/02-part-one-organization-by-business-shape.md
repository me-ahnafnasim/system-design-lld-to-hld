# Part I — Organization by Business Shape

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
