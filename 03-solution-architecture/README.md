# 03 — Solution Architecture

> How will we solve this particular business problem across systems?

Narrow to **one business problem**, often spanning several apps.

## Example — Digital admission platform
```text
Website → CRM → Admission ERP → Payment Gateway
              → SMS / WhatsApp → Analytics
```

## Scope
- Which systems + technologies together solve the problem
- Cross-system flows, build-vs-buy, integration touchpoints
- Constraints from enterprise standards (identity, data ownership)

## Checklist
- [ ] Problem + success metrics stated
- [ ] Systems involved + responsibilities
- [ ] Cross-system happy path + failure paths
- [ ] Non-goals explicit

## Artifacts
- Solution context diagram, system responsibilities table
