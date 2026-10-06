# 01 — Business Architecture

> What business capabilities / processes are we supporting?

Above software. No Singleton or Microservices here.

## Scope
- Business capabilities, processes, responsibilities, actors, boundaries

## Example — University
```text
University
├── Student Recruitment
├── Admission
├── Academic Management
├── Finance
├── HR
└── Examination
```

## Artifacts
- Capability map, process flows (BPMN-lite), RACI / actors

## Checklist
- [ ] Capability list (no tech jargon)
- [ ] End-to-end processes named
- [ ] Actors + responsibilities clear
- [ ] Boundaries between capabilities explicit

## Anti-pattern
- Jumping to tech (DB, framework) before capabilities are named.
