# Architecture Decisions — Phase 1A

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**. For Architecture Decisions, **ACCEPTED** is an Architecture Baseline disposition: it is authoritative for subsequent Architecture work, but is not a Product-level `CONFIRMED` assertion.

## Architecture decision register

| ID | Status | Decision / recommendation | Basis and consequence |
| --- | --- | --- | --- |
| AD-001 | CONFIRMED | Use PD-V1 at `product-definition-v1` / `59738725dec33272e4a9fd8fa0f14ddb0cf1c5f1` as the read-only authority input for Phase 1A. | DEC-037; Product Owner Phase 1A authorization. |
| AD-002 | CONFIRMED | Limit Phase 1A to principles, boundaries, context, runtime responsibilities, risks, and trade-offs. | DEC-038. ERD, API Contract, Task, and Coding remain outside this phase. |
| AD-003 | ACCEPTED | Prefer a modular monolith over an early service split, with explicit Web, Application Core/API, and Worker runtime boundaries. | Phase 1A Architecture Baseline; `PHASE-1A-FREEZE-RECORD.md`. |
| AD-004 | ACCEPTED | Resolve Tenant Context from trusted ingress/context information; establish identity separately; then evaluate Membership/Authorization and propagate trusted context to async work. | Phase 1A Architecture Baseline; `PHASE-1A-FREEZE-RECORD.md`. |
| AD-005 | ACCEPTED | Separate Platform control-plane operations from Tenant data-plane operations; preserve Platform Admin default-deny to Tenant business content. | Phase 1A Architecture Baseline; `PHASE-1A-FREEZE-RECORD.md`. |
| AD-006 | ACCEPTED | Keep core transaction completion separate from external execution; use durable, idempotent, retryable asynchronous work as the later implementation pattern. | Phase 1A Architecture Baseline; `PHASE-1A-FREEZE-RECORD.md`. |
| AD-007 | ACCEPTED | Make Database authoritative persistence and Application Core authoritative business-state transition authority; keep cache/queue and Object Storage non-authoritative. | Phase 1A Architecture Baseline; `PHASE-1A-FREEZE-RECORD.md`. |
| AD-008 | CONFIRMED | Any required change to PD-V1 discovered in architecture review must be raised as a Change Request rather than edited into Phase 0 documents. | DEC-025; DEC-037. |

## Complete accepted decision records

### AD-003

| Field | Record |
| --- | --- |
| ID | AD-003 |
| Status | ACCEPTED — Architecture Baseline |
| Context | The product needs coherent Tenant-scoped operations, lifecycle consistency, auditability, and background effects, but PD-V1 does not require independently deployable internal services. |
| Recommendation / Decision | Start with a modular monolith and explicit Web, Application Core/API, and Worker responsibilities. |
| Alternatives Considered | Early service split; a single undifferentiated runtime. |
| Trade-offs | A modular monolith lowers distributed consistency and operational overhead; it requires disciplined module boundaries to avoid a tightly coupled monolith. |
| Risks | Future scale, isolation, independent-release, or team-boundary needs could pressure the model. |
| Constraints | No framework, deployment topology, service API, or implementation is selected in 1A. |
| Dependencies | AD-004 through AD-007; later explicit technology and deployment decisions remain DEC-014 OPEN. |
| Reconsideration Triggers | Measurable independent scaling/release/isolation needs, operational evidence that one runtime impedes recovery or reliability, or an approved product Change Request. |
| Product Baseline References | DEC-037; DEC-038; PDM-001; FMD-005. |

### AD-004

