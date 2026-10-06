# 10 — Package / Module Design

> Which code belongs together? What is public? What deps are allowed?

Closest to source code, above classes.

## Example (TypeScript)
```text
src/
├── admission/{domain,application,infrastructure,presentation}/
├── finance/
└── shared/
```

## Concepts
- High cohesion, low coupling, info hiding, SoC
- Cohesion: functional → coincidental (prefer functional)
- Coupling: content → data (prefer data)
- Package principles: REP, CCP, CRP, ADP, SDP, SAP
- Namespaces, import/export contracts, visibility, SemVer

## Dependency rule
```text
domain (no deps) ← application ← infrastructure ← presentation
```

## Checklist
- [ ] No cycles (ADP)
- [ ] Stable modules don't depend on volatile ones (SDP)
- [ ] Public API minimal + versioned
