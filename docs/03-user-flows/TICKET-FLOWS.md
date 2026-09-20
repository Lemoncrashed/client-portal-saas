# Ticket Flows

## TK-01 — Client ticket creation and reply

**Status: CONFIRMED core action with OPEN recipient/notification policy**  
**Source support:** SRC-001 §5.11–§5.12; FMD-006.

- **Actor:** Activated Client.
- **Preconditions:** Tenant is active; Client Membership is active; Ticket is scoped to Tenant + Client Account and may reference Client Contact.
- **Trigger:** Client creates a Ticket, replies, or uploads an attachment.
- **Happy Path:** Client creates Ticket for its Client Account → Ticket is Open → Client and authorized Tenant team exchange replies; Client can view its own Ticket.
- **Alternative Paths:** Ticket may reference Client Contact and have 0..1 Staff assignee; Client adds attachment; Ticket may move to In Progress, Waiting for Client, Resolved, or Closed.
- **Failure Paths:** Attachment/validation/authorization fails; Ticket or reply is not created. Exact retry behavior is **OPEN**.
- **Permission Boundary:** Client can access only its own Ticket; cannot view Tenant-internal or another Client's Ticket.
- **State Changes:** Open / In Progress / Waiting for Client / Resolved / Closed; Resolved or Closed may reopen with Audit. Client cannot modify Priority.
- **Audit Events:** Create Ticket and Update Ticket are explicitly named in SRC-001 §13; reply/attachment audit detail is **OPEN**.
- **Notification/Email dependency:** V1 external notification channel is Email; notification failure does not roll back Ticket/reply transaction. Recipient/timing rules are **OPEN**.
- **Final State:** Ticket/reply is visible only to authorized Client and Tenant users, or operation fails without exposure.
- **OPEN issues:** Exact actor transition matrix is a LOCAL BLOCKER before Ticket Architecture / API Contract; required closure evidence is an accepted Ticket Transition Matrix. Attachment and notification policy remain separately decision-gated.

## TK-02 — Staff ticket handling

**Status: CONFIRMED core action with OPEN routing policy**  
**Source support:** SRC-001 §5.11–§5.12; FMD-006.

- **Actor:** Tenant Staff or Tenant Admin.
- **Preconditions:** Tenant is active; actor has ticket-handling authority; Ticket belongs to actor's Tenant.
- **Trigger:** Staff views/replies, assigns owner, changes status/priority, or closes Ticket.
- **Happy Path:** Staff handles Ticket, optionally assigns one Staff owner, updates status/priority as authorized, replies, and closes when resolved.
- **Alternative Paths:** Ticket is set Waiting for Client; priority is Low/Normal/High/Urgent.
- **Failure Paths:** Cross-Tenant access, unauthorized assignment, Client priority modification, or invalid transition is denied; no state change.
- **Permission Boundary:** Staff/Tenant Admin work only within their Tenant; Client-facing visibility remains limited to the associated Client.
- **State Changes:** Open / In Progress / Waiting for Client / Resolved / Closed; Resolved/Closed may reopen and audit; priority Low / Normal / High / Urgent.
- **Audit Events:** Update Ticket is source-supported; assignment, priority, closure, and reply event detail is **OPEN**.
- **Notification/Email dependency:** V1 external notification channel is Email; failure does not roll back core Ticket transaction. Recipient/timing rules are **OPEN**.
- **Final State:** Ticket reaches an authorized current status with scoped visibility.
- **OPEN issues:** Exact actor transition matrix is a LOCAL BLOCKER before Ticket Architecture / API Contract; required closure evidence is an accepted Ticket Transition Matrix. Assignment eligibility, attachment, and notification policy remain separately decision-gated.