| Field | Record |
| --- | --- |
| ID | AD-004 |
| Status | ACCEPTED — Architecture Baseline |
| Context | PD-V1 requires Tenant isolation and default-deny access, while Custom Domain, Tenant login, Anonymous, and Invitation entry may precede authenticated identity. |
| Recommendation / Decision | Resolve Tenant from trusted ingress/context information; establish Authentication/Identity separately; then evaluate Membership/Authorization for the resolved Tenant. Application ingress propagates trusted context; Workers carry or reference persisted trusted identifiers and do not re-resolve Tenant from Host/Cookie/Session. |
| Alternatives Considered | Resolve Tenant only after Authentication; trust a submitted `tenant_id`; permit every downstream component to re-infer Tenant. |
| Trade-offs | Explicit context improves isolation and auditability but requires consistent propagation through synchronous and asynchronous work. |
| Risks | Incorrect host/route trust handling or omitted context propagation can create Tenant-isolation failures. |
| Constraints | No identity provider, token/session format, host-routing implementation, or data schema is selected in 1A. |
| Dependencies | AD-003, AD-006, AD-007; OQ-010 and OQ-014 remain at their existing gates. |
| Reconsideration Triggers | A confirmed new product entry surface, an approved cross-Tenant support capability, evidence that trusted ingress cannot supply the required context, or an approved Change Request. |
| Product Baseline References | DEC-030; PDM-001; PR-001; PR-009; DEC-037. |

### AD-005

| Field | Record |
| --- | --- |
| ID | AD-005 |
| Status | ACCEPTED — Architecture Baseline |
| Context | Platform Admin has confirmed platform-operational authority but confirmed default DENY for Tenant Client/File/Ticket/Quote/PI business-data read. |
| Recommendation / Decision | Separate Platform control-plane operations from Tenant data-plane operations; platform-operational capability does not imply Tenant business-data access. |
| Alternatives Considered | Implicit Platform Admin read access; treating Platform Admin as a Tenant Member; designing break-glass access as a default V1 path. |
| Trade-offs | Strong default isolation reduces ad hoc support convenience; support-access needs require a later explicit design. |
| Risks | A vague control-plane boundary could accidentally recreate implicit cross-Tenant access. |
| Constraints | Break-glass/Support Access remains outside V1 default and is not designed in 1A. |
| Dependencies | AD-004 and AD-007; future Support Access requires its own authorization and audit decision. |
| Reconsideration Triggers | An approved Product Change Request for support access or an explicit new platform-operational product capability. |
| Product Baseline References | DEC-030; PDM-001; PR-009; DEC-037. |

### AD-006

| Field | Record |
| --- | --- |
| ID | AD-006 |
| Status | ACCEPTED — Architecture Baseline |
| Context | Webhooks, email, PDF, and Domain/DNS/SSL operations can fail independently, while confirmed core activation and Invitation state must remain recoverable and not be rolled back by notification failure. |
| Recommendation / Decision | Complete core business state synchronously, represent external effects as durable asynchronous work, and require idempotency, retry, failure isolation, recovery, correlation, and observability. |
| Alternatives Considered | Require provider success inside the interactive core transaction; synchronous best-effort calls with no durable recovery; let Worker directly own business state. |
| Trade-offs | Asynchronous recovery improves consistency and resilience but introduces eventual external-effect completion and requires operational visibility. |
| Risks | Missing deduplication, correlation, or recovery visibility can create duplicate/missing effects despite preserved core state. |
| Constraints | No queue, outbox, webhook protocol, retry schedule, provider integration, or event contract is selected in 1A. |
| Dependencies | AD-003, AD-004, AD-007; OQ-006/OQ-022/OQ-027 notification details and OQ-009 File security details remain at existing gates. |
| Reconsideration Triggers | A confirmed product rule requiring synchronous external confirmation, evidence that a provider cannot support reliable reconciliation, or an approved Change Request. |
| Product Baseline References | FMD-004; FMD-005; FMD-010; DEC-028; DEC-037. |

### AD-007

