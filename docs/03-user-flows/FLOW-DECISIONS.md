# Flow Decisions

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**.

All decisions in this register were accepted by the Phase 0C flow decision closure on 2026-09-20. These decisions refine flows where SRC-001 or Phase 0B did not prescribe operational rules.

| ID | Status | Accepted rule | Source / decision basis |
| --- | --- | --- | --- |
| FMD-001 | CONFIRMED / ACCEPTED | Tenant Admin or Staff creates Client Account; Client Account may exist without a login User. | User confirmation, 2026-09-20 |
| FMD-002 | CONFIRMED / ACCEPTED | Client Contact is independent of Client Account; Invitation is addressed to Client Contact. | User confirmation, 2026-09-20 |
| FMD-003 | CONFIRMED / ACCEPTED | Client Contact Email is unique within a Tenant. Existing Global User is reused with a Membership; no User is duplicated. Client Accounts are never automatically merged by company name. | User confirmation, 2026-09-20; BMD-001–BMD-003 |
| FMD-004 | CONFIRMED / ACCEPTED | Invitation and business Notification are separate. Invitation states are Pending / Accepted / Expired / Revoked and invitation may be resent. V1 external notifications use Email only; notification failure does not roll back the core business transaction. | User confirmation, 2026-09-20; supersedes BMD-003's earlier invitation state enum |
| FMD-005 | CONFIRMED / ACCEPTED | Tenant lifecycle is Pending Tenant → Checkout → Payment/Webhook → Activate Subscription/Tenant. Webhook handling is idempotent, retryable, manually recoverable, and must not duplicate Tenant/User/Subscription. | User confirmation, 2026-09-20; BMD-004 |
| FMD-006 | CONFIRMED / ACCEPTED | Ticket belongs to Tenant + Client Account, may reference Client Contact, and has 0..1 Staff assignee. States are Open / In Progress / Waiting for Client / Resolved / Closed. Resolved/Closed may reopen with Audit. Client cannot modify Priority. | User confirmation, 2026-09-20 |
| FMD-007 | CONFIRMED / ACCEPTED | Sent Quote content is immutable. Modification creates Revision and preserves history. Client may Accept/Reject only the current effective revision. | User confirmation, 2026-09-20; BMD-006 |
| FMD-008 | CONFIRMED / ACCEPTED | PI states are Draft / Issued / Sent / Viewed / Expired / Cancelled. Issued/Sent modification creates Revision. PI excludes Invoice, Payment, and Credit Note scope. | User confirmation, 2026-09-20; BMD-007 |
| FMD-009 | CONFIRMED / ACCEPTED | File is Tenant-bound, may associate with at most one Client Account, and is referenced by other business objects through relationships. V1 uses Soft Delete; deletion preserves Ticket/Quote/PI history. | User confirmation, 2026-09-20; BMD-008 |
| FMD-010 | CONFIRMED / ACCEPTED | Domain states are Pending / Waiting DNS / Verifying / SSL Pending / Active / Failed / Disabled. A domain belongs to one Tenant only. Failed verification can retry; Tenant Admin can remove custom domain and fall back to default subdomain; Platform Admin can disable Domain. | User confirmation, 2026-09-20 |

## Supersession note

| Status | Statement |
| --- | --- |
| CONFIRMED | FMD-004 supersedes the earlier BMD-003 invitation-state enumeration while preserving its independent invitation lifecycle and existing-User reuse principle. |
| CONFIRMED | FMD-005 refines BMD-004 with idempotency, retry, manual recovery, and no-duplication flow rules. |
| CONFIRMED | FMD-007 through FMD-010 close Phase 0C flow decisions without changing V1 exclusions from SRC-001 §15. |

