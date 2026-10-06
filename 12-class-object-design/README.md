# 12 — Class & Object Design

> How do classes / objects collaborate internally?

GoF patterns live here. Much smaller scope than System Architecture. Assumes Level 11 atoms (class, object, interface, relationships, coupling/cohesion).

## How to use this folder (guide)

All notes are flat `.md` files — no subfolders.

| File | What it is |
|---|---|
| `01-creational-patterns.md` | Singleton, Factory Method, Abstract Factory, Builder, Prototype |
| `02-structural-patterns.md` | Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy |
| `03-behavioral-patterns.md` | Strategy, Observer, Command, State, Template Method, Mediator, Iterator, Chain of Responsibility, Memento, Visitor, Interpreter |
| `04-pattern-selection-guide.md` | Fast lookup: problem → pattern |
| `05-applying-patterns-in-code.md` | Modern usage + learning order + practice |
| `REFERENCES.md` | Verification sources |

**Instructions:**
1. Read Big Picture + Why + How to Decide below.
2. Work `01 → 02 → 03` (2 patterns per category first, then the rest).
3. Use `04` when stuck on selection; `05` for learning order and self-test.
4. Prereq: Level 11 (`../11-object-oriented-programming-foundations/`) for class/object/interface/relationship basics.

Source: converted from `Design_Patterns_Study_Notes_TypeScript.docx` — examples in TypeScript, principles language-agnostic.

## 1. The Big Picture

> A design pattern is not a library or copy-paste solution. It is a reusable design approach for a recurring problem. The same pattern looks different in TypeScript, Java, C#, Python, or PHP.

| Category | Question it answers | Patterns |
|---|---|---|
| Creational | How do I create objects? | Singleton, Factory Method, Abstract Factory, Builder, Prototype |
| Structural | How do I compose objects into larger structures? | Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy |
| Behavioral | How do objects communicate and share responsibility? | Strategy, Observer, Command, State, Template Method, Mediator, Iterator, Chain of Responsibility, Memento, Visitor, Interpreter |

> Do not start by using all 23 patterns. Start with the problem — add a pattern only when it reduces real coupling, duplication, complexity, or change risk.

## 2. Why learn patterns?

- Recognize recurring problems ("this is a Strategy problem", "this needs an Adapter") instead of inventing structure from zero.
- Shared vocabulary: "wrap it with a Decorator" beats a long custom explanation.
- Reduce coupling: Factory Method separates creation from usage; Strategy separates algorithms from callers.
- Design for change: new implementations, behaviors, integrations, workflow states without rewrites.
- Frameworks click faster: DI containers, events, middleware, iterators, proxies, adapters, factories.
- Trade-offs over dogma: every pattern adds structure — learn when it pays and when a plain function/class wins.

## 3. How to decide which pattern to use

1. Object creation is the problem → Creational.
2. Classes/objects don't fit cleanly → Structural.
3. Behavior, events, workflows, responsibility flow → Behavioral.
4. One simple implementation unlikely to change → maybe no pattern.
5. Growing if/else or switch on types/states/algorithms → inspect Factory, Strategy, State, Command, Chain of Responsibility.
6. God object forming → consider Facade, Mediator, Strategy, Command, or extracting responsibilities.

> Rule of thumb: justified if it removes repeated conditionals, isolates a volatile dependency, clarifies ownership, or makes extension safer. Otherwise keep it simpler.

## OOP pillars (Level 11 recap)

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

Example: `PaymentService → PaymentStrategy (Card / bKash / Bank)`.

## Anti-patterns

- God Object, spaghetti, copy-paste, magic numbers, tight coupling, feature envy, large class, long method, shotgun surgery, primitive obsession, etc.

## Metrics

- Cyclomatic / cognitive complexity, LOC, DIT, NOC, CBO, RFC, LCOM

## Checklist

- [ ] One violation→fix example per SOLID principle
- [ ] 2 patterns per GoF category working before expanding