| Field | Record |
| --- | --- |
| ID | AD-007 |
| Status | ACCEPTED — Architecture Baseline |
| Context | File byte storage, cache/queue execution support, external providers, and durable business metadata have distinct reliability and authority characteristics. |
| Recommendation / Decision | Database is authoritative persistence; Application Core is authoritative for business-state transitions. Redis/cache/queue is non-authoritative; Object Storage stores bytes/objects only; Workers use Application Core commands/use-cases for business-state change. |
| Alternatives Considered | Treat cache/queue as business system of record; let Object Storage metadata drive authorization; allow Workers direct persistence mutations; couple Application Core directly to provider-specific policy. |
| Trade-offs | Clear authority boundaries reduce drift and bypass risk but require explicit adapter/command discipline. |
| Risks | A second write path or provider-derived business state can violate Tenant boundary, lifecycle, authorization, or audit invariants. |
| Constraints | No database/cache/object-storage product, schema, RLS mechanism, provider SDK, or deployment topology is selected in 1A. |
| Dependencies | AD-003, AD-004, AD-006; OQ-009 remains deferred to F1. |
| Reconsideration Triggers | A proven durability/consistency limitation, an approved provider replacement, operational evidence of required boundary refinement, or an approved Change Request. |
| Product Baseline References | DEC-031; PDM-005; FMD-005; DEC-037. |

## Existing OPEN questions and their gates

Phase 1A does not resolve, reclassify, or pull forward any Product Definition OPEN question. The following remain blocking only at their registered gate.

| Gate | Status | Relevant OPEN questions | 1A treatment |
| --- | --- | --- | --- |
| L1 — Commercial model | LOCAL BLOCKER | OQ-004 | Do not define subscription/billing capability detail. |
| L2 — Onboarding and identity | LOCAL BLOCKER | OQ-005, OQ-013, OQ-014, OQ-015 | Do not select identity or activation implementation. |
| L3 — Authorization and security | LOCAL BLOCKER | OQ-007, OQ-010 | Preserve confirmed policies; do not define security acceptance criteria. |
| L4 — Document and file / Quote-PI Architecture/API | LOCAL BLOCKER | OQ-008, OQ-017, OQ-018 | Do not define document API or terminal/revision matrix. |
| L5 — Domain and ticket / Ticket Architecture/API | LOCAL BLOCKER | OQ-011, OQ-021 | Do not define domain edge-case policy or Ticket transition matrix. |
| D0 — Provenance enrichment | DEFERRED / NON-BLOCKING | OQ-002 | No impact on accepted PD-V1 snapshot. |
| R1 — Release Planning / Pre-Go-Live | DEFERRED / NON-BLOCKING | OQ-003B | No release planning is entered. |
| F1 — File/Security Architecture | DEFERRED / NON-BLOCKING | OQ-009 | Do not select TTL, scanning, limits, region, or retention. |
| D1 — Notification policy | DEFERRED / NON-BLOCKING | OQ-006, OQ-022, OQ-027 | Do not specify recipients, timing, content, or retry policy. |

## Freeze change control

| Status | Rule | Reference |
| --- | --- | --- |
| CONFIRMED | AD-003 through AD-007 are accepted as the Phase 1A Architecture Baseline. `ACCEPTED` is authoritative for subsequent Architecture work but does not change PD-V1 product facts. | Architecture Approval Authority instruction, 2026-09-20; DEC-040 |
| CONFIRMED | A later Architecture change must be recorded as a new Architecture Decision or explicit superseding decision; it must not silently edit an accepted decision. | DEC-040 |
| CONFIRMED | If an Architecture change would affect PD-V1 product definition, the product Change Request process additionally applies. | DEC-025; DEC-037; DEC-040 |

## Architecture Review input

| Status | Review question |
| --- | --- |
| ACCEPTED | Modular-monolith with separate runtime responsibilities is the current Phase 1A Architecture Baseline. |
| ACCEPTED | Trusted Tenant Context and control-plane/data-plane boundary are the current Phase 1A Architecture Baseline. |
| ACCEPTED | Durable asynchronous side-effect execution is the current Phase 1A Architecture Baseline, subject to later implementation design. |
| CONFIRMED | Phase 1B — Identity & Authentication is NEXT / READY but not started; its detailed scope still requires separately authorized work. |
