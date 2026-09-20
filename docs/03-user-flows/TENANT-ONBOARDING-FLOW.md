# Tenant Onboarding Flow

## TF-01 — Tenant signup, checkout, and activation

**Status: CONFIRMED baseline with OPEN operational dependencies**  
**Source support:** SRC-001 §6.1–§6.2; BMD-004; FMD-005.

- **Actor:** Prospective Tenant Customer; platform onboarding process.
- **Preconditions:** A Pending Tenant exists; checkout can be initiated.
- **Trigger:** Tenant Customer begins Lemon Squeezy Checkout.
- **Happy Path:** Pending Tenant → Checkout → Payment/Webhook → activate Subscription/Tenant → Tenant available.
- **Alternative Paths:** An existing Global User can be linked as the initial Tenant Admin rather than duplicated, consistent with BMD-001/BMD-003.
- **Failure Paths:** Payment failure or incomplete activation remains non-active. Missing/late/duplicate webhook is handled idempotently, may retry, and may be manually recovered; it must not duplicate Tenant, User, or Subscription.
- **Permission Boundary:** Only the onboarding context and platform administration may activate a Tenant. A Tenant is not available to normal Tenant roles before activation.
- **State Changes:** Tenant access: Pending → Active. Subscription lifecycle: activated to Active after payment/webhook reconciliation. Retried/recovered processing does not duplicate business objects.
- **Audit Events:** Subscription-created/updated, payment, Tenant creation, initial-admin creation, and activation require an auditable business event; source names webhook and subscription logging but exact event vocabulary is **OPEN**.
- **Notification/Email dependency:** V1 external notification channel is Email; failure does not roll back onboarding transaction. Recipient/timing policy is **OPEN**.
- **Final State:** Active Tenant with active Subscription and initial Tenant Admin, or non-active pending/failed case awaiting defined recovery.
- **OPEN issues:** OQ-005 (required data/verification), OQ-006 (notification recipient/timing), OQ-010 (identity/session controls).

## TF-02 — Suspended Tenant recovery

**Status: CONFIRMED boundary with OPEN recovery completion dependency**  
**Source support:** SRC-001 §6.3–§6.4; BMD-005.

- **Actor:** Suspended Tenant Admin; subscription recovery context.
- **Preconditions:** Tenant Subscription lifecycle is Expired; Tenant access state is Suspended; Tenant data is retained.
- **Trigger:** Tenant Admin logs in or begins Billing/Subscription Recovery.
- **Happy Path:** Tenant Admin views suspension reason and uses Billing/Subscription Recovery; recovered subscription restores normal Tenant availability.
- **Alternative Paths:** Tenant Admin may view only Account/Profile without starting recovery.
- **Failure Paths:** Recovery/payment cannot complete or a subscription event is not reconciled; Tenant remains Suspended. Exception handling is **OPEN**.
- **Permission Boundary:** Tenant Admin only: suspension reason, Billing/Subscription Recovery, Account/Profile. Staff and Client have no business access.
- **State Changes:** Subscription lifecycle: Expired → Active after successful recovery. Tenant access state: Suspended → Active only after entitlement recovery; the two axes are not one combined lifecycle.
- **Audit Events:** Subscription Suspended is explicitly named by SRC-001 §13; recovery/resumption audit detail is **OPEN**.
- **Notification/Email dependency:** Suspension, recovery, and resumption notifications are **OPEN**.
- **Final State:** Tenant remains Suspended, or is restored to active normal access after subscription recovery.
- **OPEN issues:** Recovery completion rule, reconciliation timing, and recipient/channel for suspension and recovery notices.
