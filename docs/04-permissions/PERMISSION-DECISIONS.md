# Permission Decisions

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**.

## Confirmed authorization facts

| Status | Rule | Basis |
| --- | --- | --- |
| CONFIRMED | Cross-Tenant tenant-user access is always denied. | SRC-001 §5.2 |
| CONFIRMED | Client access is constrained to own Client Account scope. | SRC-001 §3.4; FMD-006–009 |
| CONFIRMED | Suspended Tenant Admin recovery/account-only scope; Staff/Client business denial. | BMD-005 |
| CONFIRMED | Tenant Admin/Staff and Client document/File/Ticket actions follow FMD-006–010. | Flow decisions |
| CONFIRMED | Platform Admin operates platform resources, Tenant lifecycle, domains, subscriptions, webhooks, and logs. | SRC-001 §3.1, §5.1 |

## Accepted permission decisions

| ID | Status | Decision | Impact |
| --- | --- | --- | --- |
| PDM-001 | CONFIRMED / ACCEPTED | Platform Admin defaults to DENY for Tenant Client/File/Ticket/Quote/PI data; platform operation remains allowed. Future Support Access requires separately designed explicit, time-limited, fully audited break-glass and is not V1 default. | Separates platform operation from tenant business data. |
| PDM-002 | CONFIRMED / ACCEPTED | Staff may operate daily business resources; create/edit Client Account and Client Contact; initiate Client Invitation; modify own Profile. Staff cannot invite/disable Staff, change Staff Role, or change Billing, Domain, Branding, platform settings, or other Staff profiles. | Prevents Staff becoming implicit Tenant Admin. |
| PDM-003 | CONFIRMED / ACCEPTED | Client may modify only own Contact personal fields; cannot delete self, create/delete other Contact, modify Client Account core company data, or receive a Client Admin role in V1. | Governs Client onboarding data. |
| PDM-004 | CONFIRMED / ACCEPTED | Staff may assign/reassign, modify Priority and allowed states; Client may create/reply/reopen own visible Ticket but cannot modify Priority, Assignee, or management state. Platform Admin sees platform Audit; Tenant Admin sees own-Tenant business Audit; Staff no full Audit; Client no Audit Log but own activity history; sensitive security fields are administrator-only. | Governs operations and audit exposure. |
| PDM-005 | CONFIRMED / ACCEPTED | Tenant Admin/Staff restore/reassociate same-Tenant Soft Deleted File; Tenant ownership never changes. Quote/PI use stable Document Number plus independent Revision Number; published Revision is read-only and permanently retained. Notification recipient/timing remains OPEN / DEFERRED and non-blocking. | Governs File/document integrity. |
| PDM-006 | CONFIRMED / ACCEPTED | ACTIVE is normal use; CANCELLED remains normal through paid period; after paid period without renewal/recovery, Subscription becomes EXPIRED and Tenant becomes SUSPENDED. Billing lifecycle and Tenant access state remain separate. | Governs entitlement-state semantics. |

## Review posture

| Status | Statement |
| --- | --- |
| CONFIRMED | PDM-001 through PDM-006 are accepted; notification recipient/timing remains deferred without blocking Permission Model completion. |
| REJECTED | Infer Platform Admin cross-Tenant business-data access from platform operational authority. |
| OPEN | Notification recipients, timing, content, and retry policy remain deferred operating details. |
