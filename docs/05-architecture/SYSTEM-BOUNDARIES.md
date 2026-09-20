# System Boundaries — Phase 1A

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**.

## Boundary map

| Boundary | Status | Responsible for | Must not decide or bypass |
| --- | --- | --- | --- |
| Web | PROPOSED | Presentation, navigation, session entry, request formation, and rendering for Tenant Portal or Platform operations. | It does not grant access, determine Tenant authority from untrusted input, or directly bypass application authorization. |
| Application / API | PROPOSED | Tenant Resolution boundary, Authentication handoff, Membership/Authorization evaluation, business policy, transactional state change, and audit intent. | It does not conflate Tenant Resolution with Authentication, let external delivery determine a completed core transaction, or permit downstream bypass of Application Core policy. |
| Worker | PROPOSED | Durable asynchronous execution: email delivery, PDF generation, domain provisioning/reverification, webhook follow-up, and recovery/retry handling. | It does not invent actor authority or become a second business-state authority; any business-state change uses the same Application Core command/use-case and invariants. |
| Platform control plane | PROPOSED | Platform operations, platform audit, Tenant/subscription operational state administration within confirmed permissions. | It does not default to Tenant Client/File/Ticket/Quote/PI business-data read. |
| Tenant data plane | PROPOSED | Tenant-scoped business interactions for Members, Client Accounts/Contacts, Files, Tickets, Quotes, and PIs. | It does not cross Tenant scope or imply Platform Admin access. |
| External providers | PROPOSED | Payment/subscription events, email transport, PDF runtime, domain/DNS/SSL service, object storage, and optional caching/queue infrastructure. | They cannot be treated as the authoritative source of application authorization or core lifecycle state without a later decision. |

## Tenant Resolution, Authentication, and Authorization boundary

| Status | Rule | Traceability |
| --- | --- | --- |
| PROPOSED | **Tenant Resolution** uses trusted ingress/context information only. It may occur before identity exists for Custom Domain, Tenant login surface, and Invitation entry. It is never inferred from arbitrary user-submitted `tenant_id`. | AP-002 |
| PROPOSED | **Authentication / Identity Establishment** occurs separately. It establishes identity evidence but does not establish Tenant Membership or grant authorization. | AP-002 |
| PROPOSED | **Membership / Authorization Evaluation** occurs after identity establishment and evaluates Membership, role, ownership, Tenant state, and resolved Tenant scope before business access. | AP-002; PDM-001 |
| PROPOSED | Application ingress establishes the trusted operation context. Each business operation carries Tenant scope, actor identity where present, Membership/role context where applicable, and correlation/audit provenance; downstream code does not arbitrarily re-resolve Tenant. | AP-002; AP-008 |
| PROPOSED | An asynchronous Worker receives only persisted, validated context identifiers; it does not use Host, Cookie, or Session to resolve Tenant and rechecks current applicable state before creating an externally visible effect. | AP-005; AP-006 |
| CONFIRMED | Tenant A must never access Tenant B data; Client access is constrained to its own Client Account scope and visibility rules. | PDM-001; PR-001; PR-009 |

## Authentication and authorization separation

| Aspect | Status | Boundary |
| --- | --- | --- |
| Authentication | PROPOSED | Establishes identity/session evidence after any applicable Tenant Resolution; exact identity provider, session method, MFA, recovery, and verification policy are deferred under OQ-010 and OQ-014. |
| Authorization | PROPOSED | After identity is established, evaluates resource × action × Membership/role × ownership × Tenant state for the resolved Tenant before the operation proceeds. It enforces confirmed default-deny rules and does not rely on UI visibility. |
| Product rules | CONFIRMED | The role, ownership, and Tenant-state policy defined in Phase 0 remains authoritative; Phase 1A does not alter it. (DEC-021; DEC-037) |

## Data and infrastructure responsibility boundaries

| Store / concern | Status | Proposed responsibility | Explicit non-decision |
| --- | --- | --- | --- |
| Transactional database | PROPOSED | Authoritative persistence for application business metadata and committed state. Application Core alone applies business-state transitions and invariants. | Schema, engine version, RLS mechanism, indexes, and migration design. |
| Cache / queue | PROPOSED | Non-authoritative performance or asynchronous delivery support; never a business system of record. | Provider choice or topology. |
| Object storage | PROPOSED | Store file bytes/objects only. Application Core owns File metadata, association, authorization, and lifecycle; it authorizes access before a temporary secure URL is issued. | URL TTL, scanning, size/MIME limits, region, retention, and provider. |
| Audit / observability records | PROPOSED | Record security-sensitive access and state transitions with correlation context sufficient for authorized review. | Event schema, retention, telemetry vendor, and alert thresholds. |

## External-system reliability boundary

| Concern | Status | Principle |
| --- | --- | --- |
| Inbound payment webhooks | CONFIRMED + PROPOSED | Confirmed: idempotent, retryable, manually recoverable activation with no duplicate core records. (FMD-005) Proposed: verify, persist/reconcile, then execute external follow-up from durable intent. |
| Email | CONFIRMED + PROPOSED | Confirmed: V1 external channel is email and delivery failure does not roll back core transaction. (FMD-004) Proposed: Worker owns retries/observability once an approved notification policy exists. |
| PDF | PROPOSED | Generate/export outside the request-critical business transaction when practical; document lifecycle authority stays in the application core. |
| Domain provisioning | CONFIRMED + PROPOSED | Confirmed Domain lifecycle and retry/remove/disable rules remain authoritative. (FMD-010) Proposed: provider calls and verification polling/retry are Worker responsibilities. |

## Application core and infrastructure

**PROPOSED** — Application core owns business policy and lifecycle decisions. Infrastructure adapters translate those decisions to storage, messaging, billing, email, PDF, DNS/SSL, and observability systems. Adapter failure is reported through the core’s recovery/audit boundary; it does not silently change a confirmed product rule.

**PROPOSED** — Worker execution cannot directly mutate persistence to change business state. When system-initiated work needs such a change, it invokes the same Application Core command/use-case, Tenant boundary checks, lifecycle invariants, authorization policy where applicable, and audit intent as the equivalent synchronous operation. It carries or references persisted trusted Tenant context plus stable operation identity/correlation.
