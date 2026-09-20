# Open Questions

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**. A status with no applicable item in a document is not implied.

## Source and governance

| ID | Status | Question | Why it matters |
| --- | --- | --- | --- |
| OQ-001 | CONFIRMED | Source precedence is SRC-001; SRC-002 is derived presentation only. | User governance instruction, 2026-09-20. |
| OQ-002 | OPEN | What historical author, original version, sponsor, and original approval metadata apply to SRC-001? | The Product Approval Authority has accepted the registered SHA-256 snapshot as the authoritative PD-V1 input; historical provenance enrichment remains optional. |
| OQ-003A | CONFIRMED | CLOSED — Product Definition Freeze Acceptance. | PD-V1 meets the approved Freeze PASS conditions recorded below and has explicit Product Owner approval. |
| OQ-003B | OPEN | What launch date, budget, success measures, and Release / UAT / Go-Live acceptance criteria apply? | This is separate from Product Definition Freeze and remains required for release planning. |

### Product Definition Freeze Acceptance

| Status | PD-V1 Freeze PASS condition | Evidence |
| --- | --- | --- |
| CONFIRMED | Phase 0A through 0D are COMPLETE. | `PROJECT-STATUS.md` |
| CONFIRMED | Product Scope, Core Business Model, Core User Flows, and Permission Model are confirmed. | Phase 0A–0D documents and accepted BMD/FMD/PDM decisions |
| CONFIRMED | Global Blocker count is zero. | OPEN Decision-Gate Register |
| CONFIRMED | Unresolved Major consistency issue count is zero. | Product Definition Audit v1, Remediation Round 2 |
| CONFIRMED | Every remaining OPEN item has an Owner, Decision Gate, Latest Resolution Point, and Closure Evidence. | OPEN Decision-Gate Register |
| CONFIRMED | Product Definition Audit has a Freeze PASS result. | Product Definition Audit v1, Remediation Round 2 |
| CONFIRMED | Product Owner / User has explicitly approved PD-V1. | User instruction, 2026-09-20; DEC-023, DEC-024, DEC-027 |

## Product and commercial definition

| ID | Status | Question | Why it matters |
| --- | --- | --- | --- |
| OQ-004 | OPEN | What plans, prices, quotas, trials, taxes, and upgrade/downgrade/refund rules are supported by the Lemon Squeezy subscription model? | The provider and lifecycle are named, but commercial rules are absent. |
| OQ-005 | OPEN | The source confirms payment-success webhook-driven Tenant/Admin creation; what exact data, verification, failure, and recovery rules apply before activation? | SRC-001 §6.1 supplies the happy path but not the business inputs or exceptions. |
| OQ-006 | OPEN | What are the notification and email requirements for invitations, tickets, domain state, and subscription state? | Tenant support email is named, but behavior is unspecified. |

## Behavior, security, and operations

| ID | Status | Question | Why it matters |
| --- | --- | --- | --- |
| OQ-007 | OPEN | What remaining detailed permission matrix applies to Tenant Admin, Staff, and Client? File Client upload/visibility boundaries are confirmed; other capability policy remains open. | Role descriptions are high-level outside the confirmed File boundary. |
| OQ-008 | OPEN | What terminal/revision transition details apply to Quote and PI? | Existing lifecycle, immutability, current-effective Revision, and PI Proforma boundary are confirmed; the required matrix is not yet accepted. |
| OQ-009 | OPEN | What exact short-lived URL TTL, file size/type limits, malware-scan implementation, storage region, and retention policy apply? | File ownership, visibility, default upload denial, and Restore boundaries are confirmed; these operational parameters require a later File/Security Architecture decision. |
| OQ-010 | OPEN | What identity, recovery, MFA, session, audit retention, privacy, compliance, backup, availability, and incident requirements apply? | Security topics are named without acceptance criteria. |
| OQ-011 | OPEN | Which tenant custom-domain patterns and ownership/validation edge cases must be supported? | The source gives CNAME-oriented examples but no support policy. |

## Core Business Model decisions and follow-ups

