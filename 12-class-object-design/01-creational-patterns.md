# Creational Patterns

> Focus: controlling object creation so client code is less coupled to concrete construction details.

## Singleton

What it does: Guarantees one shared instance of a class and exposes a controlled access point to it.

Use it when: A genuinely shared resource or coordinator should have one application-level instance — for example a process-wide logger/config registry. In modern TypeScript, dependency injection or module exports often replace it.

Example: A `Logger` used throughout one server process.

```ts
class Logger {
  private static instance: Logger;
  private constructor() {}
  static getInstance(): Logger {
    return this.instance ??= new Logger();
  }
}
const logger = Logger.getInstance();
```

Watch out: It creates global state and can make tests and dependencies harder to reason about. Do not use it just because "one object feels convenient."

## Factory Method

What it does: Moves object creation behind a method so client/business code depends on an interface rather than a concrete class.

Use it when: The exact implementation may vary, or a framework/library should let subclasses or extensions decide what concrete product to create.

Example: A notification creator returns `EmailNotification` or `SmsNotification` while the sender works only with `Notification`.

```ts
interface Notification { send(): void }

abstract class Creator {
  abstract create(): Notification;
  notify(): void { this.create().send(); }
}
```

Watch out: If there are only one or two stable concrete types, a simple constructor call or small factory function may be clearer.

## Abstract Factory

What it does: Creates families of related objects while hiding their concrete classes.

Use it when: Several related products must stay compatible as a family, and you may switch the whole family together.

Example: A UI theme factory creates matching `Button`, `Dialog`, and `Checkbox` components for Web and Mobile.

```ts
interface UIFactory {
  button(): Button;
  dialog(): Dialog;
}
const ui = platformFactory();
ui.button().render();
```

Watch out: It introduces many interfaces/classes. Avoid it when products do not form stable families.

## Builder

What it does: Constructs a complex object step by step, usually with readable chained methods or a director.

Use it when: An object has many optional fields, validation rules, construction steps, or multiple representations.

Example: Building an `AdmissionApplication` with program, campus, documents, scholarship and guardian data.

```ts
const application = new ApplicationBuilder()
  .program('CSE')
  .campus('Mohakhali')
  .scholarship(25)
  .build();
```

Watch out: TypeScript object literals often replace Builder for simple data objects. Use Builder when construction logic is genuinely complex.

## Prototype

What it does: Creates new objects by copying/cloning an existing configured object.

Use it when: Creating an object from scratch is expensive or repetitive and a configured template is a useful starting point.

Example: Clone a preconfigured report template and change only title/date/filters.

```ts
const springReport = baseReport.clone();
springReport.title = 'Spring 2027';
```

Watch out: Cloning nested mutable objects can cause shallow-copy bugs. Define clear copy semantics.
