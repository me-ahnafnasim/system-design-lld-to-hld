# 05 — Application Architecture

> How is one application internally organized?

## Example
```text
Admission App
Controller → Application Service → Domain → Repository
```

## Styles
- Layered, MVC, Clean, Hexagonal, Onion, Vertical Slice

## Layers
- Presentation → Application / Business Logic → Domain → Infrastructure / Persistence (DAL)

## Design approaches
- **DDD:** bounded context, ubiquitous language, context mapping, subdomains (core/supporting/generic)
- **Clean:** entities → use cases/interactors → interface adapters → frameworks/drivers
- **Hexagonal:** ports, driving (primary) vs driven (secondary) adapters

## App patterns
- DI / IoC, Repository, Unit of Work, Service Locator

## Checklist
- [ ] Dependency direction enforced (domain has no infra deps)
- [ ] Bounded contexts named
- [ ] Use-cases testable without framework

See `examples/worldconnect/` for User Service in Clean Architecture.