| ID | Status | Question | Recommendation reference |
| --- | --- | --- | --- |
| OQ-012 | CONFIRMED | CLOSED — A Global User may hold Memberships in multiple Tenants. | BMD-001 accepted, 2026-09-20 |
| OQ-013 | OPEN | Staff/Client use Membership role/type and Client Account/Profile is independent; may one User simultaneously hold Staff and Client roles? | BMD-002 confirms the model, not simultaneous-role policy. |
| OQ-014 | OPEN | Invitation states, resend support, existing-User linking, old-token invalidation on resend, and one-time acceptance are confirmed; what expiry duration and verification policy apply? | The minimum reissue/token rule is confirmed; duration and verification remain open. |
| OQ-015 | OPEN | Tenant activation and webhook idempotency/retry/manual recovery are confirmed; what required data and verification rules apply before activation? | FMD-005 confirms operational recovery, not input/verification policy. |
| OQ-016 | CONFIRMED | CLOSED — Suspended access is restricted to Tenant Admin recovery/account surfaces; Staff/Client have no business access. | BMD-005 accepted, 2026-09-20 |
| OQ-017 | OPEN | Quote Revision/history and current-revision Client action are confirmed; what numbering, Revision identification, and Quote-to-PI conversion mechanism applies? | FMD-007 confirms revision rule, not these parameters. |
| OQ-018 | OPEN | PI states, Revision/history, and Proforma-only boundary are confirmed; what numbering, Revision identification, and notification recipient/timing policy applies? | FMD-008 confirms lifecycle boundary, not these parameters. |
| OQ-019 | CONFIRMED | CLOSED — Tenant Admin/Staff may reassociate and restore same-Tenant Files; Client cannot change File association/Tenant or Restore; Client visibility is same-Tenant, own Client Account, explicitly Client-visible only. Retention parameters remain OQ-009. | DEC-031; FMD-009; PDM-005 |

## User Flow decisions

| ID | Status | Question | Why it matters |
| --- | --- | --- |
| OQ-020 | CONFIRMED | CLOSED — Tenant Admin/Staff creates Client Account; it may exist without User. Client Contact is separate, Tenant-email-unique, receives Invitation, and company name does not auto-merge accounts. | FMD-001–FMD-003 accepted, 2026-09-20 |
| OQ-021 | OPEN | Ticket ownership, Contact link, 0..1 Staff assignment, states, reopen Audit, and Client priority restriction are confirmed; what exact actor transition matrix applies? | M-04 is reclassified as a local Ticket contract decision; attachment and notification policy remain separately decision-gated. |
| OQ-022 | OPEN | V1 external notification channel is Email and failure does not roll back core transaction; what recipients, timing, content, and retry policy apply per event? | FMD-004 applies cross-flow; event-level policy remains open. |

## Permission Model decisions

| ID | Status | Question | Decision reference |
| --- | --- | --- |
| OQ-023 | CONFIRMED | CLOSED — Platform Admin defaults to no Tenant business-data read; future break-glass is explicit, time-limited, fully audited, and outside V1 default. | PDM-001 accepted, 2026-09-20 |
| OQ-024 | CONFIRMED | CLOSED — Staff daily-business, Client Account/Contact, Client Invitation, own Profile, and Staff-administration boundaries are defined. | PDM-002 accepted, 2026-09-20 |
| OQ-025 | CONFIRMED | CLOSED — Client Contact self-service is personal fields only; no deletion/other Contact management/core Client Account edit/Client Admin role. | PDM-003 accepted, 2026-09-20 |
| OQ-026 | CONFIRMED | CLOSED — Ticket role boundary and Audit visibility/redaction boundary are defined. | PDM-004 accepted, 2026-09-20 |
| OQ-027 | OPEN | File restore/reassociation and stable Document/Revision Number are confirmed; what notification recipients, timing, content, and retry policy applies? | PDM-005 accepts this as OPEN / DEFERRED and non-blocking. |
| OQ-028 | CONFIRMED | CLOSED — CANCELLED stays active through paid period; after paid period without renewal/recovery Subscription becomes EXPIRED and Tenant becomes SUSPENDED. | PDM-006 accepted, 2026-09-20 |

No question is treated as answered unless an authoritative source or explicit decision resolves it.

## OPEN decision-gate register

**CONFIRMED** — Every item currently marked `OPEN` above is governed by this register. A Blocking Level does not answer the question; it determines the latest gate at which an explicit decision or approved deferral must be evidenced. `DEFERRED / NON-BLOCKING` items remain OPEN until their stated closure evidence exists.

