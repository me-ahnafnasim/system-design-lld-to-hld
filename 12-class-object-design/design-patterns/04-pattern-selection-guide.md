# Fast Pattern Selection Guide

| If your problem sounds like... | Start by examining... |
|---|---|
| Need one shared instance | Singleton |
| Need to choose which concrete object to create | Factory Method |
| Need matching families of products | Abstract Factory |
| Need complex step-by-step construction | Builder |
| Need clone/template-based creation | Prototype |
| Third-party/legacy API does not match | Adapter |
| Two dimensions should vary independently | Bridge |
| Tree / part-whole hierarchy | Composite |
| Add optional behavior by wrapping | Decorator |
| Hide a complicated subsystem | Facade |
| Huge number of similar objects, memory issue | Flyweight |
| Control/cached/lazy/remote access to an object | Proxy |
| Swap algorithms/rules | Strategy |
| Publish event to many subscribers | Observer |
| Queue/log/retry/undo an action | Command |
| Behavior depends heavily on lifecycle state | State |
| Same workflow skeleton, customizable steps | Template Method |
| Too many objects talk directly to each other | Mediator |
| Custom traversal | Iterator |
| Request should pass through handlers | Chain of Responsibility |
| Undo/snapshot/restore state | Memento |
| Many operations over a stable object structure | Visitor |
| Small custom language/rule grammar | Interpreter |
