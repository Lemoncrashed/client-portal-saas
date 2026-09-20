# Identity and Invitation Flows

## IF-01 — Staff invitation and activation

**Status: CONFIRMED baseline with OPEN notification/security dependencies**  
**Source support:** SRC-001 §5.7; BMD-001–BMD-003; FMD-004.

- **Actor:** Tenant Admin; invited Staff; existing Global User where applicable.
- **Preconditions:** Tenant is active; Tenant Admin has Staff-management authority; target role/type is Staff.
- **Trigger:** Tenant Admin creates or sends a Staff invitation.
- **Happy Path:** Invitation Pending → Accepted; acceptance establishes/activates Staff Membership. Existing User is linked and not duplicated.
- **Alternative Paths:** A Pending invitation may be resent only by invalidating its old token and issuing a replacement token; Expired/Revoked tokens are invalid and never grant membership access.
- **Failure Paths:** Email notification failure does not roll back the invitation/business transaction; invitation remains Pending or requires resend. Identity/security failure handling is **OPEN**.
- **Permission Boundary:** Tenant Admin may invite/disable Staff for its Tenant. The invited person receives no Staff business access before Accepted.
- **State Changes:** Invitation Pending / Accepted / Expired / Revoked; acceptance activates Staff Membership exactly once. An Accepted invitation cannot be accepted again.
- **Audit Events:** Invite User is explicitly named in SRC-001 §13; acceptance/activation/disable event detail is **OPEN**.
- **Notification/Email dependency:** Invitation is separate from business Notification. V1 external channel is Email; notification failure does not roll back core transaction. Recipient/timing policy remains **OPEN**.
- **Final State:** Active Staff Membership, or a non-active invitation outcome.
- **OPEN issues:** Expiry duration, identity verification, session recovery, notification recipient/timing, and exact Staff permission matrix remain under the Identity Architecture Gate.

## IF-02 — Client invitation and activation

**Status: CONFIRMED baseline with OPEN access-policy dependency**  
**Source support:** SRC-001 §3.4, §5.8; BMD-001–BMD-003; FMD-001–FMD-004.

- **Actor:** Tenant Admin or Staff; invited Client; existing Global User where applicable.
- **Preconditions:** Tenant is active; Tenant Admin/Staff creates Client Account if needed; target Client Contact belongs to that Tenant and has Tenant-unique email.
- **Trigger:** Authorized Tenant user sends Invitation to Client Contact.
- **Happy Path:** Client Account exists independently → Contact receives Pending invitation → Accepted → existing Global User is reused or created once → Client Membership is connected to Client Account / Profile.
- **Alternative Paths:** Client Account may exist without login User; a Pending invitation may be resent only by invalidating its old token and issuing a replacement token; Expired/Revoked tokens are invalid. Accounts are not auto-merged by company name.
- **Failure Paths:** Client cannot accept/activate, or Client Account linkage cannot be completed; no Client portal access is granted.
- **Permission Boundary:** Tenant Admin/Staff create Client Account and invite Client Contact within their Tenant; Client sees only its own approved portal resources after acceptance.
- **State Changes:** Client Account created/retained; Contact Invitation Pending/Accepted/Expired/Revoked; acceptance activates Client Membership exactly once and cannot be repeated; no company-name auto-merge.
- **Audit Events:** Invite User is source-supported; Client Account linkage/activation audit detail is **OPEN**.
- **Notification/Email dependency:** Invitation delivery is Email-only in V1, separate from business Notification; delivery failure does not roll back account/invitation transaction. Recipient/timing policy remains **OPEN**.
- **Final State:** Activated Client Membership associated to a Client Account / Profile, or a non-active invitation outcome.
- **OPEN issues:** Client Account/Profile field policy, Contact lifecycle beyond invitation, detailed Client access rules, and notification recipient/timing.