| ID | Owner | Decision Authority | Category | Blocking Level | Decision Gate | Latest Resolution Point | Impact | Closure Evidence |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| OQ-002 | Product Owner / User | Product Owner / User | Source provenance | DEFERRED / NON-BLOCKING | D0 — Provenance enrichment | Before any future source-record enrichment or source replacement is approved | Historical metadata may improve provenance but cannot invalidate the accepted SHA-256 snapshot or PD-V1 baseline. | Added metadata or an explicit decision that it remains unavailable, recorded in `SOURCE-REGISTER.md` and `DECISIONS.md`. |
| OQ-003B | Product Owner / User | Product Owner / User | Release governance | DEFERRED / NON-BLOCKING | R1 — Release Planning / Pre-Go-Live | Before Release / UAT / Go-Live approval | No governed launch target, budget, success measure, or release acceptance criterion. | Approved release-governance decision and synchronized status/baseline updates. |
| OQ-004 | Product Owner / User | Product Owner / User | Commercial model | LOCAL BLOCKER | L1 — Commercial model | Before subscription/billing capability definition is accepted | Plans and entitlements cannot be consistently defined. | Approved commercial decision and affected product documents updated. |
| OQ-005 | Product Owner / User | Product Owner / User | Onboarding policy | LOCAL BLOCKER | L2 — Onboarding and identity | Before Tenant activation behavior is designed or accepted | Activation input/verification behavior is undefined. | Approved activation-policy decision and flow/lifecycle updates. |
| OQ-006 | Product Owner / User | Product Owner / User | Notification policy | DEFERRED / NON-BLOCKING | D1 — Notification policy | Before notification behavior is implemented or release communications are approved | Events lack a shared delivery policy but core transactions remain independent. | Approved notification policy or explicit approved deferral recorded in `DECISIONS.md`. |
| OQ-007 | Product Owner / User | Product Owner / User | Authorization policy | LOCAL BLOCKER | L3 — Authorization and security | Before affected Client/File/Ticket access behavior is designed or accepted | Detailed access and upload visibility can be implemented inconsistently. | Approved permission-policy decision and synchronized flow/permission updates. |
| OQ-008 | Product Owner / User | Product Owner / User | Document transitions | LOCAL BLOCKER | L4 — Quote/PI Architecture / API Contract | Before Quote/PI Architecture / API Contract | Terminal/revision behavior could diverge despite confirmed lifecycle and immutability boundaries. | Accepted Quote/PI Transition & Revision Matrix, with lifecycle/flow updates. |
| OQ-009 | Product Owner / User | Product Owner / User | File / security operational policy | DEFERRED / NON-BLOCKING | F1 — File/Security Architecture Gate | Before File/Security Architecture behavior is approved | URL TTL, limits, scanning implementation, storage, and retention parameters remain undefined; confirmed File access boundary remains safe by default. | Approved File/Security policy decision and affected flow/permission updates. |
| OQ-010 | Product Owner / User | Product Owner / User | Security and identity | LOCAL BLOCKER | L3 — Authorization and security | Before identity/security behavior is designed or accepted | Recovery, MFA, privacy, retention, resilience, and incident expectations are undefined. | Approved security/identity policy and synchronized governance updates. |
| OQ-011 | Product Owner / User | Product Owner / User | Domain lifecycle | LOCAL BLOCKER | L5 — Domain and ticket operations | Before custom-domain behavior is designed or accepted | Domain support and validation edge cases lack a support policy. | Approved domain-policy decision and flow/lifecycle updates. |
| OQ-013 | Product Owner / User | Product Owner / User | Role model | LOCAL BLOCKER | L2 — Onboarding and identity | Before multi-role Membership behavior is designed or accepted | Simultaneous Staff/Client role behavior is undefined. | Approved role-model decision and domain/permission updates. |
| OQ-014 | Product Owner / User | Product Owner / User | Invitation policy | LOCAL BLOCKER | L2 — Onboarding and identity | Before invitation behavior is designed or accepted | Expiry, verification, and uninvited activation policy are undefined. | Approved invitation-policy decision and flow/lifecycle updates. |
| OQ-015 | Product Owner / User | Product Owner / User | Onboarding policy | LOCAL BLOCKER | L2 — Onboarding and identity | Before Tenant activation behavior is designed or accepted | Required activation data and verification rules are undefined. | Approved activation-policy decision and flow/lifecycle updates. |
| OQ-017 | Product Owner / User | Product Owner / User | Document policy | LOCAL BLOCKER | L4 — Document and file operations | Before Quote workflow behavior is designed or accepted | Quote identifiers and Quote-to-PI conversion behavior remain undefined. | Approved Quote-policy decision and lifecycle/flow updates. |
| OQ-018 | Product Owner / User | Product Owner / User | Document policy | LOCAL BLOCKER | L4 — Document and file operations | Before PI workflow behavior is designed or accepted | PI identifiers and notification policy remain undefined. | Approved PI-policy decision and lifecycle/flow updates. |
| OQ-021 | Product Owner / User | Product Owner / User | Ticket transitions | LOCAL BLOCKER | L5 — Ticket Architecture / API Contract | Before Ticket Architecture / API Contract | Exact actor transitions are required to make the confirmed Ticket states enforceable. | Accepted Ticket Transition Matrix, with flow/permission updates. |
| OQ-022 | Product Owner / User | Product Owner / User | Notification policy | DEFERRED / NON-BLOCKING | D1 — Notification policy | Before notification behavior is implemented or release communications are approved | Event-specific recipients, timing, content, and retry policy remain undefined. | Approved notification policy or explicit approved deferral recorded in `DECISIONS.md`. |
| OQ-027 | Product Owner / User | Product Owner / User | Notification policy | DEFERRED / NON-BLOCKING | D1 — Notification policy | Before notification behavior is implemented or release communications are approved | File/document notification policy remains intentionally deferred. | Approved notification policy or explicit approved deferral recorded in `DECISIONS.md`. |
