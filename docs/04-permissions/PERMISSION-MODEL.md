# Permission Model

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**.

## Boundary

| Status | Statement |
| --- | --- |
| CONFIRMED | Authorization is defined as Resource × Action × Role × Ownership × Tenant State. |
| CONFIRMED | This is a business authorization model, not Architecture, ERD, API, Task, or Coding specification. |
| CONFIRMED | Cross-Tenant access by Tenant users is prohibited. |
| OPEN | Enforcement mechanism and implementation placement belong to later authorized work. |

## Rule register

Ownership abbreviations: **own-Tenant** = resource belongs to actor's Tenant; **own-Client** = resource belongs to Client Account scope; **platform** = platform operating record.

| ID | Status | Resource × Action | Role | Ownership / tenant scope | Tenant State | Result | Preconditions | Audit | Basis |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| PR-001 | CONFIRMED | Any tenant resource × any business action | Tenant Admin / Staff / Client | Cross-Tenant | all | DENY | Always | denied-access detail OPEN | SRC-001 §5.2 |
| PR-002 | CONFIRMED | Client-scoped resource × read/action | Client | own-Client, own-Tenant | ACTIVE | CONDITIONAL | active Client Membership and resource relationship | action audit per resource | SRC-001 §3.4; BMD-002 |
| PR-003 | CONFIRMED | Tenant business resource × business action | Tenant Admin / Staff / Client | own-Tenant | PENDING | DENY | Tenant not activated | activation audit | FMD-005 |
| PR-004 | CONFIRMED | Suspension reason, Billing Recovery, Account/Profile × read/recover | Tenant Admin | own-Tenant | SUSPENDED | ALLOW | active Tenant Admin Membership | recovery/account audit detail OPEN | BMD-005 |
| PR-005 | CONFIRMED | Tenant business resource × business action | Tenant Admin | own-Tenant | SUSPENDED | DENY | Always outside allowed recovery surfaces | denied-access detail OPEN | BMD-005 |
| PR-006 | CONFIRMED | Tenant business resource × business action | Staff / Client | own-Tenant | SUSPENDED | DENY | Always | denied-access detail OPEN | BMD-005 |
| PR-007 | CONFIRMED | Tenant business resource × business action | Tenant roles | own-Tenant | SUSPENDED | DENY | Subscription lifecycle is `EXPIRED`; Tenant access state is `SUSPENDED` | Subscription Suspended | PDM-006 accepted; SRC-001 §6.3 |
| PR-008 | CONFIRMED | Tenant lifecycle / Subscription × manage | Platform Admin | platform / target Tenant | all | CONDITIONAL | platform operating purpose | Tenant/subscription event | SRC-001 §3.1, §5.1 |
| PR-009 | CONFIRMED | Tenant Client / File / Ticket / Quote / PI data × read | Platform Admin | target Tenant | all Tenant access states | DENY | platform operations do not imply Tenant business-data access; any future exception is explicit break-glass only | future break-glass audit if explicitly designed | PDM-001 accepted |
| PR-010 | CONFIRMED | Tenant profile / state × manage | Tenant Admin | own-Tenant | ACTIVE | ALLOW | active Tenant Admin Membership | Tenant change | SRC-001 §3.2 |
| PR-011 | CONFIRMED | Membership / Staff × create, invite, disable | Tenant Admin | own-Tenant | ACTIVE | ALLOW | target belongs to own Tenant | Invite User / disable detail | SRC-001 §5.7 |
| PR-012 | CONFIRMED | Staff × invite, disable, role change | Staff | own-Tenant | ACTIVE | DENY | Staff has no Staff-administration authority | denied detail OPEN | PDM-002 accepted |
| PR-013 | CONFIRMED | Own Profile × read, modify | Staff | own-Tenant | ACTIVE | ALLOW | active Membership | profile audit OPEN | PDM-002 accepted |
| PR-014 | CONFIRMED | Client Account × create, edit, invite, disable | Tenant Admin / Staff | own-Tenant | ACTIVE | ALLOW | actor is authorized Tenant user | Client change / Invite User | SRC-001 §5.8; FMD-001 |
| PR-015 | CONFIRMED | Client Contact × create, edit | Tenant Admin / Staff | own-Tenant | ACTIVE | ALLOW | Contact belongs to Client Account | Contact audit detail OPEN | PDM-002 accepted |
| PR-016 | CONFIRMED | Invitation × create, resend, revoke | Tenant Admin; Staff for Client invitation | own-Tenant | ACTIVE | CONDITIONAL | Contact/Tenant scope valid; email unique per Tenant | Invite User | SRC-001 §5.7–§5.8; FMD-004 |
| PR-017 | CONFIRMED | Invitation × accept | Anonymous / Invited User | addressed Contact only | ACTIVE | CONDITIONAL | Pending invitation for recipient Contact | acceptance audit detail OPEN | FMD-002–004 |
| PR-018 | PROPOSED | Any tenant business resource × business action | Anonymous / Invited User | none | all | DENY | except PR-017 invitation acceptance | denied-access detail OPEN | derived least-access proposal |
| PR-019 | CONFIRMED | Branding × manage | Tenant Admin | own-Tenant | ACTIVE | ALLOW | active Tenant Admin Membership | branding audit detail OPEN | SRC-001 §3.2, §5.3 |
| PR-020 | CONFIRMED | Domain × add, retry, remove | Tenant Admin | own-Tenant | ACTIVE | CONDITIONAL | domain belongs only to own Tenant | Add/Verify Domain | SRC-001 §5.5–§5.6; FMD-010 |
| PR-021 | CONFIRMED | Domain × disable | Platform Admin | target Tenant domain | all | ALLOW | platform operating purpose | Domain disable audit detail OPEN | FMD-010 |
| PR-022 | CONFIRMED | File / Folder × manage, associate, soft delete | Tenant Admin / Staff | own-Tenant | ACTIVE | CONDITIONAL | File belongs own Tenant; optional one Client Account association | Upload/Delete File | SRC-001 §5.9–§5.10; FMD-009 |
| PR-023 | CONFIRMED | File × view, download | Client | own-Client, own-Tenant | ACTIVE | CONDITIONAL | File is explicitly Client-visible, associated to Client's own Client Account, and download is authorized before short-lived Temporary Secure URL issuance | Upload File; download audit OPEN | SRC-001 §5.9–§5.10; DEC-031 |
| PR-024 | CONFIRMED | File × restore / reassociate | Tenant Admin / Staff | own-Tenant | ACTIVE | CONDITIONAL | same-Tenant Soft Deleted/current File; Tenant ownership never changes | restore/reassociation audit OPEN | PDM-005 accepted |
| PR-025 | CONFIRMED | Ticket / Reply × create, reply, view | Client | own-Client, own-Tenant | ACTIVE | CONDITIONAL | Ticket belongs to Client Account | Create/Update Ticket | SRC-001 §5.11–§5.12; FMD-006 |
| PR-026 | CONFIRMED | Ticket / Reply × manage, assign 0..1 Staff, change status/priority, reopen | Tenant Admin / Staff | own-Tenant | ACTIVE | CONDITIONAL | actor ticket authority; Ticket's Client scope preserved | Update Ticket; reopen audit | SRC-001 §5.11–§5.12; FMD-006 |
| PR-027 | CONFIRMED | Ticket Priority × modify | Client | own-Client | ACTIVE | DENY | Always | denied attempt detail OPEN | FMD-006 |
| PR-028 | CONFIRMED | Ticket state transition / assignment/reassignment × allowed action | Tenant Admin / Staff | own-Tenant | ACTIVE | CONDITIONAL | Ticket scope retained; Client has no management-state/assignee action | transition audit required | PDM-004 accepted |
| PR-029 | CONFIRMED | Quote / Revision × create, send, revise, cancel | Tenant Admin / Staff | own-Tenant | ACTIVE | CONDITIONAL | Sent content creates new Revision, not overwrite | Create Quote; revision audit detail OPEN | SRC-001 §5.13; FMD-007 |
| PR-030 | CONFIRMED | Current Quote Revision × view, PDF, accept, reject | Client | own-Client, own-Tenant | ACTIVE | CONDITIONAL | revision is current effective Revision | client response audit OPEN | BMD-006; FMD-007 |
| PR-031 | CONFIRMED | PI / Revision × create, issue, send, revise, cancel | Tenant Admin / Staff | own-Tenant | ACTIVE | CONDITIONAL | Issued/Sent content creates new Revision | Create PI; revision audit detail OPEN | SRC-001 §5.14; FMD-008 |
| PR-032 | CONFIRMED | PI / Revision × view, PDF | Client | own-Client, own-Tenant | ACTIVE | CONDITIONAL | Client-associated PI only | view/download audit OPEN | SRC-001 §3.4, §5.14 |
| PR-033 | CONFIRMED | Subscription / Billing Recovery × view, recover | Tenant Admin | own-Tenant | ACTIVE / SUSPENDED | CONDITIONAL | Billing state for own Tenant; suspended scope limited by PR-004 | subscription event audit | SRC-001 §3.2, §6; BMD-005 |
| PR-034 | CONFIRMED | Audit Log × read | Tenant Admin / Staff / Client | own-Tenant / own-Client | ACTIVE | CONDITIONAL | Tenant Admin sees own-Tenant business Audit; Staff denied full log; Client denied log but may see own business activity history; sensitive security fields only authorized admins | audit-log access audit | PDM-004 accepted |
| PR-035 | CONFIRMED | Audit Log / Webhook / system log × read | Platform Admin | platform | all | ALLOW | platform operating purpose | log-access detail OPEN | SRC-001 §3.1 |
| PR-036 | PROPOSED | Platform configuration × manage | Platform Admin | platform | all | ALLOW | platform operating purpose | configuration audit detail OPEN | SRC-001 §3.1 |
| PR-037 | CONFIRMED | Ticket × reopen | Client | own-Client, own-Tenant | ACTIVE | CONDITIONAL | Ticket visible to Client; Client does not change Priority/Assignee/management state | reopen audit | PDM-004 accepted |
| PR-038 | CONFIRMED | Client Contact own profile × modify | Client | own Contact, own-Tenant | ACTIVE | CONDITIONAL | active Client Membership | profile audit OPEN | PDM-003 accepted |
| PR-039 | CONFIRMED | Client Contact × delete/create other Contact / Client Account core fields × modify | Client | own-Tenant | ACTIVE | DENY | Client has no Client Admin role | denied detail OPEN | PDM-003 accepted |
| PR-040 | CONFIRMED | Published Quote/PI Revision × modify / history × read | Tenant Admin / Staff / Client | own-Tenant / own-Client | ACTIVE | DENY modify / ALLOW history per scope | published Revision is read-only and retained | revision audit OPEN | PDM-005 accepted |
| PR-041 | CONFIRMED | Tenant business action × normal use | Tenant Admin / Staff / Client | own-Tenant | ACTIVE | CONDITIONAL | Subscription lifecycle is `CANCELLED` and current paid period has not ended | subscription audit | PDM-006 accepted |
| PR-042 | CONFIRMED | Subscription expiry × Tenant access restriction | Tenant roles | own-Tenant | SUSPENDED | DENY business after paid period without recovery | Subscription lifecycle is `EXPIRED`; Tenant access is `SUSPENDED` after current paid period ends without renewal/recovery | Subscription Suspended | PDM-006 accepted |
| PR-043 | CONFIRMED | File × upload | Client | own-Client, own-Tenant | ACTIVE | CONDITIONAL | Default DENY; Tenant Admin/Staff has explicitly enabled upload for current Client Account under a business rule. Created File is private, Tenant-bound, and associated to current Client Account. | Upload File | DEC-031 |
| PR-044 | CONFIRMED | File × change Tenant / Client Account association, restore | Client | own-Client, own-Tenant | ACTIVE | DENY | Client never changes File Tenant or Client Account ownership and has no Restore authority. | denied attempt detail OPEN | DEC-031; PDM-005 |

## Counts

| Status | Rules |
| --- | ---: |
| CONFIRMED | 42 |
| PROPOSED | 2 |
| OPEN | 0 |
| REJECTED | 0 |

**Total permission rules: 44.** Counts above classify PR-001 through PR-044 only.
