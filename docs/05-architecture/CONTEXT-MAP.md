# Context Map — Phase 1A

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**.

This is a conceptual responsibility map, not a deployment diagram, ERD, API specification, or provider selection.

```text
Platform Admin ─┐
Tenant Admin ───┼─> Web / Presentation ─> Tenant Resolution ─> AuthN ─> Membership / AuthZ ─> Application Core
Staff ──────────┤                (trusted ingress)                                              │
Client ─────────┘                                                                                ├─> Transactional State + Audit
Anonymous / Invited User ───────────────────────> Tenant Resolution ─> AuthN when applicable ───┘
                                                                                                   │
                                                                                                   └─> Durable async intent
                                                                                 │
                                                                                 v
                                                                        Worker / Recovery
                                                                      /    |      |     \
                                                            Email / PDF / Billing / Domain / Object Storage
```

## Participants and authority

| Context | Status | Owns / receives | Boundary condition |
| --- | --- | --- | --- |
| Platform Admin | CONFIRMED | Platform operations and platform audit within Phase 0 permission rules. | Default DENY for Tenant business-data content; no implied support access. DEC-030; PDM-001. |
| Tenant Admin | CONFIRMED | Tenant-level administration, recovery/account surfaces when suspended, and authorized business operations. | Always Tenant-scoped and Tenant-state constrained. BMD-005; PDM-006. |
| Staff | CONFIRMED | Day-to-day authorized Tenant operations, Client Account/Contact management, and Client invitations. | No Staff management, Billing, Domain, Branding, or Platform-setting authority. PDM-002. |
| Client | CONFIRMED | Own Client Account-scoped, visibility-constrained portal activities. | Cannot access other Client Account data, audit log, or restricted management actions. PDM-003–005. |
| Application Core / API | PROPOSED | Tenant Resolution, identity handoff, Membership/Authorization evaluation, and authoritative business-policy execution. | Must apply trusted context and policy before state read/write or secure-URL issuance; it is the sole authority for business-state transitions. |
| Worker | PROPOSED | Non-interactive asynchronous execution and retry/recovery. | Carries or references persisted trusted context; does not re-resolve Tenant from Host/Cookie/Session, independently widen access, or directly bypass Application Core business-state rules. |
| External systems | PROPOSED | Effect delivery or provider events. | No direct authority over application policy or Tenant data access. |

## Control plane versus Tenant data plane

| Plane | Status | Examples | Access rule |
| --- | --- | --- | --- |
| Platform control plane | PROPOSED | Platform operations, subscription operational processing, domain enable/disable, platform audit. | Platform authority is explicit and bounded; it does not confer default Tenant business-data inspection. |
| Tenant data plane | PROPOSED | Client Accounts, Contacts, Files, Tickets, Quotes, PI, Tenant-scoped audit. | Every operation is Tenant-scoped; role, ownership, visibility, and Tenant state remain enforced. |

## Context handoff

1. **PROPOSED** — Web forwards trusted ingress/context signals to the application boundary. Tenant Resolution may occur before identity is established for Custom Domain, Tenant login, Anonymous, and Invitation entry.
2. **PROPOSED** — Application resolves Tenant only from trusted ingress/context information. It does not treat Tenant Resolution as Authentication and does not trust arbitrary submitted `tenant_id`.
3. **PROPOSED** — Application establishes identity when applicable, then evaluates Membership, role, ownership, and Tenant state before business access.
4. **PROPOSED** — Application Core commits business state and audit intent together where the later data design supports it; external effects are represented as durable work.
5. **PROPOSED** — Worker executes authorized side effects using persisted trusted Tenant/context identifiers. A Worker business-state change invokes Application Core commands/use-cases rather than directly mutating persistence, and reports outcome/retry state without reopening a completed business transaction.

## Non-goals

| Status | Non-goal |
| --- | --- |
| REJECTED | Defining service APIs, event payloads, data ownership tables, network zones, deployment clusters, or database schema in Phase 1A. |
| REJECTED | Treating the source’s named stack or providers as selected technology. |
| OPEN | Defining any future break-glass Support Access workflow; it remains outside V1 default permission. |
