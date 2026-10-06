# Design Patterns Study Notes

Creational • Structural • Behavioral — examples in TypeScript, principles language-agnostic.

> Purpose: understand why each pattern exists, when to choose it, how to apply it, and when not to use it.

Source: `Design_Patterns_Study_Notes_TypeScript.docx` (full conversion — no content dropped).

## Contents

- [`01-creational-patterns.md`](01-creational-patterns.md) — Singleton, Factory Method, Abstract Factory, Builder, Prototype
- [`02-structural-patterns.md`](02-structural-patterns.md) — Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy
- [`03-behavioral-patterns.md`](03-behavioral-patterns.md) — Strategy, Observer, Command, State, Template Method, Mediator, Iterator, Chain of Responsibility, Memento, Visitor, Interpreter
- [`04-pattern-selection-guide.md`](04-pattern-selection-guide.md) — fast lookup table
- [`05-applying-patterns-in-code.md`](05-applying-patterns-in-code.md) — modern language usage + learning order + practice
- [`REFERENCES.md`](REFERENCES.md) — verification sources

## 1. The Big Picture

> Core idea: A design pattern is not a library or a copy-paste solution. It is a reusable design approach for a recurring software design problem. The same pattern can be implemented differently in TypeScript, Java, C#, Python, PHP, and other languages.

| Category | Question it answers | Patterns |
|---|---|---|
| Creational | How do I create objects? | Singleton, Factory Method, Abstract Factory, Builder, Prototype |
| Structural | How do I compose objects into larger structures? | Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy |
| Behavioral | How do objects communicate and share responsibility? | Strategy, Observer, Command, State, Template Method, Mediator, Iterator, Chain of Responsibility, Memento, Visitor, Interpreter |

> Important: Do not start a project by trying to use all 23 patterns. Start with the problem. Introduce a pattern only when it reduces a real source of coupling, duplication, complexity, or change risk.

## 2. Why Learn Design Patterns?

- You learn to recognize recurring design problems. Instead of inventing a structure from zero every time, you can recognize "this is a Strategy problem" or "this needs an Adapter."
- You improve communication with other developers. Pattern names act as compact architectural vocabulary. Saying "wrap it with a Decorator" communicates more than a long custom explanation.
- You reduce coupling. Many patterns separate code that changes from code that should stay stable — for example, Factory Method separates creation from usage, and Strategy separates an algorithm from the caller.
- You design for change. Patterns are useful when software must support new implementations, behaviors, integrations, or workflow states without rewriting large sections.
- You understand frameworks more easily. Dependency injection containers, event systems, middleware pipelines, UI trees, iterators, proxies, adapters, and factories are all easier to reason about after patterns.
- You learn trade-offs, not just "best practices." Every pattern adds structure. The skill is knowing when that structure is justified and when a simple function or class is better.

## 3. How to Decide Which Pattern to Use

1. Object creation is the problem: Look at Creational patterns.
2. Classes/objects do not fit together cleanly: Look at Structural patterns.
3. Behavior changes, events, workflows, or responsibility flow are the problem: Look at Behavioral patterns.
4. There is only one simple implementation and it is unlikely to change: You may not need a pattern at all.
5. You are adding many if/else or switch branches for types/states/algorithms: That is often a signal to inspect Factory, Strategy, State, Command, or Chain of Responsibility.
6. A class is becoming a "god object" that knows too much: Consider Facade, Mediator, Strategy, Command, or extracting responsibilities.

> Rule of thumb: If a pattern removes repeated conditionals, isolates a volatile dependency, clarifies ownership, or makes extension safer, it may be justified. If it only adds extra files/classes without solving a real change problem, keep the design simpler.
