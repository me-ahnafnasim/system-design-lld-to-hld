# Level 11 — OOP Foundations: The Complete Mini-Book

> **Level 11 question:** What are the language and design atoms — class, object, members, pillars, relationships — that Levels 12–13 build on?

| | |
|---|---|
| **Level** | 11 of 13 — OOP Foundations |
| **Read after** | Level 10 Package / Module Design |
| **Read before** | Level 12 Class / Object Design (SOLID, GoF) |
| **Stack** | TypeScript-first, principles language-agnostic |

## Contents

- [Part A — Building blocks (Ch 1–7, 18–21)](#part-a--building-blocks)
- [Part B — Pillars + contracts (Ch 8–13)](#part-b--pillars--contracts)
- [Part C — Relationships (Ch 14–17, 24)](#part-c--relationships)
- [Part D — Quality (Ch 22–23)](#part-d--quality)
- [Closing + bridge to Level 12](#closing--bridge-to-level-12)

---

# Part A — Building blocks

## Ch 1 — OOP overview

**Idea.** Model a system as objects holding **state** (fields) and **behavior** (methods), interacting through messages. Four pillars carry the weight: **encapsulation, abstraction, inheritance, polymorphism**.

```text
Object = identity + state + behavior
Class  = blueprint for objects with the same shape
```

**Rule:** reach for objects when you need identity + lifecycle + invariants (e.g. `Student`, `Payment`). Reach for plain functions/modules when you don't.

## Ch 2 — Class

**Idea.** A blueprint: fields, methods, construction rules, access boundaries.

```ts
class Student {
  constructor(
    public id: string,
    private gpa: number,
  ) {}
  isEligible(): boolean { return this.gpa >= 3.0; }
}
```

**Watch out:** a class with only static methods and no state is a namespace wearing a costume — use a module instead.

## Ch 3 — Object

**Idea.** A live instance: has **identity** (distinct from equal-valued twins), **state** (current field values), and a **lifecycle** (created → used → collected).

```ts
const a = new Student("s1", 3.5);
const b = new Student("s1", 3.5);
a === b; // false — different identity, same shape
```

**Rule:** identity lets you track *which* student, not just *what values*.

## Ch 4 — Property / field

**Idea.** Stored state. Every field should either uphold an invariant or be derivable — otherwise it is clutter.

```ts
class Account {
  private _balance = 0;               // invariant: never negative
  get balance(): number { return this._balance; }
  deposit(n: number): void {
    if (n <= 0) throw new Error("amount must be positive");
    this._balance += n;
  }
}
```

**Watch out:** public mutable fields leak invariants. Expose getters and intention-revealing methods instead.

## Ch 5 — Method

**Idea.** Behavior attached to an object. Prefer **commands** (change state, return little) and **queries** (return data, change nothing) kept visibly separate.

```ts
class Cart {
  private items: string[] = [];
  add(item: string): void { this.items.push(item); } // command
  count(): number { return this.items.length; }       // query
}
```

**Rule:** a query that secretly mutates is a bug factory. Pure methods (same input → same output, no side effects) are the easiest to test.

## Ch 6 — Constructor

**Idea.** The object's valid birth: take required collaborators/data, apply defaults, validate, and establish invariants — plus it is the natural **dependency-injection** entry point.

```ts
class Checkout {
  constructor(
    private gateway: PaymentGateway,   // injected, not newed inside
    private discount = 0,              // default
  ) {
    if (discount < 0 || discount > 90) throw new Error("bad discount");
  }
}
```

**Watch out:** no real work in constructors (no network, no disk). Construct fast, fail fast on invalid input, start work in explicit methods.

## Ch 7 — Access modifiers

**Idea.** Narrowest visibility that works: `public` (contract) / `private` (internals) / `protected` (extension points) / `readonly` (assign-once).

```ts
class User {
  readonly id: string;          // set once, then fixed
  private email: string;        // internals hidden
  protected role = "student";   // subclasses may adjust
  constructor(id: string, email: string) {
    this.id = id; this.email = email;
  }
  getEmail(): string { return this.email; }
}
```

**Rule:** default to `private`; promote to `protected`/`public` only with a reason. `readonly` for identity and configuration.

---

# Part B — Pillars + contracts

## Ch 8 — Encapsulation

**Idea.** Hide internals, expose contracts, protect invariants. The bundle is: private state + validated methods + minimal public surface.

```ts
class SeatMap {
  private taken = new Set<string>();
  reserve(seat: string): boolean {
    if (this.taken.has(seat)) return false;
    this.taken.add(seat); return true;
  }
}
```

Callers can't corrupt `taken` — they can only ask through `reserve`.

## Ch 9 — Abstraction

**Idea.** Keep the *what*, hide the *how*. Depend on base types and interfaces; let implementations vary behind them.

```ts
interface Notifier { send(to: string, msg: string): Promise<void>; }
async function admit(s: Student, n: Notifier): Promise<void> {
  await n.send(s.id, "Welcome!"); // works with Email/SMS/WhatsApp
}
```

**Watch out:** abstraction without variation is indirection for its own sake. Abstract when callers genuinely shouldn't know the implementation.

## Ch 10 — Inheritance

**Idea.** is-a reuse: a subclass extends a base class, inheriting members and optionally overriding behavior.

```ts
class Person { constructor(public name: string) {} }
class Teacher extends Person {
  constructor(name: string, public subject: string) { super(name); }
}
```

**Cost:** inheritance couples subclass to base-class internals (fragile base class) and fixes the taxonomy at compile time. Prefer it for true, stable is-a hierarchies with substitutable behavior (see Ch 24).

## Ch 11 — Polymorphism

**Idea.** One interface, many behaviors. Three flavors:

```ts
// 1. Subtype: same call, different behavior
const notifiers: Notifier[] = [new Email(), new Sms()];
for (const n of notifiers) await n.send("s1", "hi");

// 2. Generics (parametric): same logic, any type
function first<T>(xs: T[]): T | undefined { return xs[0]; }

// 3. Overloading (ad-hoc): same name, different signatures
function fee(base: number): number;
function fee(base: number, discount: number): number;
function fee(base: number, discount = 0): number { return base - discount; }
```

**Rule:** subtype polymorphism is the OOP workhorse; generics remove duplication without casts; overloads clarify call shapes.

## Ch 12 — Interface

**Idea.** A pure contract: *what* implementers must do, zero *how*.

```ts
interface PaymentGateway {
  charge(amount: number): Promise<void>;
}
class StripeGateway implements PaymentGateway {
  async charge(amount: number): Promise<void> { /* ... */ }
}
```

**Use when** multiple implementations must be interchangeable (Strategy, Adapter, Repository, ports in Hexagonal). A consumer depending on the interface never notices a new implementation arriving.

## Ch 13 — Abstract class

**Idea.** A partial contract *plus* shared implementation: abstract methods subclasses must fill in, concrete methods they inherit.

```ts
abstract class Importer {
  run(): void { this.validate(); this.parse(); this.save(); }
  protected validate(): void { /* shared */ }
  protected save(): void { /* shared */ }
  protected abstract parse(): void; // subclasses decide
}
```

**Interface vs abstract class:** interface = "can-do" across unrelated types, no code reuse; abstract class = "is-a" family with genuine shared logic. When in doubt, start with an interface — add the abstract base only once duplication proves it.

---

# Part C — Relationships

> Read associations as: **association** (knows) → **aggregation** (has, loosely) → **composition** (owns) → **dependency** (uses briefly).

## Ch 14 — Composition (strong has-a)

**Idea.** Owner controls the part's lifecycle: part created with, dies with, the whole. Filled diamond in UML.

```ts
class Engine { start(): void {} }
class Car {
  private engine = new Engine(); // born and dies with the car
  start(): void { this.engine.start(); }
}
```

**Use when** the part has no meaningful independent life (`Car`/`Engine`, `Order`/`OrderLine`).

## Ch 15 — Association (knows/uses)

**Idea.** The general link: one object knows another and calls it over time. Plain line in UML; add arrows/multiplicity as needed.

```ts
class Teacher {
  private students: Student[] = []; // teacher knows students
  enroll(s: Student): void { this.students.push(s); }
}
```

Every other relationship below is a *specialized* association — name the strongest one that holds.

## Ch 16 — Aggregation (weak has-a)

**Idea.** Whole-has-part, but the part **outlives** the whole. Hollow diamond in UML.

```ts
class Department {
  constructor(private members: Teacher[]) {} // teachers exist before/after
}
```

`Department`/`Teacher`, `Team`/`Player`: disband the whole, members continue. If destroying the whole must destroy the part, that's composition (Ch 14), not this.

## Ch 17 — Dependency (uses briefly)

**Idea.** Transient use: parameter, return type, or local — no stored reference. Dashed arrow in UML.

```ts
class ReportService {
  build(student: Student, fmt: Formatter): string { // uses, doesn't keep
    return fmt.format(student);
  }
}
```

**Rule:** dependencies should point toward stable abstractions (interfaces), keeping coupling low and direction acyclic.

## Ch 24 — Composition over inheritance

**Idea.** Prefer has-a wiring (composition + interfaces + DI) to is-a hierarchy. Behavior comes from collaborators you can swap at runtime, not ancestors fixed at compile time.

```ts
// ❌ brittle: every combination needs a subclass
class EmailAlert extends Alert {}
class SmsAlert extends Alert {}

// ✅ flexible: vary channel by wiring
class Alert {
  constructor(private channel: Channel) {}
  send(msg: string): void { this.channel.send(msg); }
}
```

**When inheritance still wins:** genuinely stable is-a taxonomies with substitutable behavior and shared implementation (then an abstract base, Ch 13, earns its keep). Otherwise compose.

---

# Part D — Quality

## Ch 22 — Coupling

**Idea.** How much one unit depends on another's internals. Spectrum: content (worst — reaches into privates) → common (shared globals) → control (flags steering callees) → stamp (whole structs passed) → data (best — simple values via narrow contracts).

```ts
// ❌ high coupling: caller steers internals with flags
render(user, /*isAdmin*/ true, /*verbose*/ false, /*cached*/ true);

// ✅ low coupling: narrow contract, simple data
renderProfile({ id: user.id });
```

**Fixes:** depend on interfaces, pass data not control, hide internals (Ch 18 of Level 10), break cycles with events.

## Ch 23 — Cohesion

**Idea.** How focused one unit is — do its pieces serve one purpose? Spectrum (best first): functional → sequential → communicational → procedural → temporal → logical → coincidental (worst: `utils.ts` grab-bag).

```ts
// ❌ coincidental: unrelated helpers sharing a file
export function tax(a: number): number { /* ... */ }
export function email(s: string): boolean { /* ... */ }
export function shuffle<T>(xs: T[]): T[] { /* ... */ }

// ✅ functional: one purpose, shared reason to change
export class Money {
  constructor(private cents: number) {}
  add(o: Money): Money { return new Money(this.cents + o.cents); }
}
```

**Rule:** files that change for the same reason stay together (CCP); one module = one reason to change.

---

# Closing — bridge to Level 12

You now own the atoms: birth (constructor), shape (class/interface/abstract), behavior (methods, `this`/`super`, static vs instance), hiding (access, encapsulation, abstraction), reuse (inheritance/polymorphism, preferably composition), links (association → aggregation → composition → dependency), and health (coupling down, cohesion up).

Level 12 (`../12-class-object-design/`) assumes all of this and adds judgment: SOLID violations→fixes, GRASP assignment, full GoF catalog in `design-patterns/`, anti-patterns, and metrics. If any Level 12 sentence confuses you, the missing atom is in the chapter table at the top of this level's `README.md`.

## Final checklist

- [ ] Class vs object vs interface vs abstract class definable cold
- [ ] Association / aggregation / composition / dependency drawable with lifecycle rule
- [ ] static vs instance, `this` vs `super` rules clear
- [ ] Coupling minimized (data > stamp > control), cohesion functional
- [ ] Default to composition; inherit only stable is-a families
