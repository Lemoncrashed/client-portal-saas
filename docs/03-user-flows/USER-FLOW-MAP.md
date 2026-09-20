# User Flow Map

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**.

## Phase 0C boundary

| Status | Statement |
| --- | --- |
| CONFIRMED | This map organizes end-to-end business flows from Product Baseline and accepted Phase 0B decisions. |
| REJECTED | Treat these flows as Architecture, ERD, API, Task, or Coding specifications. |
| CONFIRMED | Phase 0C is complete. Remaining flow-level OPEN dependencies are governed by the decision-gate mechanism in `docs/01-product/OPEN-QUESTIONS.md`; only their assigned blocking level and Decision Gate determine whether they block a later gate. |

## Flow inventory

| ID | Flow | Status | Primary document | OPEN dependency |
| --- | --- | --- | --- | --- |
| TF-01 | Tenant signup, checkout, and activation | CONFIRMED path | TENANT-ONBOARDING-FLOW.md | OQ-005, OQ-006 |
| TF-02 | Suspended Tenant recovery | CONFIRMED boundary | TENANT-ONBOARDING-FLOW.md | recovery completion/notification |
| IF-01 | Staff invitation and activation | CONFIRMED lifecycle | IDENTITY-INVITATION-FLOWS.md | OQ-006, OQ-010 |
| IF-02 | Client invitation and Client Account onboarding | CONFIRMED lifecycle | IDENTITY-INVITATION-FLOWS.md, CLIENT-FLOWS.md | OQ-006, OQ-007 |
| CF-01 | Client portal access | CONFIRMED boundary | CLIENT-FLOWS.md | OQ-007 |
| FF-01 | Staff file management and Client association | CONFIRMED boundary | FILE-FLOWS.md | OQ-009, OQ-019 |
| FF-02 | Client file view/download/upload | CONFIRMED boundary | FILE-FLOWS.md | OQ-007, OQ-009 |
| TK-01 | Client ticket creation/reply | CONFIRMED core action | TICKET-FLOWS.md | OQ-006, OQ-007 |
| TK-02 | Staff ticket handling | CONFIRMED core action | TICKET-FLOWS.md | assignment/notification policy |
| QF-01 | Quote issue and Client response | CONFIRMED lifecycle | QUOTE-PI-FLOWS.md | OQ-008 |
| PF-01 | PI issue and optional Quote reference | CONFIRMED boundary | QUOTE-PI-FLOWS.md | OQ-008 |
| DF-01 / SF-01 | Custom Domain; subscription cancel/expire/resume | CONFIRMED core actions | DOMAIN-SUBSCRIPTION-FLOWS.md | OQ-004, OQ-006, OQ-011 |

## Blocking flow decisions

| Status | Decision |
| --- | --- |
| CONFIRMED | FMD-001 through FMD-010 close the former blocking flow decisions for Client Account/Contact, invitations, signup recovery, Ticket, Quote, PI, File, and Domain. |
| CONFIRMED | Remaining details retain their individual Decision Gate and Blocking Level in `OPEN-QUESTIONS.md`; they are not collectively a condition for Phase 0C completion. |
