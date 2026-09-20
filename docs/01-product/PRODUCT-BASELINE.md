# Product Baseline

Status convention: **CONFIRMED** / **PROPOSED** / **OPEN** / **REJECTED**. A status with no applicable item in a document is not implied.

## Product and audience

| Status | Fact | Source |
| --- | --- | --- |
| CONFIRMED | Client Portal is a multi-tenant, fully white-label, self-hosted SaaS platform. | SRC-001 §1; SRC-002 §01 |
| CONFIRMED | It serves B2B service businesses and gives each tenant an independent client portal. | SRC-001 §1–§2; SRC-002 §01–§02 |
| CONFIRMED | Tenants must be isolated: their data, users, files, domains, and branding must not be visible across tenants. | SRC-001 §2, §5.2; SRC-002 §02, §05.2 |
| CONFIRMED | Downstream clients must not see the platform developer's brand. | SRC-001 §2, §5.4; SRC-002 §02, §05.4 |

## Roles and permitted product outcomes

| Status | Fact | Source |
| --- | --- | --- |
| CONFIRMED | Platform Admin operates tenants, subscriptions, domains, webhooks, logs, and system configuration. | SRC-001 §3.1, §5.1; SRC-002 §03, §05.1 |
| CONFIRMED | Tenant Admin manages company profile, branding, domain, staff, clients, files, tickets, Quote, PI, and billing status. | SRC-001 §3.2; SRC-002 §03 |
| CONFIRMED | Staff serve clients by viewing clients, managing files, handling tickets, and creating Quote/PI; V1 uses a standard staff role without a custom permission editor. | SRC-001 §3.3; SRC-002 §03 |
| CONFIRMED | A Client may access only its own files, tickets, Quote, PI, and profile; it cannot access other clients' or internal data. | SRC-001 §3.4; SRC-002 §03 |

## Confirmed V1 capabilities

| Status | Capability | Source |
| --- | --- | --- |
| CONFIRMED | Tenant branding: company profile; logo and favicon; primary/accent/background theme; applied to portal and document surfaces. | SRC-001 §5.3–§5.4; SRC-002 §05.3–§05.4 |
| CONFIRMED | Default tenant subdomain plus custom-domain setup, DNS guidance/verification, and managed SSL states. | SRC-001 §5.5–§5.6; SRC-002 §05.5–§05.6 |
| CONFIRMED | Staff and client lifecycle management, including create/invite/disable flows. | SRC-001 §5.7–§5.8; SRC-002 §05.7–§05.8 |
| CONFIRMED | Permissioned file sharing and client file upload where upload is allowed; files are not directly public and downloads use temporary secure URLs after authorization. | SRC-001 §5.9–§5.10; SRC-002 §05.9–§05.10 |
| CONFIRMED | Simple tickets with client creation/replies/attachments and staff assignment, status, priority, and closure. | SRC-001 §5.11–§5.12; SRC-002 §05.11–§05.12 |
| CONFIRMED | Quote and PI creation plus branded PDF download. | SRC-001 §5.13–§5.15; SRC-002 §05.13–§05.15 |
| CONFIRMED | Lemon Squeezy subscription onboarding and lifecycle handling; a tenant is suspended on actual expiry while its data is retained for subscription recovery. | SRC-001 §6; SRC-002 §06 |
| CONFIRMED | Audit logging and named security focus areas, including tenant isolation, authorization, file permission, webhook signature, and IDOR protection. | SRC-001 §12–§13; SRC-002 §12–§13 |
| CONFIRMED | The installed deployment must be self-hostable in the customer's AWS account and must not require remote licence activation, call-home, or a developer-controlled runtime backend. | SRC-001 §7–§8; SRC-002 §07–§08 |

## Source recommendations, not architecture decisions

| Status | Recommendation | Source |
| --- | --- | --- |
| PROPOSED | Next.js/React/TypeScript/Tailwind/shadcn-ui frontend; NestJS REST backend; PostgreSQL, Redis, S3; HTML/Playwright PDF; Docker/AWS deployment. | SRC-001 §9–§10; SRC-002 §09–§10 |
| PROPOSED | Tenant ID, application isolation, PostgreSQL RLS, and cross-tenant tests as layered isolation measures. | SRC-001 §9, §11.1; SRC-002 §09, §11.1 |
| PROPOSED | CloudFront multi-tenant domain handling and the proposed webhook reliability controls. | SRC-001 §11.2, §11.5; SRC-002 §11.2, §11.5 |

## Not established

| Status | Topic |
| --- | --- |
| OPEN | Tenant plans, pricing, quotas, entitlement rules, trials, taxes, and subscription change rules. |
| OPEN | Authentication method, account recovery, exact authorization matrix, data-retention periods, file-size/type limits, and external-email behavior. |
| OPEN | Accessibility, localization strategy, legal/compliance requirements, supported browsers, performance/availability targets, backup/disaster recovery, and acceptance criteria. |
