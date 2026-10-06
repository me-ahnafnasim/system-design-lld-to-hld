# Applying GoF Patterns in Modern TypeScript

TypeScript supports classes, interfaces, generics, composition, modules, functions, closures, iterables, decorators (language/framework dependent), and structural typing. Therefore a GoF pattern does not always need the class-heavy form shown in older Java/C++ examples.

Prefer the simplest implementation that preserves the pattern's intent. A factory can be a function. A strategy can be a function object. A singleton-like shared service may be an ES module export. An observer may be an event emitter. An iterator can use `Symbol.iterator`.

Patterns are architectural vocabulary, not mandatory class diagrams. Learn the intent and trade-off first; syntax comes second.

## Recommended Learning Order

| Stage | Patterns |
|---|---|
| Start first | Strategy, Factory Method, Adapter, Facade, Observer, Decorator |
| Then learn | Builder, State, Command, Chain of Responsibility, Composite, Proxy, Template Method |
| Then advanced/situational | Abstract Factory, Bridge, Mediator, Memento, Prototype, Iterator |
| Learn last / specialized | Flyweight, Visitor, Interpreter, Singleton trade-offs |

## Practice: Identify the Pattern

- You have three scholarship algorithms and want to switch them at runtime. → Strategy
- A legacy payment SDK exposes `charge()`, but your application expects `pay()`. → Adapter
- One admission event should trigger email, analytics, and CRM updates. → Observer
- Your application workflow changes drastically between New, Visited, Admitted, and Closed. → State
- You want one simple `admitStudent()` method to coordinate six internal services. → Facade
- You want authentication, permissions, validation, and rate limiting to process a request in sequence. → Chain of Responsibility
- You need to create related WebButton/WebDialog or MobileButton/MobileDialog families. → Abstract Factory
- You want to add retry and logging around an API service without modifying the service. → Decorator
