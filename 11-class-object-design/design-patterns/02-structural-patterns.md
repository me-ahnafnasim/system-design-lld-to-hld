# Structural Patterns

> Focus: arranging classes and objects so larger systems remain flexible, compatible, and understandable.

## Adapter

What it does: Converts one interface into another interface expected by the client.

Use it when: You must integrate legacy code, a third-party API, SDK, or service whose interface does not match your application contract.

Example: Your app expects `PaymentGateway.pay()`, but a provider exposes `createCharge()`.

```ts
class StripeAdapter implements PaymentGateway {
  constructor(private stripe: StripeSdk) {}
  pay(amount: number): unknown { return this.stripe.createCharge(amount); }
}
```

Watch out: Do not hide major semantic differences behind a misleading adapter. Interface conversion should preserve meaning.

## Bridge

What it does: Separates an abstraction from its implementation so both can vary independently.

Use it when: You have two dimensions of variation that would otherwise create a subclass explosion.

Example: Notifications vary by type (Alert/Reminder) and delivery channel (Email/SMS/WhatsApp).

```ts
class Alert {
  constructor(private channel: Channel) {}
  send(msg: string): void { this.channel.send(msg); }
}
```

Watch out: Bridge can be overengineering when only one dimension actually varies.

## Composite

What it does: Treats individual objects and groups of objects uniformly, usually in a tree structure.

Use it when: Your domain naturally forms part-whole hierarchies and operations should work on both leaves and containers.

Example: File and Folder both implement `size()`; Folder sums the sizes of children.

```ts
interface Node { size(): number }
class Folder implements Node {
  children: Node[] = [];
  size(): number { return this.children.reduce((n, c) => n + c.size(), 0); }
}
```

Watch out: Some operations make sense only for containers or leaves; forcing perfect uniformity can produce awkward APIs.

## Decorator

What it does: Adds behavior to an object by wrapping it, without changing the original class.

Use it when: Features must be combined dynamically and subclassing every combination would explode.

Example: Wrap a notification with logging, retry, encryption, or analytics decorators.

```ts
const service = new RetryDecorator(
  new LoggingDecorator(new ApiService())
);
```

Watch out: Many nested decorators can make runtime behavior difficult to trace. Keep responsibilities small and naming explicit.

## Facade

What it does: Provides a simple entry point over a complicated subsystem.

Use it when: Clients need a small, stable workflow while the subsystem contains many services/classes.

Example: `AdmissionFacade.admitStudent()` coordinates payment, document check, student creation and email.

```ts
class AdmissionFacade {
  admit(data: Input): void {
    // coordinate multiple subsystems
  }
}
```

Watch out: A Facade should simplify orchestration, not become a giant class containing all business logic.

## Flyweight

What it does: Shares common immutable state among many fine-grained objects to reduce memory use.

Use it when: You have a very large number of similar objects and profiling shows duplicated state is a meaningful memory cost.

Example: A map has 100,000 tree markers, but each species shares one `TreeType` object containing name/icon/color.

```ts
const type = TreeTypeFactory.get('Mango');
new Tree(x, y, type); // x/y are unique, type is shared
```

Watch out: Use only after a real memory/performance need is measured. Modern applications rarely need it for ordinary object counts.

## Proxy

What it does: Places a stand-in object in front of a real object to control access or add behavior around access.

Use it when: You need lazy loading, access control, caching, remote access, monitoring, or request interception while preserving the same interface.

Example: A `CachedUserRepositoryProxy` checks cache before calling the real API repository.

```ts
class CachedRepo implements UserRepo {
  constructor(private real: UserRepo) {}
  async get(id: string): Promise<User> { /* cache -> real */ return this.real.get(id); }
}
```

Watch out: Do not confuse Proxy with Decorator: both wrap, but Proxy primarily controls access; Decorator primarily adds responsibilities.
