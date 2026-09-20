# Core Business Model

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**. A status with no applicable item in a document is not implied.

## Boundary

| Status | Statement | Source support |
| --- | --- | --- |
| CONFIRMED | This document defines business objects, their relationships, and their lifecycle meaning only. It is not an ERD, architecture, API contract, task plan, or implementation specification. | Phase 0B instruction, 2026-09-20 |
| CONFIRMED | The platform serves multiple tenants. Tenant data, users, files, branding, domains, Quote, and PI are isolated. | SRC-001 §2, §5.2 |
| CONFIRMED | The named roles are Platform Admin, Tenant Admin, Staff, and Client. | SRC-001 §3 |
| CONFIRMED | Staff and Clients can be invited; both use accounts to log in. | SRC-001 §5.7–§5.8 |
| CONFIRMED | A paid Lemon Squeezy event leads through webhook processing to Tenant creation, Admin creation, subscription activation, and Tenant availability. | SRC-001 §6.1 |
| CONFIRMED | On actual paid-entitlement expiry, Subscription transitions to `EXPIRED` while Tenant access transitions from `ACTIVE` to `SUSPENDED`; these are separate state axes. Data is retained and access returns to `ACTIVE` after subscription recovery. | PDM-006 accepted; SRC-001 §6.3–§6.4 |

## Confirmed business nouns

| Status | Object | Business meaning | Source support |
| --- | --- | --- | --- |
| CONFIRMED | Tenant | Enterprise customer receiving its own portal, brand, domain, users, and isolated data. | SRC-001 §2, §5.2 |
| CONFIRMED | User / Account | Global login identity used by Staff and Client; a User may belong to multiple Tenants through Membership. | SRC-001 §3.3–§3.4, §5.7–§5.8; BMD-001 |
| CONFIRMED | Role | Platform Admin, Tenant Admin, Staff, or Client. | SRC-001 §3 |
| CONFIRMED | Subscription | Lemon Squeezy-backed service status that controls Tenant availability. | SRC-001 §5.1, §6 |
| CONFIRMED | Invitation | Independent Staff/Client invitation lifecycle; existing Users are linked rather than duplicated. | SRC-001 §5.7–§5.8; BMD-003 |
| CONFIRMED | Client | A Tenant's downstream customer, represented through Membership role/type and an independent Client Account / Profile business identity. | SRC-001 §3.4, §5.8; BMD-002 |
| CONFIRMED | Quote / PI | Tenant business documents that Staff or Tenant Admin can create and export as branded PDFs. | SRC-001 §5.13–§5.15 |
| CONFIRMED | File | Permissioned resource managed by Tenant Admin/Staff, associable to Client, and visible to authorized Client users. | SRC-001 §5.9–§5.10 |
| CONFIRMED | Ticket | Tenant- and Client Account-scoped service request that may link a Client Contact and have zero or one Staff assignee. | SRC-001 §5.11–§5.12; FMD-006 |
| CONFIRMED | Domain | Tenant custom-domain lifecycle resource, with a default-subdomain fallback and platform disable authority. | SRC-001 §5.5–§5.6; FMD-010 |

## Confirmed relationship model

| Status | Relationship | Recommendation | Reasoning / impact | Source support |
| --- | --- | --- | --- | --- |
| CONFIRMED | User ↔ Tenant | A Global User receives one or more Tenant-scoped Memberships that carry role/type and relationship status. | Separates global login identity from Tenant affiliation and permits multi-Tenant Users. | SRC-001 §3, §5.7–§5.8; BMD-001 |
| CONFIRMED | Membership ↔ Staff / Client | Staff and Client are Membership role/types; Client Account / Profile remains an independent business identity. | Avoids parallel account models while preserving Client business data. | SRC-001 §3.3–§3.4, §5.8; BMD-002 |
| CONFIRMED | Tenant ↔ Subscription | Pending Tenant proceeds through Checkout and Payment/Webhook to activation of Tenant and Subscription. | Payment is part of, not synonymous with, service activation. | SRC-001 §6.1; BMD-004 |
| CONFIRMED | Tenant ↔ File ↔ Client | Every File belongs to exactly one Tenant; Client association is optional and separate from ownership. Other business records reference File through relationships. | Preserves the Tenant security boundary without overloading File with every business reference. | SRC-001 §5.9–§5.10; BMD-008 |
| CONFIRMED | Tenant ↔ Quote / PI ↔ Client | Quote and PI are Tenant-scoped and identify the intended Client/customer; a PI may reference a source Quote without automatic conversion. | Supports document lifecycle and optional origin traceability without creating invoice/accounting scope. | SRC-001 §5.13–§5.15; BMD-006, BMD-007 |

## Business invariants

| Status | Invariant | Source support |
| --- | --- | --- |
| CONFIRMED | A Tenant user must not gain access to another Tenant's data by URL, request parameter, or other means. | SRC-001 §5.2 |
| CONFIRMED | A Client must not view another Client's or internal Tenant data. | SRC-001 §3.4, §5.10 |
| CONFIRMED | Files are not directly public; authorized access precedes a temporary secure download URL. | SRC-001 §5.10 |
| CONFIRMED | Client upload is default DENY. When Tenant Admin/Staff explicitly permits it for a Client Account, an uploaded File remains Tenant-bound, is associated to that Client Account, and is private by default. | User governance instruction, 2026-09-20; DEC-031 |
| CONFIRMED | Soft Delete and Restore preserve Ticket, Quote, PI, and other historical File references. | FMD-009; DEC-031 |
| CONFIRMED | Suspended Tenant data is not immediately deleted. | SRC-001 §6.3 |
| CONFIRMED | During suspension, only Tenant Admin may log in, and only for suspension reason, Billing/Subscription Recovery, and Account/Profile; Staff and Client have no business access. | BMD-005 |
| CONFIRMED | Quote lifecycle is Draft → Sent → Accepted / Rejected / Expired / Cancelled; Client may View, Download PDF, Accept, or Reject; e-signature is excluded. | SRC-001 §5.13–§5.15, §15; BMD-006 |
| CONFIRMED | PI remains Proforma only and does not extend into formal Invoice or Accounting scope. | SRC-001 §15; BMD-007 |

## Decision freeze

**CONFIRMED** — BMD-001 through BMD-008 are accepted. Their rules are business decisions layered on top of the Primary Product Source where that source was silent.
