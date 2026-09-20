# Phase 1A Architecture Principles Baseline Freeze Record

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**. **ACCEPTED** identifies an authoritative Architecture Baseline decision and does not convert that decision into a Product-level `CONFIRMED` assertion.

## Formal freeze

| Field | Status | Record | Reference |
| --- | --- | --- | --- |
| Freeze ID | CONFIRMED | ARCH-1A-V1 | Architecture Approval Authority instruction, 2026-09-20; DEC-040 |
| Freeze Date | CONFIRMED | 2026-09-20 | Architecture Approval Authority instruction, 2026-09-20; DEC-040 |
| Architecture Approval Authority | CONFIRMED | Project Owner / Architecture Approval Authority | Architecture Approval Authority instruction, 2026-09-20; DEC-040 |
| Formal approval | CONFIRMED | Phase 1A — Architecture Principles & System Boundaries is approved for Freeze. | Architecture Approval Authority instruction, 2026-09-20; DEC-040 |
| PD-V1 input baseline | CONFIRMED | Git tag `product-definition-v1`, commit `59738725dec33272e4a9fd8fa0f14ddb0cf1c5f1`. | DEC-037; DEC-038 |
| Phase 1A Architecture Audit | CONFIRMED | PASS — Blocker 0; Major 0; Minor 0; Change Request 0; unresolved 1A-specific OPEN 0. | `docs/audit/PHASE-1A-ARCHITECTURE-AUDIT.md` |

## Accepted Architecture Decisions

| ID | Status | Accepted baseline decision |
| --- | --- | --- |
| AD-003 | ACCEPTED | Modular monolith with explicit Web, Application Core/API, and Worker runtime responsibilities. |
| AD-004 | ACCEPTED | Trusted Tenant Resolution → Authentication / Identity Establishment → Membership / Authorization Evaluation → Trusted Context Propagation; Workers use persisted trusted context and do not re-resolve Tenant from Host/Cookie/Session. |
| AD-005 | ACCEPTED | Platform control plane is distinct from Tenant data plane; Platform Admin default-deny to Tenant business content remains preserved. |
| AD-006 | ACCEPTED | Core state completion is separate from durable, idempotent, retryable, observable external-effect execution. |
| AD-007 | ACCEPTED | Database is authoritative persistence; Application Core is authoritative for business-state transitions; Redis/Queue is non-authoritative; Object Storage stores bytes/objects only; Workers use Application Core commands/use-cases for business-state changes. |

## Residual governed items

| Category | Status | Record |
| --- | --- | --- |
| Remaining Local Blockers | OPEN | 12 — remain at their individual existing Decision Gates; Ticket transition matrix and Quote/PI transition-revision matrix remain later local contract gates. |
| Remaining Deferred OPEN | OPEN | 6 — remain at their individual existing Decision Gates. |
| Product Change Requests | CONFIRMED | 0 created for this Freeze. |
| Architecture exceptions | CONFIRMED | 0 recorded for this Freeze. |

## Change control and downstream authority

| Status | Rule | Reference |
| --- | --- | --- |
| CONFIRMED | AD-003 through AD-007 are authoritative for downstream Architecture work unless changed through a new Architecture Decision or explicit superseding decision. | DEC-040; `ARCHITECTURE-DECISIONS.md` |
| CONFIRMED | A change that affects PD-V1 product definition additionally requires Product Change Request → Impact Review → Decision → Approved Update. | DEC-025; DEC-037; DEC-040 |
| CONFIRMED | Phase 1B — Identity & Authentication is NEXT / READY, but is not started by this Freeze. | DEC-040; `PROJECT-STATUS.md` |
| REJECTED | Treat this Freeze as authorization for ERD, API Contract, Task, Coding, or unapproved subsequent-phase work. | DEC-040 |
