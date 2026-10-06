# 07 — Data Architecture

> How should data be structured, owned, stored and moved?

## Topics
- Relational vs NoSQL — pick per access pattern
- Schemas, data ownership (who owns what)
- Warehouse / lake, ETL, governance
- Replication (leader/follower), partitioning/sharding
- Indexing, caching strategies, event sourcing

## Example
```text
Operational DB → ETL → Data Warehouse → Power BI
```

## Checklist
- [ ] Owner per dataset
- [ ] Read/write patterns listed before DB choice
- [ ] Replication vs sharding reason recorded
- [ ] Retention / privacy / governance noted

## Artifacts
- Data ownership table, storage decision log, flow diagram
