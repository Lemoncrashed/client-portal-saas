# Resource Policies

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**.

Each policy resolves to the detailed PR rule register in `PERMISSION-MODEL.md`.

| Resource | CONFIRMED policy | Proposed / Open boundary | Rules |
| --- | --- | --- | --- |
| Tenant | Tenant Admin manages own active Tenant; platform operates Tenant lifecycle; Tenant roles never cross tenants. | Platform Admin business-data read is CONFIRMED DENY by default. | PR-001, 008–010 |
| Membership / Staff | Tenant Admin manages Staff Membership; accepted invitation activates Membership. | Staff cannot invite/disable Staff or change Staff Role; own Profile only. | PR-011–013, 016–018 |
| Client Account / Contact | Tenant Admin/Staff creates Client Account and Client Contact; Client Contact is separate, Tenant-email-unique, and receives Invitation. | Client self-service is personal fields only; no Client Admin. | PR-014–017, 038–039 |
| Branding | Tenant Admin manages own branding while active. | Staff/Client direct configuration is PROPOSED default DENY; source does not enumerate it. | PR-019 |
| Domain | Tenant Admin adds/retries/removes own Domain; Platform Admin disables; Domain has one Tenant only. | Validation-method and notification detail OPEN. | PR-020–021 |
| File / Folder | File is private, Tenant-bound, and optionally associated to one Client Account. Tenant Admin/Staff manage, reassociate, and restore only within Tenant; Soft Delete/Restore preserves historical references. Client views/downloads only explicit Client-visible Files for own Client Account; Client upload is default DENY and conditional only when enabled for that Client Account. | URL TTL, size/type limits, malware-scan implementation, and notification recipient/timing are deferred to the File/Security Architecture Gate. | PR-022–024, 043–044 |
| Ticket / Reply | Client works within own Client Account and may reopen; Staff may assign/reassign, Priority, and allowed states; Client cannot change Priority/Assignee/management state. | Notification recipient/timing OPEN / DEFERRED. | PR-025–028, 037 |
| Quote / Revision | Tenant team creates/sends/revises; sent history immutable; Client acts only on current effective Revision. | Numbering/conversion detail OPEN. | PR-029–030 |
| PI / Revision | Tenant team creates/issues/sends/revises; Client views/PDF only; no invoice/payment/credit note. | Numbering/revision identifier and audit visibility OPEN. | PR-031–032 |
| Subscription / Billing Recovery | Tenant Admin views own billing and recovery; suspension policy restricts this scope. | Commercial entitlement rules OPEN. | PR-033 |
| Audit Log | Platform Admin reads platform Audit; Tenant Admin reads own-Tenant business Audit; Client reads own activity history only. | Sensitive field/redaction policy remains administrator-scoped; detailed field list OPEN. | PR-034–035 |
