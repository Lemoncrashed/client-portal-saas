# Entity Catalog

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**. A status with no applicable item in a document is not implied.

This is a conceptual business catalog, not a database schema or ERD.

| ID | Status | Entity | Business purpose | Minimum relationship concept | Source support |
| --- | --- | --- | --- | --- | --- |
| ENT-001 | CONFIRMED | Tenant | Enterprise customer portal boundary and owner of isolated business data. | Has users, branding, domain, clients, files, tickets, Quote, PI, and subscription status. | SRC-001 §2, §5.2 |
| ENT-002 | CONFIRMED | User / Account | Global login identity for Staff or Client. | May have one or more Tenant Memberships. | SRC-001 §3.3–§3.4, §5.7–§5.8; BMD-001 |
| ENT-003 | CONFIRMED | Membership | Tenant-scoped relationship between User and Tenant, carrying role/type and status. | Belongs to one Tenant and one User; a User may have multiple Memberships. | SRC-001 §3; BMD-001 |
| ENT-004 | CONFIRMED | Role | Named access category: Platform Admin, Tenant Admin, Staff, Client. | Applied to the relevant user relationship. | SRC-001 §3 |
| ENT-005 | CONFIRMED | Client Account / Profile | Independent business identity for a downstream client and its source-defined details. | Linked to Client Membership/User where that client uses the portal. | SRC-001 §5.8; BMD-002 |
| ENT-015 | CONFIRMED | Client Contact | Contact identity independent of Client Account and invitation recipient. | Belongs to one Tenant; email is unique within that Tenant; may be linked to Client Membership/User. | FMD-002, FMD-003 |
| ENT-006 | CONFIRMED | Invitation | Independent pending offer for Staff or Client access. | Targets a Tenant and intended role/type; links existing User rather than duplicating identity. | SRC-001 §5.7–§5.8; BMD-003 |
| ENT-007 | CONFIRMED | Subscription | Lifecycle representation of Lemon Squeezy service status. | Governs Tenant availability. | SRC-001 §5.1, §6 |
| ENT-008 | CONFIRMED | Pending Tenant / Signup | Business onboarding case that proceeds through Checkout and Payment/Webhook to Tenant/Subscription activation. | Connects Pending Tenant, checkout, initial Tenant Admin, and Subscription. | SRC-001 §6.1; BMD-004 |
| ENT-009 | CONFIRMED | Quote | Tenant-created customer quotation with items, amounts, dates, terms, notes, PDF output, and immutable sent revisions. | Identifies a Tenant and Customer/Client; Client responds only to current effective Revision. | SRC-001 §5.13–§5.15; FMD-007 |
| ENT-010 | CONFIRMED | PI | Tenant-created Proforma Invoice with items, amounts, dates, terms, notes, PDF output, and revisions. | Identifies a Tenant and Customer/Client; may reference source Quote without mandatory conversion. | SRC-001 §5.14–§5.15; BMD-007; FMD-008 |
| ENT-011 | CONFIRMED | File | Permissioned Tenant file or folder content, possibly uploaded by a Client. | Belongs to exactly one Tenant; may optionally associate with at most one Client Account; V1 uses Soft Delete. | SRC-001 §5.9–§5.10; FMD-009 |
| ENT-012 | CONFIRMED | File Association | Explicit optional relationship of File to one Client Account. | Connects File and one Client Account; Ticket, Quote, and PI references stay in relationship models. | SRC-001 §5.9–§5.10; FMD-009 |
| ENT-013 | CONFIRMED | Ticket | Client service request with replies/attachments, assignment, status, priority, closure, and reopen. | Belongs to Tenant + Client Account; may reference Client Contact; has 0..1 Staff assignee. | SRC-001 §5.11–§5.12; FMD-006 |
| ENT-014 | CONFIRMED | Audit Event | Record of named security/support-relevant actions. | Identifies an action in Tenant context where applicable. | SRC-001 §13 |
| ENT-016 | CONFIRMED | Document Revision | Immutable historical revision of sent Quote or Issued/Sent PI. | Identifies current/effective document revision without overwriting history. | FMD-007, FMD-008 |
| ENT-017 | CONFIRMED | Domain | Tenant custom-domain lifecycle record. | Belongs to one Tenant only; state may be Pending, Waiting DNS, Verifying, SSL Pending, Active, Failed, or Disabled. | SRC-001 §5.5–§5.6; FMD-010 |

## Catalog guardrails

| Status | Statement |
| --- | --- |
| REJECTED | Treat this catalog as a physical schema, API definition, or authorization matrix. |
| OPEN | Exact identifiers, cardinalities, required attributes, retention, uniqueness, and deletion behavior for the accepted business entities. |
