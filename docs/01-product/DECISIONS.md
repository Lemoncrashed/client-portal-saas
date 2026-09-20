# Decisions

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**. A status with no applicable item in a document is not implied.

## Confirmed baseline decisions

| ID | Status | Decision | Basis |
| --- | --- | --- | --- |
| DEC-001 | CONFIRMED | Limit current work to Phase 0 — Project Baseline. | User instruction, 2026-09-20 |
| DEC-002 | CONFIRMED | Use CONFIRMED, PROPOSED, OPEN, and REJECTED across the knowledge base. | User instruction, 2026-09-19 |
| DEC-003 | CONFIRMED | Preserve originals unchanged and trace derived facts to registered sources. | User instruction, 2026-09-19; source ingestion |
| DEC-004 | CONFIRMED | Treat explicit source requirements as baseline facts; do not infer absent details. | User instruction, 2026-09-19 |
| DEC-005 | CONFIRMED | Treat the recommendation sections of the intake as `PROPOSED`, not as chosen architecture. | Source wording: “recommended technical solution”; Phase 0 boundary |
| DEC-006 | REJECTED | Enter Core Business Model, Architecture, Task, or Coding during this update. | User instruction, 2026-09-20 |
| DEC-007 | REJECTED | Include the §15 capability list in V1 scope. | SRC-001 §15; SRC-002 §15 |
| DEC-008 | CONFIRMED | Designate SRC-001 as the Primary Product Source. | User governance instruction, 2026-09-20 |
| DEC-009 | CONFIRMED | Classify SRC-002 as a Cursor-generated Derived Presentation, not an independent requirements source or UX prototype. | User governance instruction, 2026-09-20 |
| DEC-010 | CONFIRMED | Resolve any source difference in favor of SRC-001 and log it in the Source Register. | User governance instruction, 2026-09-20 |
| DEC-011 | CONFIRMED | Maintain `dashboard/index.html` whenever Phase, Decision, Question, or future Task status changes. | User governance instruction, 2026-09-20 |
| DEC-012 | REJECTED | Begin Phase 0B analysis as part of dashboard/governance work. | User governance instruction, 2026-09-20 |
| DEC-016 | CONFIRMED | Accept and freeze BMD-001 through BMD-008 as the Phase 0B Core Business Model rules. | User business confirmation, 2026-09-20; `docs/02-domain/BUSINESS-MODEL-DECISIONS.md` |
| DEC-017 | REJECTED | Enter Phase 0C User Flows analysis during the Phase 0B decision-freeze update. | User instruction, 2026-09-20 |
| DEC-018 | CONFIRMED | Accept and freeze FMD-001 through FMD-010 as the Phase 0C User Flow rules. | User business confirmation, 2026-09-20; `docs/03-user-flows/FLOW-DECISIONS.md` |
| DEC-019 | REJECTED | Enter Phase 0D Permission Model analysis during the Phase 0C flow-decision closure update. | User instruction, 2026-09-20 |
| DEC-020 | CONFIRMED | Begin Phase 0D Permission Model analysis only; do not enter Architecture, ERD, API, Task, or Coding. | User instruction, 2026-09-20 |
| DEC-021 | CONFIRMED | Accept and freeze PDM-001 through PDM-006 as the Phase 0D Permission Model rules. | User business confirmation, 2026-09-20; `docs/04-permissions/PERMISSION-DECISIONS.md` |
| DEC-022 | REJECTED | Enter Phase 1 Architecture during the Phase 0D permission-decision closure update. | User instruction, 2026-09-20 |
| DEC-023 | CONFIRMED | Designate Project Owner / User as Product Owner and Product Approval Authority for the Client Portal product definition. | User governance instruction, 2026-09-20 |
| DEC-024 | CONFIRMED | Establish and freeze Product Definition v1 (`PD-V1`) dated 2026-09-20, using SRC-001 at its registered SHA-256 as the primary source input. | User governance instruction, 2026-09-20; `SOURCE-REGISTER.md` |
| DEC-025 | CONFIRMED | Require every post-freeze product-definition change to follow Change Request → Impact Review → Decision → Approved Update, with source/decision traceability and Dashboard synchronization. | User governance instruction, 2026-09-20 |
| DEC-026 | CONFIRMED | Govern every remaining OPEN question through a named Owner, Decision Authority, Category, Blocking Level, Decision Gate, Latest Resolution Point, Impact, and Closure Evidence. OPEN questions remain unresolved until their evidence is recorded. | User governance instruction, 2026-09-20; `OPEN-QUESTIONS.md` |
| DEC-027 | CONFIRMED | Accept PD-V1 Freeze PASS: Phase 0A–0D complete; product scope, business model, user flows, and permission model confirmed; Global Blocker count zero; no unresolved Major consistency issue; every remaining OPEN item decision-gated; audit Freeze PASS; and explicit Product Owner approval. | User governance instruction, 2026-09-20; `OPEN-QUESTIONS.md`; Audit Remediation Round 2 |
| DEC-028 | CONFIRMED | Treat Subscription lifecycle and Tenant access as separate axes. `EXPIRED` is a Subscription state; Tenant access is only `PENDING`, `ACTIVE`, or `SUSPENDED`. | PDM-006 accepted; User governance instruction, 2026-09-20 |
| DEC-029 | CONFIRMED | Deny Staff-to-Staff invitations. Allow Staff-to-Client invitations only conditionally within valid same-Tenant Client Contact and Tenant-unique-email scope. | PDM-002 accepted; User governance instruction, 2026-09-20 |
| DEC-030 | CONFIRMED | Platform Admin defaults to confirmed DENY for Tenant Client, File, Ticket, Quote, and PI business-data read. A future break-glass Support Access capability is explicit, time-limited, fully audited, and outside V1 default. | PDM-001; PR-009; User governance instruction, 2026-09-20 |
| DEC-031 | CONFIRMED | Freeze V1 File policy: Client upload is default DENY; Tenant Admin/Staff may explicitly enable it for a Client Account. Client Files are private, Tenant-bound, associated to the current Client Account, and Client-visible only under explicit same-Tenant/own-Client authorization. Client cannot alter ownership/association or Restore; Tenant Admin/Staff may manage, reassociate, and Restore only within Tenant; Soft Delete/Restore preserves historical references. | User governance instruction, 2026-09-20; FMD-009; PDM-005 |
| DEC-032 | CONFIRMED | Reclassify M-04 as a LOCAL BLOCKER. Before Ticket Architecture / API Contract, closure evidence must be an accepted Ticket Transition Matrix. This does not alter currently confirmed Ticket states, scope, assignment, reopen audit, or Client priority boundary. | User governance instruction, 2026-09-20; FMD-006; PDM-004 |
| DEC-033 | CONFIRMED | Reclassify M-05 as a LOCAL BLOCKER. Before Quote/PI Architecture / API Contract, closure evidence must be an accepted Quote/PI Transition & Revision Matrix. Existing Revision immutability, current-effective Revision, and PI Proforma boundary remain confirmed. | User governance instruction, 2026-09-20; FMD-007–008; PDM-005 |
| DEC-034 | CONFIRMED | Complete the Core Business Model noun inventory by including Ticket and Domain as confirmed conceptual objects. | User governance instruction, 2026-09-20; SRC-001 §5.5–§5.6, §5.11–§5.12; FMD-006, FMD-010 |
| DEC-035 | CONFIRMED | Invitation resend invalidates the old token and issues a replacement; accepted invitations cannot be accepted again; expired and revoked tokens are invalid. Expiry duration and verification policy remain OPEN under the Identity Architecture Gate. | User governance instruction, 2026-09-20; FMD-004 |
| DEC-036 | CONFIRMED | Every future `CONFIRMED` assertion must cite at least one SRC, DEC, BMD, FMD, PDM, or CR reference. This is a lightweight traceability convention, not a traceability database. | User governance instruction, 2026-09-20 |
| DEC-037 | CONFIRMED / ACCEPTED | Product Owner / Product Approval Authority formally approves Product Definition v1 (`PD-V1`) as the Client Portal product-definition baseline. PD-V1 is the authoritative input for any later authorized Architecture work. Phase 0 modifications remain subject to Change Request → Impact Review → Decision → Approved Update; this approval does not itself authorize Phase 1. | Product Owner formal approval, 2026-09-20; `PD-V1-FREEZE-RECORD.md` |
| DEC-038 | CONFIRMED | Authorize Phase 1A — Architecture Principles & System Boundaries — using the read-only `PD-V1` baseline at Git tag `product-definition-v1` / commit `59738725dec33272e4a9fd8fa0f14ddb0cf1c5f1`. Phase 1A may analyze principles, context, runtime responsibility, risks, and trade-offs only; it must not alter Phase 0, enter ERD, API Contract, Task, or Coding. | User instruction, 2026-09-20; DEC-025; DEC-037 |
| DEC-040 | CONFIRMED / ACCEPTED | Project Owner / Architecture Approval Authority formally approves the Phase 1A Architecture Principles & System Boundaries Freeze (`ARCH-1A-V1`). AD-003 through AD-007 are ACCEPTED Architecture Baseline decisions, authoritative for subsequent Architecture work but not Product-level `CONFIRMED` assertions. Any later Architecture change requires a new Architecture Decision or explicit superseding decision; if it affects PD-V1, Product Change Request governance additionally applies. This approval does not begin Phase 1B. | Architecture Approval Authority formal approval, 2026-09-20; `PHASE-1A-FREEZE-RECORD.md`; DEC-025; DEC-037 |

## Pending decisions

| ID | Status | Decision needed |
| --- | --- | --- |
| DEC-013 | OPEN | Optionally enrich SRC-001 historical document metadata (version, author, sponsor, and original approval status). The SHA-256-identified snapshot is already accepted as PD-V1 authority input, so this is non-blocking provenance enrichment. |
| DEC-014 | OPEN | Approve or replace the source's proposed technical and deployment approach in a later authorized phase. |
| DEC-015 | OPEN | Decide commercial subscription rules and measurable V1 acceptance/release criteria. |
| DEC-039 | CONFIRMED | CLOSED — Architecture Review, remediation, re-review, and Audit concluded PASS; the Architecture Approval Authority accepted AD-003 through AD-007 in the ARCH-1A-V1 Freeze. A separately named and authorized Phase 1B scope remains required. | DEC-040; `PHASE-1A-FREEZE-RECORD.md`; `PHASE-1A-ARCHITECTURE-AUDIT.md` |
