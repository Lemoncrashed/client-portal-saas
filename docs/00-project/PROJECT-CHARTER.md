# Project Charter — Client Portal

## Status vocabulary

- **CONFIRMED** — Explicitly stated by a registered source or instruction.
- **PROPOSED** — A source recommendation or candidate direction; not an approved implementation decision.
- **OPEN** — Not specified, unresolved, or awaiting confirmation.
- **REJECTED** — Explicitly excluded from V1 or the current phase.

## Charter

| Status | Statement | Source |
| --- | --- | --- |
| CONFIRMED | Build a lightweight multi-tenant, fully white-label, self-hosted Client Portal SaaS for B2B service businesses, including consulting, design, software development, agency, and professional-service firms. | SRC-001 §1; SRC-002 §01 |
| CONFIRMED | A platform operator serves multiple enterprise tenants; each tenant has an independent customer portal, brand, domain, users, and business data. | SRC-001 §2, §5.2; SRC-002 §02, §05.2 |
| CONFIRMED | The core product areas are customer portal, branding, custom domain, file sharing, service tickets, Quote, Proforma Invoice (PI), and subscription-led onboarding. | SRC-001 §1, §5–§6; SRC-002 §01, §05–§06 |
| CONFIRMED | Product Definition v1 (`PD-V1`) is frozen on 2026-09-20. This baseline does not authorize Architecture, ERD, API, Task, or Coding work. | User governance instruction, 2026-09-20; Source Register |
| PROPOSED | The source recommends AWS self-hosting and a listed technology/deployment approach. These are not adopted architecture decisions in Phase 0. | SRC-001 §7, §9–§10; SRC-002 §07, §09–§10 |
| REJECTED | V1 is not a complete CRM, ERP, or marketing-automation product. | SRC-001 §1, §15; SRC-002 §01, §15 |
| OPEN | Commercial owner, delivery owner, target launch date, measurable success criteria, and approved budget are not stated. | Source analysis |

## Governance and change control

| Status | Rule | Authority / evidence |
| --- | --- | --- |
| CONFIRMED | Project Owner / User is the Product Owner and Product Approval Authority. | User governance instruction, 2026-09-20 |
| CONFIRMED | SRC-001 is the primary product source for PD-V1; its registered SHA-256 identifies the frozen source input. | `SOURCE-REGISTER.md` |
| CONFIRMED | A post-freeze product-definition change must follow **Change Request → Impact Review → Decision → Approved Update**. | User governance instruction, 2026-09-20 |
| CONFIRMED | An Approved Update must update the affected source trace, decision record, project status, and Dashboard in the same governed change. | User governance instruction, 2026-09-20; DEC-011 |
| CONFIRMED | Every future `CONFIRMED` assertion must carry at least one `SRC`, `DEC`, `BMD`, `FMD`, `PDM`, or `CR` reference. This is lightweight traceability governance, not a traceability database. | DEC-036 |
