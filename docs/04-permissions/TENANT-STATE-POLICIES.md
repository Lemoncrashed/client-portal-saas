# Tenant State Policies

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**.

## State axes

| Axis | Status | States | Rule | Reference |
| --- | --- | --- | --- | --- |
| Subscription lifecycle | CONFIRMED | ACTIVE / CANCELLED / EXPIRED; recovery or renewal returns to ACTIVE under the existing model. | `CANCELLED` remains entitled during the current paid period. `EXPIRED` is a Subscription state, never a Tenant access state. | PDM-006; DEC-028 |
| Tenant access state | CONFIRMED | PENDING / ACTIVE / SUSPENDED | Authorization is evaluated against this axis. A Subscription reaching `EXPIRED` after the paid period causes Tenant access to become `SUSPENDED`. | FMD-005; BMD-005; PDM-006; DEC-028 |
| Future inactive-equivalent | OPEN | Not defined. | Do not introduce a new access state without a governed decision. | OQ-010 |

## Tenant access policies

| Tenant access state | Actor | Resource / Action | Result | Preconditions / scope | Audit | Basis |
| --- | --- | --- | --- | --- | --- | --- |
| PENDING | Platform onboarding context | Checkout and activation progression | CONDITIONAL | Pending Tenant only | onboarding event | FMD-005 |
| PENDING | Tenant Admin / Staff / Client | Tenant business action | DENY | Tenant not active | activation/denial detail OPEN | FMD-005 |
| ACTIVE | Tenant Admin | Own Tenant authorized admin actions | ALLOW / CONDITIONAL | own Tenant and resource policy | per resource | SRC-001 §3.2 |
| ACTIVE | Staff | Own Tenant service actions | ALLOW / CONDITIONAL | own Tenant and resource policy | per resource | SRC-001 §3.3 |
| ACTIVE | Client | Own Client Account actions | CONDITIONAL | active Membership and relationship | per resource | SRC-001 §3.4 |
| SUSPENDED | Tenant Admin | Suspension reason, Billing/Subscription Recovery, Account/Profile | ALLOW | own Tenant only; Subscription is typically EXPIRED | recovery/account audit detail OPEN | BMD-005; PDM-006 |
| SUSPENDED | Tenant Admin | Other Tenant business action | DENY | always | denial detail OPEN | BMD-005 |
| SUSPENDED | Staff / Client | Any business action | DENY | always | denial detail OPEN | BMD-005 |

## State guardrails

| Status | Statement | Reference |
| --- | --- | --- |
| CONFIRMED | Tenant State never weakens the Tenant isolation boundary. | SRC-001 §5.2; PR-001 |
| CONFIRMED | Suspended Tenant Admin is deliberately narrower than active Tenant Admin. | BMD-005; PR-004–006 |
| CONFIRMED | Subscription lifecycle is distinct from Tenant access state. `CANCELLED` retains normal access through the paid period; `EXPIRED` results in `SUSPENDED` Tenant access after that period without recovery. | PDM-006; DEC-028 |
| OPEN | Notification recipients/timing and any future inactive-equivalent state definition. | OQ-006; OQ-010 |
