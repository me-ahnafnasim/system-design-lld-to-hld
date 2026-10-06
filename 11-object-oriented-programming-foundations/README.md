# 11 — Object-Oriented Programming Foundations

> What are the language and design atoms everything above is built from?

This level teaches the **foundations**: classes, objects, members, the four pillars, relationships, and quality — before Level 12 applies SOLID/GoF on top and Level 13 drills into single functions.

## Topics (all in `oop-foundations-book.md`)

| # | Topic | One-line idea |
|---|---|---|
| 1 | OOP overview | Objects model state + behavior; four pillars |
| 2 | Class | Blueprint: fields + methods + construction rules |
| 3 | Object | Live instance with identity, state, lifecycle |
| 4 | Property / field | Stored state; prefer invariants + access control |
| 5 | Method | Behavior; command vs query, pure vs effecting |
| 6 | Constructor | Valid birth: parameters, defaults, validation, DI entry |
| 7 | Access modifiers | public / private / protected / readonly; narrowest wins |
| 8 | Encapsulation | Hide internals, expose contracts, protect invariants |
| 9 | Abstraction | Keep the what, hide the how (interfaces, base types) |
| 10 | Inheritance | is-a reuse via extension; power + coupling cost |
| 11 | Polymorphism | One interface, many behaviors (subtype/generics/overload) |
| 12 | Interface | Pure contract: what implementers must do |
| 13 | Abstract class | Partial contract + shared implementation |
| 14 | Composition | Strong has-a: owner controls lifecycle |
| 15 | Association | General "uses/knows" link between objects |
| 16 | Aggregation | Weak has-a: part outlives the whole |
| 17 | Dependency | Transient uses: parameter, return, local |
| 18 | Static members | Class-level state/behavior; shared, test with care |
| 19 | Instance members | Per-object state/behavior; the default |
| 20 | `this` | Current instance; arrow vs method binding rules |
| 21 | `super` | Parent access: constructor chaining + method reuse |
| 22 | Coupling | How much modules/classes depend on each other — minimize |
| 23 | Cohesion | How focused one unit is — maximize |
| 24 | Composition over inheritance | Prefer has-a wiring to is-a hierarchy |

## How to use

1. Read `oop-foundations-book.md` Ch 1 → 24 in order.
2. Type every example (TypeScript) — don't just read.
3. Then go to Level 12 (`../12-class-object-design/`): SOLID, GRASP, GoF patterns assume all of this.

## Checklist

- [ ] Can define class vs object vs interface vs abstract class without hesitation
- [ ] Can draw association vs aggregation vs composition vs dependency correctly
- [ ] Know when `static` helps vs harms; `this`/`super` rules clear
- [ ] Can spot high coupling / low cohesion and fix toward composition
