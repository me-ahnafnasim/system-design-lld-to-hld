# Part I — Organization by Technical Shape

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
