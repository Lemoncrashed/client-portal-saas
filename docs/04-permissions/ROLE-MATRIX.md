# Role Matrix

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**.

| Resource area | Platform Admin | Tenant Admin | Staff | Client | Anonymous / Invited User | Status / basis |
| --- | --- | --- | --- | --- | --- | --- |
| Platform operation | Manage platform, tenants, subscriptions, domains, webhooks/logs | DENY | DENY | DENY | DENY | CONFIRMED for Platform Admin scope; SRC-001 §3.1 |
| Tenant profile / branding | Platform-operate only; tenant business-data read DENY | Manage own Tenant | DENY manage | View only where branded surface is exposed | DENY | CONFIRMED |
| Membership / Staff | Platform-operate Tenant state | Manage own Tenant Staff | Cannot invite/disable Staff or modify Staff Role; own Profile only | No access to internal Staff | Invitation acceptance only | CONFIRMED |
| Client Account / Contact | Tenant business-data read: CONFIRMED DENY. Future break-glass is separate, explicit, time-limited, and fully audited; not V1 default. | Create/manage own Tenant | Create/edit Client Account and Contact | Own Contact personal fields only | DENY | PDM-001; PR-009; FMD-001–003; PDM-002–003 |
| Invitation | Platform log/operate only | Create/resend/revoke | Staff → Staff invitation: CONFIRMED DENY. Staff → Client invitation: CONFIRMED CONDITIONAL, only for valid same-Tenant Client Contact with Tenant-unique email. | Accept own Contact invitation | Accept own Pending invitation | PDM-002; PR-012, PR-016; FMD-004 |
| Domain | Disable | Add/retry/remove own Tenant custom Domain | DENY | DENY | DENY | FMD-010 |
| File / Folder | Tenant business-data read: CONFIRMED DENY. Future break-glass is separate, explicit, time-limited, and fully audited; not V1 default. | Manage/restore/reassociate own Tenant | Manage/restore/reassociate own Tenant | Own associated File / allowed upload | DENY | PDM-001; PR-009; FMD-009; PDM-005 |
| Ticket / Reply | Tenant business-data read: CONFIRMED DENY. Future break-glass is separate, explicit, time-limited, and fully audited; not V1 default. | Manage own Tenant | Assign/reassign, Priority, allowed states | Own Client Account; create/reply/reopen only | DENY | PDM-001; PR-009; FMD-006; PDM-004 |
| Quote / Revision | Tenant business-data read: CONFIRMED DENY. Future break-glass is separate, explicit, time-limited, and fully audited; not V1 default. | Create/send/revise/cancel | Create/send/revise/cancel | Own current revision: view/PDF/accept/reject | DENY | PDM-001; PR-009; FMD-007 |
| PI / Revision | Tenant business-data read: CONFIRMED DENY. Future break-glass is separate, explicit, time-limited, and fully audited; not V1 default. | Create/issue/send/revise/cancel | Create/issue/send/revise/cancel | Own PI: view/PDF | DENY | PDM-001; PR-009; FMD-008 |
| Subscription / Recovery | Operate platform state | Own billing/recovery; suspended limits apply | DENY | DENY | DENY | BMD-005 |
| Audit Log | Platform Audit only | Own-Tenant business Audit | No complete Audit Log | No Audit Log; own activity history only | DENY | PDM-004 |

## Role guardrails

| Status | Statement |
| --- | --- |
| CONFIRMED | Tenant A actors never access Tenant B resources. |
| CONFIRMED | Client access is constrained to own Client Account scope. |
| CONFIRMED | Staff is not granted Tenant Admin powers by role name alone. |
| CONFIRMED | Platform Admin has no default cross-Tenant tenant-business-data read access; any future support exception needs explicit, time-limited, fully audited break-glass design. |
| OPEN | Notification recipients/timing, detailed document identifiers, and policy parameters are deferred. |
