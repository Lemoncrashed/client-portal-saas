# Domain and Subscription Flows

## DF-01 — Custom Domain setup, verification, and SSL activation

**Status: CONFIRMED domain lifecycle with OPEN notification dependency**  
**Source support:** SRC-001 §5.5–§5.6; FMD-010.

- **Actor:** Tenant Admin; platform domain-verification process.
- **Preconditions:** Tenant is active; Tenant Admin has custom-domain authority; requested domain is supplied.
- **Trigger:** Tenant Admin enters a custom domain.
- **Happy Path:** Pending → Waiting DNS → Verifying → SSL Pending → Active after Tenant Admin configures supplied DNS guidance and verification succeeds.
- **Alternative Paths:** Tenant retains/falls back to default subdomain; Tenant Admin removes custom domain; Platform Admin disables Domain; Failed verification may retry.
- **Failure Paths:** DNS is not configured/validated or SSL cannot activate; Domain becomes Failed and may retry. A domain cannot be assigned to multiple Tenants.
- **Permission Boundary:** Tenant Admin manages/removes only its Tenant domain; Platform Admin may disable Domain; a domain is owned by one Tenant only.
- **State Changes:** Pending / Waiting DNS / Verifying / SSL Pending / Active / Failed / Disabled.
- **Audit Events:** Add Domain and Verify Domain are explicitly named in SRC-001 §13; failure/retry events are **OPEN**.
- **Notification/Email dependency:** V1 external notification channel is Email; failure does not roll back Domain status transaction. Recipient/timing policy remains **OPEN**.
- **Final State:** Active custom domain with SSL, or Failed/Disabled/default-subdomain fallback.
- **OPEN issues:** OQ-011; ownership-validation method, retry limits, and notification recipient/timing.

## SF-01 — Subscription cancel, expire, suspend, and resume

**Status: CONFIRMED lifecycle with OPEN commercial/notification policy**  
**Source support:** SRC-001 §6.2–§6.4; BMD-005; FMD-005.

- **Actor:** Tenant customer/administrator; Lemon Squeezy subscription context; platform subscription process.
- **Preconditions:** Tenant access state is Active and Tenant has a Subscription lifecycle status that permits service.
- **Trigger:** Subscription is cancelled, expires, resumes, or changes through supported lifecycle events.
- **Happy Path:** Subscription becomes Cancelled while Tenant access remains Active through paid-through date → Subscription becomes Expired and Tenant access becomes Suspended; recovery/resume returns Subscription and Tenant access to Active normal service.
- **Alternative Paths:** Created, Updated, Payment Successful, Payment Failed, Cancelled, Resumed, and Expired events occur; only actual expiry triggers suspension.
- **Failure Paths:** Event may be missing, late, or duplicated; handling is idempotent, retryable, manually recoverable, and must not duplicate Tenant/User/Subscription.
- **Permission Boundary:** Suspended-mode rule applies: Tenant Admin recovery/account-only access; Staff/Client have no business access.
- **State Changes:** **Subscription axis:** Active → Cancelled (paid period retained) → Expired; recovered/renewed → Active. **Tenant access axis:** Active → Suspended only when the Subscription is Expired after the paid period; recovered/renewed entitlement → Active. Cancellation before paid-through date does not immediately suspend.
- **Audit Events:** Subscription Suspended is source-supported; cancellation, expiry, resume, and reconciliation events are **OPEN**.
- **Notification/Email dependency:** V1 external notification channel is Email; failure does not roll back subscription state transaction. Recipient/timing policy remains **OPEN**.
- **Final State:** Active normal service after recovery, or Suspended restricted service after expiry.
- **OPEN issues:** OQ-004, OQ-005, OQ-006; commercial rules, required data/verification, and notification recipient/timing.
