# Runtime Flows — Phase 1A

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**.

The flows below allocate conceptual runtime responsibility. They do not specify endpoints, schemas, queue products, provider APIs, or implementation sequences.

## 1. Tenant-scoped interactive request

| Element | Status | Flow |
| --- | --- | --- |
| Tenant Resolution | PROPOSED | Application resolves Tenant from trusted ingress/context information, including an accepted default/custom host or controlled route context. This may occur before identity is established and never treats arbitrary submitted `tenant_id` as authoritative. |
| Authentication | PROPOSED | Application establishes identity/session evidence where the entry requires it. Tenant Resolution is not Authentication. |
| Membership / Authorization | CONFIRMED + PROPOSED | After identity is established, confirmed role/ownership/Tenant-state policies apply to the resolved Tenant. Proposed Application Core evaluates them before business data is accessed. |
| File access | CONFIRMED + PROPOSED | Confirmed authorization must precede a short-lived secure URL. Proposed application/API performs the check; object storage returns bytes only after that authorization path. |
| Audit | PROPOSED | Security-sensitive decision and state-transition provenance is captured for authorized review. |

Final state: **PROPOSED** — the request is allowed only within the current authorized Tenant/role/ownership/Tenant-state scope, or receives a deny outcome without cross-Tenant disclosure.

## 2. Tenant signup, checkout, and activation

| Element | Status | Flow |
| --- | --- | --- |
| Confirmed lifecycle | CONFIRMED | Pending Tenant → Checkout → Payment/Webhook → Activate Subscription/Tenant. (FMD-005) |
| Inbound boundary | PROPOSED | Application validates and records a provider callback at a dedicated webhook boundary before applying activation behavior. |
| Consistency | CONFIRMED | Webhook processing must be idempotent, retryable, manually recoverable, and must not duplicate Tenant, User, or Subscription. |
| Follow-up | PROPOSED | After durable core processing, a Worker performs retryable downstream work such as email or provisioning without re-running core activation. If it needs a business-state change, it invokes the same Application Core command/use-case rather than directly mutating persistence. |
| Failure | CONFIRMED + PROPOSED | Confirmed recovery exists; proposed implementation records a reconcilable outcome and correlation/audit information. |

Final state: **CONFIRMED** — activation is completed once according to the confirmed lifecycle, or remains recoverable without duplicate creation. (FMD-005)

## 3. Invitation and notification delivery

| Element | Status | Flow |
| --- | --- | --- |
| Core rule | CONFIRMED | Invitation lifecycle is separate from notification. Resend invalidates the old token; accepted/expired/revoked tokens cannot be accepted. (FMD-004; DEC-035) |
| Transaction boundary | PROPOSED | Application Core commits Invitation state independently of delivery attempt. Worker delivery holds persisted trusted Tenant/context identifiers and may not use Host/Cookie/Session to re-resolve Tenant. |
| Delivery | CONFIRMED + PROPOSED | Email is the V1 external channel and delivery failure does not roll back core state. Proposed Worker handles send/retry observability once recipient/timing policy is approved. |
| Open policy | OPEN | Recipient, timing, content, retry, expiry duration, and verification policy remain decision-gated under OQ-006/OQ-014/OQ-022/OQ-027. |

Final state: **CONFIRMED** — Invitation state remains authoritative even if notification delivery fails. (FMD-004; DEC-035)

## 4. File, PDF, and document access

| Element | Status | Flow |
| --- | --- | --- |
| Authorization | CONFIRMED | Files are Tenant-bound, private by default, and Client-visible only to an authorized own Client Account scope; download requires authorization before temporary secure URL issuance. (DEC-031; PDM-005) |
| Core / storage split | PROPOSED | Application Core evaluates policy and owns File metadata, association, and lifecycle decisions; object storage stores bytes/objects only and honors the bounded delivery instruction. |
| PDF | PROPOSED | PDF rendering/export is an external-effect concern owned outside request-critical document state transition where practical. |
| History | CONFIRMED | Soft Delete/Restore cannot break Ticket/Quote/PI historical references; published Quote/PI revisions remain historical/immutable according to Phase 0 rules. (DEC-031; FMD-007; FMD-008) |
| Operational policy | OPEN | TTL, limits, scanning, region, retention, and exact PDF runtime are deferred to OQ-009/F1. |

Final state: **PROPOSED** — authorized content delivery can complete independently of document/file policy state; storage delivery failure is observable and recoverable without corrupting history.

## 5. Domain provisioning and runtime resolution

| Element | Status | Flow |
| --- | --- | --- |
| Confirmed lifecycle | CONFIRMED | Pending → Waiting DNS → Verifying → SSL Pending → Active / Failed / Disabled; failures retry, Tenant Admin may remove to default subdomain, Platform Admin may disable. (FMD-010) |
| Responsibility | PROPOSED | Application Core owns lifecycle decisions and authorization; Worker owns external DNS/SSL verification/provisioning attempts and retry. A Worker lifecycle state change uses Application Core commands/use-cases, not direct persistence mutation. |
| Request routing | PROPOSED | Web/application routing resolves a trusted Tenant context only after host/domain state is acceptable to the runtime boundary. |
| Open policy | OPEN | Supported custom-domain patterns and ownership/validation edge cases remain OQ-011/L5. |

Final state: **CONFIRMED** — the confirmed Domain lifecycle state is preserved; external-provider failure is recoverable/retryable rather than silently treated as Active. (FMD-010)

## Cross-flow reliability rules

| Status | Rule | Traceability |
| --- | --- | --- |
| PROPOSED | Persist intent before retryable external execution, deduplicate delivery using stable operation identity, and surface retry/exhaustion to authorized operations. | AP-005; AP-006 |
| CONFIRMED | Notification failure does not roll back the completed core business transaction. | FMD-004 |
| CONFIRMED | Subscription state and Tenant access state remain separate axes in every runtime interaction. | DEC-028; PDM-006 |
| PROPOSED | Correlation identifiers connect request, state transition, asynchronous work, provider result, and audit/observability records. | AP-008 |
| PROPOSED | Database is authoritative persistence; Application Core is authoritative for business-state transitions. Redis/queue and Object Storage are not alternate business-state authorities. | AP-003; AP-007 |
