# File Flows

## FF-01 — Staff file management and Client association

**Status: CONFIRMED ownership, visibility, and retention boundary with deferred File/Security Architecture policy**  
**Source support:** SRC-001 §5.9–§5.10; BMD-008; FMD-009.

- **Actor:** Tenant Admin or Staff.
- **Preconditions:** Tenant is active; actor has file-management authority; target File belongs to the actor's Tenant.
- **Trigger:** Actor creates folder, uploads, renames, deletes, searches, downloads, or associates File with Client Account.
- **Happy Path:** File is private and managed within one Tenant; authorized actor optionally associates it with at most one Client Account. Tenant Admin/Staff may manage, reassociate, and restore only within that Tenant; client-facing access follows the explicit Client-visible association and authorization.
- **Alternative Paths:** File remains Tenant-only without Client association; Ticket, Quote, PI, and other records reference File through relationships.
- **Failure Paths:** File violates unconfirmed policy, Client Account association is invalid, or authorization fails; operation does not complete.
- **Permission Boundary:** File always belongs to exactly one Tenant. Actor cannot manage another Tenant's File. File is private and not directly public. Tenant Admin/Staff may change Client Account association only within the same Tenant; Tenant ownership never changes.
- **State Changes:** File Added/Available; optional single Client association added/changed/removed; delete is Soft Delete and preserves Ticket/Quote/PI history.
- **Audit Events:** Upload File and Delete File are explicitly named in SRC-001 §13; rename/association events are **OPEN**.
- **Notification/Email dependency:** V1 external notification channel is Email; notification failure does not roll back core File transaction. Recipient/timing policy remains **OPEN**.
- **Final State:** Tenant-scoped File is available with optional Client association, or operation fails/no change.
- **OPEN issues:** OQ-009; file size/type, malware-scan implementation, storage/secure-URL TTL, and notification recipient/timing are deferred to the File/Security Architecture Gate.

## FF-02 — Client file view, download, and allowed upload

**Status: CONFIRMED Client access and upload policy with deferred File/Security Architecture policy**  
**Source support:** SRC-001 §5.9–§5.10; BMD-008; FMD-009.

- **Actor:** Activated Client.
- **Preconditions:** Tenant is active; Client Membership is active; File is in the same Tenant, associated with the Client's own Client Account, and explicitly Client-visible. Client upload is default DENY unless Tenant Admin/Staff has enabled it for that Client Account under a business rule.
- **Trigger:** Client views/downloads an authorized File or uploads a File.
- **Happy Path:** Authorization succeeds → a short-lived Temporary Secure URL is issued for download. An explicitly allowed Client upload creates a private, Tenant-bound File associated to the current Client Account.
- **Alternative Paths:** Client sees no File if the Tenant, Client Account, or explicit Client-visible condition fails; default upload denial applies unless enabled for that Client Account.
- **Failure Paths:** Authorization fails, secure URL cannot be issued, upload is not enabled, or the upload violates deferred File/Security policy; no unauthorized resource is exposed.
- **Permission Boundary:** Client may access only same-Tenant Files associated to its own Client Account and explicitly Client-visible. Client cannot change File Tenant ownership or Client Account association, and cannot Restore a File.
- **State Changes:** Download need not change a business state; an allowed upload creates private Tenant-bound File associated to current Client Account; Soft Delete/Restore by Tenant Admin/Staff preserves historical references.
- **Audit Events:** Upload File is source-supported; download/audit detail is **OPEN**.
- **Notification/Email dependency:** V1 external notification channel is Email; failure does not roll back core File transaction. Recipient/timing policy remains **OPEN**.
- **Final State:** Authorized download/upload completes within Tenant boundary, or access/operation is denied.
- **OPEN issues:** Exact URL TTL, size/type limits, malware-scan implementation, and notification recipient/timing are deferred; they do not block PD-V1 Freeze.
