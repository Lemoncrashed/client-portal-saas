# Business Model Decisions

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**. A status with no applicable item in a document is not implied.

All eight decisions below were accepted by the business decision freeze on 2026-09-20. Where SRC-001 did not prescribe a rule, the confirmed rule is traceable to that decision rather than represented as an original-source fact.

## BMD-001 — User, Tenant, and Membership

**Status: CONFIRMED / ACCEPTED**  
**Source support:** SRC-001 §3, §5.2, §5.7–§5.8 defines roles, accounts, invitations, and isolation; user-to-Tenant cardinality was not specified.  
**Accepted rule:** Use a Global User plus Tenant Membership model. A User may belong to multiple Tenants. Membership is the Tenant-scoped holder of role/type and relationship status.  
**Decision basis:** Business decision freeze, 2026-09-20.  
**Rejected alternative:** Tenant-local account as the primary identity model.

## BMD-002 — Staff and Client classification

**Status: CONFIRMED / ACCEPTED**  
**Source support:** SRC-001 §3.3–§3.4 differentiates Staff and Client responsibilities; it does not define the model.  
**Accepted rule:** Express Staff and Client through Membership role/type. Retain Client Account / Profile independently as the Client's business identity.  
**Decision basis:** Business decision freeze, 2026-09-20.  
**Rejected alternative:** Separate, parallel Staff and Client account types as the primary identity model.

## BMD-003 — Invitation and activation

**Status: CONFIRMED / ACCEPTED**  
**Source support:** SRC-001 §5.7–§5.8 allows create/invite/disable Staff and Client but does not define invitation states.  
**Accepted rule:** Staff and Client invitations use an independent Invitation lifecycle. This state enumeration is superseded by FMD-004: `Pending / Accepted / Expired / Revoked`, with resend support. An existing User is linked to the invitation outcome and is not duplicated.  
**Decision basis:** Business decision freeze, 2026-09-20.  
**Rejected alternative:** Immediate active membership/account creation as the default invitation model.

## BMD-004 — Tenant signup and subscription activation

**Status: CONFIRMED / ACCEPTED**  
**Source support:** SRC-001 §6.1 confirms the checkout, payment, webhook, Tenant/Admin creation, subscription activation, and availability flow.  
**Accepted rule:** The lifecycle is `Pending Tenant → Checkout → Payment/Webhook → Activate Tenant / Subscription`.  
**Decision basis:** Business decision freeze, 2026-09-20.  
**Rejected alternative:** Treating payment success alone as immediate Tenant availability.

## BMD-005 — Suspended Tenant behavior

**Status: CONFIRMED / ACCEPTED**  
**Source support:** SRC-001 §6.3–§6.4 confirms suspension after expiry, retained data, and recovery, but not suspended-mode access.  
**Accepted rule:** A suspended Tenant Admin may log in only to view the suspension reason, Billing/Subscription Recovery, and Account/Profile. Staff and Client have no business access while the Tenant is suspended.  
**Decision basis:** Business decision freeze, 2026-09-20.  
**Rejected alternatives:** Full portal block for Tenant Admin; read-only business access for all roles.

## BMD-006 — Quote lifecycle

**Status: CONFIRMED / ACCEPTED**  
**Source support:** SRC-001 §5.13–§5.15 confirms Quote creation, expiry date, fields, and PDF but not document outcomes.  
**Accepted rule:** V1 Quote lifecycle is `Draft → Sent → Accepted / Rejected / Expired / Cancelled`. Client may View, Download PDF, Accept, or Reject. E-signature is not included.  
**Decision basis:** Business decision freeze, 2026-09-20; no e-signature also aligns with SRC-001 §15.  
**Rejected alternative:** Minimal lifecycle without acceptance/rejection tracking.

## BMD-007 — PI lifecycle

**Status: CONFIRMED / ACCEPTED**  
**Source support:** SRC-001 §5.14–§5.15 confirms PI creation/PDF; §15 excludes online payment, recurring invoice, and accounting.  
**Accepted rule:** PI remains a Proforma lifecycle and does not become formal Invoice or Accounting scope. A PI may reference a source Quote; Quote-to-PI conversion is not mandatory or automatic.  
**Decision basis:** Business decision freeze, 2026-09-20.  
**Rejected alternative:** Extending PI with formal invoice, accounting, or automatic conversion behavior.

## BMD-008 — File ownership and Client association

**Status: CONFIRMED / ACCEPTED**  
**Source support:** SRC-001 §5.9–§5.10 confirms Tenant-side management, Client association, Client upload when allowed, and secure scoped access; ownership/cardinality were not specified.  
**Accepted rule:** Every File belongs to exactly one Tenant as its security boundary. A File may optionally associate with a Client Account. Ticket, Quote, PI, and other business records reference Files through relationship models; File does not absorb all business foreign keys.  
**Decision basis:** Business decision freeze, 2026-09-20.  
**Rejected alternative:** Uploader-account ownership as the File's primary security boundary.

## Decision summary

| Status | Count | Meaning |
| --- | ---: | --- |
| CONFIRMED | 8 | All eight Phase 0B business-model decisions are accepted. |
| PROPOSED | 0 | No Phase 0B recommendation remains pending approval. |
| OPEN | 0 | The eight BMD approval questions are closed; separate operational-detail questions may remain elsewhere. |
| REJECTED | 8 | One or more alternative models were explicitly not selected for each BMD. |
