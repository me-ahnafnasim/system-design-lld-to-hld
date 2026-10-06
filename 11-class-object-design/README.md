# 11 — Class & Object Design

> How do classes / objects collaborate internally?

GoF patterns live here. Much smaller scope than System Architecture.

## OOP pillars
- Encapsulation (hiding, access control, invariants)
- Abstraction (abstract classes, interfaces, hierarchies)
- Inheritance (single/multiple, mixins, override/overload — prefer composition)
- Polymorphism (subtype, generics, overloading, dynamic/static dispatch)

## SOLID + companions
- S/O/L/I/D + DRY, KISS, YAGNI, Composition-over-inheritance, Tell-Don't-Ask, Law of Demeter, Hollywood, Program-to-interface

## GRASP
- Expert, Creator, Low Coupling, High Cohesion, Controller, Polymorphism, Pure Fabrication, Indirection, Protected Variations

## Relationships
- Association, Aggregation (weak has-a), Composition (strong has-a), Inheritance (is-a), Realization, Dependency

## Access & encapsulation
- public / private / protected / package-private / internal, getters/setters, immutability, readonly

## GoF patterns — full notes in TypeScript

Start here: [`design-patterns-typescript/`](design-patterns-typescript/) (converted from `Design_Patterns_Study_Notes_TypeScript.docx`).

- [`design-patterns-typescript/01-creational-patterns.md`](design-patterns-typescript/01-creational-patterns.md)
- [`design-patterns-typescript/02-structural-patterns.md`](design-patterns-typescript/02-structural-patterns.md)
- [`design-patterns-typescript/03-behavioral-patterns.md`](design-patterns-typescript/03-behavioral-patterns.md)
- [`design-patterns-typescript/04-pattern-selection-guide.md`](design-patterns-typescript/04-pattern-selection-guide.md)
- [`design-patterns-typescript/05-typescript-application-guide.md`](design-patterns-typescript/05-typescript-application-guide.md)

Quick index:
- **Creational:** Singleton, Factory Method, Abstract Factory, Builder, Prototype
- **Structural:** Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy
- **Behavioral:** Chain of Responsibility, Command, Interpreter, Iterator, Mediator, Memento, Observer, State, Strategy, Template Method, Visitor

Example: `PaymentService → PaymentStrategy (Card / bKash / Bank)`.

## Anti-patterns
- God Object, spaghetti, copy-paste, magic numbers, tight coupling, feature envy, large class, long method, shotgun surgery, primitive obsession, etc.

## Metrics
- Cyclomatic / cognitive complexity, LOC, DIT, NOC, CBO, RFC, LCOM

## Checklist
- [ ] One violation→fix example per SOLID principle
- [ ] 2 patterns per GoF category working before expanding
