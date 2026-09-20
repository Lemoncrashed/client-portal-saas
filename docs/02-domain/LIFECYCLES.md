# Lifecycles

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**. A status with no applicable item in a document is not implied.

## Tenant signup, subscription, and activation

| Status | Lifecycle statement | Source support |
| --- | --- | --- |
| CONFIRMED | The source flow is: Tenant Customer → Lemon Squeezy Checkout → Payment Successful → Webhook → Create Tenant → Create Admin → Activate Subscription → Tenant Available. | SRC-001 §6.1 |
| CONFIRMED | Subscription events include Created, Updated, Payment Successful, Payment Failed, Cancelled, Resumed, and Expired. | SRC-001 §6.2 |
| CONFIRMED | Cancellation does not end service before the paid-through date. | SRC-001 §6.4 |
| CONFIRMED | Subscription lifecycle and Tenant access state are separate axes. Subscription uses `ACTIVE`, `CANCELLED` (while the paid period remains), and `EXPIRED`; Tenant access uses `PENDING`, `ACTIVE`, and `SUSPENDED`. When the paid period ends without renewal/recovery, Subscription becomes `EXPIRED` and Tenant access becomes `SUSPENDED`. | PDM-006 accepted; SRC-001 §6.3–§6.4 |
| CONFIRMED | Tenant signup lifecycle is `Pending Tenant → Checkout → Payment/Webhook → Activate Subscription/Tenant`; webhook processing is idempotent, retryable, manually recoverable, and never duplicates Tenant/User/Subscription. | FMD-005. |
| OPEN | Required Tenant input data and verification rules before activation. | FMD-005 confirms recovery behavior, not input policy. |

### Canonical entitlement and access relationship

| Status | Subscription lifecycle | Tenant access state | Meaning | Reference |
| --- | --- | --- | --- | --- |
| CONFIRMED | ACTIVE | ACTIVE | Normal paid service access. | PDM-006; DEC-028 |
| CONFIRMED | CANCELLED, current paid period not ended | ACTIVE | Cancellation does not remove normal access before paid-through time. | PDM-006; SRC-001 §6.4; DEC-028 |
| CONFIRMED | EXPIRED | SUSPENDED | Paid entitlement has ended without renewal/recovery; Tenant Admin recovery-only boundary applies. | PDM-006; BMD-005; DEC-028 |
| CONFIRMED | Recovered / renewed to ACTIVE | ACTIVE | Reconciliation restores normal access after recovery. | PDM-006; DEC-028 |
| REJECTED | Treat `EXPIRED` as a Tenant access state. | — | `EXPIRED` belongs to Subscription lifecycle only. | DEC-028 |

## Staff / Client invitation and account activation

| Status | Lifecycle statement | Source support |
| --- | --- | --- |
| CONFIRMED | Tenant Admin can create/invite/disable Staff and Clients; Staff use accounts to log in. | SRC-001 §5.7–§5.8 |
| CONFIRMED | Invitation states are `Pending / Accepted / Expired / Revoked`; resend is supported. | FMD-004 supersedes BMD-003's earlier state enumeration. |
| CONFIRMED | Invitation activation links an existing User where present and does not duplicate an account. Invitation is sent to Client Contact for Client onboarding. | BMD-003; FMD-002–FMD-004. |

## Suspended Tenant

| Status | Lifecycle statement | Source support |
| --- | --- | --- |
| CONFIRMED | A Tenant may be suspended on actual subscription expiry; data is not immediately deleted and subscription recovery permits continued use. | SRC-001 §6.3 |
| CONFIRMED | Suspended is a service-access state distinct from deletion. Tenant Admin may access only suspension reason, Billing/Subscription Recovery, and Account/Profile. | BMD-005. |
| CONFIRMED | Staff and Client have no business access while their Tenant is suspended; normal operations resume only after recovery. | BMD-005. |

## Quote lifecycle

| Status | Lifecycle statement | Source support |
| --- | --- | --- |
| CONFIRMED | Tenant Admin/Staff can create Quote; it has number, customer, dates, line items, terms/notes, and branded PDF download. | SRC-001 §5.13–§5.15 |
| CONFIRMED | V1 lifecycle is `Draft → Sent → Accepted / Rejected / Expired / Cancelled`. | BMD-006. |
| CONFIRMED | Client may View, Download PDF, Accept, or Reject a Quote. E-signature is not included. | BMD-006; SRC-001 §15 excludes electronic signature. |
| CONFIRMED | Sent Quote content is not overwritten; modification creates Revision and preserves history. Client accepts/rejects only the current effective Revision. | FMD-007. |
| OPEN | Quote numbering, Revision identification, and Quote-to-PI conversion mechanism. | FMD-007 confirms revision rule, not these parameters. |

## PI lifecycle

| Status | Lifecycle statement | Source support |
| --- | --- | --- |
| CONFIRMED | PI can be created with number, customer, dates, items, financial fields, payment terms/notes, and branded PDF download. | SRC-001 §5.14–§5.15 |
| CONFIRMED | PI remains Proforma and does not extend to formal Invoice or Accounting scope. | BMD-007; SRC-001 §15. |
| CONFIRMED | A PI may reference a source Quote; this link is optional and no automatic Quote-to-PI conversion is required. | BMD-007. |
| CONFIRMED | PI lifecycle is Draft / Issued / Sent / Viewed / Expired / Cancelled; Issued/Sent modification creates Revision and preserves history. | FMD-008. |
| REJECTED | Infer online invoice payment, recurring invoicing, or accounting lifecycle from PI creation. | SRC-001 §15 explicitly excludes those capabilities. |
| OPEN | PI numbering, Revision identification, and notification recipient/timing policy. | FMD-008 settles lifecycle/revision and excludes invoice/payment/accounting scope. |

## File ownership and association

| Status | Lifecycle statement | Source support |
| --- | --- | --- |
| CONFIRMED | Tenant Admin/Staff can upload, download, delete, rename, search, organize folders, and associate a File with Client; Clients can upload when allowed and view/download their own files. | SRC-001 §5.9 |
| CONFIRMED | File access requires authorization before a temporary secure URL; direct public access is not allowed. | SRC-001 §5.10 |
| CONFIRMED | Every File belongs to exactly one Tenant as its security boundary; Client association is optional. | BMD-008. |
| CONFIRMED | Ticket, Quote, PI, and other business records reference File through relationship models rather than by adding all business foreign keys to File. | BMD-008. |
| CONFIRMED | File associates with at most one Client Account. V1 deletion is Soft Delete and preserves Ticket/Quote/PI history. | FMD-009. |
| CONFIRMED | Tenant Admin/Staff may reassociate and Restore only within the same Tenant; Client cannot alter File Tenant/Client Account association or Restore. Client visibility is same-Tenant, own Client Account, explicitly Client-visible only; Soft Delete/Restore preserves historical references. | DEC-031; FMD-009; PDM-005 |
| OPEN | Exact URL TTL, file size/type limits, malware-scan implementation, storage region, and retention parameters. | OQ-009; deferred to the File/Security Architecture Gate. |
