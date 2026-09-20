# Architecture Principles — Phase 1A

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**.

## Scope and authority

| Status | Principle | Traceability |
| --- | --- | --- |
| CONFIRMED | `PD-V1`, frozen at Git tag `product-definition-v1` / commit `59738725dec33272e4a9fd8fa0f14ddb0cf1c5f1`, is the read-only product-definition input for this work. | DEC-037; `PD-V1-FREEZE-RECORD.md` |
| CONFIRMED | Phase 1A defines architecture principles and system boundaries only. It does not define an ERD, API contract, Task, implementation, deployment configuration, or code. | DEC-038 |
| CONFIRMED | A change to a Phase 0 product definition cannot be made silently from architecture work; it requires Change Request → Impact Review → Decision → Approved Update. | DEC-025; DEC-037 |
| CONFIRMED | Cross-Tenant data access is denied, Platform Admin default access to Tenant business data is denied, and authorization remains scoped by Tenant, role, ownership, and Tenant state. | DEC-030; PDM-001; `PERMISSION-MODEL.md` |

## Proposed principles

| ID | Status | Principle | Rationale / trade-off |
| --- | --- | --- | --- |
| AP-001 | PROPOSED | Start as a modular monolith with explicit module boundaries, while retaining separately runnable Web, application/API, and Worker runtime responsibilities. | Keeps early delivery and consistency simpler than a service mesh while making later extraction possible. It is not a deployment or technology selection. |
| AP-002 | PROPOSED | Resolve Tenant Context from trusted ingress/context information before it is needed; establish identity separately; then evaluate Membership and Authorization after identity is established. Propagate the resulting trusted context explicitly from Application ingress. | Prevents client-supplied Tenant identifiers from becoming an authority signal or Tenant Resolution from being conflated with Authentication. Exact resolution, session, and storage mechanics are deferred. |
| AP-003 | PROPOSED | Treat Web as presentation/session-entry, Application Core/API as business and authorization enforcement, and Worker as asynchronous execution that invokes the same Application Core commands/use-cases for any business-state change. | Makes authority, transaction, retry ownership, and the single business-state authority legible without deciding framework or endpoints. |
| AP-004 | PROPOSED | Keep Platform operations separate from Tenant business-data operations. Platform Admin control-plane capability must not imply Tenant data-plane read capability. | Preserves PD-V1 default-deny and leaves any future break-glass design as a separately authorized capability. |
| AP-005 | PROPOSED | Commit core state changes synchronously; execute unreliable or long-running external effects asynchronously from durable intent. | Limits external failure from rolling back confirmed business transactions. A concrete outbox/queue pattern is a later design decision. |
| AP-006 | PROPOSED | Make every externally delivered operation idempotent, retryable, observable, and recoverable by an authorized operator. | Aligns with confirmed onboarding webhook recovery behavior without specifying provider integration internals. |
| AP-007 | PROPOSED | Keep Application Core policy independent of infrastructure adapters. Database is authoritative persistence; cache/queue is non-authoritative execution support; Object Storage stores bytes only; email, PDF, billing, domain, and observability providers sit behind explicit boundary contracts. | Reduces provider coupling and prevents a second business-state authority; exact ports, schemas, and SDKs are not decided in 1A. |
| AP-008 | PROPOSED | Treat audit, security, and observability as cross-cutting concerns at every trust boundary and state transition. | Supports confirmed audit expectations, Tenant isolation, and recovery investigation without inventing retention or telemetry requirements. |

## Responsibilities that remain product constraints

| Status | Constraint | Traceability |
| --- | --- | --- |
| CONFIRMED | A File always belongs to one Tenant; access is authorized before a short-lived secure URL is issued. Exact TTL and file-security parameters remain OPEN. | DEC-031; PDM-005; OQ-009 |
| CONFIRMED | Payment/webhook activation must be idempotent, retryable, and manually recoverable without duplicate Tenant, User, or Subscription creation. | FMD-005; `TENANT-ONBOARDING-FLOW.md` |
| CONFIRMED | Invitation and business notification are distinct; email delivery failure does not roll back the core business transaction. | FMD-004; OQ-006; OQ-022 |
| CONFIRMED | Subscription lifecycle and Tenant access are separate state axes. | DEC-028; PDM-006 |

