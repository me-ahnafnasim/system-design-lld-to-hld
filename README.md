# System Design: LLD to HLD

A structured, interview-focused guide to System Design — from **Low-Level Design (LLD)** to **High-Level Design (HLD)**.

> OOP → SOLID → Design Patterns → LLD Case Studies → Scalability Fundamentals → HLD Building Blocks → HLD Case Studies

## Why this repo?

System Design resources are usually either too theoretical or scattered. This repo is a single, progressive path:

1. **Solid foundations** — OOP, SOLID, UML
2. **LLD** — patterns + real class-level case studies
3. **HLD** — scalability, distributed systems, real architecture case studies
4. **Interview prep** — frameworks, checklists, and revision notes

## Repository structure

```text
.
├── 01-low-level-design/
│   ├── 01-oop-fundamentals/      # Encapsulation, abstraction, inheritance, polymorphism, UML
│   ├── 02-solid-principles/      # S.O.L.I.D + DRY, KISS, YAGNI, composition over inheritance
│   ├── 03-design-patterns/
│   │   ├── creational/           # Singleton, Factory, Builder, Prototype, Abstract Factory
│   │   ├── structural/           # Adapter, Decorator, Facade, Proxy, Composite
│   │   └── behavioral/           # Strategy, Observer, State, Chain of Responsibility, etc.
│   └── 04-lld-case-studies/      # Parking Lot, Elevator, Tic-Tac-Toe, Splitwise, etc.
├── 02-high-level-design/
│   ├── 01-fundamentals/          # Scalability, latency vs throughput, CAP, consistency models
│   ├── 02-building-blocks/       # Load balancing, caching, CDN, queues, rate limiting, etc.
│   ├── 03-databases/            # SQL vs NoSQL, sharding, replication, indexing
│   └── 04-hld-case-studies/      # URL shortener, rate limiter, notification system, etc.
├── 03-interview-prep/            # Frameworks, checklists, 30/60-min plans, revision sheets
├── assets/
│   └── diagrams/                 # Source diagrams (excalidraw/drawio/png)
├── docs/
│   ├── ROADMAP.md                # Suggested learning order
│   ├── GLOSSARY.md               # LLD/HLD terminology
│   └── RESOURCES.md              # Curated books, videos, papers
```

Each folder has its own `README.md` with objectives and checklist.

## Getting started

1. Start with [`docs/ROADMAP.md`](docs/ROADMAP.md) for the learning order.
2. Work through `01-low-level-design` before `02-high-level-design`.
3. Use `03-interview-prep` for timed practice.

No build step required — this is a docs + examples repo. Code examples are in Java (primary) with language-agnostic UML where possible.

## Conventions

- Folder names: `kebab-case`, numbered by learning order (`01-`, `02-`)
- Docs: Markdown, one concept per file
- Diagrams: stored in `assets/diagrams/`, linked from markdown
- Code examples: one folder per pattern / case study with `README.md` + source

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for details.

## Roadmap

- [ ] LLD: OOP + SOLID notes + UML examples
- [ ] LLD: 10 classic design patterns with Java examples
- [ ] LLD: 4 end-to-end case studies
- [ ] HLD: fundamentals + building blocks
- [ ] HLD: 5 end-to-end case studies
- [ ] Interview: frameworks + mock templates

## Contributing

Contributions welcome. See [`CONTRIBUTING.md`](CONTRIBUTING.md).

## License

MIT — see [`LICENSE`](LICENSE).
