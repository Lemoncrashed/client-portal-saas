# Quote and PI Flows

## QF-01 — Quote draft, issue, and Client response

**Status: CONFIRMED lifecycle with OPEN numbering/notification dependency**  
**Source support:** SRC-001 §5.13–§5.15; BMD-006; FMD-007.

- **Actor:** Tenant Admin or Staff; activated Client for response.
- **Preconditions:** Tenant is active; creator has Quote authority; intended Client/customer is identified.
- **Trigger:** Tenant Admin/Staff creates or sends Quote.
- **Happy Path:** Draft → Sent → Client may View / Download PDF / Accept / Reject only the current effective Revision → Quote becomes Accepted or Rejected; unacted Sent Quote can become Expired; authorized cancellation yields Cancelled.
- **Alternative Paths:** Quote remains Draft before sending; it may become Expired or Cancelled without Client response.
- **Failure Paths:** Invalid data, unauthorized actor, Client access mismatch, or attempt to overwrite Sent content prevents operation; modification creates Revision instead.
- **Permission Boundary:** Tenant Admin/Staff create/manage Quote within Tenant; Client can only View/Download/Accept/Reject its own Quote. E-signature is excluded.
- **State Changes:** Draft / Sent / Accepted / Rejected / Expired / Cancelled; Sent-content modification creates Revision and retains history.
- **Audit Events:** Create Quote is explicitly named in SRC-001 §13; send, client response, expiry, cancellation, and PDF-download event details are **OPEN**.
- **Notification/Email dependency:** V1 external notification channel is Email; notification failure does not roll back Quote transaction. Recipient/timing rules are **OPEN**.
- **Final State:** Quote has one of the accepted lifecycle states and remains scoped to Tenant/Client.
- **OPEN issues:** OQ-008 — terminal/revision transition details are a LOCAL BLOCKER before Quote/PI Architecture / API Contract; required closure evidence is an accepted Quote/PI Transition & Revision Matrix. Numbering, conversion, and notification policy remain separately decision-gated.

## PF-01 — PI issue and optional Quote reference

**Status: CONFIRMED Proforma lifecycle with OPEN numbering/notification dependency**  
**Source support:** SRC-001 §5.14–§5.15; BMD-007; FMD-008; SRC-001 §15.

- **Actor:** Tenant Admin or Staff; Client for view/download.
- **Preconditions:** Tenant is active; creator has PI authority; intended Client/customer is identified; optional source Quote may be selected.
- **Trigger:** Tenant Admin/Staff creates or issues PI.
- **Happy Path:** Draft → Issued → Sent → Viewed by Client; optional source Quote reference is retained if selected. Unacted PI may Expire; authorized cancellation yields Cancelled.
- **Alternative Paths:** PI is created without Quote; Quote-to-PI conversion is not mandatory or automatic.
- **Failure Paths:** Invalid data, cross-Tenant Client/Quote reference, unauthorized actor, or attempt to overwrite Issued/Sent content prevents issue; modification creates Revision instead.
- **Permission Boundary:** Tenant Admin/Staff create/manage PI in their Tenant; Client sees only its own PI. PI does not authorize online payment, recurring invoice, or accounting activity.
- **State Changes:** Draft / Issued / Sent / Viewed / Expired / Cancelled; Issued/Sent modification creates Revision and retains history.
- **Audit Events:** Create PI is explicitly named in SRC-001 §13; issue/reference/PDF-download audit detail is **OPEN**.
- **Notification/Email dependency:** V1 external notification channel is Email; notification failure does not roll back PI transaction. Recipient/timing rules are **OPEN**.
- **Final State:** Tenant-scoped Proforma PI is available to its authorized Client, with optional Quote reference.
- **OPEN issues:** OQ-008 — terminal/revision transition details are a LOCAL BLOCKER before Quote/PI Architecture / API Contract; required closure evidence is an accepted Quote/PI Transition & Revision Matrix. Numbering and notification policy remain separately decision-gated.