## Assumptions, risks, and trade-offs

| Status | Item | Analysis |
| --- | --- | --- |
| PROPOSED | Modular-monolith starting point | Avoids premature distributed consistency, operations, and debugging costs. It may need later extraction when measurable scaling, isolation, team, or independent-release evidence exists. |
| PROPOSED | Explicit context propagation | Reduces cross-Tenant mistakes but requires each asynchronous operation to carry trusted Tenant and actor provenance. |
| OPEN | Security and identity acceptance criteria | MFA, recovery, privacy, retention, backup, availability, and incident expectations are not defined; this is OQ-010 at L3. |
| OPEN | File-security operating policy | URL TTL, scan, limits, type policy, storage region, and retention remain OQ-009 at F1. |
| OPEN | Provider and operational topology | Source technology and deployment recommendations remain candidates only; DEC-014 remains OPEN. |

## Tenant-context order

| Order | Status | Boundary |
| --- | --- | --- |
| 1. Tenant Resolution | PROPOSED | Resolve a prospective Tenant only from trusted ingress/context information, such as an accepted host or controlled route context. It is not Authentication and must not trust an arbitrary submitted `tenant_id`. Custom Domain, Tenant login surface, and Invitation entry may require this resolution before a user is authenticated. |
| 2. Authentication / Identity Establishment | PROPOSED | Establish whether an identity is present and valid. This does not itself grant Membership, Tenant access, or business authorization. Session, token, recovery, MFA, and provider details remain outside 1A. |
| 3. Membership / Authorization Evaluation | PROPOSED | After identity is established, evaluate the relevant Membership, role, ownership, and Tenant state for the resolved Tenant before a business operation proceeds. |
| 4. Context propagation | PROPOSED | Application ingress creates the trusted operation context and propagates it downstream. A downstream component does not freely re-infer Tenant from host, cookie, session, or caller input. A Worker carries or references persisted trusted identifiers and never re-resolves Tenant from Host/Cookie/Session. |

## Authoritative write boundary

| Status | Principle |
| --- | --- |
| PROPOSED | Database is authoritative persistence, and Application Core is the sole authority for business-state transitions, invariants, authorization checks, Tenant boundary checks, lifecycle rules, and required audit intent. |
| PROPOSED | Redis / cache / queue is not authoritative business state. Object Storage persists bytes/objects only and owns neither business authorization nor business state. |
| PROPOSED | Worker is an asynchronous execution mechanism, not a second business-logic authority. A Worker business-state modification invokes the same Application Core command/use-case and invariants used for a synchronous request; direct persistence mutation must not bypass them. |
| PROPOSED | A system-initiated background operation retains or references trusted Tenant context, stable operation identity/correlation, and auditability. |

## What 1A decides versus defers

| Area | Status in 1A | Boundary |
| --- | --- | --- |
| Principles, trust boundaries, runtime responsibility, failure-consistency principles | PROPOSED | In scope for Architecture Review. |
| Data schema, ERD, keys, indexes, RLS design, migration plan | REJECTED | Deferred; not entered in 1A. |
| HTTP/API events, payloads, endpoint contracts, versioning | REJECTED | Deferred; not entered in 1A. |
| Framework, database/cache/provider selection, cloud topology, IaC, secrets, CI/CD | REJECTED | Source recommendations remain PROPOSED pending later authorized work. |
| Ticket transition matrix and Quote/PI transition-revision matrix | OPEN | Local gates L5 and L4; resolve only before their named architecture/API-contract decision points. |
| The remaining 1B–1J sequence | OPEN | Not Planned Yet. It must be explicitly authorized and named before analysis begins. |
