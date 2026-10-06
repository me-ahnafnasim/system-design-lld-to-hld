# Part II — Distribution

Chapters 19–20. When a module must live beyond one application.

---

## Chapter 19 — Library / Reusable Package

```text
packages/
├── validation/
├── logger/
├── auth-client/
└── ui-components/
```

These may ship as npm packages, Java JARs, .NET packages, or Python packages.

**Use when** functionality needs reuse across applications. At this point release and versioning concerns (SemVer, changelogs, compatibility) become part of module design — REP starts to matter as much as clean code.

---

## Chapter 20 — Monorepo Package Design

Example TypeScript monorepo:

```text
repo/
├── apps/
│   ├── web/
│   ├── admin/
│   └── api/
└── packages/
    ├── auth/
    ├── database/
    ├── contracts/
    └── ui/
```

Typical tooling: pnpm workspaces, Nx, Turborepo, npm workspaces.

The central design question becomes:

```text
Can package A depend on package B?
```

Answer it with explicit dependency rules (Part III), or the monorepo quietly becomes a distributed ball of mud.

**Takeaway:** distribution upgrades a folder into a product. Version it, document its public API, and enforce who may depend on it.
