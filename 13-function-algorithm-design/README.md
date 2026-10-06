# 13 — Function / Algorithm Design

> How does an individual behavior actually work?

Usually not called "architecture", but completes the hierarchy.

## Example
```ts
function calculateScholarship(marks: number, need: number): number
```

## Topics
- Algorithm choice, control flow, time/space complexity
- Validation, error handling, pure functions, recursion, data structures

## Search example
```text
Search → linear? binary? hash lookup? (pick per size + ordering + memory)
```

## Checklist
- [ ] Complexity noted (not prematurely optimized)
- [ ] Edge + error cases covered
- [ ] Pure where possible (testability)
