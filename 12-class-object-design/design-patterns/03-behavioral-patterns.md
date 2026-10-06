# Behavioral Patterns

> Focus: organizing algorithms, communication, workflows, events, and responsibility between objects.

## Strategy

What it does: Encapsulates interchangeable algorithms behind one interface.

Use it when: The same task has multiple algorithms/rules and the caller should select or swap them without condition-heavy code.

Example: Admission scholarship calculation differs for ResultBased, CampaignBased, and EmployeeReferral.

```ts
interface DiscountStrategy { calc(fee: number): number }
class Checkout {
  constructor(private strategy: DiscountStrategy) {}
  total(fee: number): number { return this.strategy.calc(fee); }
}
```

Watch out: If the behavior is trivial and never changes, separate strategy classes can add unnecessary indirection.

## Observer

What it does: Lets subscribers register for notifications when a subject/event changes.

Use it when: One event should trigger multiple independent reactions without the publisher knowing each concrete receiver.

Example: When `StudentAdmitted` occurs: send email, update analytics, issue ID request, notify CRM.

```ts
eventBus.on('student.admitted', sendEmail);
eventBus.on('student.admitted', updateAnalytics);
eventBus.emit('student.admitted', student);
```

Watch out: Large event systems can create hidden control flow. Use clear event names, ownership, logging, and failure rules.

## Command

What it does: Wraps an action/request as an object so it can be queued, logged, retried, scheduled, or undone.

Use it when: Actions need first-class lifecycle management rather than immediate method calls.

Example: Create `AdmitStudentCommand` and queue it; or store UI commands for undo/redo.

```ts
interface Command { execute(): Promise<void> }
queue.push(new SendCampaignEmailCommand(payload));
```

Watch out: For simple synchronous calls, command objects may be ceremony without benefit.

## State

What it does: Moves state-specific behavior into separate state objects so behavior changes when internal state changes.

Use it when: An object has a real lifecycle with distinct states and many operations depend on the current state.

Example: `AdmissionApplication` behaves differently in New, InContact, Visited, Admitted, Closed.

```ts
class Application {
  constructor(private state: AppState) {}
  moveNext(): void { this.state = this.state.next(this); }
}
```

Watch out: Do not use State for a tiny enum with one or two simple conditions. It pays off when state-dependent behavior is substantial.

## Template Method

What it does: Defines the fixed skeleton of an algorithm in a base class while subclasses override selected steps.

Use it when: Several workflows share the same sequence but customize some steps.

Example: Import pipeline always validate → transform → save, while CSV and Excel importers implement parsing differently.

```ts
abstract class Importer {
  run(): void { this.validate(); this.parse(); this.save(); }
  abstract parse(): void;
  protected validate(): void {}
  protected save(): void {}
}
```

Watch out: It relies on inheritance. In TypeScript, composition/Strategy is often more flexible when variations must be combined dynamically.

## Mediator

What it does: Centralizes communication among collaborating objects so they do not directly depend on each other.

Use it when: Many components communicate in a tangled many-to-many network.

Example: A dialog/form mediator coordinates ProgramDropdown, CampusDropdown, ScholarshipField and SubmitButton.

```ts
class FormMediator {
  changed(source: Component): void { /* coordinate others */ }
}
```

Watch out: The mediator itself can become a god object. Keep coordination logic focused and split mediators by context when needed.

## Iterator

What it does: Provides a standard way to traverse a collection without exposing its internal representation.

Use it when: Clients should traverse lists, trees, paginated datasets, or custom collections consistently.

Example: A `LeadCollection` exposes an iterator regardless of whether leads come from an array, cursor, or tree.

```ts
for (const lead of leadCollection) {
  console.log(lead.name);
}
```

Watch out: JavaScript/TypeScript already provide iterable protocols; implement a custom Iterator only when traversal behavior is genuinely custom.

## Chain of Responsibility

What it does: Passes a request through a sequence of handlers until one handles it or all have had a chance.

Use it when: Processing consists of independent stages, filters, validators, middleware, or escalation rules that should be reorderable.

Example: HTTP middleware: authentication → authorization → validation → handler.

```ts
auth.setNext(permission).setNext(validation);
await auth.handle(request);
```

Watch out: Long chains can make it unclear which handler produced a result. Add tracing and define stop/continue semantics explicitly.

## Memento

What it does: Captures an object's state so it can later be restored without exposing internal details.

Use it when: You need undo, checkpoints, drafts, or rollback of object state.

Example: A report editor stores snapshots before each major change so the user can undo.

```ts
const snapshot = editor.save();
editor.changeLayout();
editor.restore(snapshot);
```

Watch out: Snapshots can consume memory and may contain sensitive data. Decide what state to capture and how long to retain it.

## Visitor

What it does: Separates operations from a stable object structure by dispatching operations to visitor objects.

Use it when: The object hierarchy changes rarely, but you frequently add new operations across all object types.

Example: A document AST has Paragraph, Image, Table nodes; visitors export to HTML, Markdown, or analytics.

```ts
interface Visitor { visitImage(x: ImageNode): void }
node.accept(exportVisitor);
```

Watch out: Visitor makes adding new node types expensive because every visitor may need changes. Prefer it when operations change more often than the structure.

## Interpreter

What it does: Represents a small language/grammar as objects and evaluates expressions against a context.

Use it when: You have a small, stable domain-specific language or rule syntax that must be interpreted repeatedly.

Example: Admission rule: `GPA >= 4.0 AND program = CSE` represented as expression objects and evaluated against an applicant.

```ts
interface Expr { eval(ctx: Context): boolean }
const rule = new AndExpr(
  new GpaAtLeast(4.0), new ProgramIs('CSE')
);
```

Watch out: For complex grammars, use a parser library, parser generator, or established expression engine. Interpreter becomes unwieldy as grammar grows.
