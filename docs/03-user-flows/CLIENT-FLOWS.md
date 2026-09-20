# Client Flows

## CF-01 — Client Account onboarding and portal entry

**Status: CONFIRMED access boundary with OPEN profile/permission details**  
**Source support:** SRC-001 §3.4, §5.8–§5.10; BMD-002–BMD-003; FMD-001–FMD-004.

- **Actor:** Tenant Admin/Staff; Client; activated Client Membership.
- **Preconditions:** Tenant is active; Client Account / Profile has been created by Tenant Admin/Staff; Client Contact invitation has reached Accepted.
- **Trigger:** Client signs in to the Tenant portal.
- **Happy Path:** Global User authenticates → Tenant-scoped Client Membership is resolved → Client accesses own dashboard, Files, Tickets, Quote, PI, and Profile only.
- **Alternative Paths:** Client Account/Profile can exist before login User; an existing Global User is reused; Client Accounts are not auto-merged by company name.
- **Failure Paths:** Authentication, Membership resolution, or Client Account linkage fails; portal access is denied. Recovery details are **OPEN**.
- **Permission Boundary:** Client cannot access another Client's or internal Tenant data; cross-Tenant access is prohibited.
- **State Changes:** No mandatory business state change on successful portal entry; active Membership is used for access.
- **Audit Events:** Login is explicitly named in SRC-001 §13; Client portal-access event detail is **OPEN**.
- **Notification/Email dependency:** Login and onboarding notifications are **OPEN**.
- **Final State:** Client reaches only authorized personal resources, or access is denied.
- **OPEN issues:** Client Account/Profile field policy, detailed permission matrix, account recovery, and notification recipient/timing.

## CF-02 — Client profile update

**Status: CONFIRMED limited capability with OPEN field/approval policy**  
**Source support:** SRC-001 §3.4, §14; Client Profile is a named page.

- **Actor:** Activated Client.
- **Preconditions:** Active Tenant and active Client Membership.
- **Trigger:** Client opens Profile and submits an update.
- **Happy Path:** Client views and updates only the permitted part of its own profile.
- **Alternative Paths:** Client abandons the update; no profile change occurs.
- **Failure Paths:** Validation, authorization, or suspension boundary prevents update; details are **OPEN**.
- **Permission Boundary:** Client is limited to its own profile; suspended Tenant Client has no business access.
- **State Changes:** Client Profile data may change; editable fields and approval requirements are **OPEN**.
- **Audit Events:** Profile-change event taxonomy is **OPEN**.
- **Notification/Email dependency:** Profile-update notification is **OPEN**.
- **Final State:** Authorized profile update is retained, or no change occurs.
- **OPEN issues:** Editable fields, review/approval, audit, and notifications.
